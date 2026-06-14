# DustEthic

**Standard ouvert pour transformer des dusts crypto en dons vérifiables pour des ONG.**

Date de reprise : 2026-06-14  
Statut : **Phase 0 - standard à réaligner, pas un service en production**.

## En une phrase

DustEthic ne collecte pas les dons. DustEthic définit la règle du jeu pour que des petits soldes crypto inutilisés puissent devenir des dons ONG vérifiables, avec consentement clair, frais visibles, agrégation transparente et preuve publique.

## ADN du projet

Le fichier prioritaire de reprise est [ADN.md](ADN.md).

Règle fondatrice : **on raisonne en unités crypto, pas en fiat**. Les équivalents CHF, EUR ou USD peuvent aider à lire, mais ils ne sont pas la source de vérité.

## Le problème

Un portefeuille crypto accumule souvent des restes : petits soldes après échanges, airdrops inutiles, fonds oubliés sur des réseaux peu utilisés. Pris séparément, ces montants peuvent coûter plus cher à déplacer que leur valeur utile. Agrégés proprement avec d'autres montants du même actif, ils peuvent devenir utiles.

## La proposition

DustEthic n'est pas une plateforme fermée. C'est une spécification ouverte que plusieurs acteurs pourraient implémenter : wallets, relayeurs, ONG, auditeurs et développeurs.

Le standard doit clarifier :

1. l'intention de don ;
2. l'agrégation par actif et par réseau ;
3. la politique gas ;
4. les frais et commissions éventuelles ;
5. le montant net reçu par l'ONG ;
6. la preuve publique ;
7. les rôles et responsabilités ;
8. les limites juridiques, techniques et opérationnelles.

## Principes non négociables

- **Consentement explicite** : aucun don sans action claire de l'utilisateur.
- **Crypto comme source de vérité** : la répartition se fait dans l'actif donné.
- **Agrégation conditionnelle** : un lot ne doit partir que s'il est économiquement cohérent.
- **Frais visibles** : gas, commission, réserve éventuelle et net ONG doivent être compréhensibles.
- **Sans garde opaque** : le modèle cible doit éviter qu'un acteur puisse disposer seul des fonds.
- **Preuve vérifiable** : chaque lot exécuté doit produire une trace lisible et techniquement contrôlable.
- **Pas de token DustEthic** : le projet n'a pas de token et n'en a pas besoin pour exister.
- **Pas de promesse d'impact** : l'impact doit être prouvé par les lots exécutés, pas annoncé par slogan.

## Statut réel

- Phase : **Phase 0 - reprise du standard et de la documentation**.
- Collecte de fonds : **aucune collecte par DustEthic**.
- Partenaires : **aucun partenaire wallet ou ONG officiellement validé à ce stade dans ce dépôt**.
- Mainnet : **aucun contrat DustEthic audité ou recommandé en production**.
- Priorité actuelle : **restaurer l'ADN et le standard avant de reprendre le prototype technique**.

## Documents principaux

- [ADN DustEthic](ADN.md)
- [Standard DustEthic](STANDARD.md)
- [Feuille de route](ROADMAP.md)
- [Veille technique](TECHNICAL-WATCH.md)
- [Communauté et Discord](COMMUNITY.md)
- [Identité visuelle](BRAND.md)
- [Sources officielles](SOURCES.md)
- [Contribuer](CONTRIBUTING.md)
- [Sécurité](SECURITY.md)

## Archives à préserver

Les archives historiques ne doivent pas être effacées : elles contiennent une partie importante de l'ADN du projet, notamment le guide v1.1-draft et les livres blancs relayeurs/wallets.

Chemin : [archives/](archives/)

## Position légale et prudence

DustEthic publie une spécification ouverte. Selon la juridiction et l'architecture retenue, un opérateur d'agrégation, un relayeur, un wallet ou une ONG peut relever de règles applicables aux paiements, dons, actifs numériques, fiscalité ou intermédiaires. Toute expérimentation réelle doit être validée par des conseils qualifiés avant lancement.

> Chaque grain compte, seulement si la preuve tient.
