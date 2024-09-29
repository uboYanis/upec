# TP : Utilisation de Grep avec Expressions Régulières

## Objectif du TP
L'objectif de ce TP est de se familiariser avec l'utilisation de l'outil `grep` pour effectuer des recherches de motifs dans des fichiers texte. Nous explorerons les expressions régulières pour améliorer la précision de nos recherches et apprendrons à utiliser des options avancées de `grep`.

## Création des fichiers de test
Avant de commencer les exercices, nous allons créer quelques fichiers texte contenant des exemples de données. Voici un script simple pour créer des fichiers de test :

```bash
mkdir tp_grep
cd tp_grep
echo -e "CGT\nGA\nA B C\nCGT est un syndicat\nBonjour\nAAAGT\nGTA\norganisation\nGroupe GA" > test1.txt
echo -e "CGT\nGA\nAAAGT\nA\nB\nC\nGroupe CGT\nOrganisation" > test2.txt
echo -e "Groupe G\nGG\nCGT\nCGT GA\nG\nA\nAA\n" > test3.txt
```

Ces fichiers contiendront diverses lignes de texte que nous utiliserons pour tester les commandes `grep`.

## Exercice 1 : Rechercher des motifs simples

### Question 1.a
**Rechercher toutes les lignes contenant le mot "CGT".**

**Indice :** Utilisez simplement `grep` suivi du mot à rechercher.

**Solution :**
```bash
grep "CGT" *.txt
```

**Explication :** Cette commande recherche le mot "CGT" dans tous les fichiers texte du répertoire. Les lignes contenant ce mot seront affichées.

---

### Question 1.b
**Rechercher toutes les lignes contenant le mot "GA".**

**Indice :** Suivez la même logique que pour la recherche précédente.

**Solution :**
```bash
grep "GA" *.txt
```

**Explication :** Cette commande fonctionne de manière identique à la précédente, mais elle recherche les lignes contenant le mot "GA".

---

## Exercice 2 : Utiliser les expressions régulières

### Question 2.a
**Rechercher toutes les lignes qui commencent par "A".**

**Indice :** Utilisez le caractère `^` pour indiquer le début de la ligne.

**Solution :**
```bash
grep "^A" *.txt
```

**Explication :** Ici, `^A` indique que nous recherchons toutes les lignes dont le premier caractère est "A". Les lignes correspondantes seront affichées.

---

### Question 2.b
**Rechercher toutes les lignes qui se terminent par "organisation".**

**Indice :** Utilisez le caractère `$` pour indiquer la fin de la ligne.

**Solution :**
```bash
grep "organisation$" *.txt
```

**Explication :** Dans cette commande, `organisation$` signifie que nous recherchons les lignes qui se terminent par le mot "organisation".

---

### Question 2.c
**Rechercher toutes les lignes contenant soit "CGT" soit "GA".**

**Indice :** Utilisez le caractère `|` pour le "OU".

**Solution :**
```bash
grep "CGT\|GA" *.txt
```

**Explication :** Ici, nous utilisons `CGT\|GA` pour rechercher les lignes contenant soit "CGT" soit "GA". L'utilisation de `\` permet d'échapper le caractère `|`.

---

### Question 2.d
**Rechercher toutes les lignes contenant un mot qui commence par "G" suivi de n'importe quel caractère.**

**Indice :** Utilisez le point `.` qui représente n'importe quel caractère.

**Solution :**
```bash
grep "G." *.txt
```

**Explication :** Dans cette commande, `G.` signifie que nous recherchons les lignes contenant un mot qui commence par "G" et est suivi de n'importe quel caractère.

---

### Question 2.e
**Rechercher toutes les lignes contenant un nombre quelconque de "A" consécutifs.**

**Indice :** Utilisez `*` pour indiquer zéro ou plusieurs occurrences.

**Solution :**
```bash
grep "A*" *.txt
```

**Explication :** La commande `A*` recherche les lignes contenant zéro ou plusieurs occurrences de "A". Cela inclut même les lignes qui n'ont pas "A".

---

### Question 2.f
**Rechercher toutes les lignes contenant exactement trois "A" consécutifs.**

**Indice :** Utilisez `\{n\}` pour spécifier un nombre exact.

**Solution :**
```bash
grep "A\{3\}" *.txt
```

**Explication :** Ici, `A\{3\}` signifie que nous recherchons les lignes contenant exactement trois "A" consécutifs.

---

## Exercice 3 : Options avancées de `grep`

### Question 3.a
**Compter le nombre de lignes contenant "CGT" et afficher les numéros de ligne.**

**Indice :** Utilisez l'option `-n` pour afficher les numéros de ligne.

**Solution :**
```bash
grep -n "CGT" *.txt
```

**Explication :** Cette commande affichera les lignes contenant "CGT" avec leur numéro de ligne respectif.

---

### Question 3.b
**Rechercher récursivement dans tous les fichiers du répertoire.**

**Indice :** Utilisez l'option `-r` pour une recherche récursive.

**Solution :**
```bash
grep -r "CGT" *
```

**Explication :** Cette commande recherchera "CGT" dans tous les fichiers, y compris ceux dans les sous-répertoires.

---

### Question 3.c
**Afficher le nombre total d'occurrences de "CGT".**

**Indice :** Utilisez l'option `-c` pour compter les occurrences.

**Solution :**
```bash
grep -c "CGT" *.txt
```

**Explication :** Cette commande affichera le nombre de lignes contenant "CGT" dans chaque fichier texte.

---

### Question 3.d
**Afficher le contexte autour des occurrences de "CGT" (2 lignes avant et 2 lignes après).**

**Indice :** Utilisez l'option `-C` suivie d'un nombre pour indiquer le contexte.

**Solution :**
```bash
grep -C 2 "CGT" *.txt
```

**Explication :** Cette commande affichera les lignes contenant "CGT" ainsi que 2 lignes avant et 2 lignes après chaque occurrence pour donner du contexte.

---

## Conclusion
Ce TP vous a permis de vous familiariser avec l'utilisation de `grep` et les expressions régulières pour rechercher des motifs dans des fichiers texte. Vous avez appris à utiliser des expressions régulières simples et avancées, ainsi que des options pratiques pour améliorer vos recherches.
