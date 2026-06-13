# Standard DustEthic - brouillon de travail

Date : 2026-06-13  
Statut : **draft**, non audité, non final, non utilisable en production.

## Objectif

Définir un standard ouvert permettant de transformer des dusts crypto en dons vérifiables pour des ONG, sans imposer un fournisseur unique et sans transformer DustEthic en dépositaire.

## Définitions

- **Dust** : petit solde crypto dont l'utilisation isolée est économiquement ou techniquement peu rationnelle.
- **Intention de don** : autorisation explicite donnée par l'utilisateur pour affecter un montant, un actif et un réseau à une ONG ou campagne.
- **Lot** : ensemble d'intentions ou de micro-transferts agrégés selon une règle publiée.
- **Relayeur** : opérateur technique qui orchestre l'agrégation, l'exécution ou la publication des preuves selon une politique publique.
- **ONG bénéficiaire** : organisation qui reçoit le net du lot sur une adresse vérifiée.

## Flux cible

1. Le wallet détecte des dusts ou laisse l'utilisateur choisir un actif.
2. L'utilisateur choisit une ONG ou une campagne.
3. Le wallet affiche les frais, les limites, les risques et le statut réel du don.
4. L'utilisateur signe une intention ou une autorisation limitée.
5. Les intentions sont agrégées par actif et par réseau.
6. Un lot est exécuté seulement si les seuils de viabilité sont atteints.
7. L'ONG reçoit le net.
8. Une preuve publique est publiée.

## Formule minimale

```text
Net ONG = montant agrégé - gas remboursé - commission éventuelle - réserve technique éventuelle
```

Toutes les valeurs doivent être publiées dans l'actif concerné. Les équivalents fiat peuvent aider à la lecture, mais ne doivent pas être la source de vérité.

## Politique de frais

Un lot conforme doit publier avant exécution :

- le réseau ;
- l'actif ;
- le seuil minimum ;
- la fenêtre d'agrégation ;
- la politique gas ;
- la commission éventuelle ;
- le plafond total de frais ;
- les risques d'échec ou d'expiration.

Règle de travail : viser des frais totaux bas, visibles et plafonnés. Aucun frais caché n'est conforme.

## Exigences minimales pour une preuve de lot

Une preuve DustEthic doit inclure au minimum :

- identifiant de lot ;
- version du standard ;
- date d'exécution ;
- réseau ;
- actif ;
- montant agrégé ;
- gas réel ;
- commission éventuelle ;
- réserve technique éventuelle ;
- net ONG ;
- adresse bénéficiaire ;
- hash de transaction ou preuve équivalente ;
- politique de frais utilisée ;
- statut : exécuté, expiré, annulé ou échoué.

## Actifs acceptables en priorité

Le standard doit être prudent. Les premiers tests doivent privilégier des actifs liquides, largement supportés et traçables : ETH, USDC, USDT et équivalents techniquement simples.

À refuser par défaut : tokens illiquides, tokens taxés, honeypots, actifs sans marché utile, actifs avec règles de transfert non standard, réseaux sans explorateur fiable ou frais supérieurs au montant agrégé.

## Non-custodial par défaut

L'architecture cible doit limiter les pouvoirs unilatéraux :

- l'utilisateur conserve ses clés ;
- les autorisations sont ciblées et limitées ;
- les fonds ne doivent pas dépendre d'une garde opaque ;
- les contrats, logs ou opérateurs doivent être auditables ;
- les rôles d'administration doivent être minimisés ou explicitement documentés.

## Points ouverts

- Format exact de l'intention de don.
- Stratégie de révocation ou expiration.
- Liste de réseaux de test.
- Politique de plafonnement des frais.
- Modèle pour reçus, exports ONG et fiscalité.
- Qualification juridique des relayeurs selon les pays.
- Gouvernance des versions du standard.

## Anti-objectifs

DustEthic ne doit pas devenir :

- un token spéculatif ;
- une collecte opaque ;
- une promesse d'impact non prouvée ;
- un service financier présenté comme prêt avant audit ;
- une solution qui masque les frais blockchain ;
- une dépendance à un acteur unique.
