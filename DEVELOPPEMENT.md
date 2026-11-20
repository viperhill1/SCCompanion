# SC Companion — rapport de développement

> **Codé et entretenu par Viperhill** · version **0.56** · Windows · français / English
> Soutenir le projet : https://buymeacoffee.com/viperhill

**English summary** — SC Companion is a free Windows companion app for Star Citizen, focused on crafting. It reads the game's own files (Data.p4k) to know every blueprint, material, mining location and ship component, then tells you what you are missing, where to mine it, where to buy what can't be crafted, and how to fit your ships (erkul-style configurator with interactive power management). The interface is available in French and English. This document describes the features, data sources, calculation rules, architecture and version history.

---

## 1. Présentation

SC Companion aide à **fabriquer (crafting) dans Star Citizen**. Tout part d'une question simple : *qu'est-ce que je veux fabriquer, et qu'est-ce qu'il me manque ?* L'app répond avec la liste des matériaux, la route de minage la plus courte, les boutiques où acheter ce qui ne se fabrique pas, et un configurateur de vaisseau qui envoie un setup complet au crafting.

Principes du projet :
- **Les données viennent du jeu installé.** L'app lit `Data.p4k` (base DataCore `Game2.dcb` et textes `global.ini`). Elle est donc à jour à chaque patch, sans dépendre d'un site web. scmdb.net reste un secours si le jeu n'est pas sur le PC.
- **Le jeu n'est jamais modifié.** L'app lit seulement : les fichiers du jeu, le `Game.log`, les captures d'écran.
- **Aucun contournement du chiffrement de CIG.** Les définitions de coque chiffrées ne sont pas lues (voir *Limites*).
- **Le crafting d'abord.** Les autres outils (minage, lieux, raffineries, vaisseaux) sont là pour le servir.

## 2. Fonctionnalités

### Crafting
- Navigation par tuiles (ex. Vaisseau › Boucliers › Militaire › Grade A) ou recherche par nom, en anglais ou en français.
- Fiche de chaque plan : matériaux, quantités, qualité minimale, stock dans l'inventaire, statut (prêt / partiel / manque).
- Légende sous la liste : colonne BP (✓ possédé, **M** obtenable par mission, · aucune source connue) et colonne Stock (prêt, partiel, manque).
- **Simulateur de qualité** : un curseur par matériau, préréglages MIN / 500 / 750 / 900 / MAX, saisie au clavier, effet calculé sur l'objet (dégâts, PV, vitesse quantique, consommation…). Les qualités choisies partent avec le plan dans la file.
- **Obtenir le plan** : missions qui donnent ce plan, avec la chance, le rang demandé et le nombre de plans tirés.
- « J'ai ce plan » : suivi des plans possédés, à la main ou automatiquement (voir *Synchroniser mes plans*).

