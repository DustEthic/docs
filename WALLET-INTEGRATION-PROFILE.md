# DustEthic Wallet Integration Profile v0.1

Date : 2026-10-03. **Proposition DustEthic, Phase 0, non auditée.**
Ce profil n'est pas un ERC. Les adaptateurs et le Wallet Kit décrits ici ne sont pas implémentés. La simulation du site utilise uniquement des données fictives.

## But

Permettre à un wallet d'identifier, autoriser, faire sponsoriser et vérifier un don, avec les capacités dont il dispose. DustEthic fournit des règles communes ; le wallet conserve les clés et l'interface de signature. Aucun opérateur, token ou fonds central DustEthic n'est imposé.

## ZERO-OR-WAIT

- `user_network_fee = 0` et `additional_user_payment = 0`.
- Le sponsor prend en charge tous les coûts réseau nécessaires au parcours : activation éventuelle du compte, approval, transfert, création de compte de réception, stockage ou réserve requis et tentative échouée.
- Aucun frais n'est prélevé dans un autre actif du donateur. Le profil de sponsoring externe v0.1 exige aussi `gas_reimbursed_from_donation = 0` : une avance remboursée sur les dons n'est pas présentée comme du sponsoring externe.
- Le montant signé constitue le maximum débité pour le don. Une commission ou réserve éventuelle du lot doit être annoncée, plafonnée et acceptée ; elle réduit le net ONG et ne constitue pas un paiement supplémentaire.
- Sans financement valide, compatible et suffisant, le wallet affiche `WAIT` ; aucune soumission payante par l'utilisateur, aucun achat automatique de token natif, swap ou bridge imposé.
- Une intention en attente peut expirer. Un devis sponsor n'est pas une garantie d'inclusion ; l'absence de paiement utilisateur doit aussi être imposée par l'adaptateur d'exécution.

Le profil interdit la bascule automatique vers un autre mode de frais. Les simulations indiquent 0 pour l'utilisateur et un coût distinct pour le sponsor : le réseau n'est pas nécessairement gratuit.

## Détection et décision

Le wallet peut utiliser son inventaire local ; ERC-7811 est une option si disponible, pas une dépendance. L'absence de `wallet_getAssets` n'empêche pas une intégration interne au wallet.

Chaque candidat est contrôlé séparément : actif et réseau, solde, politique de dust, comportement de transfert, méthode d'autorisation, destination vérifiée et acceptation de cet actif par l'ONG, sponsor et montant net minimal.

| État | Signification |
|---|---|
| ELIGIBLE | Un chemin compatible et financé peut être proposé ; le consentement reste requis. |
| WAIT | Financement, seuil ou capacité temporairement insuffisants ; aucun don exécuté. |
| EXCLUDED | Actif, destination ou comportement non pris en charge ; raison affichée. |
| EXPIRED / CANCELLED | Intention périmée ou retirée selon le mécanisme disponible. |
| SUBMITTED | Opération envoyée ; le don n'est pas encore confirmé. |
| CONFIRMED / FAILED | Résultat du réseau contrôlé ; seule la confirmation de réception établit le don. |

Les lots sont séparés par actif, réseau et bénéficiaire. Des valeurs d'actifs différents ne s'additionnent pas. La valeur fiat reste indicative.

## Intention commune, autorisations propres au réseau

Champs proposés : `profile_version`, `intent_id`, `network`, `asset_id`, `amount_atomic`, `beneficiary`, `recipient`, `expires_at`, `nonce`, `max_user_network_fee`, `max_additional_user_payment`, `max_batch_deduction`, `min_ngo_amount`, `adapter_id` et politique de révocation.

Les montants sont des chaînes d'entiers en unités minimales, avec identifiant d'actif, réseau et décimales vérifiés. Les adresses, domaines de signature, bénéficiaire, montant et expiration doivent être liés à l'autorisation effective ; un JSON de métadonnées n'est pas une protection à lui seul. Le payload signé varie entre EVM et Solana. Le modèle métier commun ne signifie pas une signature universelle.

## Trois chemins à qualifier en premier

