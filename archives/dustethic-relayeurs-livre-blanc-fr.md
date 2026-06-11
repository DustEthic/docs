# [2025-12-10] - Livre blanc DustEthic - Relayeurs

## 0. Avertissement important - statut du document

Ce document mélange deux types d'informations:

1. **Éléments actuels et vérifiables**
   - Description du Standard DustEthic tel que publié sur les dépôts officiels et le site dustethic.org.
   - Références techniques publiques sur les wallets, l'account abstraction, ERC 4337, EIP 7702, les paymasters, les relayeurs, etc.

2. **Projections, exemples et hypothèses de conception**
   - Tous les montants, pourcentages, taux d'adoption, volumes de dons, modèles de commissions, scénarios de gains et roadmaps présentés dans ce document sont des **illustrations de travail**.
   - Ils ne décrivent pas la performance réelle d'un wallet, d'un relayer, d'une ONG ou du projet DustEthic, et peuvent être revus ou abandonnés.

En conséquence:

- Ce document **ne constitue pas**:
  - une promesse de gains, de rendement ou de performance future,
  - une offre de produit financier,
  - un engagement contractuel de DustEthic ou d'un partenaire.

- Chaque acteur concerné (wallet, relayer, ONG, utilisateur) doit:
  - effectuer ses propres vérifications techniques, juridiques, fiscales et réglementaires,
  - adapter ou rejeter les exemples proposés si ceux ci ne sont pas compatibles avec son contexte.

- En cas de contradiction entre ce document et:
  - le Standard DustEthic publié sur les dépôts officiels,
  - ou la documentation technique la plus récente,
  ce sont **les sources officielles les plus récentes** qui prévalent.

Ce document doit être lu comme un **support de réflexion et de conception**, et non comme une description définitive d'un système en production.

---

## 1. Résumé exécutif

Cette section du livre blanc décrit le rôle des relayeurs dans l’écosystème DustEthic, les modèles de commissions envisagés, les pistes d’intégration technique et quelques ordres de grandeur pour les gains potentiels.

Les relayeurs DustEthic s’inspirent des briques existantes de l’Account Abstraction sur Ethereum et sur les L2 (ERC-4337, EntryPoint, UserOperations, bundlers, paymasters), ainsi que des travaux autour d’EIP-7702 qui permet aux comptes externes de bénéficier temporairement de fonctionnalités de smart accounts.

Objectifs principaux :
- Rendre l’utilisation des dusts simple pour l’utilisateur final.
- Optimiser les coûts de gas via des relayeurs spécialisés.
- Offrir aux ONG des flux traçables et auditables.
- Proposer un modèle économique viable pour les opérateurs sans token spéculatif ni promesse de rendement.

---

## 2. Contexte et rôle des relayeurs DustEthic

### 2.1 Le problème des "dusts"

Les "dusts" sont des petits soldes résiduels, souvent trop faibles pour être déplacés de manière rentable à cause des frais de gas ou des frais de retrait. L’idée DustEthic est de transformer ces montants dormants en micro-dons agrégés vers des ONG partenaires.

Problème clé : le coût fixe d’une transaction on-chain reste significatif même pour de très petits montants, d’où la nécessité d’agréger des opérations et d’optimiser le gas.

### 2.2 Rôle spécifique d’un relayeur DustEthic

Un relayeur DustEthic est un opérateur technique qui :
- Reçoit des intentions de don provenant de wallets compatibles DustEthic.
- Agrège ces intentions (bundling) pour réduire le coût moyen par don.
- Soumet les transactions sur la blockchain en payant le gas au nom des utilisateurs (directement ou via un paymaster).
- Se fait rembourser le gas et sa commission via un contrat DustEthic et les flux de dons.
- Publie des preuves vérifiables de bonne exécution pour les donateurs et les ONG.

Dans l’architecture proposée, le relayeur est un composant remplaçable. Plusieurs relayeurs peuvent coexister et être mis en concurrence (frais, SLA, réseaux supportés, etc.).

---

## 3. Architecture générale proposée

### 3.1 Inspirations techniques

L’architecture DustEthic côté relayeurs s’inspire de :
- ERC-4337 : UserOperations, EntryPoint, bundlers et paymasters pour implémenter l’Account Abstraction au dessus du protocole.
- EIP-7702 : transaction de type "setCode" qui permet à un EOA de se comporter temporairement comme un smart account durant une transaction.
- Les infra existantes de relayers AA (Gelato, Biconomy, etc.) qui offrent déjà des services de transactions gasless et de paymasters sur plusieurs réseaux.

DustEthic ne réinvente pas ces briques, mais propose une couche de standardisation "orientée dons" au dessus.

