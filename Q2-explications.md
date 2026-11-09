# ShopWise - Q2 | Notes de conception

## Base : périmètre de l'énoncé
La question 2 demande un modèle de données relationnel pour **Clients**, **Fidélisation** et **Rendez-vous**, plus un schéma PDF et un script SQL (structure + données de test).

## Les 5 tables proposées
1. `commerces` : permet d'isoler les données de plusieurs commerces (contrainte générale de multi-commerce).
2. `clients` : fiche client rattachée à un commerce ; email unique **par commerce**.
3. `services` : catalogue de services d'un commerce ; chaque service possède une durée et une règle de points.
4. `rendez_vous` : réservation d'un service par un client, avec date/heure et statut.
5. `transactions_fidelite` : historique immuable des gains ; une seule attribution par rendez-vous.

## Choix de conception qui ne sont pas imposés par l'énoncé
- PostgreSQL comme SGBD relationnel.
- `commerces` pour préparer le multi-commerce, et `services` comme entité dédiée plutôt qu'un texte libre.
- Les statuts : `PLANIFIE`, `HONORE`, `ANNULE`.
- Les points dépendent du service (`points_attribues`) : 10, 20, 15 dans le jeu d'essai. L'énoncé ne fixe ni barème ni durée.
- Le solde est calculé comme la somme des gains, sans stocker de solde redondant.
- Les dates/horaires sont en `TIMESTAMPTZ` (heure locale à convertir/conserver avec fuseau).
- Un rendez-vous terminé est considéré comme terminal ; on ne le bascule pas dans un autre état sans règle de compensation explicite.

## Règles métier à implémenter plus tard en Spring Boot
- Au changement d'état `PLANIFIE -> HONORE`, créer **une seule** transaction de fidélité dans la même transaction BD.
- Au changement d'état `PLANIFIE -> ANNULE`, n'attribuer aucun point.
- Interdire qu'un rendez-vous utilise le service d'un autre commerce (également garanti par les clés étrangères composites).
- Ne pas permettre deux gains pour le même rendez-vous (contrainte `UNIQUE`).
- Ne jamais déclarer qu'une identification côté client protège les données tant que l'API est ouverte, conformément à la contrainte technique de l'énoncé. La user story « se connecter » nécessite un arbitrage pédagogique explicite.

## Exécution locale de la base
À lancer depuis PowerShell dans la racine du projet, avec Docker Desktop démarré :

```powershell
docker run -d --name shopwise-db -e POSTGRES_USER=shopwise -e POSTGRES_PASSWORD=shopwise_dev -e POSTGRES_DB=shopwise -p 5433:5432 -v shopwise_pgdata:/var/lib/postgresql/data postgres:17
```

Attendre que la base soit prête :

```powershell
docker exec shopwise-db pg_isready -U shopwise -d shopwise
```

Copier le fichier SQL vers le conteneur, puis l'exécuter :

```powershell
docker cp .\bdd\script.sql shopwise-db:/tmp/script.sql
docker exec shopwise-db psql -v ON_ERROR_STOP=1 -U shopwise -d shopwise -f /tmp/script.sql
```

Une fois le script chargé, vérifier les tables et les soldes :

```powershell
docker exec shopwise-db psql -U shopwise -d shopwise -c "\dt shopwise.*"
docker exec shopwise-db psql -U shopwise -d shopwise -c "SELECT * FROM shopwise.v_soldes_fidelite ORDER BY client_id;"
```

**ATTENTION** : la commande SQL `DROP SCHEMA IF EXISTS shopwise CASCADE` efface toutes les données du schéma `shopwise`. C'est un script d'initialisation pour une base **de test seulement**, pas un script de migration de production.

**Note** : le mot de passe ci-dessus est une valeur fictive pour un exercice LOCAL, à ne pas réutiliser dans une application exposée ; ne pas versionner de vrai secret.
