<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Beitragen zu WIA SOOM</h1>
<p align="center"><strong>Wir freuen uns über Ihre Beiträge!</strong></p>
<p align="center">Ob es sich um einen Bugfix, ein neues Feature, ein Plugin oder eine Übersetzung handelt – jeder Beitrag zählt.</p>

---

## Inhaltsverzeichnis

- [Verhaltenskodex](#code-of-conduct)
- [Wie man Bugs meldet](#-how-to-report-bugs)
- [Wie man Features vorschlägt](#-how-to-suggest-features)
- [Wie man ein Plugin einreicht](#-how-to-submit-a-plugin)
- [Wie man einen Pull Request einreicht](#-how-to-submit-a-pull-request)
- [Übersetzungsbeiträge (254 Sprachen)](#-translation-contributions-254-languages)
- [Entwicklungssetup](#-development-setup)

---

## Verhaltenskodex

Wir verpflichten uns, allen eine einladende und inklusive Erfahrung zu bieten.

- **Seien Sie respektvoll.** Behandeln Sie jeden mit Würde.
- **Seien Sie konstruktiv.** Geben Sie hilfreiches Feedback, keine destruktive Kritik.
- **Seien Sie inklusiv.** Wir unterstützen 254 Sprachen und heißen Mitwirkende aus jedem Land der Erde willkommen.
- **Keine Belästigung.** Null Toleranz für Diskriminierung jeglicher Art.

---

## 🐛 Wie man Bugs meldet

1. Gehen Sie zu [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klicken Sie auf **"New Issue"**
3. Wählen Sie die Vorlage **"Bug Report"**
4. Fügen Sie Folgendes hinzu:
   - WIA SOOM Version (Einstellungen → Über)
   - Betriebssystem und Version (Windows/macOS/Linux)
   - Schritte zur Reproduktion
   - Erwartetes vs. tatsächliches Verhalten
   - Screenshots oder Terminalausgaben, wenn möglich

---

## 💡 Wie man Features vorschlägt

1. Gehen Sie zu [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klicken Sie auf **"New Issue"**
3. Wählen Sie die Vorlage **"Feature Request"**
4. Beschreiben Sie:
   - Welches Problem Sie lösen
   - Wie Sie sich die Funktionsweise vorstellen
   - Alle Alternativen, die Sie in Betracht gezogen haben

---

## 🔌 Wie man ein Plugin einreicht

WIA SOOM hat ein leistungsstarkes Pluginsystem – Sie können Ihr eigenes Plugin in 5 Minuten erstellen.

### Schnellstart
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Vollständige Anleitung

Lesen Sie die **[Plugin-Entwickleranleitung](docs/PLUGIN_DEVELOPER_GUIDE.md)** für:
- Vollständige API-Referenz
- Funktionierende Beispiele
- Schritt-für-Schritt-Tutorials
- Best Practices und Sicherheitsregeln

### Reichen Sie Ihr Plugin ein

1. Forken Sie [Plugin Store](https://wiasoom.com)
2. Fügen Sie Ihr Plugin zu `plugins/{your-plugin-name}/` hinzu
3. Reichen Sie einen Pull Request ein
4. Nach der Überprüfung erscheint Ihr Plugin im Plugin Store für alle Benutzer!

---

## 🔀 Wie man einen Pull Request einreicht

### Für die Hauptanwendung (wia-soom)

1. Forken Sie das Repository
2. Erstellen Sie einen Feature-Branch: `git checkout -b feat/my-feature`
3. Nehmen Sie Ihre Änderungen vor
4. Testen Sie lokal:
   ```bash
   ```
5. Committen Sie mit einer klaren Nachricht:
   ```
   feat: dunklen Modus zu den Einstellungen hinzufügen
   ```
6. Pushen Sie und öffnen Sie einen PR gegen `main`

### Commit-Nachricht-Konvention

| Präfix | Verwendung für |
|--------|----------------|
| `feat:` | Neues Feature |
| `fix:` | Bugfix |
| `docs:` | Nur Dokumentation |
| `refactor:` | Code-Umstrukturierung (keine Verhaltensänderung) |
| `i18n:` | Übersetzungsupdates |
| `plugin:` | Plugin-bezogene Änderungen |

### PR-Checkliste

- [ ] Code läuft ohne Fehler
- [ ] Keine hardcodierten Strings (verwenden Sie i18n-Schlüssel)
- [ ] Keine `console.log` in Produktionscode zurückgelassen
- [ ] Bestehende Tests bestehen weiterhin

---

## 🌐 Übersetzungsbeiträge (254 Sprachen)

WIA SOOM unterstützt **254 Sprachen** – von Amharisch bis Zulu, einschließlich Braille und RTL-Sprachen.

### Wie Übersetzungen funktionieren

- Basis-Sprachdatei: `src/renderer/src/i18n/en.json`
- Alle 254 Sprachdateien befinden sich im selben Verzeichnis
- Übersetzungen erfolgen über `scripts/translate-patch.js` (GPT-4o-mini API)

### Wie man Übersetzungen beiträgt

#### Option 1: Eine spezifische Übersetzung korrigieren

1. Finden Sie die Sprachdatei: `src/renderer/src/i18n/{lang-code}.json`
2. Korrigieren Sie die falsche Übersetzung
3. Reichen Sie einen PR mit der Änderung ein

#### Option 2: Fehlende Schlüssel hinzufügen
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Maschinenübersetzungen überprüfen

Viele unserer 254 Sprachen wurden maschinell übersetzt. Bewertungen von Muttersprachlern sind unglaublich wertvoll!

1. Wählen Sie Ihre Sprachdatei aus
2. Überprüfen Sie die Übersetzungen
3. Korrigieren Sie alle ungeschickten oder falschen Übersetzungen
4. Reichen Sie einen PR ein

### Sprachcodes

Wir verwenden standardisierte ISO 639-1-Codes (z. B. `ko`, `en`, `ja`, `ar`, `hi`) mit regionalen Varianten, wo nötig (z. B. `zh-CN`, `pt-BR`).

---

## 🛠 Entwicklungssetup

### Voraussetzungen

- Node.js 18+
- npm 9+
- Git

### Einrichtung
```bash
```
### Build
```bash
```
> Hinweis: Der Standard-2GB-Heap reicht aufgrund der 254 Sprachdateien + Monaco-Editor-Bundle (~38MB Renderer) nicht aus.

### Projektstruktur
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

## 🙏 Danke

Jeder Beitrag macht WIA SOOM besser für Entwickler auf der ganzen Welt.

Egal, ob du einen Tippfehler korrigierst, einen String übersetzt, ein Plugin baust oder ein großes Feature hinzufügst — **du bist Teil dieser Geschichte.**

---

<p align="center"><em>Gebaut mit ❤️ von SmileStory Inc. und Mitwirkenden weltweit.</em></p>