# Livre Blanc Wallets - DustEthic

**Statut : Phase 0 - conception** · **Version : v0.1-draft** · **Date : 2026-06-11** · **Licence : CC BY 4.0**

> Spécification technique à destination des équipes développant un wallet. Pour une vue d'ensemble non technique, lisez d'abord le [Guide](../guide/guide-fr.md).

## Sommaire

- [À propos de ce document](#à-propos-de-ce-document)
- [Conventions de langage](#conventions-de-langage)
- [Le modèle en bref](#le-modèle-en-bref)
- [Périmètre normatif](#périmètre-normatif)
- [Les six principes](#les-six-principes)
- [Architecture du flux](#architecture-du-flux)
- [Mode immédiat](#mode-immédiat)
- [Mode agrégé](#mode-agrégé)
- [Compatibilité des wallets](#compatibilité-des-wallets)
- [Prise en charge des frais réseau](#prise-en-charge-des-frais-réseau)
- [Actifs et réseaux supportés](#actifs-et-réseaux-supportés)
- [Reçu et traçabilité](#reçu-et-traçabilité)
- [Sécurité](#sécurité)
- [Liste des ONG](#liste-des-ong)
- [Conformité au standard](#conformité-au-standard)
- [Statut, versionnage et contribution](#statut-versionnage-et-contribution)

## À propos de ce document

Ce livre blanc précise comment un wallet implémente le standard DustEthic. Il s'adresse à des développeurs et architectes wallet.

Les points encore ouverts à ce stade de la conception sont signalés ainsi :

> **À définir (v0.x)** : description de la question ouverte.

Ces encadrés ne sont pas des oublis : ce sont des décisions de conception non encore arrêtées, exposées en clair conformément à la transparence radicale du projet. Toute proposition pour les trancher est bienvenue par Issue ou Pull Request.

## Conventions de langage

Les mots-clés **DOIT**, **NE DOIT PAS**, **DEVRAIT**, **NE DEVRAIT PAS** et **PEUT** sont à interpréter au sens de la RFC 2119 / BCP 14. Une implémentation conforme respecte toutes les exigences **DOIT** et **NE DOIT PAS**.

## Le modèle en bref

DustEthic décrit comment un wallet transforme les soldes résiduels (dusts) d'un utilisateur en don direct à une ONG, sans intermédiaire, sans commission, sans token. Le wallet est l'acteur central : il détecte, présente, prépare et envoie l'opération après signature, sans jamais détenir les fonds en dépôt.

La formule de répartition est : **montant brut - frais réseau = net pour l'ONG**. Aucune autre déduction n'est autorisée.

## Périmètre normatif

**Le standard définit (le wallet DOIT s'y conformer) :**

- L'interface utilisateur minimale : détection des dusts, consentement explicite, signature.
- La formule de répartition : montant brut - frais réseau = net ONG, sans commission.
- Les exigences de transparence on-chain : traçabilité complète des flux.
- Les exigences de sécurité : non-custodial, opt-in, opérations atomiques.
- La liste blanche initiale d'actifs et de réseaux supportés.

**Le standard laisse libre (au choix du wallet ou de l'utilisateur) :**

- Le choix de l'ONG ou de la cause, par l'utilisateur.
- Le design et la marque du wallet.
- Le mode d'intégration retenu (immédiat ou agrégé).
- Le seuil de dust, configurable par l'utilisateur.
- Les services tiers utilisés pour la prise en charge des frais réseau.

## Les six principes

1. **Non-custodial.** Les fonds DOIVENT rester dans le wallet de l'utilisateur jusqu'à la signature explicite. Aucun acteur NE DOIT détenir les fonds en dépôt à un moment quelconque.
2. **Zéro commission.** Le wallet NE DOIT extraire aucune valeur économique sur le don. Une implémentation prélevant une commission n'est pas conforme.
3. **Opt-in fort.** Chaque don DOIT recevoir un consentement explicite. Aucune automatisation cachée. Un mode périodique PEUT être proposé ; il DOIT rester optionnel et révocable en un geste.
4. **Transparence on-chain.** Toutes les transactions DOIVENT être publiques et auditables sur l'explorateur de la blockchain. Aucune étape intermédiaire opaque.
5. **Modularité.** Le wallet PEUT être un EOA classique, un smart account ERC-4337, ou un EOA temporairement smart via EIP-7702.
6. **Minimalisme.** Le standard n'introduit ni token, ni fonds géré, ni nouvel acteur intermédiaire. Il s'appuie sur les briques existantes de l'écosystème.

## Architecture du flux

Le parcours type d'un don comporte cinq étapes, identiques quel que soit le mode.

1. **Détection.** Le wallet liste les soldes par token et par réseau, et identifie ceux situés sous le seuil configuré par l'utilisateur.
2. **Configuration.** L'utilisateur sélectionne les tokens à inclure, fixe son seuil de dust et choisit son mode de balayage (manuel ou périodique).
3. **Choix de l'ONG.** L'utilisateur sélectionne une organisation dans une liste publique vérifiable, ou saisit une adresse de réception personnalisée.
4. **Signature.** L'utilisateur signe l'opération. Selon la configuration du wallet, les frais réseau sont pris en charge en stablecoin, sponsorisés, ou payés en jeton natif.
5. **Trace.** La transaction apparaît immédiatement sur l'explorateur. Un reçu est généré localement par le wallet.

> **À définir (v0.x)** : le seuil de dust par défaut recommandé. La valeur reste configurable par l'utilisateur dans tous les cas ; seul un défaut conseillé reste à fixer.

## Mode immédiat

Mode par défaut.

- **Principe.** Le dust part directement du wallet de l'utilisateur vers le wallet de l'ONG, en une seule transaction. Pas d'agrégation, pas d'attente.
- **Quand l'utiliser.** Pour la majorité des cas : dusts dont la valeur dépasse le coût d'un paymaster (typiquement supérieurs à quelques cents sur L2).
- **Flux technique.** Wallet → (paymaster pour frais réseau si nécessaire) → ONG. Une seule signature, une seule transaction on-chain.
- **Avantages.** Don immédiat, audit trivial, pas d'attente, aucun smart contract intermédiaire à déployer ou auditer.

## Mode agrégé

Mode optionnel. C'est un raffinement, pas le cœur du standard.

- **Principe.** Plusieurs dusts sont rassemblés dans un smart contract d'agrégation sur une période courte, puis transférés à l'ONG en une seule transaction de sortie.
- **Quand l'utiliser.** Pour les dusts vraiment microscopiques (inférieurs au coût d'un paymaster), les tokens illiquides nécessitant un swap groupé, ou la simplification comptable côté ONG (une transaction au lieu de cent).
- **Flux technique.** Wallets → smart contract d'agrégation → ONG.
- **Non-custodial.** Le smart contract DOIT rester non-custodial : les fonds DOIVENT rester libérables par l'utilisateur tant que la fenêtre d'agrégation est ouverte.
- **Avantages.** Frais réseau mutualisés, meilleure efficacité, comptabilité ONG simplifiée.

> **À définir (v0.x)** : la durée de la fenêtre d'agrégation, l'interface de référence du smart contract d'agrégation, ses exigences d'audit, et le comportement exact en cas de swap de tokens illiquides.

## Compatibilité des wallets

Le standard n'impose pas un modèle de signature unique. Trois architectures sont supportées.

- **EOA classique.** Wallet à clé privée. L'utilisateur signe une transaction classique. Les frais réseau sont payés en jeton natif, sauf prise en charge par un service tiers.
- **Smart account (ERC-4337).** L'utilisateur signe une UserOperation, traitée par un bundler. Un paymaster PEUT sponsoriser les frais ou les faire payer en stablecoin.
- **EOA temporaire smart (EIP-7702).** Wallet classique agissant ponctuellement comme un smart account pour une transaction. Combine la simplicité de l'EOA et la flexibilité du smart account.

## Prise en charge des frais réseau

Le standard ne prescrit pas un mécanisme unique de paiement des frais. Selon l'architecture et la configuration du wallet, les frais réseau PEUVENT être :

- payés en jeton natif du réseau par l'utilisateur ;
- sponsorisés par un paymaster ;
- payés en stablecoin via un paymaster.

L'objectif est que l'utilisateur n'ait pas nécessairement besoin de détenir le jeton natif du réseau pour faire un don. Le choix du service tiers est laissé libre au wallet.

## Actifs et réseaux supportés

Le standard prévoit une liste blanche initiale d'actifs et de réseaux. Son rôle est de cadrer les premières implémentations sur des actifs et des réseaux éprouvés.

> **À définir (v0.x)** : la composition exacte de la liste blanche initiale (actifs et réseaux), ainsi que les critères d'ajout et le processus de mise à jour. Elle sera arrêtée publiquement avant la première implémentation de référence.

## Reçu et traçabilité

Chaque don DOIT être traçable on-chain : la transaction est publique sur l'explorateur du réseau concerné. En complément, le wallet DEVRAIT générer un reçu localement pour l'utilisateur.

> **À définir (v0.x)** : le contenu minimal du reçu local (champs requis) et son format, afin d'assurer une cohérence entre wallets.

## Sécurité

- **Non-custodial.** À aucun moment un tiers ne détient les fonds. Voir principe 1.
- **Opt-in.** Aucune opération sans consentement explicite. Voir principe 3.
- **Atomicité.** Les opérations DEVRAIENT être atomiques : soit le don aboutit, soit l'état de l'utilisateur reste inchangé, sans état intermédiaire exploitable.

> **À définir (v0.x)** : les exigences d'atomicité précises pour le mode agrégé et les cas multi-réseaux.

## Liste des ONG

L'utilisateur choisit librement le bénéficiaire, soit dans une liste publique vérifiable, soit via une adresse personnalisée. La liste publique a pour but de réduire le risque d'erreur d'adresse et de fournir des bénéficiaires aux adresses vérifiées.

> **À définir (v0.x)** : la gouvernance de la liste publique d'ONG (qui vérifie, selon quels critères, par quel processus d'ajout et de retrait, et où elle est hébergée).

## Conformité au standard

Une implémentation est conforme si elle respecte l'ensemble des conditions suivantes :

- Elle ne prélève aucune commission ; seul le coût des frais réseau est déduit du montant brut.
- Elle ne détient jamais les fonds en dépôt (non-custodial).
- Elle exige un consentement explicite pour chaque don, et tout mode périodique est optionnel et révocable.
- Elle assure une traçabilité on-chain complète, sans étape intermédiaire opaque.
- Elle respecte la liste blanche initiale d'actifs et de réseaux.

Une EIP formelle est en cours de rédaction pour proposer ce standard à l'écosystème Ethereum. Elle n'est pas publiée à ce stade.

## Statut, versionnage et contribution

Ce document est un brouillon (v0.1-draft), en Phase 0, appelé à évoluer publiquement. Les versions successives seront tracées dans le `CHANGELOG.md` du dépôt.

Toute proposition d'amélioration ou critique se soumet par Issue ou Pull Request sur le dépôt [DustEthic/docs](https://github.com/DustEthic).

---

*Ce document est publié sous licence [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). DustEthic v0.1-draft, Phase 0.*
