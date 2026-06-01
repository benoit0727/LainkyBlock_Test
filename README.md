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