### 3.2 Flux conceptuel d’un don via relayeur

Scénario simplifié :
1. L’utilisateur ouvre un wallet compatible DustEthic et voit ses dusts éligibles (par réseau et par token).
2. Il configure un don (par exemple "donner tous mes dusts USDC sur ce L2 vers l’ONG X").
3. Le wallet construit :
   - Une UserOperation ou une meta-transaction contenant :
     - L’ordre de transfert des dusts.
     - Les métadonnées DustEthic (ONG, campagne, tags).
4. Le relayeur DustEthic :
   - Reçoit et valide la demande.
   - Regroupe plusieurs opérations dans un bundle quand cela fait sens économiquement.
   - Soumet le tout via un contrat DustEthic (qui peut s’appuyer sur l’EntryPoint si l’on utilise ERC-4337).
5. Le contrat DustEthic :
   - Déplace les montants depuis les comptes des donateurs vers une adresse de collecte DustEthic pour l’ONG.
   - Enregistre les données nécessaires à la transparence (événements, journaux, identifiants de campagnes).
6. Périodiquement ou au-delà d’un seuil, le contrat transfère les fonds agrégés vers l’ONG (adresse contrôlée par l’ONG elle-même).

### 3.3 Couches logicielles

On peut décomposer un relayeur DustEthic en plusieurs couches :
- Couche 1 - "AA / Relay" : bundler et paymaster ERC-4337, ou équivalent, éventuellement opéré par un provider tiers.
- Couche 2 - "DustEthic Core" : logique spécifique aux dons en dusts (contrats, règles d’agrégation, limites, logs).
- Couche 3 - "API et intégration wallets / ONG" : endpoints pour les wallets, dashboards et exports pour les ONG, monitoring.

---

## 4. Modèles de commissions envisagés

### 4.1 Variables et notation

Pour une période donnée (par exemple un mois) :
- V : volume total de dons transitant par un relayeur (en USD équivalent).
- c : taux de commission global (par exemple de 0.5 % à 3 % du volume).
- G : coût total de gas payé par le relayeur sur la période.
- C_infra : coûts d’infrastructure et d’exploitation (serveurs, audits, support).
- R_net : revenu net du relayeur.

Formule générique :
R_net = V × c - G - C_infra

Les modèles ci-dessous se distinguent principalement par "qui supporte c" et comment il est affiché.

### 4.2 Modèle 1 - Commission côté donateur

Principe :
- À chaque don, un pourcentage du montant est prélevé pour rémunérer le relayeur (et éventuellement une quote-part pour DustEthic Core).

Exemple conceptuel :
- c = 1 % du montant donné.
- L’utilisateur voit :
  - "Vous donnez 1.00, dont 0.99 pour l’ONG et 0.01 pour l’infrastructure DustEthic."

Avantages :
- Alignement direct entre volume de dons et revenu.
- Modèle simple à comprendre.

Inconvénients :
- Certains donateurs peuvent être sensibles à l’idée de "frais sur don".
- Nécessite une communication transparente et honnête.

### 4.3 Modèle 2 - Commission côté ONG

Principe :
- L’ONG accepte que chaque don DustEthic soit crédité net de frais.
- L’utilisateur voit "Vous donnez 1.00 à l’ONG X", et l’ONG reçoit par exemple 0.99.

Avantages :
- Expérience donateur très simple.
- Proche de nombreux modèles existants de PSP et de plateformes de dons.

Inconvénients :
- Les ONG sont attentives au niveau global de frais.
- Doit être justifié par de nouveaux flux de dons "incrémentaux" (dérivés des dusts qui seraient sinon perdus).

### 4.4 Modèle 3 - Commission mixte

Principe :
- La commission est répartie entre donateur et ONG.
- Par exemple :
  - 0.5 % côté donateur.
  - 0.5 % côté ONG.

Avantages :
- Répartition plus équilibrée de l’effort.
- Permet de réduire la perception de frais pour chacun.

Inconvénients :
- Plus complexe à expliquer.
- Peut nécessiter une UX fine pour éviter toute confusion.

### 4.5 Modèle 4 - Abonnement B2B pour wallets et plateformes

Principe :
- Les wallets ou plateformes paient un abonnement (ou achètent des "crédits") à un relayeur DustEthic pour un volume de transactions inclus.
- Les coûts sont supportés au niveau B2B, éventuellement sans affichage direct de frais au donateur.

Avantages :
- Aligné avec les modèles déjà utilisés par certains providers AA et relayers.
- Prévisibilité des coûts pour les wallets partenaires.

Inconvénients :
- Demande des accords commerciaux spécifiques.
- Peut limiter DustEthic aux acteurs ayant une taille critique suffisante.

