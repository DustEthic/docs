# ADN DustEthic - référence de reprise

**Mise à jour du 03.10.2026 :** lire le [Wallet Profile v0.1](WALLET-INTEGRATION-PROFILE.md) pour le parcours ZERO-OR-WAIT. Les mécanismes gas de juin restent historiques ; la conversion de secours n’est pas un fallback du profil.

Date : 2026-06-14
Statut : document de reprise, Phase 0, non final, non audité.

## Idée centrale

DustEthic transforme une idée simple en standard ouvert : regrouper des petits soldes crypto inutilisés pour les diriger vers des ONG, avec consentement clair, frais visibles et preuve publique.

Le projet ne commence pas par un produit. Il commence par une règle commune, ouverte, critiquable et vérifiable.

## Principe fondateur

On raisonne en unités crypto, pas en fiat.

La répartition se fait dans l'actif donné. Les équivalents CHF, EUR ou USD peuvent aider à comprendre, mais ils ne doivent pas devenir la source de vérité.

## Ce que DustEthic est

- Une initiative ouverte.
- Un standard en construction.
- Une méthode pour rendre des micro-dons crypto vérifiables.
- Un cadre commun pour wallets, relayeurs, ONG, auditeurs et développeurs.
- Un projet Phase 0, fait pour être critiqué avant d'être utilisé.

## Ce que DustEthic n'est pas

- Une ONG.
- Une plateforme de collecte.
- Un dépositaire de fonds.
- Un service prêt à l'emploi.
- Un token.
- Une promesse de rendement ou d'impact garanti.
- Une solution magique qui supprime les frais blockchain.

## Les quatre piliers

1. Consentement : aucun don sans action explicite de l'utilisateur.
2. Agrégation : un dust isolé peut être inutile, un lot peut devenir utile.
3. Frais visibles : gas, commission, réserve éventuelle et net ONG doivent être compréhensibles.
4. Preuve publique : une intention n'est pas une preuve d'exécution.

## Politique gas à préserver

- L2-first.
- Exécution seulement si le lot est viable.
- Ratio de travail : dons agrégés / gas estimé >= T, avec T >= 30 comme repère historique.
- Pool gas relayeur si possible.
- Conversion minimale seulement comme filet de sécurité documenté.
- Plafond public des coûts, par exemple 15 % comme repère historique.

## Ordre de reprise

1. Retrouver l'ADN.
2. Stabiliser le standard.
3. Définir la preuve de lot.
4. Définir le format d'intention.
5. Clarifier les rôles wallets, relayeurs et ONG.
6. Reprendre seulement ensuite un prototype ou un simulateur.

## Phrase de référence

DustEthic ne collecte pas les dons. DustEthic définit la règle du jeu pour que des dusts crypto puissent devenir des dons ONG vérifiables, avec consentement clair, frais visibles et preuve publique.

Chaque grain compte, seulement si la preuve tient.

## Clarification du 03.10.2026 — parcours wallet

Le [profil wallet v0.1](WALLET-INTEGRATION-PROFILE.md) formalise ZERO-OR-WAIT : aucun paiement utilisateur supplémentaire ; sans sponsor suffisant, l'intention attend ou expire. Le profil de sponsoring externe ne rembourse pas le gas sur le don. Les déductions éventuelles du lot restent visibles et bornées avant consentement.

Les paramètres gas de juin ci-dessus restent historiques. Leur conversion de secours n'est pas un fallback du profil wallet : un sponsor absent ne déclenche ni swap, ni bridge, ni demande de paiement. Les trois adaptateurs proposés ne sont pas implémentés. La règle crypto, les clés chez l'utilisateur et l'absence de caisse ou de token DustEthic sont conservées.
