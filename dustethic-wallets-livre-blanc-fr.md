# [2025-12-10] DustEthic - Livre blanc "Wallets" v1.0 (FR)

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

## 1. Contexte et objectifs

DustEthic vise à transformer les "dusts" de wallets non custodial (petits soldes résiduels difficilement utilisables) en micro dons agrégés et traçables au profit de projets d intérêt général, via un standard ouvert et documenté.

Les wallets sont un acteur clé du Standard DustEthic. Sans eux, il n y a ni détection des dusts, ni UX simple, ni distribution de masse.  
Ce livre blanc se concentre sur:

- le rôle fonctionnel des wallets dans l écosystème DustEthic
- les pistes d intégration technique (EOA, Account Abstraction, EIP 4337, EIP 7702)
- les modèles de commissions envisagés pour les wallets
- des exemples de gains potentiels (non contractuels)
- la maquette fonctionnelle du module DustEthic dans un wallet

---

## 2. Rôle du wallet dans le Standard DustEthic

### 2.1 Rappel des acteurs

Dans la vision DustEthic, on retrouve au minimum:

- Donateur: utilisateur final, propriétaire du wallet non custodial
- Wallet: application qui expose l interface utilisateur et signe les opérations
- Relayer / Aggregator DustEthic: service qui agrège les micro montants et gère la logique de don
- Paymaster / Bundler (AA): service technique qui gère le sponsoring de gas et le packaging des UserOperations
- ONG / Projet bénéficiaire: destinataire final des fonds
- Standard DustEthic: jeux de règles, API et bonnes pratiques documentés publiquement

### 2.2 Position du wallet

Le wallet est:

- le point d entrée UX: activation, consentement, configuration, suivi
- le "contrôleur" du consentement: rien ne se fait sans action explicite (opt in, signature)
- l orienteur technique:
  - il choisit quels flux exposer à DustEthic (tokens, réseaux, fréquences)
  - il décide, ou laisse l utilisateur décider, s il accepte ou non de partager une commission avec le relayer

Le Standard DustEthic doit rester neutre sur les choix business précis, mais:

- impose la transparence sur les commissions
- impose la traçabilité minimale des flux
- interdit la transformation de DustEthic en produit financier spéculatif (pas de promesse de rendement)

---

## 3. Parcours utilisateur dans le wallet

### 3.1 Activation (opt in fort)

1. L utilisateur ouvre son wallet non custodial.
2. Un module "DustEthic" est proposé:
   - via un onglet dédié
   - via une carte dans un "hub" d utilitaires
   - ou via une bannière suggestion ("Convertir vos dusts en dons").

3. L écran d introduction explique en termes simples:
   - ce qu est un "dust"
   - le principe d agrégation et de don
   - qui sont les acteurs (wallet, relayer, ONG)
   - comment sont gérées les commissions, avec transparence

4. L utilisateur doit:
   - accepter les conditions DustEthic (standard + privacy)
   - confirmer que DustEthic ne crée aucune promesse de rendement
   - choisir une ou plusieurs catégories d ONG ou une liste d ONG proposées

### 3.2 Première configuration

L utilisateur choisit au minimum:

- Réseaux et tokens concernés:
  - ex: Ethereum L2 A, L2 B, éventuellement L1 avec prudence
- Seuil de "dust" par token:
  - ex: moins de 2 USD d équivalent valeur par token est considéré comme "dust" (paramétrable)
- Mode d action:
  - Mode 1: "Balayage manuel" (Dust Sweep) - l utilisateur déclenche l opération ponctuellement
  - Mode 2: "Balayage périodique" - ex: mensuel ou trimestriel
  - Mode 3 (optionnel): "Balayage opportuniste" - ex: lors d une transaction, si une certaine condition est remplie

### 3.3 Balayage manuel (scénario minimal viable)

1. L utilisateur ouvre "DustEthic" et appuie sur "Analyser mes dusts".
2. Le wallet:
   - interroge la blockchain ou un indexer pour lister les soldes
   - applique les règles de seuil
   - construit une liste de "candidats au don" par token / réseau

3. L utilisateur choisit:
   - quels tokens inclure ou exclure
   - l ONG ou la catégorie d ONG cible (si non déjà configurée)

