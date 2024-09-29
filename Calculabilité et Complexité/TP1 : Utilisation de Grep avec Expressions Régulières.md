# TP : Utilisation de Grep avec Expressions Régulières

## Objectif du TP
L'objectif de ce TP est de se familiariser avec l'utilisation de l'outil `grep` pour effectuer des recherches de motifs dans des fichiers texte. Nous explorerons les expressions régulières pour améliorer la précision de nos recherches et apprendrons à utiliser des options avancées de `grep`.

## Prérequis
- Avoir accès à un terminal Unix/Linux.
- Connaissance de base des commandes Unix.

## Création des fichiers de test
Avant de commencer les exercices, nous allons créer quelques fichiers texte contenant des exemples de données. Voici un script simple pour créer des fichiers de test :

```bash
mkdir tp_grep
cd tp_grep
echo -e "CGT\nGA\nA B C\nCGT est un syndicat\nBonjour\nAAAGT\nGTA\norganisation\nGroupe GA\nCeci est un exemple.\nQuelle est la situation ?\nA A A\nAAAGT\nCGT GA\nBonjour tout le monde.\n1A\nABAC\nBACGTA\n" > adn.data
echo -e "root:x:0:0:root:/root:/bin/bash\ndaemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin\ngava:x:1000:1000:gava,,,:/home/gava:/bin/bash\nmysql:x:125:134:MySQL Server,,,:/nonexistent:/bin/false" > passwd.data
echo -e "CGT\nGA\nA B C\nCGT est un syndicat\nBonjour\nAAAGT\nGTA\norganisation\nGroupe GA\nGroupe G\nGG\nCGT\nCGT GA\nG\nA\nAA\nroot:x:0:0:root:/root:/bin/bash\ndaemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin\ngava:x:1000:1000:gava,,,:/home/gava:/bin/bash\nmysql:x:125:134:MySQL Server,,,:/nonexistent:/bin/false" > fichier.txt
```

## Concepts de base sur grep

### Qu'est-ce que grep ?
`grep` est un utilitaire en ligne de commande utilisé pour rechercher des chaînes de caractères à l'intérieur de fichiers. Il utilise des expressions régulières pour faire correspondre les motifs recherchés.

### Syntaxe de base
La syntaxe générale de la commande `grep` est la suivante :

```bash
grep [options] motif [fichiers]
```

- `motif` : Le motif que vous souhaitez rechercher, souvent spécifié à l'aide d'expressions régulières.
- `fichiers` : Les fichiers dans lesquels effectuer la recherche.

### Options courantes
- `-i` : Ignore la casse lors de la recherche.
- `-v` : Inverse le critère de recherche (affiche les lignes ne correspondant pas au motif).
- `-r` : Recherche récursive dans les sous-répertoires.
- `-E` : Active les expressions régulières étendues (permet des motifs plus complexes).
- `-n` : Affiche le numéro de ligne des résultats.
- `-c` : Affiche uniquement le nombre de lignes correspondantes.

## Exercices pratiques

### Exercice 1 : Rechercher des motifs simples

#### Question 1.a
**Rechercher toutes les lignes contenant le mot "CGT".**

```bash
grep "CGT" adn.data
```

#### Question 1.b
**Rechercher toutes les lignes contenant le mot "GA".**

```bash
grep "GA" adn.data
```

#### Question 1.c
**Rechercher toutes les lignes ne finissant pas par un caractère de fin de phrase (. ! ? :)**

```bash
grep -v "[.!?:]$" adn.data
```

#### Question 1.d
**Rechercher toutes les lignes où apparaissent plusieurs "A" consécutifs (au moins 2).**

```bash
grep "A\{2,\}" adn.data
```

#### Question 1.e
**Rechercher toutes les lignes où plusieurs "A" ne terminent pas la ligne.**

```bash
grep "A\{2,\}" adn.data | grep -v "A\{2,\}$"
```

#### Question 1.f
**Rechercher les lignes contenant "C", puis "G", puis "T" dans l’ordre, avec n’importe quel caractère entre ces trois caractères.**

```bash
grep "C.*G.*T" adn.data
```

### Exercice 2 : Utiliser des expressions régulières avancées

#### Question 2.a
**Pour un fichier quelconque, donnez une expression régulière qui liste tous les mots contenant une lettre doublée.**

```bash
grep -E '\b([a-zA-Z])\1\b' fichier.txt
```

#### Question 2.b
**Complétez avec une expression régulière qui donne les mots avec lettre doublée qui ne sont pas des noms propres.**

```bash
grep -E '\b([a-z])([a-z]*)\1\1\b' fichier.txt
```

### Exercice 3 : Options avancées de grep

#### Question 3.a
**Rechercher dans le fichier passwd.data tous les utilisateurs dont le nom contient un chiffre.**

```bash
grep '[0-9]' passwd.data
```

### Exercice 4 : Recherches avancées avec grep

#### Question 4.a
**Rechercher toutes les lignes d'un fichier contenant un mot qui commence par une voyelle.**

```bash
grep -E '\b[aeiouAEIOU][a-zA-Z]*\b' fichier.txt
```

#### Question 4.b
**Rechercher les lignes contenant des adresses email.**

```bash
grep -E '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' fichier.txt
```

### Exercice 5 : Utilisation de grep avec des fichiers CSV

#### Question 5.a
**Rechercher dans un fichier CSV toutes les lignes contenant un nom spécifique.**

```bash
grep "NomSpecific" fichier.csv
```

#### Question 5.b
**Rechercher les lignes d'un fichier CSV où le score d'un élève est supérieur à 80 (en supposant que le score est dans la troisième colonne).**

```bash
awk -F, '$3 > 80' fichier.csv
```

## Conclusion
Ce TP vous a permis de vous familiariser avec l'utilisation de `grep` et les expressions régulières pour rechercher des motifs dans des fichiers texte. Vous avez appris à utiliser des expressions régulières simples et avancées, ainsi que des options pratiques pour améliorer vos recherches.

### Ressources supplémentaires
- Manuels de `grep` : `man grep`
- Tutoriels en ligne sur les expressions régulières et `grep`
