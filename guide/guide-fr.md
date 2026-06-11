# Guide DustEthic

**Statut : Phase 0 - conception** · **Version : v0.1-draft** · **Date : 2026-06-11** · **Licence : CC BY 4.0**

> Document d'entrée pour comprendre DustEthic dans son ensemble : le problème, le modèle, les acteurs et le parcours d'un don. La spécification technique détaillée se trouve dans le [Livre Blanc Wallets](../livre-blanc-wallets/livre-blanc-wallets-fr.md).

## Sommaire

- [DustEthic en une phrase](#dustethic-en-une-phrase)
- [Le problème des dusts](#le-problème-des-dusts)
- [Un standard, pas une plateforme](#un-standard-pas-une-plateforme)
- [Les acteurs](#les-acteurs)
- [Le parcours d'un don](#le-parcours-dun-don)
- [Les deux modes](#les-deux-modes)
- [Le périmètre du standard](#le-périmètre-du-standard)
- [Les six principes](#les-six-principes)
- [Compatibilité des wallets](#compatibilité-des-wallets)
- [Statut du projet](#statut-du-projet)
- [Pour aller plus loin](#pour-aller-plus-loin)

## DustEthic en une phrase

DustEthic est une spécification ouverte qui décrit comment un wallet crypto peut transformer les soldes résiduels d'un utilisateur en don direct à une ONG, sans intermédiaire, sans commission et sans token.

Ce n'est pas un service, ni une entreprise, ni une collecte. C'est une convention technique publique que n'importe quel wallet peut implémenter.

## Le problème des dusts

Ouvrez votre wallet crypto et comptez les résidus : quelques cents d'un stablecoin oubliés sur un réseau, des fractions de jeton coincées sur un autre, des tokens d'airdrops anciens devenus illiquides. Individuellement, ces montants ne valent presque rien. Vous ne pouvez ni les vendre (les frais de réseau dépassent leur valeur), ni les utiliser, ni les balayer simplement vers une cause.

Ces "dusts" dorment et encombrent. Ce n'est pas un cas isolé : c'est la situation dans une grande partie des wallets crypto. À l'échelle de l'écosystème, cela représente une valeur fractionnée considérable, aujourd'hui inutilisable.

## Un standard, pas une plateforme

DustEthic ne collecte pas les dons et ne détient jamais de fonds. Le projet définit une règle du jeu commune, puis laisse chaque acteur autonome.

Concrètement, cela veut dire qu'il n'y a aucun intermédiaire qui s'interpose entre le donateur et l'ONG. Le don part du wallet de l'utilisateur vers l'adresse de l'ONG. DustEthic décrit comment cette opération doit se dérouler pour être transparente, sûre et gratuite, rien de plus.

## Les acteurs

Le modèle ne fait intervenir que trois rôles, sans nouvel intermédiaire.

**L'utilisateur (le donateur)** possède les dusts. Il choisit l'ONG, configure ses préférences et signe l'opération. Ses fonds restent dans son wallet jusqu'à la signature explicite.

**Le wallet** est l'acteur central. C'est lui qui implémente le standard : il détecte les dusts, présente l'interface, prépare l'opération et l'envoie après signature. Il ne détient jamais les fonds en dépôt.

**L'ONG (le bénéficiaire)** publie des adresses de réception vérifiables et reçoit les dons directement, sans déduction de commission.

## Le parcours d'un don

Le flux type se décrit en cinq étapes, identiques quel que soit le mode choisi.

1. **Détection.** Le wallet liste les soldes par token et par réseau, et identifie ceux situés sous un seuil configuré par l'utilisateur (par exemple : moins de 2 USD par token).
2. **Configuration.** L'utilisateur sélectionne les tokens à inclure, fixe son seuil de dust et choisit son mode de balayage (manuel ou périodique).
3. **Choix de l'ONG.** L'utilisateur sélectionne une organisation dans une liste publique vérifiable, ou saisit une adresse de réception personnalisée.
4. **Signature.** L'utilisateur signe l'opération dans son wallet, comme une transaction normale. Selon la configuration du wallet et les services tiers utilisés, les frais réseau sont pris en charge en stablecoin, sponsorisés, ou payés en jeton natif. L'utilisateur n'a pas nécessairement besoin de détenir le jeton natif du réseau.
5. **Trace.** La transaction apparaît immédiatement sur l'explorateur de la blockchain. Un reçu est généré localement par le wallet.

La formule de répartition est volontairement simple : **montant brut - frais réseau = net pour l'ONG**. DustEthic n'ajoute aucune commission. Une implémentation qui prélèverait une commission ne serait pas conforme au standard.

## Les deux modes

Le standard prévoit deux modes d'intégration, au choix du wallet.

**Mode immédiat (par défaut).** Chaque dust part directement du wallet de l'utilisateur vers le wallet de l'ONG, en une seule transaction, sans attente ni agrégation. C'est le mode adapté à la majorité des cas, grâce aux évolutions récentes des wallets et des services de prise en charge des frais réseau.

**Mode agrégé (optionnel).** Pour les dusts vraiment microscopiques ou les tokens illiquides, le wallet peut utiliser un smart contract d'agrégation qui regroupe les dons sur une période courte avant un transfert unique vers l'ONG. Ce smart contract est non-custodial : les fonds restent récupérables par l'utilisateur tant que la fenêtre d'agrégation est ouverte. C'est un raffinement, pas le cœur du standard.

## Le périmètre du standard

DustEthic décrit comment se fait un don, pas pour qui ni avec qui. Cette séparation stricte protège l'autonomie de chaque acteur.

**Ce que le standard définit :**

- L'interface utilisateur minimale dans le wallet (détection, consentement, signature).
- La formule de répartition des fonds (montant brut - frais réseau = net ONG).
- Les exigences de transparence on-chain (traçabilité complète des flux).
- Les exigences de sécurité (non-custodial, opt-in, opérations atomiques).
- La liste blanche initiale d'actifs et de réseaux supportés.

**Ce que le standard laisse libre :**

- Le choix de l'ONG ou de la cause par l'utilisateur.
- Le design et la marque du wallet qui implémente.
- Le mode d'intégration retenu (immédiat ou agrégé).
- Le seuil de dust, configurable par l'utilisateur.
- Les services tiers utilisés pour la prise en charge des frais réseau.

## Les six principes

Six principes sont non-négociables. Une implémentation qui en enfreint un n'est pas conforme.

1. **Non-custodial.** Les fonds restent dans le wallet de l'utilisateur jusqu'à la signature explicite. Aucun acteur ne détient les fonds en dépôt à un moment quelconque.
2. **Zéro commission.** DustEthic n'extrait aucune valeur économique.
3. **Opt-in fort.** Chaque don exige un consentement explicite. Aucune automatisation cachée. Le mode périodique reste optionnel et révocable en un clic.
4. **Transparence on-chain.** Toutes les transactions sont publiques et auditables sur l'explorateur de la blockchain. Aucune étape intermédiaire opaque.
5. **Modularité.** Le standard est compatible avec les wallets EOA classiques, les smart accounts ERC-4337 et les EOA temporairement smart via EIP-7702.
6. **Minimalisme.** Zéro token, zéro fonds géré, zéro nouvel acteur intermédiaire. Le standard s'appuie uniquement sur les briques existantes de l'écosystème.

## Compatibilité des wallets

Le standard n'impose pas un modèle de signature unique. Trois architectures de wallet sont supportées.

- **EOA classique.** Wallet à clé privée traditionnel. L'utilisateur signe une transaction classique. Les frais réseau sont payés en jeton natif, sauf si un service tiers les prend en charge.
- **Smart account (ERC-4337).** Wallet basé sur un smart contract. L'utilisateur signe une UserOperation, traitée par un bundler. Un paymaster peut sponsoriser les frais ou les faire payer en stablecoin.
- **EOA temporaire smart (EIP-7702).** Wallet classique agissant ponctuellement comme un smart account pour une transaction. Combine la simplicité de l'EOA et la flexibilité du smart account.

## Statut du projet

DustEthic est en Phase 0 : phase de conception, bénévole et non lucrative. Il n'y a ni token, ni collecte, ni produit commercial.

Le projet applique une transparence radicale : la spécification est publique sur GitHub, en français et en anglais, et toute proposition d'amélioration ou critique peut être soumise par Issue ou Pull Request. Le standard est un brouillon (v0.1-draft), incomplet et appelé à évoluer publiquement.

## Pour aller plus loin

- **[Livre Blanc Wallets](../livre-blanc-wallets/livre-blanc-wallets-fr.md)** : la spécification technique détaillée, point d'entrée recommandé pour les équipes qui développent un wallet.
- **Site officiel** : https://dustethic.org
- **Communauté** : [GitHub](https://github.com/DustEthic) · [Discord](https://discord.gg/fVFc26GV) · [Bluesky](https://bsky.app/profile/dustethic.bsky.social)

---

*Ce document est publié sous licence [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). DustEthic v0.1-draft, Phase 0.*
