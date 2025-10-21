# DUSTETHIC - LE GUIDE COMPLET [2025-10-21]

**Version**: 1.1-draft  
**Date**: Octobre 2025  
**Dernière mise à jour**: [2025-10-21]  
**Statut**: Phase 0 - Document de cadrage

> ⚠️ Avertissement - Phase 0  
> - Document informatif - pas un conseil financier ou juridique.  
> - Le projet est en recherche - certaines capacités dépendent d’ERC-4337, de paymasters et de l’usage de L2.  
> - Les montants de référence sont en unités crypto. Les équivalents € ne sont qu’indicatifs.  
> - Transparence exigée: commission, fenêtres d’agrégation, réseaux supportés et option gas choisie doivent être affichés publiquement.

---

## 🎯 Principe fondamental

**On raisonne en crypto, pas en fiat.**  
Les montants sont comptés en unités natives de la chaîne utilisée. Exemple: vous donnez 0.0100 ETH, l’ONG reçoit 0.0090 ETH si la commission annoncée est 10%. Cette logique **neutralise la volatilité dans la répartition** entre acteurs. La **valeur en fiat reste fluctuante** tant que chacun n’a pas converti sa part.  
Dans ce guide, les équivalents € ne servent qu’à l’intelligibilité.

### 🔌 Politique gas v0.1 - par défaut

- **L2-first**: opérations priorisées sur des L2 à faibles frais (ex. Optimism, Arbitrum) afin de rendre le coût gas marginal.  
- **Pool gas du relayeur**: le relayeur maintient un pool du jeton natif requis pour le gas (ex. ETH sur L2 EVM). **Aucune conversion n’est faite sur les dons** pour financer la commission.  
- **Filet de sécurité optionnel**: si le pool gas est insuffisant, une **conversion minimale et documentée on-chain** peut être déclenchée pour acheter le jeton gas, sans modifier la formule de répartition.  
- **Affichage standard**:  
  - Formule de calcul: `Net ONG = Montant brut - gas - frais réseau - commission`  
  - Commission affichée **en pourcentage de la crypto donnée** (ex. 7% en ETH si don en ETH)  
- **Transparence**: l’option gas retenue (pool gas, L2-first, filet de sécurité) est **déclarée publiquement** par chaque relayeur.

---

## 📚 Sommaire

