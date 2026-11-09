# ShopWise — Étude de cas Bloc 3

Projet d'entraînement du Bloc 3 :
Intégration, industrialisation et déploiement du logiciel.

## Présentation

ShopWise est une application de gestion destinée
aux commerces de proximité.

## Technologies

- Backend : Java 21 / Spring Boot
- Frontend : Angular 22
- Base de données : PostgreSQL
- Industrialisation : Docker / GitHub Actions

## Fonctionnalités

- Gestion des clients
- Gestion des rendez-vous
- Fidélisation des clients

## Workflow Git

Le projet utilise un workflow basé sur des branches de fonctionnalités et des Pull Requests.

### 1. Organisation des branches

- `main` : branche principale contenant le code stable.
- `feature/issue-X-description` : développement d'une fonctionnalité.
- `fix/issue-X-description` : correction d'une anomalie.
- `docs/description` : documentation du projet.

Chaque fonctionnalité est développée dans une branche dédiée.

### 2. Convention de commits

Les commits respectent une convention inspirée des Conventional Commits :

- `feat` : ajout d'une fonctionnalité
- `fix` : correction d'un bug
- `test` : ajout ou modification de tests
- `docs` : documentation
- `refactor` : restructuration du code
- `chore` : maintenance et configuration

Exemple :
`feat(clients): ajouter la création d'un client refs #1`

### 3. Cycle de développement

1. Sélectionner une Issue dans GitHub Projects.
2. Passer son statut à "In progress".
3. Créer une branche à partir de `main`.
4. Développer la fonctionnalité et ses tests.
5. Effectuer des commits liés à l'Issue.
6. Pousser la branche sur GitHub.
7. Créer une Pull Request vers `main`.
8. Vérifier les tests et les éventuels contrôles CI.
9. Fusionner la Pull Request dans `main`.
10. Passer l'Issue à "Done".

### 4. Suivi des tâches

Les tâches sont gérées avec GitHub Issues et GitHub Projects.

Le tableau Kanban comprend trois colonnes :

- **Todo** : tâches à réaliser.
- **In progress** : tâches en cours de développement.
- **Done** : tâches terminées et validées.

Les Issues sont également classées à l'aide de labels par module et statut.

Les Pull Requests peuvent utiliser `Closes #X` pour fermer automatiquement l'Issue liée après leur fusion dans la branche principale.
