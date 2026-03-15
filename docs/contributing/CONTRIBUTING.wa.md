<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Contribuer a WIA SOOM</h1>
<p align="center"><strong>Nos contributions sont les bienvenues !</strong></p>
<p align="center">Que ce soit un correctif, une nouvelle fonctionnalité, un plugin ou une traduction — chaque contribution compte.</p>

---

## Table des Matières

- [Code de Conduite](#code-de-conduite)
- [Comment Signaler des Bugs](#-comment-signaler-des-bugs)
- [Comment Suggérer des Fonctionnalités](#-comment-suggérer-des-fonctionnalités)
- [Comment Soumettre un Plugin](#-comment-soumettre-un-plugin)
- [Comment Soumettre une Pull Request](#-comment-soumettre-une-pull-request)
- [Contributions de Traduction (254 Langues)](#-contributions-de-traduction-254-langues)
- [Configuration de Développement](#-configuration-de-développement)

---

## Code de Conduite

Nous nous engageons à offrir une expérience accueillante et inclusive pour tout le monde.

- **Soyez respectueux.** Traitez tout le monde avec dignité.
- **Soyez constructif.** Offrez des retours utiles, pas de critiques destructrices.
- **Soyez inclusif.** Nous soutenons 254 langues et accueillons des contributeurs de tous les pays de la Terre.
- **Pas de harcèlement.** Tolérance zéro pour toute forme de discrimination.

---

## 🐛 Comment Signaler des Bugs

1. Allez sur [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliquez sur **"Nouvelle Problématique"**
3. Choisissez le modèle **"Rapport de Bug"**
4. Incluez :
   - Version de WIA SOOM (Paramètres → À propos)
   - OS et version (Windows/macOS/Linux)
   - Étapes pour reproduire
   - Comportement attendu vs. réel
   - Captures d'écran ou sortie terminal si possible

---

## 💡 Comment Suggérer des Fonctionnalités

1. Allez sur [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Cliquez sur **"Nouvelle Problématique"**
3. Choisissez le modèle **"Demande de Fonctionnalité"**
4. Décrivez :
   - Quel problème vous résolvez
   - Comment vous l'imaginez fonctionner
   - Toutes les alternatives que vous avez envisagées

---

## 🔌 Comment Soumettre un Plugin

WIA SOOM a un système de plugin puissant — vous pouvez créer votre propre plugin en 5 minutes.

### Démarrage Rapide
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Guide Complet

Lisez le **[Guide du Développeur de Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** pour :
- Référence API complète
- Exemples fonctionnels
- Tutoriels étape par étape
- Meilleures pratiques et règles de sécurité

### Soumettez Votre Plugin

1. Forkez [Plugin Store](https://wiasoom.com)
2. Ajoutez votre plugin dans `plugins/{nom-de-votre-plugin}/`
3. Soumettez une Pull Request
4. Après révision, votre plugin apparaîtra dans le Plugin Store pour tous les utilisateurs !

---

## 🔀 Comment Soumettre une Pull Request

### Pour l'application principale (wia-soom)

1. Forkez le dépôt
2. Créez une branche de fonctionnalité : `git checkout -b feat/ma-fonctionnalité`
3. Apportez vos modifications
4. Testez localement :
   ```bash
   ```
5. Commitez avec un message clair :
   ```
   feat: ajouter un basculement du mode sombre dans les paramètres
   ```
6. Poussez et ouvrez une PR contre `main`

### Convention de Message de Commit

| Préfixe | Utilisé pour |
|---------|--------------|
| `feat:` | Nouvelle fonctionnalité |
| `fix:`  | Correctif de bug |
| `docs:` | Documentation uniquement |
| `refactor:` | Restructuration de code (sans changement de comportement) |
| `i18n:` | Mises à jour de traduction |
| `plugin:` | Changements liés aux plugins |

### Checklist de PR

- [ ] Le code s'exécute sans erreurs
- [ ] Pas de chaînes codées en dur (utilisez des clés i18n)
- [ ] Pas de `console.log` laissé dans le code de production
- [ ] Les tests existants passent toujours

---

## 🌐 Contributions de Traduction (254 Langues)

WIA SOOM prend en charge **254 langues** — de l'amharique au zoulou, y compris le braille et les langues RTL.

### Comment Fonctionne la Traduction

- Fichier de langue de base : `src/renderer/src/i18n/en.json`
- Tous les fichiers de langue de 254 sont dans le même répertoire
- La traduction se fait via `scripts/translate-patch.js` (API GPT-4o-mini)

### Comment Contribuer aux Traductions

#### Option 1 : Corriger une traduction spécifique

1. Trouvez le fichier de langue : `src/renderer/src/i18n/{code-langue}.json`
2. Corrigez la traduction incorrecte
3. Soumettez une PR avec le changement

#### Option 2 : Ajouter des clés manquantes
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3 : Réviser les traductions automatiques

Beaucoup de nos 254 langues ont été traduites par machine. Les révisions par des locuteurs natifs sont incroyablement précieuses !

1. Choisissez votre fichier de langue
2. Révisez les traductions
3. Corrigez les traductions maladroites ou incorrectes
4. Soumettez une PR

### Codes de Langue

Nous utilisons des codes standard ISO 639-1 (par exemple, `ko`, `en`, `ja`, `ar`, `hi`) avec des variantes régionales si nécessaire (par exemple, `zh-CN`, `pt-BR`).

---

## 🛠 Configuration de Développement

### Prérequis

- Node.js 18+
- npm 9+
- Git

### Configuration
```bash
```
### Construction
```bash
```
> Remarque : Le tas par défaut de 2 Go n'est pas suffisant en raison des 254 fichiers de langue + le bundle de l'éditeur Monaco (~38 Mo de rendu).

### Structure du Projet
```
wia-soom/
├── src/
│   ├── main/          # Electron main process
│   ├── renderer/      # React frontend
│   └── preload/       # Preload scripts
├── docs/              # Documentation
├── scripts/           # Build & automation scripts
└── prompts/           # AI prompt engineering
```
---

## 🙏 Merci

Chacune des contributions rend WIA SOOM meilleur pour les développeurs du monde entier.

Que vous corrigiez une faute de frappe, traduisiez une chaîne, construisiez un plugin ou ajoutiez une fonctionnalité majeure — **vous faites partie de cette histoire.**

---

<p align="center"><em>Construit avec ❤️ par SmileStory Inc. et des contributeurs du monde entier.</em></p>