- [Principe fondamental](#principe-fondamental)
- [Politique gas v0.1 - par defaut](#-politique-gas-v01---par-defaut)
- [1) Le probleme reel aujourdhui](#1-le-probleme-reel-aujourdhui)
- [2) La solution proposee par DustEthic](#2-la-solution-proposee-par-dustethic)
- [3) Flux operationnel realiste](#3-flux-operationnel-realiste)
- [4) Gas, conversions et options de conception](#4-gas-conversions-et-options-de-conception)
- [5) Volatilite - principes et strategies](#5-volatilite-principes-et-strategies)
- [6) Acteurs et responsabilites](#6-acteurs-et-responsabilites)
- [7) Donateurs - mode demploi](#7-donateurs---mode-demploi)
- [8) ONG - integration, compta, conformite](#8-ong---integration-compta-conformite)
- [9) Relayeurs - exigences minimales norme v01](#9-relayeurs---exigences-minimales-de-la-norme-dustethic-v01)
- [10) References du marche et positionnement](#10-references-du-marche-et-positionnement)
- [11) Roadmap](#11-roadmap)
- [12) Rejoindre le projet](#12-rejoindre-le-projet)
- [13) Licence](#13-licence)
- [14) Notes et references](#14-notes-et-references)

---

## 1) Le problème réel aujourd’hui

- Sur Ethereum L1, **les frais de gas sont payés en ETH** et peuvent dépasser de petits dons. Sur L2, ils sont beaucoup plus faibles mais jamais nuls.  
- Plusieurs L2 EVM utilisent aussi **ETH comme jeton gas** (ex. Arbitrum, Optimism). Polygon PoS utilise **POL** depuis la migration MATIC→POL.  
- Conséquence: un micro-don isolé est souvent inefficace sur L1, parfois acceptable sur L2, et dépend des conditions réseau.

---

## 2) La solution proposée par DustEthic

**Agrégation + transparence on-chain + répartition en unités crypto**:

- Des **relayeurs** agrègent des micro-dons pendant une période limitée, puis effectuent **un transfert groupé** vers l’ONG.  
- La part ONG, les frais réseau et la commission sont **calculés en pourcentage de la crypto donnée**, pas en équivalent €.  
- La traçabilité se fait via des explorateurs publics (ex. Etherscan pour Ethereum).

**Briques techniques déjà existantes**:

- **Account Abstraction ERC-4337** avec **paymasters** pour sponsoriser le gas du donateur.  
- **EIP-2612 permit** quand disponible, pour des approbations par signature sans transaction on-chain d’approve.

---

## 3) Flux opérationnel réaliste

**Étape 1 - Don**  
- Don via smart account AA avec paymaster: **gas sponsorisé**, le donateur ne paie pas directement.  
- Don via EOA + token avec permit: **approbation sans gas** puis don relayé.  
- Don via EOA + token sans permit: une **approbation payante** peut être nécessaire, selon le token.

**Étape 2 - Agrégation**  
- Dons collectés dans un smart contract d’agrégation. Déclencheurs recommandés: seuil de montant, fenêtre de temps max, fenêtre de gas acceptable.

**Étape 3 - Transfert groupé**  
- Une transaction unique expédie les fonds vers l’ONG.  
- Formule standardisée:  
  - `Net ONG = Montant brut - gas - frais réseau - commission`

**Étape 4 - Répartition publique**  
- Dons, conversions éventuelles et virement final sont consultables sur l’explorateur de la chaîne.

---

## 4) Gas, conversions et options de conception

**Contraintes physiques**: sur EVM, le gas se paie dans le **jeton natif** de la chaîne utilisée. Exemples: ETH sur Ethereum, Optimism, Arbitrum. **POL** sur Polygon PoS.  
Pour respecter le principe « pas de conversion pour la commission », DustEthic v0.1 propose des **options explicites** pour financer le gas:

- **Option A - Pool gas du relayeur**: le relayeur maintient un pool du jeton gas requis. Pas de conversion sur les dons.  
- **Option B - Conversion minimale documentée**: prélèvement pro-rata en nature pour acheter le jeton gas, journalisé on-chain, sans impacter la formule de répartition au-delà du coût gas.  
- **Option C - L2-first**: opérer prioritairement sur des L2 à faibles frais afin que le gas soit marginal.  
- **Option D - Sponsors**: paymasters sponsorisés par des partenaires qui alimentent le gas, remboursés périodiquement.

---

## 5) Volatilité: principes et stratégies

**Règle**: la répartition se fait en unités crypto. Les pourcentages restent constants, la valeur en € varie tant que l’ONG et le relayeur n’ont pas converti.

**Stablecoins**: réduisent la volatilité mais **n’éliminent pas le risque** (depeg, gel d’adresses, risque émetteur).

**Stratégies après réception**:  
- **ONG**: vendre immédiatement, vendre partiellement, ou conserver selon la politique interne et la tolérance au risque.  
- **Relayeur**: vente régulière, conservation, ou approche mixte.

---

## 6) Acteurs et responsabilités

- **Donateurs**: émettent de petits montants, idéalement via AA pour éviter de payer le gas directement.  
- **Relayeurs**: opèrent l’agrégation, publient des paramètres publics, respectent la norme v0.1 et tiennent un journal public des opérations sensibles.  
- **ONG**: reçoivent directement dans leur wallet, mettent en place une politique de conversion et un minimum de procédures de conformité.

---

## 7) Donateurs - mode d’emploi

1) Choisir un relayeur conforme DustEthic.  
2) Connecter son wallet.  
3) Sélectionner l’ONG.  
4) Saisir le montant en **crypto**.  
5) Signer. Selon le cas, le don est gasless via AA ou permit. Sinon une approbation payante peut survenir selon le token.

**Coût pour le donateur**: idéalement nul via AA ou permit. Sinon, uniquement l’approbation initiale si requise par le token. Les frais gas du transfert final sont **mutualisés** et déduits avant versement à l’ONG.

**Suivi**: chaque don et le virement final sont visibles sur l’explorateur de la chaîne.

---

## 8) ONG - intégration, compta, conformité

**Wallet recommandé**: **Safe** (ex-Gnosis Safe) en multi-signature pour la garde.  
**Conversion en fiat**: via un échange enregistré, selon vos politiques.  
**Comptabilité**: enregistrer la valeur au moment de la réception, définir une politique de conversion, tracer les adresses.  
**Conformité minimale**: même en non-custodial, adoptez un **screening des adresses** et une politique écrite. Référentiels: **OFAC** et **FATF R.15 / Travel Rule**. Les obligations exactes dépendent de votre juridiction et de votre statut.

---

## 9) Relayeurs - exigences minimales de la norme DustEthic v0.1

**Transparence**  
- Code open-source. Paramètres publics: commission en %, fenêtres d’agrégation, réseaux supportés, option gas choisie.  
- Dashboard lisible on-chain.

**Non-custodial**  
- Fonds détenus par des smart contracts. Gouvernance technique sans pouvoir de retrait unilatéral.

**Gouvernance technique**  
- Rôles d’admin sous **multi-sig Safe**. Timelock sur changements critiques. Procédures d’urgence.

**Sécurité**  
- Audit indépendant avant mainnet. Bug bounty après lancement.

**Gas et conversions**  
- Choisir explicitement Option A, B, C ou D et l’afficher publiquement.  
- Commission toujours en pourcentage de la crypto donnée.  
- Si une conversion de gas est nécessaire, la journaliser on-chain.

**AA et compatibilité**  
- Support ERC-4337 et paymaster recommandé sur L2. L’EntryPoint publié est la référence d’implémentation.

