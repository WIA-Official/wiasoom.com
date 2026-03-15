<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Συμβολή στο WIA SOOM</h1>
<p align="center"><strong>Θα θέλαμε τις συμβολές σας!</strong></p>
<p align="center">Είτε πρόκειται για διόρθωση σφαλμάτων, νέα λειτουργία, πρόσθετο ή μετάφραση — κάθε συμβολή μετράει.</p>

---

## Πίνακας Περιεχομένων

- [Κώδικας Συμπεριφοράς](#code-of-conduct)
- [Πώς να Αναφέρετε Σφάλματα](#-how-to-report-bugs)
- [Πώς να Προτείνετε Λειτουργίες](#-how-to-suggest-features)
- [Πώς να Υποβάλετε ένα Πρόσθετο](#-how-to-submit-a-plugin)
- [Πώς να Υποβάλετε ένα Pull Request](#-how-to-submit-a-pull-request)
- [Συμβολές Μετάφρασης (254 Γλώσσες)](#-translation-contributions-254-languages)
- [Ρύθμιση Ανάπτυξης](#-development-setup)

---

## Κώδικας Συμπεριφοράς

Δεσμευόμαστε να παρέχουμε μια φιλόξενη και συμπεριληπτική εμπειρία για ��λους.

- **Να είστε σεβαστοί.** Να αντιμετωπίζετε όλους με αξιοπρέπεια.
- **Να είστε εποικοδομητικοί.** Να προσφέρετε χρήσιμη ανατροφοδότηση, όχι καταστροφική κριτική.
- **Να είστε συμπεριληπτικοί.** Υποστηρίζουμε 254 γλώσσες και καλωσορίζουμε τους συνεισφέροντες από κάθε χώρα της Γης.
- **Καμία παρενόχληση.** Μηδενική ανοχή για διακρίσεις κάθε είδους.

---

## 🐛 Πώς να Αναφέρετε Σφάλματα

1. Πηγαίνετε στο [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Κάντε κλικ στο **"New Issue"**
3. Επιλέξτε το πρότυπο **"Bug Report"**
4. Συμπεριλάβετε:
   - Έκδοση WIA SOOM (Ρυθμίσεις → Σχετικά)
   - Λειτουργικό σύστημα και έκδοση (Windows/macOS/Linux)
   - Βήματα για αναπαραγωγή
   - Αναμενόμενη έναντι πραγματικής συμπεριφοράς
   - Στιγμιότυπα οθόνης ή έξοδο τερματικού αν είναι δυ��ατόν

---

## 💡 Πώς να Προτείνετε Λειτουργίες

1. Πηγαίνετε στο [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Κάντε κλικ στο **"New Issue"**
3. Επιλέξτε το πρότυπο **"Feature Request"**
4. Περιγράψτε:
   - Ποιο πρόβλημα επιλύετε
   - Πώς φαντάζεστε ότι θα λειτουργεί
   - Οποιεσδήποτε εναλλακτικές έχετε εξετάσει

---

## 🔌 Πώς να Υποβάλετε ένα Πρόσθετο

Το WIA SOOM διαθέτει ένα ισχυρό σύστημα προσθέτων — μπορείτε να δημιουργήσετε το δικό σας πρόσθετο σε 5 λεπτά.

### Γρήγορη Εκκίνηση
§§§CHUNK_SEPARATOR§§§
### Πλήρης Οδηγός

Διαβάστε τον **[Οδηγό Ανάπτυξης Πρόσθετων](docs/PLUGIN_DEVELOPER_GUIDE.md)** για:
- Πλήρη αναφορά API
- Λειτουργικά παραδείγματα
- Βήμα-βήμα οδηγίες
- Καλές πρακτικές και κανόνες ασφαλείας

### Υποβάλετε το Πρόσθετό σας

1. Fork [Plugin Store](https://wiasoom.com)
2. Προσθέστε το πρόσθετό σας στο `plugins/{your-plugin-name}/`
3. Υποβάλετε ένα Pull Request
4. Μετά την αναθεώρηση, το πρόσθετό σας θα εμφανιστεί στο Plugin Store για όλους τους χρήστες!

---

## 🔀 Πώς να Υποβάλετε ένα Pull Request

### Για την κύρια εφαρμογή (wia-soom)

1. Fork το αποθετήριο
2. Δημιουργήστε ένα feature branch: `git checkout -b feat/my-feature`
3. Κάντε τις αλλαγές σας
4. Δοκιμάστε το τοπικά:
   ```bash
   ```
5. Δεσμεύστε με ένα σαφές μήνυμα:
   ```
   feat: add dark mode toggle to settings
   ```
6. Push και ανοίξτε ένα PR κατά του `main`

### Σύμβαση Μηνυμάτων Δέσμευσης

| Πρόθεμα | Χρήση για |
|--------|---------|
| `feat:` | Νέα λειτουργία |
| `fix:` | Διόρθωση σφάλματος |
| `docs:` | Μόνο τεκμηρίωση |
| `refactor:` | Αναδιάρθρωση κώδικα (χωρίς αλλαγή συμπεριφοράς) |
| `i18n:` | Ενημερώσεις μετάφρασης |
| `plugin:` | Αλλαγές σχετικές με πρόσθετα |

### Λίστα Ελέγχου PR

- [ ] Ο κώδικας εκτελείται χωρίς σφάλματα
- [ ] Καμία σκληρά κωδικοποιημένη συμβολοσειρά (χρησιμοποιήστε κλειδιά i18n)
- [ ] Καμία `console.log` δεν έχει μείνει στον παραγωγικό κώδικα
- [ ] Οι υπάρχουσες δοκιμές εξακολουθούν να περνούν

---

## 🌐 Συμβολές Μετάφρασης (254 Γλώσσες)

Το WIA SOOM υποστηρίζει **254 γλώσσες** — από τα Αμαρικά μέχρι τα Ζουλού, συμπεριλαμβανομένων των Μπράιγ και γλωσσών RTL.

### Πώς Λειτουργεί η Μετάφραση

- Βασικό αρχείο γλώσσας: `src/renderer/src/i18n/en.json`
- Όλα τα 254 αρχεία γλώσσας βρίσκονται στον ίδιο φάκελο
- Η μετάφραση γίνεται μέσω του `scripts/translate-patch.js` (GPT-4o-mini API)

### Πώς να Συμβάλετε σε Μεταφράσεις

#### Επιλογή 1: Διορθώστε μια συγκεκριμένη μετάφραση

1. Βρείτε το αρχείο γλώσσας: `src/renderer/src/i18n/{lang-code}.json`
2. Διορθώστε τη λανθασμένη μετάφραση
3. Υποβάλετε ένα PR με την αλλαγή

#### Επιλογή 2: Προσθέστε ελλείποντα κλειδιά
§§§CHUNK_SEPARATOR§§§
#### Επιλογή 3: Αναθεωρήστε τις μηχανικές μεταφράσεις

Πολλές από τις 254 γλώσσες μας έχουν μεταφραστεί αυτόματα. Οι αναθεωρήσεις από φυσικούς ομιλητές είναι εξαιρετικά πολύτιμες!

1. Επιλέξτε το αρχείο γλώσσας σας
2. Αναθεωρήστε τις μεταφράσεις
3. Διορθώστε οποιεσδήποτε άβολες ή λανθασμένες μεταφράσεις
4. Υποβάλετε ένα PR

### Κωδικοί Γλωσσών

Χρησιμοποιούμε τους τυπικούς κωδικούς ISO 639-1 (π.χ., `ko`, `en`, `ja`, `ar`, `hi`) με περιφερειακές παρ��λλαγές όπου χρειάζεται (π.χ., `zh-CN`, `pt-BR`).

---

## 🛠 Ρύθμιση Ανάπτυξης

### Προαπαιτούμενα

- Node.js 18+
- npm 9+
- Git

### Ρύθμιση
§§§CHUNK_SEPARATOR§§§
### Δημιουργία
§§§CHUNK_SEPARATOR§§§
> Σημείωση: Η προεπιλεγμένη μνήμη 2GB δεν είναι αρκετή λόγω των 254 αρχείων γλώσσας + πακέτου Monaco editor (~38MB renderer).

### Δομή Έργου
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 Ευχαριστώ

Κάθε συνεισφορά κάνει το WIA SOOM καλύτερο για προγραμματιστές σε όλο τον κόσμο.

Είτε διορθώνετε ένα τυπογραφικό λάθος, μεταφράζετε μια συμβολοσειρά, δημιουργείτε ένα plugin ή προσθέτετε μια σημαντική δυνατότητα — **είστε μέρος αυτής της ιστορίας.**

---

<p align="center"><em>Κατασκευασμένο με ❤️ από την SmileStory Inc. και τους συνεισφέροντες παγκοσμίως.</em></p>
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```

#### Option 3: Review machine translations

Many of our 254 languages were machine-translated. Native speaker reviews are incredibly valuable!

1. Pick your language file
2. Review the translations
3. Fix any awkward or incorrect translations
4. Submit a PR

### Language Codes

We use standard ISO 639-1 codes (e.g., `ko`, `en`, `ja`, `ar`, `hi`) with regional variants where needed (e.g., `zh-CN`, `pt-BR`).

---

## 🛠 Development Setup

### Prerequisites

- Node.js 18+
- npm 9+
- Git

### Setup

```bash
```

### Build

```bash
```

> Note: The default 2GB heap is not enough due to the 254 language files + Monaco editor bundle (~38MB renderer).

### Project Structure

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

## 🙏 Thank You

Every contribution makes WIA SOOM better for developers around the world.

Whether you fix a typo, translate a string, build a plugin, or add a major feature — **you are part of this story.**

---

<p align="center"><em>Built with ❤️ by SmileStory Inc. and contributors worldwide.</em></p>
