# DustEthic

Standard ouvert pour réunir la poussière crypto dispersée et la transformer en dons aux ONG, par agrégation par actif et sans garde.

## En une phrase

La poussière crypto, ces soldes minuscules dispersés sur des dizaines de chaines, est individuellement inutilisable mais collectivement énorme. DustEthic est un standard ouvert qui la réunit actif par actif, à travers de nombreux portefeuilles, sans que personne n'en prenne la garde, pour la verser à des ONG.

## Le problème

Un portefeuille accumule des miettes : restes d'échanges, airdrops, soldes laissés sur des chaines qu'on n'utilise plus. Seule, chaque miette ne vaut pas le cout de son déplacement. Mais la meme poussière, collectée par actif à travers des millions de portefeuilles, forme des sommes parfaitement utilisables. Aujourd'hui, cet argent dort.

## L'idée : agréger par actif, sans garde

La bonne unité d'agrégation n'est pas le portefeuille, c'est l'actif. On ne balaie pas atomiquement les dizaines de chaines d'un utilisateur, c'est impossible. On collecte chaque actif à travers de nombreux portefeuilles, jusqu'à former une somme convertible et donnable. Et tout cela sans garde : l'utilisateur signe, garde ses clés, et personne ne détient jamais ses fonds.

## Comment ca marche

1. **Détection.** Le portefeuille identifie les soldes sous un seuil configurable.
2. **Autorisation.** L'utilisateur autorise le don, sans céder ses clés et sans détenir de jeton natif pour le gas.
3. **Agrégation par actif.** Chaque actif rejoint un dispositif sans garde et s'accumule à travers les portefeuilles.
4. **Conversion et versement.** Quand un tas atteint un seuil de viabilité, il est converti puis versé à l'ONG. La trace reste publique on-chain.

## Principes

- **Agrégation par actif.** Le coeur du standard. Ce qui, isolé, ne vaut pas son déplacement devient viable une fois additionné.
- **Sans garde.** L'utilisateur conserve ses clés. Aucun acteur ne détient les fonds.
- **Cross-chain.** Au-delà de l'EVM, le standard s'appuie sur les primitives natives de chaque écosystème.
- **Gratuit pour le donateur.** Mécène par défaut, sinon commission en nature plafonnée et transparente.
- **Standard ouvert.** Une interface, pas un produit. Portefeuilles, opérateurs de tas et ONG l'implémentent librement.
- **Transparence on-chain.** Tous les flux sont publics et auditables.

## Sans garde, concrètement

Sur les grands écosystèmes, la délégation native de frais et l'autorisation ciblée permettent à un tiers de payer le gas et à l'utilisateur d'autoriser un transfert précis, sans partage de clé. L'accumulation se fait dans des contrats autonomes sans clé d'administration, ou via des opérateurs transparents. Personne n'a le pouvoir de disposer des fonds.

## Modèle de cout

Le don ne coute rien au donateur. Par défaut, un mécène finance les frais. À défaut, une commission en nature, plafonnée par le standard et transparente on-chain, finance les opérations. Aucune commission cachée n'est conforme.

## Périmètre et limites

L'agrégation par actif rend viable la grande majorité des poussières, meme minuscules, dès lors qu'elles sont nombreuses. Restent honnetement hors d'atteinte : les jetons sans marché, qui ne peuvent etre ni valorisés ni convertis ; les résidus figés sur des réseaux abandonnés ou les frais dépassent la valeur ; une miette isolée inférieure à sa part des frais mutualisés.

## Statut

Phase 0, conception et spécification. Projet bénévole, non lucratif. Aucun partenaire wallet ou ONG, aucun opérateur identifié à ce jour.

## Note réglementaire

Selon la juridiction et la conception du contrat, l'opérateur d'agrégation peut relever de règles applicables aux intermédiaires financiers. Le projet est en Phase 0 ; cette qualification doit etre validée par un conseil juridique selon l'implémentation. DustEthic, en tant que projet, publie une spécification et ne détient aucun fonds.

## Structure du dépot

- `guide/` : guide d'ensemble (FR et EN).
- `livre-blanc-wallets/` : spécification détaillée cote portefeuille (FR et EN).
- `eip/` : brouillon d'EIP (licence CC0).
- `archives/` : versions antérieures conservées pour mémoire.

## Licences

- Spécification et documentation : CC BY 4.0
- Code de référence : MIT
- EIP : CC0

## Liens

- Site : https://dustethic.org
- Discord : https://discord.gg/fVFc26GV
- GitHub : https://github.com/DustEthic