| Adaptateur proposé | Préconditions | Travail du développeur wallet |
|---|---|---|
| `evm-wallet-paymaster` | EIP-5792, compte/chaîne compatibles, sponsoring effectif ; ERC-7677 si disponible. | Détecter les capacités, construire les appels, contrôler l'atomicité nécessaire, refuser les coûts utilisateur, suivre le statut et la réception. |
| `evm-token-authorization` | Token explicitement qualifié pour ERC-7758, ou ERC-2612 avec exécuteur borné. | Afficher les données typées, lier l'intention à la destination, faire soumettre par le relayeur sponsor, contrôler les allowances résiduelles et les nonces. |
| `solana-fee-payer` | Wallet et token compatibles, sponsor externe et destination qualifiée. | Construire le transfert exact, contrôler toutes les instructions et les comptes, financer les coûts annexes, réunir les signatures et vérifier la réception. |

Avec ERC-2612, la deadline limite la soumission du permit, pas la durée de l'allowance déjà créée. Un permit ne lie pas seul le transfert final à une ONG : un exécuteur doit faire respecter l'intention. Si une révocation réseau exige des frais, elle doit être sponsorisée, ou le chemin refusé et sa limite explicitée. Une expiration affichée dans l'UI ne révoque pas une permission on-chain.

EIP-7702 reste optionnel : le don ne doit pas imposer une délégation persistante mal expliquée. ERC-7715, ERC-7674 et ERC-8255 restent des pistes de permissions, distinctes du service paymaster ERC-7677.

Stellar, Aptos et Sui rejoignent la veille, sans annonce d'intégration DustEthic. Payer les frais ne résout pas automatiquement les réserves, la liquidité ou l'acceptation de l'actif par l'ONG.

## Enveloppe de résultat proposée

La preuve commune décrit `intent_ids`, version, adaptateur, réseau, actif, bénéficiaire, brut, déductions, net ONG, frais utilisateur, sponsor réel, coût sponsor, références réseau et statut de finalité. Un hash de transaction seul ne prouve ni la réception ni l'absence de frais utilisateur. Vérifier les effets sur les soldes et l'identité du payeur réel.

Le coût natif du sponsor est exprimé séparément ; il n'est pas soustrait d'un montant de don d'un autre actif. Les coûts et déductions du lot restent publiés dans l'actif donné. Les identités de sponsors et ONG dans les exemples sont fictives.

Voir [les exemples fictifs](examples/wallet-profile-v0.1.json). Ils partagent une enveloppe, pas un schéma de production validé. Aucun `$schema` public stabilisé n'est revendiqué.

## Wallet Kit : chantier proposé

API cible : `detectEligibleDust()`, `buildDonationIntent()`, `findSponsoredExecution()`, `requestAuthorization()`, `submit()` et `verifyDonationProof()`.

À produire après revue du profil : JSON Schemas, vecteurs de test signés, adaptateurs minimaux et fixtures réseau. Le kit ne détiendra ni clés ni fonds. Un registre ouvert de sponsors est une idée ultérieure, pas un service existant ni une caisse DustEthic.

## Critères de validation

1. Sponsor absent, refusé, expiré ou insuffisant : `WAIT`, aucun débit, aucune transaction de secours payante.
2. Sponsor disparu après consentement : financement revérifié avant soumission ; échec à charge du sponsor.
3. Token, réseau, destination, montant, nonce ou date incorrects : rejet.
4. Approval résiduel, frais préalables ou besoin de token natif non couvert : rejet du chemin.
5. Aucun swap, bridge, mélange d'actifs ou de bénéficiaires implicite.
6. Tentatives dupliquées ou rejouées : pas de second don.
7. Même actif et montant fictifs par chemin : même enveloppe et mêmes règles de calcul ; références réseau propres à chaque adaptateur.
8. Une simulation, une intention et une transaction soumise ne sont jamais étiquetées comme un don confirmé.

## Sources et périmètre

Les règles ZERO-OR-WAIT, états et interfaces du kit sont des **propositions propres à DustEthic**, datées du 03.10.2026. Elles ne sont pas attribuées à Ethereum ou à un wallet.
Les fonctionnalités et statuts des standards externes sont sourcés dans [TECHNICAL-WATCH.md](TECHNICAL-WATCH.md) et [SOURCES.md](SOURCES.md), vérifiés le 03.10.2026. Un statut Final ne démontre pas la compatibilité d'un wallet ou d'un token.
