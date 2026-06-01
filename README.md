# 🛠️ Guide de Personnalisation - LainkyBlock Addon

Ce guide explique comment modifier facilement les **butins (drops)**, la **rareté**, et la **fréquence d'apparition (spawn)** de votre addon **LainkyBlock** pour Minecraft 1.20.1.

---

## 📝 1. Personnaliser les Butins (`drops.txt`)

Le fichier `drops.txt` gère tout ce qui se passe quand le bloc est cassé. Chaque ligne représente un drop ou un événement.

### A. Gérer la rareté avec le paramètre `@luck`
À la fin de chaque ligne, vous trouverez `@luck=X` (ex: `@luck=1`, `@luck=-2`). Cela indique sur quel type de bloc le butin peut tomber :
* **`@luck=3` (Légendaire / Très rare)** : Réservé aux objets surpuissants. N'apparaît que sur des blocs très chanceux (ex: fabriqués avec des diamants).
* **`@luck=2` (Rare)** : Objets utiles ou matériaux rares.
* **`@luck=1` ou `@luck=0` (Commun / Neutre)** : Objets de base (nourriture, lingots, haches normales). Apparaît sur les blocs standards trouvés dans la nature.
* **`@luck=-1` ou `@luck=-2` (Malchanceux / Pièges)** : Monstres, pièges à enclumes. N'apparaît que si le bloc est négatif (ex: fabriqué avec de la chair putréfiée).

*👉 Pour rendre un objet plus rare, augmentez sa valeur de `@luck` (max 3).*

---

### B. Modifier la quantité d'objets (`amount`)
Pour modifier le nombre d'objets donnés, changez le paramètre `amount` :
* **Quantité fixe** : `amount=64` (donnera exactement 64 cookies).
* **Quantité aléatoire** : `amount=#rand(min, max)`. 
  * *Exemple* : `amount=#rand(1,5)` donnera entre 1 et 5 diamants au hasard.

---

### C. Personnaliser le nom et la couleur d'un objet (`NBTTag`)
Vous pouvez renommer un objet et lui donner une couleur spéciale dans l'inventaire en modifiant le tag `display` :
* *Syntaxe* : `NBTTag=(display=(Name=#jsonStr(text="Votre Nom",color=couleur,bold=true)))`
* *Couleurs disponibles* : `cyan`, `yellow`, `gold`, `red`, `dark_red`, `green`, etc.

---

### D. Regrouper plusieurs actions (`group(...)`)
Si vous voulez donner plusieurs objets en même temps, faire apparaître un monstre ET envoyer un message dans le tchat, utilisez les groupes séparés par des points-virgules `;` :
```text
group(
    ID=minecraft:diamond,amount=3;
    type=message,ID="Bravo ! Tu as trouve le tresor !";
    type=entity,ID=minecraft:firework_rocket
)@luck=1
```

---

## 🌍 2. Régler la Fréquence d'Apparition dans le Monde (`natural_gen.txt`)

Le fichier `natural_gen.txt` gère le taux d'apparition naturelle de votre bloc sur la carte.

Voici comment sont structurées les lignes :
```text
>minecraft:overworld
type=block,ID=lucky:lainkyblock@chance=600
```

### ⚙️ Comment modifier la densité ?
Le paramètre `@chance=X` détermine la rareté par chunk (zone de 16x16 blocs).
⚠️ **Règle d'or : Plus le nombre est PETIT, plus le bloc apparaît SOUVENT !**

* **Pour avoir BEAUCOUP de blocs** (génération fréquente) :
  Diminuez le nombre. *Exemple :* `@chance=100` (1 chance sur 100 par chunk).
* **Pour avoir TRÈS PEU de blocs** (génération rare) :
  Augmentez le nombre. *Exemple :* `@chance=1200` (1 chance sur 1200 par chunk).

---

## 🎨 3. Ajouter des Textures et Sons personnalisés

Si vous utilisez le dossier non zippé dans votre dossier `addons/lucky/`, vous pouvez faire des modifications en direct :

1. **Textures** : Remplacez l'image PNG dans `assets/lucky/textures/block/lainkyblock.png`.
2. **Rechargement rapide** : En jeu, appuyez sur **`F3 + T`** pour voir vos modifications de texture s'appliquer instantanément sans relancer le jeu !

## 🏛️ 4. Intégrer le LainkyBlock dans des Structures (ex: Born in Chaos)

Vous pouvez remplacer des blocs par défaut (comme des coffres ou des blocs de valeur) dans les ruines et donjons d'un mod pour y placer vos **LainkyBlocks** !

### A. Extraire le fichier de structure `.nbt`
1. Trouvez le fichier `.jar` du mod de donjon (ex: `born_in_chaos_[Forge]1.20.1_1.7.5.jar`) dans votre dossier `mods`.
2. Ouvrez le `.jar` avec un logiciel d'archive comme **7-Zip** ou **WinRAR**.
3. Allez dans `data/[nom_du_mod]/structures/` et extrayez le fichier `.nbt` de la structure voulue (ex: `graveyard.nbt`).

---

### B. Modifier le fichier `.nbt` sans ouvrir Minecraft (Via NBTExplorer)
1. Ouvrez le fichier `.nbt` avec le logiciel **NBTExplorer** (ou un éditeur de NBT en ligne).
2. Déroulez le dossier **`palette`** (la liste des blocs utilisés dans la structure).
3. Repérez l'entrée du bloc que vous voulez remplacer (par exemple, un coffre `minecraft:chest` ou un bloc décoratif) :
   * Remplacez la valeur de son tag **`Name`** par : **`lucky:lainkyblock`**.
   * **Nettoyage :** Si c'était un coffre, faites un clic droit sur le sous-dossier **`Properties`** et cliquez sur **`Delete`** (car le Lucky Block n'utilise pas les propriétés `facing` ou `waterlogged`).
4. **Supprimer l'inventaire du coffre (Très Important ⚠️) :**
   * Allez dans le dossier **`tiles`** (ou **`block_entities`**) à la racine du fichier `.nbt`.
   * Parcourez les éléments pour trouver celui qui a la position (`pos`) correspondant à votre coffre.
   * Clic droit sur cet élément ➡️ **`Delete`** pour effacer ses données d'inventaire et éviter tout conflit.
5. Enregistrez les modifications.

---

### C. Déploiement dans l'Addon
Placez le fichier `.nbt` ainsi modifié à cet endroit précis dans votre dossier d'addon :
📂 `LainkyBlock_Addon/data/[nom_du_mod]/structures/[nom_de_la_structure].nbt`

*Exemple pour Born in Chaos :*  
📂 `LainkyBlock_Addon/data/born_in_chaos_v1/structures/ruin_castle.nbt`

Désormais, dès que cette ruine apparaîtra dans votre monde, le **LainkyBlock** sera déjà généré à l'intérieur.
