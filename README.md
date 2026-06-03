# SaveForge - Éditeur et Comparateur Haute Performance de Sauvegardes de Jeux

SaveForge est une application web mono-fichier (Single Page Application) autonome, conçue pour l'inspection chirurgicale, la modification en temps réel et l'analyse différentielle de fichiers de sauvegarde de jeux vidéo. Bien qu'optimisée pour les structures de données générées par les moteurs RPG Maker (XP, VX, VX Ace, MV, MZ), l'architecture de SaveForge lui permet de traiter de manière générique des flux JSON, Base64, binaires bruts et des flux compressés complexes.

## 1. Justifications Architecturales et Choix Technologiques

### Conception Zero-Framework et Performance de Rendu
Le choix architectural central de SaveForge repose sur l'absence de frameworks d'interface utilisateur (tels que React, Vue ou Angular) au profit d'une manipulation directe et optimisée du Document Object Model (DOM). Les fichiers de sauvegarde de jeux contiennent fréquemment des dizaines de milliers de nœuds imbriqués. L'utilisation d'un DOM virtuel ou d'un système de réactivité par proxy aurait induit une surcharge mémoire et des blocages de l'interface utilisateur (UI blocking) inacceptables lors des phases de recherche globale ou d'expansion d'arborescence.
* Rendu itératif sélectif : L'arborescence utilise des structures d'affichage conditionnel basées sur des classes CSS pour masquer ou afficher les nœuds enfants, évitant des cycles de recalcul de mise en page (reflows) massifs.
* Grid Layout et Isolation : L'interface utilise CSS Grid au niveau de la structure principale pour isoler la zone de navigation (Sidebar), le panneau de contrôle et l'espace de visualisation (View Area), garantissant un défilement indépendant sans impact sur le thread principal.

### Stratégie No-Blocking UI
Pour maintenir la réactivité de l'application lors d'opérations lourdes comme la recherche par expression régulière sur l'intégralité d'un graphe d'objets, le moteur de recherche et le module Find & Replace opèrent via des itérations directes sur la structure de données sous-jacente encapsulée dans l'objet global `App`. Les fonctions de synchronisation visuelle minimisent les écritures dans le DOM en reconstruisant uniquement les fragments visibles.

---

## 2. Cartographie des Dépendances

L'application est entièrement contenue dans un fichier unique, limitant ses dépendances à deux bibliothèques tierces chargées de manière asynchrone via des réseaux de diffusion de contenu (CDN) sécurisés :

1. **LZ-String (v1.4.4)** : Essentielle pour la gestion des sauvegardes RPG Maker MV et MZ. Ces moteurs compressent les objets de sauvegarde JSON en chaînes de caractères encodées en Base64 via un algorithme de compression propriétaire basé sur LZW.
   * Documentation officielle : [https://pieroxy.net/lua/lz-string/](https://pieroxy.net/lua/lz-string/)[cite: 2]
2. **Tabler Icons (v3.19.0)** : Fournit un jeu d'icônes vectorielles complet via une feuille de style CSS et une police web, évitant le chargement d'images matricielles lourdes.
   * Documentation officielle : [https://tabler.io/icons](https://tabler.io/icons)[cite: 2]

---

## 3. Analyse Technique des Composants Complexes

### Déconstruction du Désérialiseur Ruby Marshal (Marshal 4.8)
Les versions de RPG Maker antérieures à MV (XP, VX, VX Ace) s'appuient sur le langage Ruby et son mécanisme natif de sérialisation binaire nommé `Marshal`. SaveForge intègre une implémentation complète et autonome en JavaScript pur pour analyser ces flux d'octets sans interpréteur côté serveur.

La classe `Marshal` instanciée dans le code décode le flux de la manière suivante :
1. **Validation du Header** : Le pointeur de lecture `this.p` commence à l'index `2`, sautant volontairement les deux premiers octets du flux représentant la version (historiquement `0x04 0x08` pour Marshal 4.8).
2. **Gestion des Entiers Signés Compressés (`int()`)** : Le format Marshal compresse les entiers sur un nombre variable d'octets en fonction de leur magnitude. Le premier octet fait office d'indicateur :
   * Si la valeur est `0`, l'entier vaut `0`.
   * Si la valeur est comprise entre `5` et `127`, il s'agit d'un entier positif direct (`valeur - 5`).
   * Si la valeur est comprise entre `128` et `251`, il s'agit d'un entier négatif direct.
   * Les valeurs de `1` à `4` et `252` à `255` indiquent le nombre d'octets suivants à lire pour reconstituer l'entier sur 8, 16, 24 ou 32 bits (avec extension de signe).
3. **Table de Symboles (Cache de Liens) et d'Objets** : Pour optimiser l'espace, Marshal stocke les chaînes récurrentes (comme les noms de variables ou de propriétés) sous forme de symboles (`:`). Chaque symbole lu est inséré dans un tableau `this.sym`. Les occurrences ultérieures font référence à l'index du symbole via le type de jeton `;`. Le même principe s'applique aux structures complexes d'objets (`o`, `[`, `{`) via le tableau `this.obj` et le jeton `@`.
