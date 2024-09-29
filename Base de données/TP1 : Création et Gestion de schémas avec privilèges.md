
### TP1 : Création et Gestion de schémas avec privilèges

#### Objectif

- Création d'utilisateurs (schémas) dans Oracle.
- Gestion des privilèges.
- Création de tables dans un schéma.
- Ajouter / modifier / supprimer des données.

#### Étapes

1. **Connexion à Oracle**
    - Connecte-toi à ta base de données Oracle en tant qu'utilisateur ayant les privilèges nécessaires (par exemple, en tant
      qu'utilisateur `SYS` ou un autre administrateur).

    - **_Solution_**

      ```console
      docker exec -it oracle-db sqlplus sys/admin@//localhost:1521/FREEPDB1 as SYSDBA
      ```

2. **Création des Utilisateurs**
    - Crée un utilisateur : `data_owner`.

    - **_Solution_**

        ```sql
        create user data_owner identified by "Supersecurepassword!";
        ```
      **Explication :**
        - **CREATE USER :** Cette commande crée un nouvel utilisateur dans la base de données.
        - **IDENTIFIED BY :** Définit le mot de passe de l'utilisateur.



3. **Attribution de Privilèges**

- Dans le contexte des bases de données, donnez moi la définition d'un Privilège et la définition d'un Schéma


- **Privilège 1:**
    - Attribuez un privilège qui permet à l'tilisateurs `data_owner` de se connecter à la base de données. Ensuite, expliquez l'importance de ce privilège pour l'exécution d'autres actions dans la base de données.

    - **_Solution_**

       ```sql
       grant create session to data_owner;
       ```
      **Explication :**
        - **Privilège :** CREATE SESSION
        - **Description :** Ce privilège permet à l'utilisateur de se connecter à la base de données.
        - **Importance :** C'est un privilège de base. Sans cela, un utilisateur ne peut pas établir de connexion avec la base de données, et par conséquent, il ne peut exécuter aucune action.



- **Privilège 2:**
    - Attribuez un privilège qui permet à l'tilisateurs `data_owner` de créer des tables dans leur propre schéma. Ensuite, expliquez pourquoi ce privilège est essentiel pour la gestion des données dans la base de données.

    - **_Solution_**

       ```sql
       grant create table to data_owner;
       ```
      **Explication :**
        - **Privilège :** CREATE TABLE
        - **Description :** Permet à l'utilisateur de créer des tables dans son propre schéma.
        - **Importance :** Ce privilège est crucial pour la gestion des données. Les utilisateurs doivent pouvoir créer des tables pour stocker et gérer les informations dans la base de données.



4. **Création de tables**

> **INFO :** Ouvrez un nouveau terminal.

- Connectez-vous à la base de données avec l'utilisateur **data_owner**.
    - **_Solution_**

        ```console
        docker exec -it oracle-db sqlplus data_owner/Supersecurepassword!@//localhost:1521/FREEPDB1
        ```


- Créez une table **customers** avec 2 colonnes : customer_id et customer_name.
    - _**Solution**_

        ```sql
        create table customers (customer_id integer not null primary key,customer_name varchar2(100) not null);
        ``` 

4. **Insertion de données**

    - Ajoute deux **customers:**
        - `id = 1` | `nom = john`
        - `id = 2` | `nom = doe`

    - **_Solution_**

      Sur le compte `sys`, accorder à l'utilisateur `data_owner` un quota.

       ```sql
       ALTER USER data_owner QUOTA 10M ON users;
       ```
      **Explication :**
        - **Description :** Permet à l'administrateur de modifier les attributs d'un utilisateur, notamment en lui attribuant une quota d'espace de stockage spécifique sur un tablespace donné (dans cet exemple, `10M` sur le tablespace `users`).
        - **Importance :** La gestion des quotas est essentielle pour contrôler l'utilisation des ressources dans la base de données. Cela permet d'éviter qu'un utilisateur n'utilise tout l'espace disponible, garantissant ainsi que les ressources sont réparties équitablement entre tous les utilisateurs et permettant une gestion efficace de l'espace de stockage.

      Sur le compte `sys`, attribuer le privilège d'insertion à l'utilisateur `data_owner` sur la table `customers` de son `schéma`.

       ```sql
       GRANT INSERT ON data_owner.customers TO data_owner;
       ```

      **Explication :**
        - **Privilège :** INSERT
        - **Description :** Permet à l'utilisateur `data_owner` d'insérer de nouvelles lignes dans la table `customers` de son `schéma`.
        - **Importance :** Ce privilège est crucial pour la gestion des données, car il permet aux utilisateurs de contribuer en ajoutant des informations à la base de données. Sans ce privilège, l'utilisateur ne pourrait pas introduire de nouvelles données, ce qui limiterait ses capacités à gérer efficacement les informations de la table `customers`.

      Sur le compte `data_owner`, insérer les données.

       ```sql
       insert into customers values ( 1, 'john' );
       insert into customers values ( 2, 'doe' );
       ``` 

5. **Modification de données**

    - Modifier le nom du customer ayant l'id = 2.

    - **_Solution_**

      Sur le compte `sys`, attribuer le privilège de modification à l'utilisateur `data_owner` sur la table `customers` de son `schéma`.

       ```sql
       GRANT UPDATE ON data_owner.customers TO data_owner;
       ```

      **Explication :**
        - **Privilège :** UPDATE
        - **Description :** Permet à l'utilisateur `data_owner` de modifier des lignes dans la table `customers` de son `schéma`.
        - **Importance :** Ce privilège est essentiel pour la gestion des données, car il permet aux utilisateurs de mettre à jour les informations dans la base de données. Sans ce privilège, l'utilisateur ne pourrait pas modifier les données, ce qui limiterait sa capacité à maintenir les informations à jour et précises.

      Sur le compte `data_owner`, modifier la ligne.

       ```sql
       UPDATE data_owner.customers SET customer_name = 'Iron' WHERE customer_id = 2;
       ``` 

6. **Suppression de données**

    - Supprimer le client ayant l'id = 2.

   - **_Solution_**

      Sur le compte `sys`, attribuer le privilège de suppression à l'utilisateur `data_owner` sur la table `customers` de son `schéma`.
    
      ```sql
      GRANT DELETE ON data_owner.customers TO data_owner;
      ```
     **Explication :**
       - **Privilège :** DELETE
       - **Description :** Permet à l'utilisateur data_owner de supprimer des lignes dans la table customers de son schéma.
       - **Importance :** Ce privilège est essentiel pour la gestion des données, car il permet aux utilisateurs de supprimer des informations obsolètes ou incorrectes dans la base de données. Sans ce privilège, l'utilisateur ne pourrait pas gérer les erreurs ou les données non valides, compromettant ainsi la qualité de la base.
    Sur le compte data_owner, supprimer la ligne.

      Sur le compte data_owner, supprimer la ligne.
      ```sql
      DELETE FROM data_owner.customers WHERE customer_id = 2;
      ```
