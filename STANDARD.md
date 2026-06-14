# Standard DustEthic - brouillon de reprise

Date : 2026-06-14  
Statut : **draft**, Phase 0, non audité, non final, non utilisable en production.

Ce document réaligne le standard avec l'ADN historique du projet. Il doit être lu avec [ADN.md](ADN.md).

## Objectif

Définir un standard ouvert permettant de transformer des dusts crypto en dons vérifiables pour des ONG, sans imposer un fournisseur unique et sans transformer DustEthic en dépositaire.

## Principe fondamental

DustEthic raisonne en unités crypto, pas en fiat.

Si un lot agrège un actif donné, la répartition du lot, les frais, la commission éventuelle, la réserve technique éventuelle et le net ONG doivent être exprimés dans cet actif ou en pourcentage de cet actif. Les équivalents fiat peuvent aider à la lecture, mais ne doivent pas être la source de vérité.

## Définitions

- **Dust** : petit solde crypto dont l'utilisation isolée est économiquement ou techniquement peu rationnelle.
- **Intention de don** : autorisation explicite donnée par l'utilisateur pour affecter un montant, un actif et un réseau à une ONG ou campagne.
- **Lot** : ensemble d'intentions ou de micro-transferts agrégés selon une règle publiée.
- **Relayeur** : opérateur technique qui orchestre l'agrégation, l'exécution ou la publication des preuves selon une politique publique.
- **ONG bénéficiaire** : organisation qui reçoit le net du lot sur une adresse vérifiée.
- **Plafond de campagne** : limite publique maximale des coûts totaux d'un lot ou d'une campagne.

## Flux cible

1. Le wallet détecte des dusts ou laisse l'utilisateur choisir un actif.
2. L'utilisateur choisit une ONG ou une campagne.
3. Le wallet affiche le réseau, l'actif, les frais, les limites et le statut réel de l'opération.
4. L'utilisateur signe une intention ou une autorisation limitée.
5. Les intentions sont agrégées par actif et par réseau.
6. Un lot est exécuté seulement si les seuils de viabilité sont atteints.
7. L'ONG reçoit le net.
8. Une preuve publique est publiée.

## Formule standard

```text
Net ONG = montant agrégé - gas remboursé - commission relayeur éventuelle - réserve technique éventuelle
```

Toutes les valeurs doivent être publiées dans l'actif concerné. Les équivalents fiat peuvent être affichés en complément, pas comme source de vérité.

## Politique gas v0.2 - base de travail

### L2-first

Les opérations doivent privilégier les réseaux à faibles frais, notamment les L2 EVM quand les outils, les explorateurs et la sécurité sont suffisants.

### Exécution conditionnelle

Un lot ne doit être exécuté que si le ratio suivant atteint un seuil publié :

```text
montant agrégé / gas estimé >= T
```

Repère de travail historique : **T >= 30**, soit un gas estimé proche de 3 % maximum du lot avant commission.

### Pool gas relayeur

Le modèle privilégié est que le relayeur maintienne un pool du jeton natif nécessaire au gas. Dans ce cas, les dons ne sont pas convertis pour financer le gas. Le gas réel est remboursé selon une règle publique et vérifiable.

### Filet de sécurité

Si le pool gas est insuffisant, une conversion minimale peut être envisagée uniquement si elle est :

- documentée ;
- limitée ;
- visible publiquement ;
- techniquement vérifiable ;
- compatible avec le plafond de campagne.

### Plafond public

Un lot conforme doit publier un plafond de coûts avant exécution. Repère historique :

```text
gas + commission + réserve technique <= 15 % du montant agrégé
```

Ce seuil est un repère de travail, pas une promesse commerciale. Le standard doit préciser les variantes acceptables.

## Politique de frais

Un lot conforme doit publier avant exécution :

- le réseau ;
- l'actif ;
- le seuil minimum ;
- la fenêtre d'agrégation ;
- la politique gas ;
- la commission éventuelle ;
- la réserve technique éventuelle ;
- le plafond total de frais ;
- les risques d'échec, d'expiration ou de non-exécution.

Aucun frais caché n'est conforme à DustEthic.

## Acteurs et responsabilités

### Donateur

Le donateur doit pouvoir choisir l'actif, le réseau, le bénéficiaire, comprendre les frais, signer explicitement et retrouver une preuve.

### Wallet ou plateforme

Le wallet est le point d'entrée UX. Il doit éviter les permissions dangereuses, afficher le statut réel et ne jamais présenter une intention comme un don exécuté.

### Relayeur

Le relayeur doit publier son barème, ses réseaux supportés, son option gas, ses seuils, ses plafonds, ses journaux utiles, ses liens on-chain et son architecture.

Le relayeur ne doit pas être une boîte noire.

### ONG

L'ONG doit disposer d'une adresse vérifiée, comprendre les frais, recevoir le net, accéder aux preuves et définir sa propre politique de conservation ou conversion.

## Non-custodial par défaut

L'architecture cible doit limiter les pouvoirs unilatéraux :

- l'utilisateur conserve ses clés ;
- les autorisations sont ciblées et limitées ;
- les fonds ne doivent pas dépendre d'une garde opaque ;
- les contrats, logs ou opérateurs doivent être auditables ;
- les rôles d'administration doivent être minimisés ou explicitement documentés.

## Exigences minimales pour une preuve de lot

Une preuve DustEthic doit inclure au minimum :

- identifiant de lot ;
- version du standard ;
- date d'exécution ;
- réseau ;
- actif ;
- montant agrégé ;
- gas estimé ;
- gas réel ;
- commission éventuelle ;
- réserve technique éventuelle ;
- net ONG ;
- adresse bénéficiaire ;
- hash de transaction ou preuve équivalente ;
- politique de frais utilisée ;
- relayeur utilisé si applicable ;
- statut : exécuté, expiré, annulé ou échoué.

## Actifs acceptables en priorité

Les premiers tests doivent privilégier des actifs liquides, largement supportés et traçables : ETH, USDC, USDT et équivalents techniquement simples.

À refuser par défaut : tokens illiquides, tokens taxés, honeypots, actifs sans marché utile, actifs avec règles de transfert non standard, réseaux sans explorateur fiable ou frais supérieurs au montant agrégé.

## Points ouverts prioritaires

- Format exact de l'intention de don.
- Stratégie de révocation ou expiration.
- Schéma JSON de preuve de lot.
- Politique de plafonnement des frais.
- Modèle pour reçus, exports ONG et fiscalité.
- Qualification juridique des relayeurs, wallets et ONG selon les pays.
- Gouvernance des versions du standard.

## Anti-objectifs

DustEthic ne doit pas devenir :

- un token spéculatif ;
- une collecte opaque ;
- une promesse d'impact non prouvée ;
- un service présenté comme prêt avant audit ;
- une solution qui masque les frais blockchain ;
- une dépendance à un acteur unique ;
- une boîte noire qui capte la valeur au nom du bien commun.