### 4.6 Combinaisons possibles

Un même écosystème DustEthic peut laisser coexister plusieurs modèles, par exemple :
- Par défaut : commission côté ONG avec plafond.
- Pour certains wallets premium : prise en charge B2B.
- Pour certaines campagnes : commission réduite ou nulle (sponsorisée par un tiers).

---

## 5. Gains potentiels pour un relayeur DustEthic

Ces scénarios sont purement illustratifs et ne constituent pas des promesses de revenus.

### 5.1 Hypothèses d’exemple

Supposons :
- Scénario A - volume faible : V = 50 000 USD par mois.
- Scénario B - volume moyen : V = 500 000 USD par mois.
- Scénario C - volume élevé : V = 5 000 000 USD par mois.
- Taux de commission : c = 1 %.
- Coûts (gas + infra) représentant 30 % de la commission brute.

Alors :
- Commission brute = V × c.
- R_net ≈ 70 % de V × c.

### 5.2 Calculs simplifiés

- Scénario A :
  - Commission brute : 50 000 × 1 % = 500.
  - R_net ≈ 350.
- Scénario B :
  - Commission brute : 500 000 × 1 % = 5 000.
  - R_net ≈ 3 500.
- Scénario C :
  - Commission brute : 5 000 000 × 1 % = 50 000.
  - R_net ≈ 35 000.

Lecture critique :
- En dessous d’un certain volume, un relayeur isolé ne finance pas facilement des audits, du support et une infra robuste.
- Au-delà d’un certain volume, un relayeur spécialisé ou un provider AA déjà en place peut trouver un intérêt économique réel.

### 5.3 Sensibilité au coût du gas

Facteurs clés :
- Réseaux ciblés (L2 peu chers vs L1 congestionnés).
- Stratégie d’agrégation (seuil minimum par bundle).
- Politique de "throttling" pour ne pas exécuter des dons une par un si le volume est trop faible.

Conclusion : dans la pratique, les relayeurs DustEthic auront intérêt à privilégier les L2 pour la majorité des flux en dusts.

---

## 6. Intégration des relayeurs dans l’écosystème DustEthic

### 6.1 Intégration côté wallets

Un wallet compatible DustEthic devrait idéalement :
- Afficher clairement :
  - Les dusts détectés et éligibles.
  - Le modèle de frais associé (côté ONG, côté donateur, mixte, B2B).
- Utiliser un SDK ou un schéma de métadonnées DustEthic standardisé pour :
  - Construire les opérations de dons.
  - Référencer le ou les relayeurs utilisés.
  - Récupérer les informations de suivi des dons (hash, statut, ONG, campagne).

Option importante :
- Laisser le choix du relayeur à l’utilisateur ou au moins le rendre visible (par exemple "Relayeur DustEthic A, L2 X, commission 1 %").

### 6.2 Intégration côté ONG

Pour les ONG, un relayeur DustEthic devrait fournir :
- Un tableau de bord :
  - Montants reçus par campagne, par token, par réseau.
  - Liste des transactions avec hash on-chain.
- Des exports (CSV, API) vers leur comptabilité ou leurs systèmes internes.
- Des options de cash-out :
  - Maintien en crypto.
  - Conversion vers fiat via des partenaires (fonctionnalité à traiter au niveau réglementaire et KYC/AML).

### 6.3 Intégration avec des providers AA existants

Un relayeur DustEthic peut être :
- Soit opéré directement par un provider AA existant (Gelato, Biconomy, etc.).
- Soit une couche DustEthic au dessus de ces providers, avec :
  - Routage multi-réseaux.
  - Normalisation des logs de dons.
  - Unification de l’UX pour les wallets et ONG.

Dans tous les cas, le "Standard DustEthic - Relayeurs" devrait définir des exigences minimales :
- Format des métadonnées.
- Événements on-chain et off-chain.
- Transparence vis-à-vis des frais et des partenaires utilisés.

---

## 7. Gouvernance, transparence et sécurité

### 7.1 Contrats et standard publics

Pour limiter les risques :
- Les contrats DustEthic utilisés par les relayeurs devraient être open source.
- Le standard "DustEthic - Relayeurs" devrait être publié publiquement (par exemple sur GitHub) et versionné.

Objectif : permettre aux auditeurs, aux ONG et aux communautés de vérifier le code et de suivre les évolutions.

### 7.2 Preuve de bonne exécution

Attendus :
- Chaque flux de dons doit être traçable :
  - Hash de transaction.
  - Montant, token, réseau.
  - Relayeur utilisé.
  - ONG bénéficiaire et éventuelle campagne.