4. Le wallet présente un récapitulatif:
   - montant total estimé du don (en stablecoin ou en équivalent USD)
   - commissions totales (relayer + wallet), clairement détaillées
   - estimation des frais de gas et qui les prend en charge

5. L utilisateur signe la transaction ou UserOperation:
   - soit directement si EOA
   - soit via un smart wallet AA
   - DustEthic ne modifie pas le modèle de signature du wallet

### 3.4 Modes automatiques et garde fous

Les modes automatiques sont puissants mais plus sensibles.  
Principes de prudence:

- activation explicite par l utilisateur, avec un plafond de don mensuel
- possibilité de "pause" et de désactivation en un clic
- limite stricte sur le montant par opération (ex: pas plus de X USD par sweep)
- notifications claires après chaque sweep

---

## 4. Architecture technique côté wallet

### 4.1 EOA vs Account Abstraction

Deux grands cas d usage:

1. Wallets EOA classiques:
   - l utilisateur signe des transactions classiques envoyées vers un smart contract DustEthic (Aggregator)
   - les frais de gas sont payés en token natif, sauf si un mécanisme externe sponsorise la transaction

2. Wallets compatibles Account Abstraction (ERC 4337, EIP 7702):
   - l utilisateur signe des UserOperations ou des transactions "sponsorisées"
   - un Paymaster prend en charge les frais de gas
   - la logique DustEthic peut être intégrée avec moins de friction dans les flows existants

Le Standard DustEthic doit rester compatible avec les deux, mais encourager les intégrations AA car elles facilitent l UX (sponsoring de gas, batching, logique programmable).

### 4.2 Intégration avec relayeurs, bundlers, paymasters

Côté wallet, l intégration peut se faire via un "DustEthic Wallet Connector" qui fournit:

- une API de détection de dusts (ou s appuie sur un indexer externe)
- une API de simulation:
  - estimation des montants
  - estimation des gas et des commissions
- une API de construction:
  - creation de la transaction ou UserOperation prête à signer
- une API de suivi:
  - état des opérations
  - reçus de dons (hash, ONG, timestamp)

Dans un contexte ERC 4337:

- le relayer DustEthic peut agir comme:
  - un bundler spécialisé
  - un paymaster qui sponsorise certains UserOperations DustEthic  
  (par exemple pour des montants minimum ou des campagnes spécifiques)

---

## 5. Modèle de commissions pour les wallets

### 5.1 Principes de base

- Les commissions sont prises sur la "part DustEthic" des flux, jamais sur le solde de base de l utilisateur.
- Les pourcentages doivent rester modestes (proche ou inférieur aux ordres de grandeur habituels dans les wallets et exchanges pour des services analogues).
- Le découpage des commissions doit être transparent pour l utilisateur:
  - part reversée à l ONG
  - part destinée au relayer
  - part éventuellement destinée au wallet
  - part éventuelle dédiée au financement du Standard DustEthic (open source, audits, etc.)

### 5.2 Grille illustrative (exemple de travail)

Exemple non contractuel pour illustrer l ordre de grandeur:

- Montant brut donné par l utilisateur: 100 unités
- Commission globale DustEthic: 3 % du flux (3 unités)
- Répartition possible:
  - 1.5 % (1.5 unités) pour le relayer (infrastructure, gas, risques)
  - 1.0 % (1 unité) pour le wallet (intégration, UX, support)
  - 0.5 % (0.5 unité) pour un fonds DustEthic (maintenance du standard, audits)

Montant net reçu par les ONG: 97 unités.

Le Standard DustEthic ne fige pas ces pourcentages mais impose:

- affichage clair des commissions
- interdiction de masquer des frais supplémentaires dans des conversions obscures
- possibilité pour l utilisateur de refuser toute commission côté wallet (ex: mode "full ONG")

### 5.3 Scénarios alternatifs

- Mode "zéro commission wallet":
  - 0 % wallet
  - relayer financé par d autres revenus (subventions, sponsoring, etc.)
- Mode "campagne sponsorisée":
  - commission réduite ou nulle, payée par un sponsor externe
  - le sponsor peut être mentionné dans l interface
- Mode "wallet premium":
  - le wallet prend une commission légèrement supérieure, en échange de services additionnels (reporting détaillé, branding ONG spécifique, etc.)
  - ce mode doit rester minoritaire et clairement indiqué

