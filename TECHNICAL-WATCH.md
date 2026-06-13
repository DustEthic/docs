# Veille technique DustEthic

Date : 2026-06-13  
Statut : document de veille, pas une décision d'implémentation.

DustEthic doit rester technologiquement neutre. Les protocoles ci-dessous sont des pistes à surveiller ou tester, pas des briques déjà intégrées.

## Priorités de veille

| Sujet | Utilité possible pour DustEthic | Prudence |
|---|---|---|
| HTTP 402 / x402 | Demander ou exprimer un micro-paiement ou une intention via HTTP | Ne pas réduire DustEthic à un paywall |
| ERC-4337 | Smart accounts, UserOperations, bundlers, paymasters | Coût réel, compatibilité wallet, sécurité des paymasters |
| EIP-7702 | Batching et sponsoring possibles pour comptes EOA | Risque élevé si l'utilisateur délègue sans comprendre |
| EIP-5792 | Interface wallet pour lots d'appels | Dépend de l'adoption wallet |
| ERC-7677 | Permissions et paymasters temporaires | Surface de risque UX/sécurité à auditer |
| ERC-7683 | Intents cross-chain | Brouillon, ne pas présenter comme standard final |
| CCTP | Transferts USDC natifs cross-chain | Spécifique USDC/Circle, pas neutre par défaut |
| Safe / multisig | Réception ONG plus sûre | Gouvernance ONG à vérifier |

## Hypothèse d'architecture testable

1. Wallet ou interface détecte des dusts.
2. L'utilisateur signe une intention limitée.
3. Un relayeur ou paymaster paie ou mutualise le gas selon une politique publique.
4. Les intentions sont regroupées par actif.
5. Une exécution ne part que si le lot atteint un seuil viable.
6. La preuve de lot est publiée.

## Règles de prudence

- Ne jamais dire qu'un don est exécuté avant preuve.
- Ne jamais masquer les frais réseau.
- Ne jamais demander de seed phrase ou clé privée.
- Ne jamais créer de permission illimitée par défaut.
- Ne jamais accepter un token à transfert non standard sans audit.
- Ne jamais présenter un protocole en draft comme une base stable.

## Sources officielles à suivre

- x402 : <https://docs.cdp.coinbase.com/x402/welcome>
- ERC-4337 : <https://eips.ethereum.org/EIPS/eip-4337>
- EIP-7702 : <https://eips.ethereum.org/EIPS/eip-7702>
- EIP-5792 : <https://eips.ethereum.org/EIPS/eip-5792>
- ERC-7677 : <https://eips.ethereum.org/EIPS/eip-7677>
- ERC-7683 : <https://eips.ethereum.org/EIPS/eip-7683>
- CCTP : <https://developers.circle.com/stablecoins/cctp-getting-started>
- Safe : <https://safe.global/>

## Décision de communication

Le site, le Discord et les README doivent employer les formulations suivantes :

- “piste à tester”, pas “intégré” ;
- “standard en construction”, pas “service disponible” ;
- “non audité”, pas “sécurisé” ;
- “frais visibles”, pas “gratuit garanti” ;
- “sans garde opaque”, pas “risque zéro”.
