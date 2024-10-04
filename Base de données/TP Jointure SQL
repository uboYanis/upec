
Creér un fichier docker-compose.yml

```yaml
version: '3.8'

services:
  db:
    image: postgres:16.4-bullseye
    container_name: postgres_db
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: admin
      POSTGRES_DB: tp_dbb
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  postgres_data:
```

Ouvrez un terminal et naviguez jusqu'au répertoire contenant le fichier docker-compose.yml

Exécutez la commande suivante pour démarrer le conteneur en mode détaché :

```console
docker-compose up -d
```

Pour vérifier que le conteneur PostgreSQL fonctionne correctement, exécutez la commande suivante :

```console
docker-compose ps
```

Vous pouvez vous connecter à la base de données PostgreSQL en utilisant DBeaver

Adresse : localhost
Port : 5432
Nom d'utilisateur : admin
Mot de passe : admin
Nom de la base de données : tp_dbb


==========================================================

Voici les requêtes SQL pour créer les tables et insérer les jeux de données que nous avons décrits. Ces requêtes vous permettront de configurer votre base de données PostgreSQL pour les exercices de jointure.

1. Création des tables
Création de la table clients
```sql
CREATE TABLE clients (
    client_id SERIAL PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    ville VARCHAR(50) NOT NULL
);
```

Création de la table commandes

```sql
CREATE TABLE commandes (
    commande_id SERIAL PRIMARY KEY,
    client_id INT REFERENCES clients(client_id),
    produit VARCHAR(50) NOT NULL,
    montant DECIMAL(10, 2) NOT NULL
);
```
Création de la table produits

```sql
CREATE TABLE produits (
    produit_id SERIAL PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    categorie VARCHAR(50) NOT NULL
);
```
Création de la table produit_commandes

```sql
CREATE TABLE produit_commandes (
    commande_id INT REFERENCES commandes(commande_id),
    produit_id INT REFERENCES produits(produit_id),
    PRIMARY KEY (commande_id, produit_id)
);
```

2. Insertion des données

Insertion dans la table clients

```sql
INSERT INTO clients (nom, ville) VALUES
('Alice', 'Paris'),
('Bob', 'Lyon'),
('Charlie', 'Marseille'),
('David', 'Toulouse'),
('Eva', 'Nantes');
```

Insertion dans la table commandes

```sql
INSERT INTO commandes (client_id, produit, montant) VALUES
(1, 'Ordinateur', 1200),
(1, 'Imprimante', 150),
(2, 'Smartphone', 800),
(3, 'Tablette', 300),
(4, 'Téléviseur', 600),
(4, 'Enceinte', 250),
(5, 'Moniteur', 300);
```

Insertion dans la table produits

```sql
INSERT INTO produits (nom, categorie) VALUES
('Ordinateur', 'Électronique'),
('Imprimante', 'Électronique'),
('Smartphone', 'Téléphonie'),
('Tablette', 'Électronique'),
('Téléviseur', 'Électronique'),
('Enceinte', 'Audio'),
('Moniteur', 'Électronique'),
('Webcam', 'Accessoire');
```

Insertion dans la table produit_commandes

```sql
INSERT INTO produit_commandes (commande_id, produit_id) VALUES
(1, 1),
(2, 2),
(3, 3),
(4, 4),
(5, 5),
(6, 6),
(7, 7);
```