**Liste blanche d’actifs v1**  
- Ethereum et L2 EVM: ETH, USDC, USDT.  
- Polygon PoS: opération possible mais **gas en POL** - prévoir la logistique correspondante.  
- Refuser par défaut tokens illiquides, taxés, honeypots ou sans permit si l’expérience devient impraticable.

**Conformité minimale**  
- Politique AML proportionnée, screening basique, journalisation des refus.

---

## 10) Références du marché et positionnement

- Des plateformes de dons crypto **classiques** existent et ciblent surtout les dons moyens ou élevés avec conversion rapide en fiat.  
  - **Every.org**: conversion instantanée en USD, commission broker 1% + frais réseau.  
  - **The Giving Block**: packages d’abonnement et frais de traitement, détails communiqués commercialement.  
- **DustEthic** se positionne sur les **micro-dons par agrégation** et la **répartition native en crypto** sur des L2 à faibles frais.

---

## 11) Roadmap

**Phase 0 - Fondations [Q4 2025]**  
- Spécification norme v0.1  
- Liste blanche d’actifs et réseaux v1  
- Design des contrats d’agrégation et du paymaster - testnet  
- Politique sécurité et gouvernance

**Phase 1 - Développement [2026]**  
- Implémentation de référence open-source  
- Tests Sepolia et L2 correspondants  
- Audit tiers  
- Pilotes avec 1 relayeur et 2 ONG

**Phase 2 - Lancement [2026+]**  
- Déploiement mainnet + 2 L2  
- 3-5 relayeurs conformes, 10+ ONG  
- Dashboard communautaire

**Phase 3 - Extension [2027+]**  
- Plus de L2, éventuellement autres écosystèmes EVM  
- Gouvernance élargie si traction

---

## 12) Rejoindre le projet

- **Développeurs**: smart accounts, paymasters, agrégateurs. Références ERC-4337 et EntryPoint pour démarrer.  
- **ONG**: tester avec un wallet **Safe** et une politique interne de conversion.  
- **Relayeurs**: piloter une implémentation L2-first et documenter publiquement les métriques gas et délais.  
- **Communauté**: feedback, traductions, contenus.

Liens utiles:  
- Site web: https://dustethic.org  
- Discord: https://discord.gg/fVFc26GV  
- GitHub: https://github.com/DustEthic  
- Bluesky: @dustethic.bsky.social

---

## 13) Licence

- Texte: **CC BY 4.0**  
- Code futur: **MIT**

---

## 14) Notes et références

- **ERC-4337 - Account Abstraction**: Documentation et EntryPoint  
  - https://docs.erc4337.io/  
  - https://docs.erc4337.io/smart-accounts/entrypoint-explainer.html  
  - Aperçu AA par OpenZeppelin: https://docs.openzeppelin.com/contracts/5.x/account-abstraction

- **EIP-2612 - Permit**  
  - OpenZeppelin guide: https://docs.openzeppelin.com/contracts-cairo/2.x/guides/erc20-permit  
  - Tutoriel QuickNode: https://www.quicknode.com/guides/ethereum-development/transactions/how-to-use-erc20-permit-approval

- **Gas - définitions et coûts réseau**  
  - Etherscan Gas Tracker: https://etherscan.io/gastracker  
  - Optimism - estimation des frais: https://docs.optimism.io/app-developers/transactions/estimates  
  - Arbitrum - FAQ gas en ETH: https://docs.arbitrum.io/learn-more/faq  
  - Polygon PoS - migration MATIC→POL: https://polygon.technology/blog/matic-to-pol-migration-is-99-complete-everything-you-need-to-know

- **Volatilité stablecoins**  
  - USDC depeg Mars 2023 (analyse académique): https://www.mdpi.com/2674-1032/3/4/30  
  - Synthèse presse: https://www.investopedia.com/usdc-loses-peg-7254222

- **Sécurité de garde**  
  - Safe (ex-Gnosis Safe): https://docs.safe.global/ et https://safe.global/wallet

- **Conformité - AML, OFAC, FATF**  
  - OFAC - FAQ virtual currency: https://ofac.treasury.gov/faqs/topic/1626  
  - OFAC - Guidance PDF: https://ofac.treasury.gov/media/913571/download?inline=  
  - FATF - Targeted Update R.15 (2024): https://www.fatf-gafi.org/en/publications/Fatfrecommendations/targeted-update-virtual-assets-vasps-2024.html  
  - FATF - Targeted Update (2025, PDF): https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/2025-Targeted-Upate-VA-VASPs.pdf.coredownload.pdf  
  - Best Practices Travel Rule (2025, PDF): https://www.fatf-gafi.org/content/dam/fatf-gafi/recommendations/Best-Practices-Travel-Rule-Supervision.pdf

- **Plateformes de dons existantes**  
  - Every.org crypto - 1% broker + réseau: https://www.every.org/crypto et https://support.every.org/hc/en-us/articles/1500007842902-Are-there-any-fees-for-donating-cryptocurrency  
  - The Giving Block - offre et frais via packages: https://thegivingblock.com/ et https://thegivingblock.com/packages/

---

**Fin du Guide DustEthic v1.1-draft**
