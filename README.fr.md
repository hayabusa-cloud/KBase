# KBase

Base de connaissances structurée et auto-organisée pour agents de programmation IA.

[English](README.md) | [中文](README.zh-CN.md) | [Español](README.es.md) | [日本語](README.ja.md) | [Français](README.fr.md)

## Contexte

KBase persiste les connaissances de l'agent — règles, références, décisions, compétences — entre les sessions, pour que les agents démarrent informés au lieu de repartir de zéro.

## Structure

| Répertoire | Rôle |
|------------|------|
| `ROOT/` | Méta-principes, amorçage, commandes |
| `refs/` | Connaissances externes vérifiées |
| `notes/` | Observations et décisions de projet |
| `skills/` | Flux de travail automatisés |
| `rules/` | Contraintes et conventions |
| `env/` | Configurations d'environnement |
| `perms/` | Secrets et identifiants (dans gitignore) |
| `hooks/` | Callbacks événementiels |
| `plans/` | Plans de tâches autogénérés |

Chaque répertoire contient deux fichiers : `AGENTS.md` (guide pour l'agent) et `INDEX.md` (table des matières).

## Principes

1. **Persistance active** — Les agents sauvegardent leurs découvertes immédiatement, sans instruction.
2. **Sources faisant autorité** — Toute connaissance est vérifiée contre des sources primaires. Les connaissances non vérifiées ne sont pas stockées.
3. **Documentation d'abord** — Mettre à jour la documentation avant d'agir. En cas de conflit, mettre à jour la source de niveau inférieur.
4. **Dispatch de compétences** — Vérifier les compétences existantes avant tout travail ad-hoc.

## Protocole de Communication

KBase définit une notation structurée pour l'interaction humain–agent. Spécification complète : [`ROOT/communication.md`](ROOT/communication.md)

| Notation | Sémantique |
|----------|------------|
| `[[ ... ]]` | Livrable obligatoire |
| `[ ... ]` | Livrable optionnel |
| `(( ... ))` | Contrainte obligatoire — violation = blocked |
| `( ... )` | Contrainte préférentielle — meilleur effort |
| `( ... ) > ~( ... )` | Préférence avec réduction — préférer gauche ; réduire la tendance vers droite |
| `?( ... )` | Requête de validation |
| `< ... >` | Contexte d'entrée |
| `<< ... >>` | Pointeur de référence |
| `->` | Flux de contrôle — `?( Condition ) -> [ Action ]` |
| `[ X ] : ( Y )` | Revue-correction (souple) — examiner X selon Y ; corriger si non satisfait |
| `[[ X ]] : (( Y ))` | Revue-correction (stricte) — revue approfondie de X ; Y doit être satisfait |

États : `ready` → `done` (normal) ou `ready` → `blocked` (conflit/entrée manquante).

## Autorité

| Niveau | Source |
|--------|--------|
| 1 | `ROOT/` |
| 2 | `rules/` |
| 3 | `refs/` |
| 4 | `notes/` |
| 5 | Externe |

Le niveau supérieur prévaut. En cas de conflit, mettre à jour la source de niveau inférieur.

## Cycle de vie

| Transition | Condition |
|------------|-----------|
| notes → refs | Vérifié contre une source primaire |
| notes → rules | Le motif se répète, devient une contrainte |
| plans → notes/rules | Plan terminé, conclusions extraites |

## Démarrage rapide

1. Cloner : `git clone https://github.com/hayabusa-cloud/KBase.git`
2. Configurer l'agent de programmation IA pour pointer vers le répertoire `KBase/`.
3. Travailler. L'agent lit `AGENTS.md` au démarrage de session et maintient la base de connaissances automatiquement.

## Auto-Save

L'agent sauvegarde automatiquement les découvertes dans le répertoire approprié :

| Découverte | Destination |
|------------|-------------|
| Contraintes, conventions | `rules/` |
| Observations, décisions, patterns | `notes/` |
| Connaissances externes vérifiées | `refs/` |
| Flux de travail automatisés | `skills/` |
| Plans de tâches | `plans/` |

Pour ajouter du contenu manuellement, créer un fichier Markdown dans le répertoire cible et ajouter une entrée dans son `INDEX.md`.

## Bootstrap

Au démarrage de session, l'agent lit dans cet ordre :

`AGENTS.md` → `ROOT/AGENTS.md` → `ROOT/instructions.md` → `rules/INDEX.md` → `skills/INDEX.md` → `notes/INDEX.md`

Après compaction du contexte, l'agent relit au minimum les trois premiers fichiers.

## Commandes

```
research <topic>     Collecter et vérifier des informations ; sauvegarder dans refs/ ou notes/
dive into <topic>    Recherche approfondie en plusieurs passes avec des sources de pointe
review <subject>     Évaluer par rapport à KBase et à la recherche web
tidy <scope>         Réorganiser structure et contenu ; tidy all pour l'ensemble de KBase
recall <topic>       Chercher dans KBase les connaissances existantes avant toute recherche externe
save <topic>         Sauvegarder une découverte dans le répertoire approprié ; mettre à jour INDEX.md
promote <file>       Déplacer une note vers refs/ ou rules/ ; mettre à jour les deux INDEX.md
```

## Licence

[MIT](LICENSE) — ©2026 [Hayabusa Cloud Co., Ltd.](https://code.hybscloud.com)
