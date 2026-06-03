# SaveForge

SaveForge est un éditeur avancé de fichiers de sauvegarde orienté RPG et jeux utilisant des formats JSON, LZString ou Ruby Marshal.

L'application fonctionne entièrement dans le navigateur, sans installation ni serveur, et permet d'explorer, comparer, modifier et exporter des sauvegardes de manière rapide et sécurisée.

## Fonctionnalités

### 📂 Chargement de sauvegardes

Prise en charge de plusieurs formats :

* JSON
* LZString (RPG Maker MV/MZ)
* Base64 JSON
* Ruby Marshal (`.rvdata`, `.rvdata2`, `.rxdata`)
* Formats binaires génériques
* Données collées directement depuis le presse-papiers

Formats acceptés :

```text
.rpgsave
.rmmzsave
.rvdata
.rvdata2
.rxdata
.json
.nson
.dat
.data
.sav
.save
```

---

## 🌳 Exploration des données

* Affichage hiérarchique (Tree View)
* Affichage plat (Flat View)
* Navigation rapide dans les structures complexes
* Dépliage/repliage des branches
* Statistiques rapides pour les sauvegardes RPG Maker

---

## ✏️ Édition en temps réel

Modification directe des valeurs :

* Nombres
* Chaînes de caractères
* Booléens
* Valeurs nulles

Fonctions intégrées :

* Ajout de clés
* Suppression de clés
* Incrémentation/Décrémentation rapide
* Édition contextuelle

---

## ↩️ Historique des modifications

* Undo (Ctrl + Z)
* Redo (Ctrl + Y)
* Historique des changements
* Indication visuelle des fichiers modifiés

---

## 🔍 Recherche avancée

Recherche dans :

* Les clés
* Les valeurs

Options disponibles :

* Respect de la casse
* Mot entier
* Expressions régulières (Regex)

---

## 🔄 Find & Replace

Recherche et remplacement global :

* Texte simple
* Expressions régulières
* Clés et valeurs
* Types numériques et booléens

Prévisualisation avant application des modifications.

---

## 📊 Comparaison de sauvegardes

Comparaison multi-fichiers avec :

* Détection des différences
* Filtres personnalisés
* Regroupement par catégories
* Calcul des écarts numériques (Δ)
* Export CSV des différences

---

## 🎯 Navigation rapide

### Jump To Path

Accès instantané à une propriété spécifique :

```text
system._battleCount
actors._data.1._level
party._gold
```

---

## 🎮 Support RPG Maker

Détection automatique des structures RPG Maker MV/MZ :

* System
* Party
* Actors
* Variables
* Switches
* Map
* Player
* Screen
* Timer

Affichage de statistiques utiles :

* Or
* Nombre de sauvegardes
* Combats
* Victoires
* Position du joueur
* Niveau et HP des personnages

---

## 📤 Export

Export des sauvegardes modifiées dans leur format d'origine lorsque cela est possible.

Formats pris en charge :

* JSON
* LZString
* Base64 JSON

---

## 🛠️ Technologies utilisées

* HTML5
* CSS3
* JavaScript Vanilla
* LZString
* Tabler Icons

---

## 🚀 Utilisation

1. Ouvrir `index.html` dans un navigateur moderne.
2. Glisser-déposer une ou plusieurs sauvegardes.
3. Explorer ou modifier les données.
4. Exporter les fichiers mis à jour.

Aucune installation requise.

---

## 📄 Licence

Projet distribué sous la licence de votre choix.
