
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



================================================


Exercice 1: Liste des clients avec leurs commandes
Question : Écrivez une requête pour obtenir une liste de tous les clients avec leurs commandes. Incluez le nom du client, la ville et le produit commandé.

Solution :

```sql
SELECT c.nom AS nom_client, c.ville, co.produit
FROM clients c
INNER JOIN commandes co ON c.client_id = co.client_id;
```

Exercice 3: Montant total des commandes par client
Question : Écrivez une requête pour obtenir le montant total des commandes pour chaque client. Incluez le nom du client et le montant total des commandes.

```sql
sql
Copier le code
SELECT c.nom AS nom_client, SUM(co.montant) AS total_commandes
FROM clients c
INNER JOIN commandes co ON c.client_id = co.client_id
GROUP BY c.nom;
```

Exercice 4: Liste des produits commandés avec les clients
Question : Écrivez une requête pour lister tous les produits commandés avec le nom du client et le montant de chaque commande.

Solution :

```sql
SELECT c.nom AS nom_client, co.produit, co.montant
FROM commandes co
INNER JOIN clients c ON co.client_id = c.client_id;
```

Exercice 5: Liste des produits disponibles sans commandes
Question : Écrivez une requête pour afficher tous les produits qui n'ont pas été commandés.

Solution :

```sql
SELECT p.nom AS produit
FROM produits p
LEFT JOIN produit_commandes pc ON p.produit_id = pc.produit_id
WHERE pc.commande_id IS NULL;
```

Exercice 6: Détails des commandes avec informations sur les produits
Question : Écrivez une requête pour obtenir le détail de chaque commande avec le nom du produit, le montant et la catégorie du produit.

Solution :

```sql
SELECT co.commande_id, co.produit, co.montant, p.categorie
FROM commandes co
JOIN produit_commandes pc ON co.commande_id = pc.commande_id
JOIN produits p ON pc.produit_id = p.produit_id;
```

Exercice 7: Compte des commandes par ville
Question : Écrivez une requête pour compter le nombre de commandes passées par ville.

Solution :

```sql
SELECT c.ville, COUNT(co.commande_id) AS nombre_commandes
FROM clients c
INNER JOIN commandes co ON c.client_id = co.client_id
GROUP BY c.ville;
```

Exercice 8: Clients ayant passé plusieurs commandes
Question : Écrivez une requête pour afficher les clients qui ont passé plus d'une commande.

Solution :

```sql
SELECT c.nom AS nom_client, COUNT(co.commande_id) AS nombre_commandes
FROM clients c
INNER JOIN commandes co ON c.client_id = co.client_id
GROUP BY c.nom
HAVING COUNT(co.commande_id) > 1;
```

Exercice 9: Produits commandés et leur montant total
Question : Écrivez une requête pour afficher chaque produit commandé et le montant total généré par chaque produit.

Solution :

```sql
SELECT p.nom AS produit, SUM(co.montant) AS montant_total
FROM produits p
JOIN produit_commandes pc ON p.produit_id = pc.produit_id
JOIN commandes co ON pc.commande_id = co.commande_id
GROUP BY p.nom;
```

Exercice 10: Détails des commandes avec clients
Question : Écrivez une requête pour obtenir les détails des commandes, y compris le nom du client, le produit commandé et le montant.

Solution :

```sql
SELECT c.nom AS nom_client, co.produit, co.montant
FROM commandes co
INNER JOIN clients c ON co.client_id = c.client_id;
```



