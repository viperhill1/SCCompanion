<p align="center"><img src="docs/icon.png" width="128" alt="SC Companion"></p>

# SC Companion

**A Star Citizen companion app, focused on crafting.** Windows · English / français · free
Coded and maintained by **Viperhill**.

👉 **[Download the latest version](https://github.com/viperhill1/SCCompanion/releases/latest)**

*[Version française plus bas](#français)*

---

## What it does

- **Crafting**: pick a blueprint, set your target qualities, and the app tells you exactly which materials you are missing, and which missions give that blueprint.
- **Craft queue**: your projects grouped by ship or setup, with the shopping list of materials.
- **Mining route**: the fewest stops to get what you are missing, with the real chance of finding each ore and the signatures to look for on the scanner.
- **Ships**: a configurator (components, crafting qualities, pip-based power management, sustained DPS, shields, signatures, quantum range). Send your setup to crafting in one click.
- **Cart**: what can't be crafted, and where to buy it (UEX community prices, route, total).
- **Inventory and refining**: by hand or from screenshots of the freight manager and the refinery. Your owned blueprints are ticked automatically while you play.
- **Live signature scanner**: the app reads the number shown by the mining scanner and tells you which rock it is ("7 080 → 2 × Beryl"), in the app or in an overlay on top of the game. Rocks with an ore you need are marked ★. Game in Borderless mode.

The data is read **straight from your installed game files**: the app is up to date with every patch, and it never modifies the game.

## Install

1. Download `SCCompanion-vX.XX.zip` from [Releases](https://github.com/viperhill1/SCCompanion/releases).
2. "Extract all" wherever you like (Desktop, `C:\Games`…). Avoid `Program Files`: the app keeps your data next to it, in `data`.
3. Run `SCCompanion.exe`, then click **UPDATE** at the top right. Do it again after every game patch.

Windows may show a SmartScreen warning the first time (the exe is not signed yet): "More info" → "Run anyway".

The full guide is inside the app: **?** button or **F1**. The app starts in English; switch to French with the **⚙** button.

## Updating

**New version of the app** — the app tells you when one is out.
1. Download the new `SCCompanion-vX.XX.zip` from [Releases](https://github.com/viperhill1/SCCompanion/releases) and "Extract all" to a temporary folder.
2. Close SC Companion.
3. Copy all the extracted files into your current SCCompanion folder and choose "Replace the files in the destination".
4. Run `SCCompanion.exe`: the new version shows at the top left.
5. Click **UPDATE** once.

⚠️ Never delete the `data` folder: it holds your inventory, queue, blueprints, hangar and settings (language, folders). The zip doesn't contain one, so it is never overwritten. To be safe, you can copy it first.

**After a game patch**: no new app version needed. Click **UPDATE** (or F5).

## More

- [Overview](DEVELOPMENT.md): features, requirements, version history ([version française](DEVELOPPEMENT.md)).
- [Licence](LICENCE.txt): free for personal use, all rights reserved.
- Support the project: ☕ https://buymeacoffee.com/viperhill

Credits: [UEX](https://uexcorp.space) community prices, refinery bonuses from scmdb.net, Aaron Halo bands by CaptSheppard, mission titles from MrKraken's StarStrings mod, power management logic from erkul.games.

---

## Français

**Une app compagnon pour Star Citizen, centrée sur le crafting.** Windows · gratuit · codée et entretenue par **Viperhill**.

👉 **[Télécharger la dernière version](https://github.com/viperhill1/SCCompanion/releases/latest)**

### Ce qu'elle fait

- **Crafting** : choisis un plan, règle les qualités visées, et l'app te dit exactement quels matériaux il te manque, et quelles missions donnent ce plan.
- **File de craft** : tes projets groupés par vaisseau ou par setup, avec la liste de courses des matériaux.
- **Route de minage** : le moins d'arrêts possible pour aller chercher ce qui manque, avec la vraie chance de trouver chaque minerai et les signatures à repérer au scanner.
- **Vaisseaux** : un configurateur (composants, qualités de fabrication, gestion d'énergie par pastilles, DPS soutenu, bouclier, signatures, portée quantique). Envoie ton setup au crafting en un clic.
- **Panier** : ce qui ne se fabrique pas, et où l'acheter (prix de la communauté UEX, itinéraire, total).
- **Inventaire et raffinage** : à la main ou par captures d'écran du gestionnaire de fret et de la raffinerie. Tes plans possédés sont cochés tout seuls pendant que tu joues.
- **Scanner de signatures en direct** : l'app lit le nombre affiché par le scanner de minage et te dit quelle roche c'est (« 7 080 → 2 × Beryl »), dans l'app ou dans un overlay au-dessus du jeu. Les roches qui contiennent un minerai dont tu as besoin sont marquées ★. Jeu en mode Borderless.

Les données sont lues **directement dans les fichiers du jeu installé** : l'app est à jour à chaque patch, et elle ne modifie jamais le jeu.

### Installer

1. Télécharge `SCCompanion-vX.XX.zip` dans les [Releases](https://github.com/viperhill1/SCCompanion/releases).
2. Fais « Extraire tout » où tu veux (Bureau, `C:\Jeux`…). Évite `Program Files` : l'app garde tes données à côté d'elle, dans `data`.
3. Lance `SCCompanion.exe`, puis clique **UPDATE** (**METTRE À JOUR** en français) en haut à droite. Refais-le après chaque patch du jeu.

Windows peut afficher un avertissement SmartScreen au premier lancement (l'exe n'est pas encore signé) : « Informations complémentaires » → « Exécuter quand même ».

L'app démarre en anglais : passe-la en français avec le bouton **⚙**. Le guide complet est dans l'app : bouton **?** ou touche **F1**.

### Mettre à jour

**Nouvelle version de l'app** — l'app t'avertit quand il y en a une.
1. Télécharge le nouveau `SCCompanion-vX.XX.zip` dans les [Releases](https://github.com/viperhill1/SCCompanion/releases) et fais « Extraire tout » dans un dossier temporaire.
2. Ferme SC Companion.
3. Copie tous les fichiers extraits dans ton dossier SCCompanion actuel et choisis « Remplacer les fichiers dans la destination ».
4. Lance `SCCompanion.exe` : la nouvelle version s'affiche en haut à gauche.
5. Clique **METTRE À JOUR** une fois.

⚠️ Ne supprime jamais le dossier `data` : il contient ton inventaire, ta file, tes plans, ton hangar et tes réglages (langue, dossiers). Le zip n'en contient pas, il n'est donc jamais écrasé. Par prudence, tu peux en faire une copie avant.

**Après un patch du jeu** : pas besoin de nouvelle version de l'app. Clique **METTRE À JOUR** (ou F5).

### Plus d'infos

- [Présentation](DEVELOPPEMENT.md) ([English](DEVELOPMENT.md)) : fonctionnalités, configuration requise, historique des versions.
- [Licence](LICENCE.txt) : gratuit pour un usage personnel, tous droits réservés.
- Soutenir le projet : ☕ https://buymeacoffee.com/viperhill

---

*Star Citizen is a trademark of Cloud Imperium Rights LLC. SC Companion is an unofficial fan tool, not affiliated with Cloud Imperium Games. · Star Citizen est une marque de Cloud Imperium Rights LLC. SC Companion est un outil de fan non officiel, sans lien avec Cloud Imperium Games.*
