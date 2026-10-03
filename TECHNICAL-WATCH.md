# Veille technique DustEthic

Vérification des sources officielles : **2026-10-03**. Statut : veille, pas intégrations validées.

Le [Wallet Integration Profile v0.1](WALLET-INTEGRATION-PROFILE.md) propose une couche d'adaptation. Les drafts restent facultatifs ; aucun wallet, token ou sponsor n'est annoncé compatible sans qualification.

## Statuts et possibilités

| Source primaire | Statut observé au 03.10.2026 | Fonction |
|---|---|---|
| [EIP-5792](https://eips.ethereum.org/EIPS/eip-5792) | Final | Wallet Call API : capacités, appels groupés et suivi de statut. |
| [ERC-4337](https://eips.ethereum.org/EIPS/eip-4337) | Final | Smart accounts, UserOperations et paymasters. |
| [EIP-7702](https://eips.ethereum.org/EIPS/eip-7702) | Final | Délégation de code des EOA ; activation et pouvoirs à contrôler. |
| [ERC-7677](https://eips.ethereum.org/EIPS/eip-7677) | Review | Interface web de paymaster pour EIP-5792 ; aucune garantie de sponsoring. |
| [ERC-7811](https://eips.ethereum.org/EIPS/eip-7811) | Draft | wallet_getAssets : découverte des actifs exposés par le wallet. |
| [ERC-7758](https://eips.ethereum.org/EIPS/eip-7758) | Review | Transfert par autorisation signée, pour tokens compatibles. |
| [ERC-2612](https://eips.ethereum.org/EIPS/eip-2612) | Final | Permit signe une allowance ; il ne transfère pas le don. |
| [ERC-7674](https://eips.ethereum.org/EIPS/eip-7674) | Review | Approval temporaire dans une transaction ; allowance persistante distincte. |
| [ERC-8255](https://eips.ethereum.org/EIPS/eip-8255) | Draft | Approvals expirants, avec exceptions de compatibilité prévues. |
| [ERC-7715](https://eips.ethereum.org/EIPS/eip-7715) | Draft | Demandes de permissions au wallet ; périmètre à borner. |
| [ERC-7836](https://eips.ethereum.org/EIPS/eip-7836) | Draft | Préparation des appels et soumission après signature. |
| [ERC-7821](https://eips.ethereum.org/EIPS/eip-7821) | Draft | Interface minimale pour exécuter des lots. |
| [EIP-8141](https://eips.ethereum.org/EIPS/eip-8141) | Draft | Proposition de frames de validation, exécution et paiement du gas. |
| [Solana](https://solana.com/docs/payments/send-payments/payment-processing/fee-abstraction) | Documentation réseau | Un fee payer distinct peut financer les frais. |
| [Stellar](https://developers.stellar.org/docs/build/guides/transactions/fee-bump-transactions) | Documentation réseau | Fee-bump : paiement des frais par un autre compte. |
| [Aptos](https://aptos.dev/build/guides/sponsored-transactions) | Documentation réseau | Transactions sponsorisées avec fee payer. |
| [Sui](https://docs.sui.io/develop/transaction-payment/sponsor-txn) | Documentation réseau | Transactions sponsorisées par des objets de gas du sponsor. |

## Corrections et limites

- ERC-7677 décrit le service paymaster, **pas des permissions temporaires**. ERC-7715 concerne les permissions ; ERC-7674 et ERC-8255 concernent les approvals des tokens.
- Un paymaster peut facturer des ERC-20 : abstraction des frais ne signifie pas sponsoring gratuit. Le profil DustEthic refuse tout paiement utilisateur supplémentaire.
- ERC-2612 nécessite un transfert ultérieur et un exécuteur qui respecte la destination ; la deadline d'un permit ne fait pas expirer l'allowance qu'il a créée.
- ERC-7811 est facultatif : garder la détection locale et limiter les informations partagées avec les sponsors. Pas de scan global par défaut.
- Sur Solana, choisir un sponsoring intégral. Une abstraction où le donateur paie en token n'est pas un chemin gratuit pour lui. Les coûts de comptes et les contraintes de transfert doivent être qualifiés.
- Le fee-bump Stellar ne couvre pas à lui seul tous les besoins de réserves. Aptos et Sui nécessitent aussi une qualification de l'opération, du wallet et de la réception.
- Aucun transfert de stablecoin Sui n'est annoncé universellement sans gas. La veille retient les transactions sponsorisées ; les exceptions de gas nul demandent une étude séparée.
- EIP-8141 reste Draft ; ne pas l'utiliser comme condition du prototype actuel.

## Ordre de travail proposé

1. Revoir le profil et ses cas d'échec ZERO-OR-WAIT.
2. Spécifier les schémas, les montants atomiques et les preuves.
3. Tester séparément EVM wallet/paymaster, EVM par autorisation et Solana fee payer.
4. Contrôler que les résultats utilisent une même enveloppe, avec preuves natives vérifiables.
5. Développer un Wallet Kit minimal seulement après ces vérifications.

## Pistes historiques conservées

HTTP 402/x402, ERC-7683, CCTP et Safe restent dans le radar historique. Ils ne sont pas nécessaires au profil v0.1. Leur statut et leur adéquation n'ont pas été requalifiés dans cette mise à jour ; ne pas prendre les notes de juin comme un état courant. Aucun swap ou bridge automatique n'est ajouté.

## Règle de communication

Dire « proposition », « à qualifier », « simulation fictive » ou « source vérifiée le 03.10.2026 ». Distinguer statut d'une spécification, support réseau et adoption wallet. Ne pas annoncer un kit livré, une intégration validée, un don exécuté ou un partenaire absent.
