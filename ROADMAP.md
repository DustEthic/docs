# Feuille de route DustEthic

Date : 2026-06-14  
Statut : feuille de route de reprise, Phase 0.

## Principe de reprise

Le prototype wallet, le code et les démonstrations techniques sont mis au second plan tant que le standard n'a pas retrouvé son ADN.

Priorité : standard ouvert, règle crypto, frais visibles, preuve publique, rôles clairs, prudence juridique.

## Phase 0A - Retrouver l'ADN

Objectif : restaurer le socle conceptuel de DustEthic avant toute construction technique.

Livrables :

- `ADN.md` ;
- `STANDARD.md` réaligné ;
- `README.md` réaligné ;
- audit des archives ;
- liste des écarts entre documents récents et archives ;
- décision claire sur ce qui devient canonique.

Critère de sortie : une personne externe doit comprendre que DustEthic est un standard ouvert Phase 0, pas une plateforme, pas un token, pas une collecte, pas une promesse d'impact.

## Phase 0B - Stabiliser le standard

Objectif : transformer l'ADN en règles vérifiables.

Livrables :

- format d'intention de don ;
- schéma de preuve de lot ;
- politique gas v0.2 stabilisée ;
- actifs acceptés/refusés ;
- rôles donateur, wallet, relayeur, ONG ;
- exigences minimales de transparence ;
- modèle de versionnement du standard.

Critère de sortie : une implémentation de test peut produire une intention et une preuve cohérentes sans prétendre être en production.

## Phase 0C - Aligner les supports publics

Objectif : éviter que GitHub, site, Discord et documentation racontent des versions différentes.

Livrables :

- textes Discord alignés ;
- page site alignée ;
- README des dépôts alignés ;
- sources internes hiérarchisées ;
- mentions de prudence uniformes.

Critère de sortie : une même phrase de référence peut être reprise partout sans contradiction.

## Phase 1 - Spécifier

Objectif : produire des schémas et règles assez précis pour être testés.

Livrables :

- JSON Schema d'intention ;
- JSON Schema de preuve de lot ;
- règles d'expiration/révocation ;
- règle de plafond de frais ;
- scénarios d'échec ;
- premiers exemples de lots fictifs.

Critère de sortie : on peut simuler un lot complet, du consentement à la preuve, sans toucher de fonds réels.

## Phase 2 - Simuler

Objectif : démontrer le flux sans fonds réels.

Livrables :

- simulateur de lots ;
- exemples de preuves ;
- page de preuve lisible ;
- tests automatisés ;
- documentation d'installation.

Critère de sortie : le flux est compréhensible et vérifiable sans mainnet.

## Phase 3 - Prototyper testnet

Objectif : passer d'une simulation à une démonstration technique limitée.

Livrables :

- prototype testnet ;
- UX de signature explicite ;
- limites de montants ;
- logs et preuves ;
- revue sécurité interne.

Critère de sortie : prouver le flux technique, sans collecte réelle et sans promesse publique.

## Phase 4 - Auditer

Objectif : transformer un prototype en base sérieuse.

Livrables :

- audit sécurité ;
- revue UX des signatures ;
- revue juridique ;
- revue comptable ONG ;
- plan de gestion des incidents ;
- politique de versionnement.

Critère de sortie : connaître les risques avant toute expérimentation publique.

## Phase 5 - Pilote limité

Objectif : tester un cas réel sous contrôle, seulement si les revues précédentes sont concluantes.

Conditions minimales :

- ONG bénéficiaire identifiée et consentante ;
- adresse de réception vérifiée ;
- politique juridique validée ;
- montants plafonnés ;
- flux audité ;
- preuve publique ;
- possibilité d'arrêt immédiat.

## Ce qui n'est pas planifié maintenant

- lancement d'un token DustEthic ;
- collecte de fonds par DustEthic ;
- promesse de rendement ;
- déploiement mainnet sans audit ;
- automatisation qui masque les signatures ou les frais ;
- reprise du prototype wallet avant réalignement du standard.
