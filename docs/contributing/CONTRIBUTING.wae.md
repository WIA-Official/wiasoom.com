<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Beitragen zu WIA SOOM</h1>
<p align="center"><strong>Wir freuen uns über deine Beiträge!</strong></p>
<p align="center">Ob es sich um einen Bugfix, eine neue Funktion, ein Plugin oder eine Übersetzung handelt — jeder Beitrag zählt.</p>

---

## Inhaltsverzeichnis

- [Verhaltenskodex](#code-of-conduct)
- [Wie man Bugs meldet](#-how-to-report-bugs)
- [Wie man Funktionen vorschlägt](#-how-to-suggest-features)
- [Wie man ein Plugin einreicht](#-how-to-submit-a-plugin)
- [Wie man einen Pull Request einreicht](#-how-to-submit-a-pull-request)
- [Übersetzungsbeiträge (254 Sprachen)](#-translation-contributions-254-languages)
- [Entwicklungssetup](#-development-setup)

---

## Verhaltenskodex

Wir setzen uns dafür ein, allen eine einladende und inklusive Erfahrung zu bieten.

- **Sei respektvoll.** Behandle alle mit Würde.
- **Sei konstruktiv.** Biete hilfreiches Feedback, keine destruktive Kritik.
- **Sei inklusiv.** Wir unterstützen 254 Sprachen und heißen Mitwirkende aus jedem Land der Erde willkommen.
- **Keine Belästigung.** Null Toleranz für Diskriminierung jeglicher Art.

---

## 🐛 Wie man Bugs meldet

1. Gehe zu [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klicke auf **"New Issue"**
3. Wähle die Vorlage **"Bug Report"**
4. Füge hinzu:
   - WIA SOOM Version (Einstellungen → Über)
   - Betriebssystem und Version (Windows/macOS/Linux)
   - Schritte zur Reproduktion
   - Erwartetes vs. tatsächliches Verhalten
   - Screenshots oder Terminalausgaben, wenn möglich

---

## 💡 Wie man Funktionen vorschlägt

1. Gehe zu [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Klicke auf **"New Issue"**
3. Wähle die Vorlage **"Feature Request"**
4. Beschreibe:
   - Welches Problem du löst
   - Wie du dir die Funktionsweise vorstellst
   - Alle Alternativen, die du in Betracht gezogen hast

---

## 🔌 Wie man ein Plugin einreicht

WIA SOOM hat ein leistungsstarkes Pluginsystem — du kannst dein eigenes Plugin in 5 Minuten erstellen.

### Schnellstart
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Vollständiger Leitfaden

Lies die **[Plugin-Entwickleranleitung](docs/PLUGIN_DEVELOPER_GUIDE.md)** für:
- Vollständige API-Referenz
- Arbeitsbeispiele
- Schritt-für-Schritt-Tutorials
- Best Practices und Sicherheitsregeln

### Reiche dein Plugin ein

1. Forke [Plugin Store](https://wiasoom.com)
2. Füge dein Plugin zu `plugins/{your-plugin-name}/` hinzu
3. Reiche einen Pull Request ein
4. Nach der Überprüfung erscheint dein Plugin im Plugin Store für alle Benutzer!

---

## 🔀 Wie man einen Pull Request einreicht

### Für die Hauptanwendung (wia-soom)

1. Forke das Repository
2. Erstelle einen Feature-Branch: `git checkout -b feat/my-feature`
3. Nimm deine Änderungen vor
4. Teste lokal:
   ```bash
   ```
5. Committe mit einer klaren Nachricht:
   ```
   feat: füge Umschalter für den Dunkelmodus zu den Einstellungen hinzu
   ```
6. Push und öffne einen PR gegen `main`

### Commit-Nachricht-Konvention

| Prefix | Verwendung für |
|--------|----------------|
| `feat:` | Neue Funktion |
| `fix:` | Bugfix |
| `docs:` | Nur Dokumentation |
| `refactor:` | Code-Umstrukturierung (keine Verhaltensänderung) |
| `i18n:` | Übersetzungsupdates |
| `plugin:` | Plugin-bezogene Änderungen |

### PR-Checkliste

- [ ] Code läuft fehlerfrei
- [ ] Keine hardcodierten Strings (verwende i18n-Keys)
- [ ] Keine `console.log` in Produktionscode
- [ ] Bestehende Tests bestehen weiterhin

---

## 🌐 Übersetzungsbeiträge (254 Sprachen)

WIA SOOM unterstützt **254 Sprachen** — von Amharisch bis Zulu, einschließlich Braille und RTL-Sprachen.

### Wie Übersetzungen funktionieren

- Basis-Sprachdatei: `src/renderer/src/i18n/en.json`
- Alle 254 Sprachdateien befinden sich im selben Verzeichnis
- Übersetzungen erfolgen über `scripts/translate-patch.js` (GPT-4o-mini API)

### Wie man Übersetzungen beiträgt

#### Option 1: Eine spezifische Übersetzung korrigieren

1. Finde die Sprachdatei: `src/renderer/src/i18n/{lang-code}.json`
2. Korrigiere die falsche Übersetzung
3. Reiche einen PR mit der Änderung ein

#### Option 2: Fehlende Keys hinzufügen
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Option 3: Maschinenübersetzungen überprüfen

Viele unserer 254 Sprachen wurden maschinell übersetzt. Bewertungen von Muttersprachlern sind unglaublich wertvoll!

1. Wähle deine Sprachdatei aus
2. Überprüfe die Übersetzungen
3. Korrigiere alle ungeschickten oder falschen Übersetzungen
4. Reiche einen PR ein

### Sprachcodes

Wir verwenden standardisierte ISO 639-1-Codes (z. B. `ko`, `en`, `ja`, `ar`, `hi`) mit regionalen Varianten, wo nötig (z. B. `zh-CN`, `pt-BR`).

---

## 🛠 Entwicklungssetup

### Voraussetzungen

- Node.js 18+
- npm 9+
- Git

### Setup
```bash
```
### Build
```bash
```
> Hinweis: Der Standard-Heap von 2 GB reicht nicht aus aufgrund der 254 Sprachdateien + Monaco-Editor-Bundle (~38 MB Renderer).

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

## 🙏 Merci

Jede Beitrag macht WIA SOOM besser für Entwickler auf der ganzen Welt.

Ob du einen Tippfehler korrigierst, einen String übersetzt, ein Plugin baust oder ein großes Feature hinzufügst — **du bist Teil dieser Geschichte.**

---

<p align="center"><em>Gebaut mit ❤️ von SmileStory Inc. und Mitwirkenden weltweit.</em></p>