### File de craft
- Groupée **par setup** (« Paladin Viper », « Paladin #1 »…) ; les ajouts manuels vont dans « Divers ».
- Chaque setup se coche ou se décoche pour la route de minage, se marque « Fabriqué » (retire ses matériaux de l'inventaire) ou se retire.
- Liste de courses : matériaux requis, qualité minimale, alloué depuis le stock, manquant, quantité brute à miner selon le rendement de raffinage.

### Panier
- Tout ce qui ne se fabrique pas (pas de plan dans le jeu, ou « Acheter » choisi) va dans le Panier, pas dans la file.
- Articles groupés par setup, avec quantités.
- **Où acheter** : prix de la communauté UEX, boutiques choisies pour faire le moins d'arrêts possible, itinéraire et total en aUEC. Menus déroulants pour changer la boutique d'un groupe ou d'un article. Vue « tout ensemble » ou « groupé par setup ».

### Route de minage
- Le plus petit nombre de lieux pour miner ce qui manque, par méthode (vaisseau, ROC, à pied), avec les signatures à chercher au scanner.
- Indication « Aller à » pour les lieux sans marqueur évident (ex. ceintures de Pyro = points de Lagrange PYR1 à PYR6).

### Inventaire
- Saisie à la main ou **par captures d'écran** du gestionnaire de fret et de la raffinerie (OCR intégré à Windows, sans logiciel externe).
- Deux vues pleine hauteur : « Mon inventaire » et « Mes raffinages ». Les commandes prêtes sont signalées en vert, et l'app ouvre « Mes raffinages » dès qu'une capture de raffinerie est lue.

### Vaisseaux (configurateur)
- 253 vaisseaux et véhicules, 3 386 composants, lus dans les fichiers du jeu.
- Remplacement de composants compatibles (type et taille), supports d'arme et lance-missiles, filtres par classe, grade et type d'arme, colonnes EM, PV, énergie minimale.
- Qualité de fabrication par composant, avec effet sur les stats.
- **Tableau de bord** : dégâts (salve, rafale, soutenu, par source), défense (bouclier, résistances, blindage, déflexion, signatures), vol et quantique (vitesses, rotations, portée, recharge du boost).
- **Gestion de l'énergie interactive** (comme erkul.games) : une colonne de pastilles par composant, boucliers en une seule colonne, minimum d'allumage en bloc indivisible, modes SCM / NAV, répartition automatique. Effets en direct sur le DPS soutenu, la régénération du bouclier, le refroidissement, la recharge du boost et les signatures EM / IR.
- Identiques regroupés (« 4 × CF-557 »), à détailler au besoin.
- **Mon hangar** : configurations enregistrées, synchronisées entre PC, envoi au crafting en un clic.

### Minerais, Lieux, Raffineries
- Fiche de chaque minerai : propriétés de minage, lieux, **chance réelle** de le trouver, roches qui le contiennent (minerai principal ou secondaire, avec la teneur).
- Guide de chaque lieu : part de chaque méthode de minage, types de roches, signatures selon la taille de l'amas, bandes de l'Aaron Halo par station de départ.
- Raffineries : bonus de rendement par station et par minerai.

### Autres
- **Synchroniser mes plans** : lecture du `Game.log` (et des sauvegardes de logs) pour cocher les plans reçus, en direct pendant que tu joues si tu veux. Fonctionne aussi avec le jeu en français (noms traduits ramenés au nom anglais du plan). Un bouton « Copier le diagnostic » résume ce que l'app a lu, pour le support.
- **Synchro ☁** entre ordinateurs via un dossier partagé (OneDrive…) : inventaire, file, plans, hangar.
- Interface en **français ou en anglais** ; noms du jeu en anglais ou en français officiel ; titres de mission enrichis si le mod StarStrings est installé.
- Guide d'utilisation intégré (bouton ? ou F1).

## 3. Sources de données

| Donnée | Source |
|---|---|
| Plans, matériaux, qualités, effets | Fichiers du jeu (`Game2.dcb`) |
| Minerais, compositions des roches, lieux, poids des groupes de gisements | Fichiers du jeu |
| Missions qui donnent des plans | Fichiers du jeu (+ titres du mod StarStrings de MrKraken s'il est installé) |
| Vaisseaux, équipement d'origine, composants, réserves d'énergie | Fichiers du jeu |
| Noms français officiels | `global.ini` français du jeu |
| Prix et boutiques | Communauté UEX (uexcorp.space), API publique |
| Bonus des raffineries par station | Relevé de scmdb.net |
| Position des bandes de l'Aaron Halo | PDF « Refinery to Aaron Halo Mining Routes » de CaptSheppard |
| Secours si le jeu n'est pas installé | scmdb.net |

## 4. Règles de calcul

Les règles ci-dessous sont appliquées aux valeurs lues dans le jeu. Celles d'énergie suivent la logique publiée par erkul.games et ont été vérifiées contre erkul et spviewer.

**Énergie**
- Minimum d'un composant = arrondi(pastilles × fraction minimale du jeu). Le minimum se prend en bloc : le composant est éteint ou au moins à son minimum.
- Effet d'un composant = (pastilles actives ÷ pastilles totales) × multiplicateur du palier de puissance (bas / moyen / haut).
- **Plusieurs générateurs** : chacun donne arrondi((segments + bonus de fabrication) ÷ n) + (n − 1) × bonus de sa taille. Le bonus de taille vient de la table du jeu (S0 à S4 = 0 à 4). Vérifié : Paladin, 3 × JS-400 = 33 pastilles ; Q0 → 10, Q706 → 12, Q1000 → 12 par générateur.
- Réserve des armes : ratio = sélection ÷ points utiles. Arme à énergie : charge et recharge proportionnelles au ratio ; DPS soutenu = DPS × tir ÷ (tir + délai + recharge).
- Refroidissement : demande = pastilles utilisées + Σ(pastilles × palier) des boucliers, radar, support de vie et quantique ; % = demande ÷ production des refroidisseurs.
- Boost : recharge = capacitorRegenPerSec × courbe de Bézier du jeu ; temps = capacitorMax ÷ recharge. Vérifié : Paladin 2/6 → 44,5 s, 6/6 → 26,7 s.
- Signatures : EM des générateurs, armes et composants selon leur puissance ; IR des refroidisseurs selon le refroidissement ; × modificateurs du blindage.

**Quantique**
- Portée (Gm) = réservoir quantique (SCU) × 1000 ÷ consommation en voyage (mSCU/Gm, état « Travelling » du moteur). Vérifié : Stalker 120 Gm, Cutlass 209 Gm.

**Minage**
- Part d'une méthode à un lieu = poids du groupe ÷ somme des groupes du lieu (plantes et épaves incluses). Ex. Wala : vaisseau 13,8 %, ROC 28,7 %, à pied 57,5 %.
- Chance réelle d'un minerai = probabilité dans sa méthode × part de la méthode. Ex. Beryl à Wala : 20 % × 13,8 % ≈ 2,8 %.

**Qualité de fabrication**
- Chaque matériau de la recette a des segments d'effet (multiplicatifs ou additifs) selon sa qualité de 0 à 1000 ; 500 = valeurs de base du jeu.

## 5. Architecture technique

- **Moteur** : LÖVE 11.5 (Lua / LuaJIT), une seule fenêtre, interface dessinée sur mesure. Environ 11 500 lignes de Lua.
- **Lecture du jeu** : lecteur `Data.p4k` (zip64) et décompression Zstandard par FFI (`libzstd.dll`), lecteur DataCore `Game2.dcb`, accès fichier Windows au-delà de 2 Go par l'API Win32. Extraction en environ 10 secondes, dans un thread séparé (l'interface reste fluide).
- **Données** : converties en tables Lua dans `data\` (minage, crafting, missions, vaisseaux, boutiques, noms français). Ces fichiers sont lus dans un bac à sable sans accès au système ; le code compilé y est refusé.
- **Sans fenêtres noires** : les téléchargements et les dossiers passent par l'API Windows (FFI), pas par des commandes console.
- **Traduction** : l'interface est écrite en français ; `lang/en.lua` contient les traductions exactes et les modèles pour les textes construits.

```
app.lua                 démarrage, onglets, en-tête, mise à jour
lib/                    interface (ui), traduction (i18n), JSON, sérialisation
src/gd/                 lecture du jeu : p4k, dcb, zstd, extraction (minage, crafting, missions), vaisseaux
src/screens/            un fichier par onglet (crafting, file, panier, route, inventaire, vaisseaux…)
src/power.lua           gestion de l'énergie (pastilles, générateurs, refroidissement, boost, signatures)
src/route.lua           calcul de la route de minage
src/uex.lua             prix des boutiques (UEX)
src/ocr.lua             import par captures d'écran
src/gamelog.lua         lecture du Game.log (plans reçus)
src/cloudsync.lua       synchronisation entre ordinateurs
src/publish.lua         préparation de la version publique (développement seulement)
lang/en.lua             traduction anglaise
```

## 6. Version publique

- Le code est compilé en bytecode LuaJIT (sans informations de débogage) et intégré à `SCCompanion.exe`. Il n'y a pas de dossier de code à côté de l'exe.
- La version publique ne charge jamais de code externe (dossier `app` posé à côté, dossier utilisateur %APPDATA%).
- Fichiers livrés : `SCCompanion.exe`, les DLL nécessaires (LÖVE, LuaJIT, SDL, OpenAL, zstd), `README.txt` (anglais puis français), `LICENCE.txt`, `LICENCES-TIERS.txt`. Le dossier `data` est créé au premier lancement et contient les données de l'utilisateur.
- **Licence** : tous droits réservés (Viperhill). Usage personnel gratuit ; le partage de l'archive officielle non modifiée est permis ; modification, décompilation et redistribution modifiée interdites sans accord écrit. Les composants libres gardent leurs licences.

## 7. Limites connues

- Les définitions de coque des vaisseaux sont chiffrées par CIG et ne sont pas lues : pas de masse, de dimensions, de PV par partie de coque ni d'accélérations.
- Le DPS soutenu ne tient pas compte de la chauffe des armes balistiques.
- La répartition automatique de l'énergie est une version simplifiée de celle d'erkul.
- La chance réelle compte encore une roche où le minerai est secondaire (5-10 %) comme une roche complète ; la route de minage utilise encore la probabilité dans la méthode.
- L'emplacement des lieux « Pyro Belt » vient de star-citizen.wiki (données 4.8) ; « Pyro Deep Space Asteroids » est à confirmer.
- Les prix UEX viennent de la communauté et peuvent différer du jeu.

## 8. Historique des versions

| Version | Nouveautés |
|---|---|
| ≤ 0.32 | Base de l'app : crafting par tuiles, file de craft, route de minage, inventaire (à la main et par captures), lieux, raffineries, Aaron Halo, synchro des plans par Game.log, synchro ☁, lecture directe des fichiers du jeu. |
| 0.33 | App bilingue français / anglais, fenêtre ⚙ Réglages, titres de mission du mod StarStrings, bouton de don, crédit Viperhill. |
| 0.34 | Configurateur de vaisseau : extraction des vaisseaux et composants, écran Vaisseaux, configurations, envoi au crafting ; supports d'arme et lance-missiles, synchro des configurations, portée quantique. |
| 0.35 | Qualité de fabrication par composant, Mon hangar, panier avec prix UEX, classification des armes, filtres, cartes du tableau de bord. |
| 0.36 | Gestion de l'énergie interactive ; regroupement des composants identiques. |
| 0.37 – 0.38 | Minimum d'allumage en bloc, boucliers en une colonne ; colonnes Min. énergie / Énergie dans le choix d'un composant. |
| 0.39 | File de craft groupée par setup, panier des setups, bouton « → Craft » dans le hangar. |
| 0.40 – 0.43 | Plusieurs générateurs : malus de taille (table du jeu) et bonus de fabrication, vérifiés avec erkul et spviewer. |
| 0.44 | Préréglages de qualité et saisie au clavier partout. |
| 0.45 – 0.47 | Part de chaque méthode de minage par lieu, chance réelle, roches composées de plusieurs minerais. |
| 0.48 | Les qualités du simulateur partent avec le plan dans la file. |
| 0.49 – 0.51 | Menu déroulant des stations de l'Aaron Halo ; indication « Aller à » pour les lieux de Pyro. |
| 0.52 – 0.53 | Onglet Panier séparé de la file, menus déroulants des boutiques. |
| 0.54 | Icône de l'app (exe, fenêtre et barre des tâches). |
| 0.55 | Version publique verrouillée, licence, lecture des données renforcée. |
| 0.56 | Import des plans avec le jeu en français, messages plus clairs et diagnostic à copier ; légende BP / Stock dans le Crafting ; vue « Mes raffinages » séparée dans l'Inventaire. |

## 9. Feuille de route

- Optimiseur de vaisseau : meilleur setup selon un objectif (DPS soutenu, bouclier, furtivité, portée) et des contraintes (énergie, refroidissement, plans possédés, budget).
- Assistant IA optionnel, avec la clé d'API de chaque joueur, branché sur l'optimiseur.
- Route de minage basée sur la chance réelle, pondérée par la teneur.
- Signature de code de l'exe (plus d'avertissement Windows SmartScreen).

## 10. Crédits et licences

- Code et entretien : **Viperhill**.
- Données de jeu : Star Citizen (Cloud Imperium Games), lues dans l'installation de l'utilisateur.
- Prix : communauté **UEX** (uexcorp.space). Bonus de raffinerie : **scmdb.net**. Bandes de l'Aaron Halo : **CaptSheppard**. Titres de mission : mod **StarStrings** de **MrKraken**. Logique de gestion de l'énergie : **erkul.games**.
- Composants libres : LÖVE (zlib) et ses bibliothèques (LuaJIT, SDL2, OpenAL Soft, mpg123…), Zstandard (BSD), polices Rajdhani (OFL) et DejaVu. Voir `LICENCES-TIERS.txt`.

Star Citizen est une marque de Cloud Imperium Rights LLC. SC Companion est un outil de fan non officiel, sans lien avec Cloud Imperium Games ni Roberts Space Industries.