---

## 6. Gains potentiels pour un wallet (exemples non contractuels)

Ces scénarios sont uniquement destinés à illustrer l effet de l échelle.  
Ils ne constituent pas des promesses de revenus.

### 6.1 Exemple 1 - Wallet moyen

Hypothèses purement illustratives:

- 1 000 000 d utilisateurs actifs mensuels (MAU)
- 10 % activent DustEthic dans leur wallet
- 2 balayeurs de dusts par utilisateur et par an
- Montant moyen de don par balayeur: 5 USD
- Commission wallet: 1 % du montant donné

Calcul approximatif:

- Utilisateurs DustEthic: 100 000
- Nombre de balayeurs annuels: 100 000 x 2 = 200 000
- Volume total de dons: 200 000 x 5 USD = 1 000 000 USD
- Commission wallet 1 %: environ 10 000 USD par an

Ce n est pas "massif" à court terme, mais:

- cela peut couvrir une partie des coûts d intégration
- l image de marque "pro social" peut avoir une valeur marketing significative
- le potentiel augmente si DustEthic se généralise et si les montants moyens évoluent

### 6.2 Exemple 2 - Wallet très large échelle

Même logique, mais avec:

- 10 000 000 d utilisateurs
- 15 % d adoption DustEthic
- 3 balayeurs par utilisateur et par an
- 7 USD moyens par balayeur
- 1 % de commission wallet

Ordres de grandeur:

- Utilisateurs DustEthic: 1 500 000
- Balayeurs annuels: 4 500 000
- Volume dons: environ 31 500 000 USD
- Commission 1 %: environ 315 000 USD par an

Cela montre que DustEthic est un flux "longue traine":

- faible montant par utilisateur
- mais potentiellement significatif à grande échelle, tout en restant très supportable pour le donateur

---

## 7. Maquette fonctionnelle DustEthic dans un wallet

### 7.1 Principes UX

La maquette DustEthic côté wallet doit:

- être rassurante (pas d action automatique cachée)
- être lisible même pour des utilisateurs non experts
- montrer clairement:
  - les montants estimés
  - les commissions
  - l impact global du don

### 7.2 Écran 1 - Introduction

Contenu typique:

- Titre: "Transformez vos dusts en dons"
- Explication courte:
  - "DustEthic vous permet de transformer vos petits soldes inutilisés en micro dons agrégés pour des ONG."
- Trois points clés:
  - "Vous gardez le contrôle."
  - "Les commissions sont transparentes."
  - "Les dons sont traçables sur la blockchain."

Actions:

- Bouton "Configurer DustEthic"
- Lien "En savoir plus" vers la documentation publique DustEthic

### 7.3 Écran 2 - Sélection des tokens et seuils

Contenu:

- Liste des tokens et réseaux détectés
- Pour chaque token:
  - solde total
  - estimation de la part "dust" (en dessous du seuil)
  - case à cocher "inclure dans DustEthic"
- Paramètres:
  - seuil minimal par token (par défaut, ex: 2 USD)
  - fréquence de balayage (manuel, mensuel, trimestriel)

### 7.4 Écran 3 - Sélection des ONG

Options:

- Choix rapide de catégories:
  - environnement, éducation, santé, etc.
- Mode "liste d ONG partenaires" pré approuvées
- Possibilité future de listes personnalisées ou de filtres régionaux

### 7.5 Écran 4 - Récapitulatif et commissions

Affiche:

- estimation du don total (avant commission)
- détail des commissions:
  - x % relayer
  - y % wallet
  - z % standard DustEthic
- montant net estimé pour les ONG
- estimation des frais de gas et qui les prend en charge

Call to action:

- Bouton principal: "Signer et lancer le balayage DustEthic"
- Option secondaire: "Sauvegarder la configuration et quitter"

### 7.6 Écran 5 - Historique et reçus

Pour chaque opération DustEthic:

- date et heure
- réseau / token
- montant brut, commissions, montant net ONG
- identifiant de transaction on chain (lien vers explorer)
- ONG bénéficiaire (ou pool d ONG)

---

## 8. Risques, contraintes et questions ouvertes

