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
echo -e "test@gmail.com\nCGT\nGA\nA B C\nCGT est un syndicat\nBonjour\nAAAGT\nGTA\norganisation\nGroupe GA\nCeci est un exemple.\nQuelle est la situation ?\nA A A\nAAAGT\nCGT GA\nBonjour tout le monde.\n1A\nABAC\nBACGTA\n" > adn.data
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

**Explication** : Cette commande cherche toutes les lignes dans le fichier `adn.data` qui contiennent le mot "CGT". Le motif est sensible à la casse par défaut, donc "cgt" ne sera pas inclus.

#### Question 1.b
**Rechercher toutes les lignes contenant le mot "GA".**

```bash
grep "GA" adn.data
```

**Explication** : Ici, nous recherchons toutes les lignes contenant "GA". Comme précédemment, la recherche est sensible à la casse.

#### Question 1.c
**Rechercher toutes les lignes ne finissant pas par un caractère de fin de phrase (. ! ? :).**

```bash
grep -v '[.!?:]$' adn.data
```

**Explication** : L'option `-v` inverse le critère de recherche. Cela affiche les lignes qui ne se terminent pas par l'un des caractères de fin de phrase spécifiés.

#### Question 1.d
**Rechercher toutes les lignes où apparaissent plusieurs "A" consécutifs (au moins 2).**

```bash
grep "A\{2,\}" adn.data
```

**Explication** : Cette commande utilise une expression régulière qui cherche des occurrences de la lettre "A" répétée au moins deux fois de suite.

#### Question 1.e
**Rechercher toutes les lignes où plusieurs "A" ne terminent pas la ligne.**

```bash
grep "A\{2,\}" adn.data | grep -v "A\{2,\}$"
```

**Explication** : Nous cherchons d'abord les lignes avec plusieurs "A" puis filtrons ces résultats pour exclure celles qui se terminent par "A" répété.

#### Question 1.f
**Rechercher les lignes contenant "C", puis "G", puis "T" dans l’ordre, avec n’importe quel caractère entre ces trois caractères.**

```bash
grep "C.*G.*T" adn.data
```

**Explication** : Ici, `.*` représente n'importe quel caractère (y compris aucun) et permet de chercher "C", suivi de "G", puis de "T" dans cet ordre, quelle que soit la distance entre eux.

### Exercice 2 : Utiliser des expressions régulières avancées

#### Question 2.a
**Pour un fichier quelconque, donnez une expression régulière qui liste tous les mots contenant une lettre doublée.**

```bash
grep -E '\b([a-zA-Z])\1\b' fichier.txt
```

**Explication** : Cette commande utilise l'option `-E` pour les expressions régulières étendues et recherche les mots contenant des lettres doublées, identifiées par `\1`, qui fait référence à la lettre capturée juste avant.

### Exercice 3 : Options avancées de grep

#### Question 3.a
**Rechercher dans le fichier passwd.data tous les utilisateurs dont le nom contient un chiffre.**

```bash
grep '[0-9]' passwd.data
```

**Explication** : Cette commande recherche toutes les lignes dans `passwd.data` contenant au moins un chiffre. Cela peut aider à identifier les utilisateurs ayant des noms qui incluent des chiffres.

### Exercice 4 : Recherches avancées avec grep

#### Question 4.a
**Rechercher toutes les lignes d'un fichier contenant un mot qui commence par une voyelle.**

```bash
grep -E '\b[aeiouAEIOU][a-zA-Z]*\b' fichier.txt
```

**Explication** : Ici, nous cherchons des mots qui commencent par une voyelle (qu'elle soit en majuscule ou en minuscule) et qui sont suivis de zéro ou plusieurs lettres.

#### Question 4.b
**Rechercher les lignes contenant des adresses email.**

```bash
grep -E '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' fichier.txt
```

**Explication** : Cette commande recherche les lignes qui contiennent des adresses email, en utilisant une expression régulière qui capture le format standard d'une adresse email.

## Conclusion
Ce TP vous a permis de vous familiariser avec l'utilisation de `grep` et les expressions régulières pour rechercher des motifs dans des fichiers texte. Vous avez appris à utiliser des expressions régulières simples et avancées, ainsi que des options pratiques pour améliorer vos recherches.