- Les relayeurs devraient exposer :
  - Des endpoints publics pour vérifier les opérations à partir d’un identifiant DustEthic.
  - Des rapports agrégés accessibles aux ONG et, dans une certaine mesure, au public.

### 7.3 Gestion des clés et risques opérationnels

Risques principaux :
- Compromission des clés du relayeur.
- Mauvaise configuration des limites de gas.
- Attaques de type griefing sur des paymasters mal protégés.

Mesures possibles :
- Utiliser des smart accounts, multi-sig ou mécanismes de récupération sociale pour les comptes opérateurs.
- Limiter les montants par opération, par campagne et par période.
- Définir des mécanismes d’arrêt d’urgence (pause) en cas de comportement anormal.

---

## 8. Points de vigilance et analyse critique

Dans l’esprit "avocat du diable", certains points méritent une attention particulière.

### 8.1 Viabilité économique

- Le modèle dépend fortement du volume global agrégé.
- Il existe un risque de "longue traversée du désert" si l’adoption par les wallets et les ONG est lente.
- La pression pour réduire les frais au minimum peut rendre l’équation économique difficile pour les relayeurs.

### 8.2 Contraintes réglementaires

- Selon les juridictions, agréger des fonds et les reverser à des ONG peut être assimilé à :
  - Un service de paiement.
  - Une activité d’intermédiation financière.
- Les exigences KYC/AML peuvent devenir significatives, surtout dès que des conversions fiat entrent en jeu.

Une analyse juridique par pays ou région sera indispensable avant toute mise en production.

### 8.3 Transparence vs confidentialité

- Une transparence maximale est positive pour la confiance, mais :
  - Elle peut révéler des patterns sensibles (par exemple les dons vers des ONG dans des pays à risque).
  - Elle peut nécessiter des mécanismes d’anonymisation ou de pseudonymisation dans certains cas.

### 8.4 Évolutions rapides de l’AA et des L2

- L’écosystème ERC-4337, EIP-7702, les L2 et les solutions de relayers évoluent très vite.
- Un standard DustEthic trop rigide risque de devenir obsolète.
- À l’inverse, un standard trop vague serait difficile à implémenter correctement.

Conclusion : la gouvernance du standard devra accepter des mises à jour régulières, avec une attention forte à la compatibilité ascendante.

---

## 9. Roadmap proposée pour les relayeurs DustEthic

Proposition indicative (sans engagement) :

1. Phase 0 - Prototype interne
   - 1 relayeur de référence.
   - 1 ou 2 L2 supportées.
   - 1 ou 2 ONG pilotes.
   - UX minimale pour tester bout en bout (wallet de test, dashboard basique).

2. Phase 1 - Standardisation
   - Rédaction publique de "DustEthic - Standard Relayeurs v1".
   - Publication sur GitHub (FR/EN).
   - Définition d’un premier SDK ou schéma d’intégration pour wallets.

3. Phase 2 - Multi-relayeurs
   - Ouverture à plusieurs opérateurs indépendants.
   - Définition de critères de "listing" (techniques, éthiques, SLA).
   - Possibilité pour les wallets de sélectionner ou de basculer entre relayeurs.

4. Phase 3 - Marketplace et gouvernance élargie
   - Marketplace publique des relayeurs DustEthic (frais, réseaux, historique).
   - Participation des ONG et wallets à la gouvernance du standard (structure à définir).

---

## 10. Références (pour cette partie relayeurs)

Ces sources externes ne définissent pas DustEthic, mais décrivent les briques techniques qui inspirent ce design.

1. ERC-4337 - Account Abstraction, composants UserOperation, EntryPoint, bundlers, paymasters - documentation officielle et survols techniques.  
2. Documentation OpenZeppelin "Account Abstraction" - explication des éléments clés d’ERC-4337 et de la logique de vérification / exécution.  
3. Articles et overviews sur ERC-4337 (par exemple Etherscan, Stackup) - notions d’alt-mempool, flux opérationnel et implications UX.  
4. Articles et guides sur les paymasters et les transactions gasless (Alchemy, GoldRush, Biconomy, etc.).  
5. Articles et documentations sur EIP-7702 et la mise à jour Pectra (Consensys, Alchemy, Circle, QuickNode, etc.).  
6. Documentation Biconomy sur l’Account Abstraction, les smart accounts et les paymasters, y compris les tutoriels de transactions gasless.  
7. Documentation et blogs Gelato Relay - infrastructure de relayers pour des transactions gasless et paymasters multi-réseaux.  
8. Articles de blog et tutoriels généralistes sur les transactions gasless, les meta-transactions et les modèles économiques des providers AA.  