### 8.1 Risques UX et réputation

- Confusion possible entre:
  - don volontaire et
  - "prélèvement automatique" non compris  
  D où la nécessité d un opt in explicite et réversible.

- Perception négative si:
  - les commissions wallet sont jugées trop élevées
  - la transparence est insuffisante

### 8.2 Contraintes techniques

- Complexité de l intégration AA (ERC 4337, EIP 7702) pour les équipes non familières
- Gestion cross chain:
  - différents types de tokens
  - frais de gas variables selon les réseaux
- Nécessité de logs et de reçus robustes pour la traçabilité et les audits

### 8.3 Contraintes réglementaires

- La qualification de ce type de service peut varier selon les juridictions:
  - service de don
  - service de payment
- Nécessité d éviter toute assimilation à un produit financier ou d investissement:
  - pas de token DustEthic avec promesse de rendement
  - pas d intéressement financier du donateur

### 8.4 Questions ouvertes

- Niveau de standardisation acceptable pour les wallets:
  - API minimale commune
  - libertés d UX
- Gouvernance du standard:
  - qui valide les évolutions
  - comment intégrer les retours des wallets et ONG
- Modèle de financement du standard lui même:
  - part des commissions
  - sponsoring externe
  - modèle 100 % open source avec donations

---

## 9. Roadmap d intégration pour un wallet partenaire (vue macro)

1. Phase 0 - Découverte
   - revue du Standard DustEthic
   - identification des contraintes internes (juridiques, UX, tech)

2. Phase 1 - Prototype interne
   - intégration minimale:
     - balayage manuel
     - un seul réseau
     - une ou deux ONG
   - tests sur testnet

3. Phase 2 - Pilote limité
   - déploiement sur un petit segment d utilisateurs volontaires
   - collecte de feedback UX
   - ajustement des seuils, des messages et des commissions

4. Phase 3 - Déploiement progressif
   - ouverture à un pourcentage plus large d utilisateurs
   - ajout de réseaux et d ONG
   - amélioration des outils de reporting

5. Phase 4 - Standardisation et optimisation
   - participation aux discussions du Standard DustEthic
   - alignement sur les futures versions du standard
   - optimisation des coûts et des flux

---

## 10. Résumé critique (vue "avocat du diable")

Points forts potentiels pour un wallet:

- nouvelle source de revenus modérée mais recurrente
- positionnement pro social et différenciant
- possibilité de mutualiser les coûts avec d autres wallets via un standard ouvert

Points faibles et risques:

- rentabilité incertaine à court terme
- risque de mauvaise compréhension par les utilisateurs
- charge de maintenance (tech, compliance, support)

Conclusion provisoire:

- DustEthic ne sera probablement pas "la" source de revenu principale d un wallet.
- En revanche, bien intégré, il peut:
  - améliorer l image de marque
  - ajouter un flux de revenus complémentaire raisonnable
  - rendre concret un engagement "Web3 for good" sans token spéculatif.

---

## 11. Sources externes (références techniques)

Ces sources sont citées à titre de base technique générale sur les sujets Account Abstraction, paymasters et modèles de revenus des wallets.  
Elles ne sont pas affiliées à DustEthic.

- ERC 4337 - Account Abstraction (documentation et spécification):  
  https://docs.erc4337.io  
  https://ethereum-magicians.org/t/erc-4337-account-abstraction-via-entry-point-contract-specification/7160
- Aperçu account abstraction et smart wallets:  
  https://www.alchemy.com/overviews/what-is-account-abstraction  
  https://docs.stackup.fi/docs/understanding-erc-4337
- Paymasters et sponsoring de gas:  
  https://www.alchemy.com/overviews/what-is-a-paymaster
- EIP 7702 et transformation d EOAs en smart accounts:  
  https://www.quicknode.com/guides/ethereum-development/smart-contracts/eip-7702-smart-accounts  
  https://www.openfort.io/blog/eip-7702-with-erc-4337
- Modèles de revenus des wallets et exchanges (frais, commissions):  
  https://streamflow.finance/blog/how-does-a-crypto-wallet-make-money  
  https://www.coinsclone.com/how-do-crypto-wallets-make-money  
  https://trusteeglobal.eu/academy/how-cryptocurrency-fees-are-calculated
