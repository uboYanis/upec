# TP : Création et Gestion de schémas avec privilèges en utilisant des rôles

## Objectif
- Créer des utilisateurs dans Oracle.
- Gérer les privilèges avec des rôles.
- Créer des tables dans un schéma.
- Ajouter, modifier et supprimer des données.

## Étapes

### 1. Connexion à Oracle as SYSDBA

```console
docker exec -it oracle-db sqlplus sys/admin@//localhost:1521/FREEPDB1 as SYSDBA
```

### 2. Création des Utilisateurs

**Pourquoi :** Les utilisateurs doivent être créés pour gérer les différentes opérations dans la base de données.

**Solution :** Créez les utilisateurs suivants : `data_owner`, `data_manager`, et `app_user`.

```sql
create user data_owner identified by "Supersecurepassword!";
create user data_manager identified by "Supersecurepassword!";
create user app_user identified by "Supersecurepassword!";
```

**Explication :** Chaque utilisateur est créé avec un mot de passe. Ces utilisateurs auront différents rôles et privilèges dans le système.

### 3. Attribution de Privilèges de Connexion et de Création

**Pourquoi :** Avant de pouvoir créer des tables, l'utilisateur `data_owner` a besoin de privilèges de connexion et de création.

**Solution :** Attribuez les privilèges nécessaires.

```sql
grant create session to data_owner;
grant create table to data_owner;
ALTER USER data_owner QUOTA 10M ON users;

grant create session to data_manager;
grant create session to app_user;
```

**Explication :**
- Le privilège `create session` permet à l'utilisateur de se connecter à la base de données.
- Le privilège `create table` permet à l'utilisateur de créer des tables dans son schéma.

### 4. Création des Rôles

**Pourquoi :** Les rôles permettent de regrouper des privilèges, facilitant ainsi leur gestion.

**Solution :** Créez les rôles suivants : `data_reader_role` et `data_editor_role`.

```sql
create role data_reader_role;
create role data_editor_role;
```

**Explication :** Les rôles définis ici représentent des ensembles de privilèges. Par exemple, `data_reader_role` sera destiné aux utilisateurs ayant besoin d'accéder aux données sans les modifier.

### 5. Création de Tables

**Pourquoi :** Avant de pouvoir insérer des données, il faut d'abord créer une structure pour stocker ces données.

**Solution :** Connectez-vous à la base de données avec l'utilisateur `data_owner` et créez une table `customers`.

```console
docker exec -it oracle-db sqlplus data_owner/Supersecurepassword!@//localhost:1521/FREEPDB1
```

Ensuite, exécutez :

```sql
create table customers (customer_id integer not null primary key, customer_name varchar2(100) not null);
```

**Explication :**
- Ici, la table `customers` est créée avec deux colonnes : `customer_id` (identifiant unique pour chaque client) et `customer_name` (nom du client). `customer_id` est défini comme clé primaire pour garantir l'unicité.

### 6. Attribution de Privilèges aux Rôles

**Pourquoi :** Il est essentiel d'attribuer des privilèges spécifiques aux rôles pour contrôler les actions que les utilisateurs peuvent effectuer.

**Solution :** Attribuez les privilèges suivants aux rôles :

```sql
grant select on data_owner.customers to data_reader_role;
grant select, insert, update, delete on data_owner.customers to data_editor_role;
```

**Explication :**
- Le rôle `data_reader_role` reçoit le privilège de sélection (`SELECT`) sur la table `customers`, permettant aux utilisateurs de lire les données.
- Le rôle `data_editor_role` reçoit le privilège de sélection (`SELECT`), reçoit des privilèges d'insertion (`INSERT`), de mise à jour (`UPDATE`) et de suppression (`DELETE`), permettant aux utilisateurs de lire et modifier les données.

### 7. Attribution des Rôles aux Utilisateurs

**Pourquoi :** L'attribution des rôles aux utilisateurs permet de contrôler l'accès et les privilèges de chaque utilisateur en fonction de leurs responsabilités.

**Solution :**

```sql
grant data_reader_role to app_user;
grant data_editor_role to data_manager;
```

**Explication :**
- `app_user` reçoit le rôle `data_reader_role`, ce qui lui permet uniquement de lire les données.
- `data_manager` reçoit le rôle `data_editor_role`, ce qui lui permet de modifier les données dans la table.

### 8. Insertion de Données

**Pourquoi :** Les données doivent être ajoutées à la table pour pouvoir les manipuler.

**Solution :** Connectez-vous avec l'utilisateur `data_manager` et insérez des données dans la table `customers`.

```console
docker exec -it oracle-db sqlplus data_manager/Supersecurepassword!@//localhost:1521/FREEPDB1
```

Ensuite, exécutez :

```sql
insert into data_owner.customers values (1, 'John');
insert into data_owner.customers values (2, 'Doe');
insert into data_owner.customers values (3, 'Alice');
commit; -- Validation de la transaction
```

**Explication :** Les commandes `INSERT` ajoutent des enregistrements à la table `customers`. La commande `COMMIT` assure que les modifications sont enregistrées de manière permanente dans la base de données.

### 9. Vérification des Données

**Pourquoi :** Il est crucial de vérifier que les données ont été correctement insérées.

**Solution :** Connectez-vous avec l'utilisateur `app_user` et effectuez une requête pour vérifier les données :

```console
docker exec -it oracle-db sqlplus app_user/Supersecurepassword!@//localhost:1521/FREEPDB1
```

Ensuite, exécutez :

```sql
set lines 256; -- Pour améliorer la lisibilité des résultats.
select * from data_owner.customers;
```

**Explication :** La commande `SELECT` récupère toutes les lignes de la table `customers`, permettant à l'utilisateur de voir les données qui ont été insérées.

### 10. Modification de Données

**Pourquoi :** Les utilisateurs peuvent avoir besoin de mettre à jour les informations existantes.

**Solution :** Connectez-vous avec l'utilisateur `data_manager` pour modifier le nom du client ayant l'id = 2.

```sql
UPDATE data_owner.customers SET customer_name = 'Iron' WHERE customer_id = 2;
commit; -- Validation de la transaction
```

**Explication :** La commande `UPDATE` modifie le nom du client avec `customer_id` égal à 2. Encore une fois, le `COMMIT` est utilisé pour enregistrer cette modification.

### 11. Suppression de Données

**Pourquoi :** Les utilisateurs peuvent également avoir besoin de supprimer des données obsolètes.

**Solution :** Connectez-vous avec l'utilisateur `data_manager` pour supprimer un client de la table `customers`.

```sql
DELETE FROM data_owner.customers WHERE customer_id = 1;
commit; -- Validation de la transaction
```

**Explication :** La commande `DELETE` supprime le client ayant `customer_id` égal à 1. Une fois de plus, le `COMMIT` assure que cette opération de suppression est enregistrée dans la base de données.

## Conclusion
Ce TP a démontré comment créer des utilisateurs, des rôles et des tables dans Oracle, tout en gérant les privilèges d'accès. L'utilisation de rôles simplifie la gestion des autorisations et garantit que chaque utilisateur a accès uniquement aux fonctionnalités nécessaires à son rôle. La validation des transactions avec `COMMIT` est essentielle pour garantir que toutes les modifications apportées à la base de données sont permanentes et visibles pour les autres utilisateurs.
