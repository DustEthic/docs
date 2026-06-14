# Reprise ADN DustEthic - audit court

Date : 2026-06-14
Branche : `recovery/adn-v1-2026-06-14`

## Diagnostic

Les fichiers récents du dépôt `DustEthic/docs` sont prudents, mais trop appauvris par rapport aux archives et au dépôt de travail historique `s-informatique/dustethic`.

Le problème principal n'est pas une erreur de prudence. La prudence est nécessaire. Le problème est la perte de structure : principe crypto, politique gas v0.2, plafond de campagne, rôle des relayeurs, preuve de lot, modèle ONG/wallet et récit ouvert du standard.

## Sources internes utilisées

- `DustEthic/docs/archives/dustethic-guide-v1.1-draft-fr.md`
- `DustEthic/docs/archives/dustethic-guide-v1.1-draft-en.md`
- `DustEthic/docs/archives/dustethic-relayeurs-livre-blanc-fr.md`
- `DustEthic/docs/archives/dustethic-wallets-livre-blanc-fr.md`
- `s-informatique/dustethic/docs/standard/STANDARD-DUSTETHIC.md`
- `s-informatique/dustethic/docs/technical/TECHNICAL-MODEL.md`
- `s-informatique/dustethic/README.md`

## Décisions de reprise

1. Ne pas supprimer les archives.
2. Ne pas réécrire `main` brutalement.
3. Créer une branche de redressement.
4. Restaurer d'abord l'ADN, puis le standard.
5. Mettre le prototype wallet/code au second plan tant que le standard n'est pas réaligné.

## Changements faits dans cette branche

- Ajout de `ADN.md` comme garde-fou de reprise.
- Réécriture de `README.md` pour remettre le projet dans son axe : standard ouvert, Phase 0, frais visibles, preuve publique, pas de token.
- Réécriture de `STANDARD.md` pour restaurer :
  - principe crypto > fiat ;
  - politique gas v0.2 ;
  - ratio `montant agrégé / gas estimé >= T` ;
  - repère `T >= 30` ;
  - plafond public historique de 15 % ;
  - rôles donateur, wallet, relayeur, ONG ;
  - preuve de lot minimale ;
  - anti-objectifs.

## Suite logique

1. Relire `STANDARD.md` sur cette branche.
2. Aligner `ROADMAP.md` avec la reprise ADN.
3. Aligner `SOURCES.md` avec la priorité des sources internes.
4. Créer ensuite les schémas d'intention et de preuve.
5. Reprendre le dépôt `DustEthic/dustethic` seulement après stabilisation du standard.
