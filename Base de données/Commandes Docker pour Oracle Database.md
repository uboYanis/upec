# Détails des Commandes Docker pour Oracle Database

## 1. Tirer l'image Docker d'Oracle Database

```bash
docker pull container-registry.oracle.com/database/free:latest
```

-   **`docker pull`** : Cette commande télécharge une image Docker à partir d'un registre.
-   **`container-registry.oracle.com/database/free:latest`** : Il s'agit de l'URL de l'image à télécharger.
    -   **`container-registry.oracle.com`** : C'est le registre Docker d'Oracle.
    -   **`database/free`** : Indique que nous voulons l'image de la base de données Oracle gratuite.
    -   **`latest`** : Cela spécifie que nous souhaitons télécharger la dernière version disponible de l'image.

## 2. Vérifier les images Docker disponibles

```bash
docker images
```
- **`docker images`** : Cette commande affiche la liste des images Docker disponibles sur votre machine locale.
    -    Vous verrez des informations telles que l'ID de l'image, le nom de l'image, la balise, la date de création et la taille.
  
## 3. Exécuter le conteneur Oracle Database


```bash
docker run --name oracle-db -d -p 1521:1521 -e ORACLE_PWD=admin container-registry.oracle.com/database/free:latest
```

-   **`docker run`** : Cette commande crée et démarre un nouveau conteneur à partir d'une image Docker.
-   **`--name oracle-db`** : Cette option nomme le conteneur `oracle-db`. Cela permet de référencer le conteneur plus facilement dans les commandes ultérieures.
-   **`-d`** : Cette option exécute le conteneur en mode détaché (en arrière-plan).
-   **`-p 1521:1521`** : Cela mappe le port 1521 du conteneur (port par défaut pour Oracle) au port 1521 de la machine hôte, permettant ainsi d'accéder à Oracle Database depuis l'extérieur du conteneur.
-   **`-e ORACLE_PWD=admin`** : Cela définit une variable d'environnement à l'intérieur du conteneur. Ici, `ORACLE_PWD` est défini comme `admin`, qui sera le mot de passe pour l'utilisateur `SYS` de la base de données.
-   **`container-registry.oracle.com/database/free:latest`** : C'est l'image Docker que nous utilisons pour créer le conteneur.

## 4. Vérifier que le conteneur est en cours d'exécution

```bash
docker ps
```
-  **`docker ps`** : Cette commande affiche une liste des conteneurs Docker en cours d'exécution.
    -    Vous pouvez voir des détails tels que l'ID du conteneur, le nom, l'état, le port mappé, etc.

## 5. Se connecter à la base de données Oracle

```bash
docker exec -it oracle-db sqlplus sys/admin@//localhost:1521/FREEPDB1 as SYSDBA
```
-   **`docker exec`** : Cette commande permet d'exécuter une commande dans un conteneur en cours d'exécution.
-   **`-it`** : Ces options permettent d'exécuter le conteneur en mode interactif. `-i` garde l'entrée standard ouverte, et `-t` alloue un pseudo-terminal.
-   **`oracle-db`** : C'est le nom du conteneur dans lequel nous voulons exécuter la commande.
-   **`sqlplus`** : C'est l'outil de ligne de commande d'Oracle pour interagir avec la base de données.
-   **`sys/admin@//localhost:1521/FREEPDB1`** : Il s'agit de la chaîne de connexion à la base de données.
    -   **`sys`** : Nom d'utilisateur de la base de données, en général utilisé pour les opérations administratives.
    -   **`admin`** : Mot de passe de l'utilisateur `SYS`.
    -   **`localhost:1521`** : Adresse et port où la base de données est accessible (ici, depuis l'hôte local).
    -   **`FREEPDB1`** : Nom de la base de données à laquelle nous nous connectons.
-   **`as SYSDBA`** : Cela spécifie que nous voulons nous connecter avec des privilèges d'administrateur.
