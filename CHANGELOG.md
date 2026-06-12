# Changelog - DustEthic Docs

Toutes les évolutions notables de la documentation sont consignées ici.

> **Note sur le versionnage.** À partir du pivot de juin 2026, le projet repart sur une ligne **v0.x** pour refléter honnêtement son stade réel (Phase 0 - conception). La ligne **v1.1-draft** décrivait un modèle relayeur à commission, abandonné lors du pivot et désormais archivé. La numérotation v0.x ne traduit pas une régression, mais une remise à zéro assumée vers un standard ouvert plus simple.

## [2026-06-12] - v0.2-draft

### Changé
- Coeur du standard déplacé vers l'agrégation par actif non-custodial, qui devient le mécanisme central.
- « Zéro commission » remplacé par « Gratuit pour le donateur » : mécène par défaut, sinon commission en nature plafonnée et transparente.
- Le role d'agrégation, précédemment écarté, est réintégré sous forme sans garde (contrats autonomes sans clé d'administration ou opérateurs transparents), et non plus comme intermédiaire custodial.
- Portée élargie au cross-chain, au-delà de l'EVM, via les primitives natives de délégation de frais et d'autorisation propres à chaque écosystème.

### Ajouté
- Section périmètre et limites assumant les cas durs : jetons illiquides, réseaux abandonnés couteux, miette isolée sous le seuil des frais mutualisés.
- Note réglementaire Phase 0 sur la qualification possible de l'opérateur d'agrégation comme intermédiaire financier.

### Inchangé
- Statut Phase 0, bénévolat, absence de partenaires.
- Licences : spécification CC BY 4.0, code MIT, EIP CC0.

## [2026-06-11] v0.1-draft - Pivot vers le standard ouvert

### Changé
- **Pivot de modèle** : abandon du modèle reposant sur des relayeurs et une commission, au profit d'un standard ouvert que les wallets implémentent directement, sans intermédiaire, sans commission, sans token.
- **Formule de répartition** simplifiée : `montant brut - frais réseau = net ONG`. Suppression du terme de commission et du concept de réserve technique.
- **Licence** de la documentation : passage en CC BY 4.0.

### Ajouté
- **Guide** complet (FR/EN) : vue d'ensemble, acteurs, parcours d'un don.
- **Livre Blanc Wallets** (FR/EN) : spécification technique, six principes non-négociables, flux en cinq étapes, deux modes (immédiat par défaut, agrégé optionnel), compatibilité EOA / ERC-4337 / EIP-7702, prise en charge des frais réseau.
- **Périmètre normatif** explicite : ce que le standard définit, ce qu'il laisse libre.
- **Points "À définir"** exposés en clair : seuil par défaut, fenêtre et contrat d'agrégation, composition de la liste blanche d'actifs et de réseaux, format du reçu local, atomicité fine, gouvernance de la liste d'ONG.
- Squelette d'une **EIP** à soumettre ultérieurement à l'écosystème Ethereum (non publiée à ce stade).

### Archivé
- Anciens documents du modèle relayeur déplacés dans `archive/` avec en-tête d'obsolescence : livre blanc relayeurs (FR/EN), guide v1.1-draft (FR/EN), ancien livre blanc wallets (FR/EN).

---

## Archivé - ligne relayeur (obsolète)

Les entrées ci-dessous décrivent l'ancien modèle relayeur, conservé pour l'historique. Elles ne décrivent plus le standard actuel.

### [2025-10-21] v1.1-draft
- Raisonnement en crypto plutôt qu'en fiat rendu opérationnel.
- Politique gas v0.1 : L2-first, pool gas relayeur, filet de sécurité documenté on-chain.
- Formule de l'époque : `Net ONG = montant brut - gas - frais réseau - commission`.
- Clarifications account abstraction : ERC-4337, paymasters, cas permit contre approbation payante.
- Liste blanche v1 : ETH, USDC, USDT sur Ethereum et L2, Polygon PoS avec gas en POL.
- Gouvernance technique : multi-sig, timelock, procédures d'urgence.
- Conformité minimale : screening d'adresses, références FATF et OFAC.
- Ajouts : tables des matières cliquables et callouts.
