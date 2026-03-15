<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM में योगदान देना</h1>
<p align="center"><strong>हम आपके योगदान का स्वागत करते हैं!</strong></p>
<p align="center">चाहे वह बग फिक्स हो, नई विशेषता, प्लगइन, या अनुवाद — हर योगदान महत्वपूर्ण है।</p>

---

## सामग्री की तालिका

- [आचार संहिता](#code-of-conduct)
- [बग कैसे रिपोर्ट करें](#-how-to-report-bugs)
- [विशेषताएँ कैसे सुझाएँ](#-how-to-suggest-features)
- [प्लगइन कैसे सबमिट करें](#-how-to-submit-a-plugin)
- [पुल अनुरोध कैसे सबमिट करें](#-how-to-submit-a-pull-request)
- [अनुवाद योगदान (254 भाषाएँ)](#-translation-contributions-254-languages)
- [विकास सेटअप](#-development-setup)

---

## आचार संहिता

हम सभी के लिए एक स्वागत योग्य और समावेशी अनुभव प्रदान करने के लिए प्रतिबद्ध हैं।

- **सम्मानजनक रहें।** सभी के साथ गरिमा के साथ व्यवहार करें।
- **संरचनात्मक रहें।** सहायक प्रतिक्रिया दें, विनाशकारी आलोचना नहीं।
- **समावेशी रहें।** हम 254 भाषाओं का समर्थन करते हैं और पृथ्वी के हर देश से योगदानकर्ताओं का स्वागत करते हैं।
- **कोई उत्पीड़न नहीं।** किसी भी प्रकार के भेदभाव के लिए शून्य सहिष्णुता।

---

## 🐛 बग कैसे रिपोर्ट करें

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) पर जाए���
2. **"New Issue"** पर क्लिक करें
3. **"Bug Report"** टेम्पलेट चुनें
4. शामिल करें:
   - WIA SOOM संस्करण (Settings → About)
   - OS और संस्करण (Windows/macOS/Linux)
   - पुनरुत्पादन के चरण
   - अपेक्षित बनाम वास्तविक व्यवहार
   - यदि संभव हो तो स्क्रीनशॉट या टर्मिनल आउटपुट

---

## 💡 विशेषताएँ कैसे सुझाएँ

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) पर जाएं
2. **"New Issue"** पर क्लिक करें
3. **"Feature Request"** टेम्पलेट चुनें
4. वर्णन करें:
   - आप किस समस्या का समाधान कर रहे हैं
   - आप इसे कैसे काम करते हुए कल्पना करते हैं
   - आपने किन विकल्पों पर विचार किया है

---

## 🔌 प्लगइन कैसे सबमिट करें

WIA SOOM में एक शक्तिशाली ���्लगइन प्रणाली है — आप 5 मिनट में अपना खुद का प्लगइन बना सकते हैं।

### त्वरित प्रारंभ
§§§CHUNK_SEPARATOR§§§
### पूर्ण गाइड

**[प्लगइन डेवलपर गाइड](docs/PLUGIN_DEVELOPER_GUIDE.md)** पढ़ें:
- पूर्ण API संदर्भ
- कार्यशील उदाहरण
- चरण-दर-चरण ट्यूटोरियल
- सर्वोत्तम प्रथाएँ और सुरक्षा नियम

### अपना प्लगइन सबमिट करें

1. [Plugin Store](https://wiasoom.com) को फोर्क करें
2. अपने प्लगइन को `plugins/{your-plugin-name}/` में जोड़ें
3. एक पुल अनुरोध सबमिट करें
4. समीक्षा के बाद, आपका प्लगइन सभी उपयोगकर्ताओं के लिए प्लगइन स्टोर में दिखाई देगा!

---

## 🔀 पुल अनुरोध कैसे सबमिट करें

### मुख्य ऐप (wia-soom) के लिए

1. रिपॉजिटरी को फोर्क करें
2. एक फीचर ब्रांच बनाएं: `git checkout -b feat/my-feature`
3. अपने परिवर्तन करें
4. स्थानीय रूप से परीक्षण करें:
   ```bash
   ```
5. स्पष्ट संदेश के साथ कमिट करें:
   ```
   feat: सेटिंग्स में डार्क मोड टॉगल जोड़ें
   ```
6. `main` के खिलाफ पुश करें और एक PR खोलें

### कमिट संदेश सम्मेलन

| प्रीफिक्स | उपयोग के लिए |
|-----------|--------------|
| `feat:`   | नई विशेषता  |
| `fix:`    | बग फिक्स     |
| `docs:`   | केवल दस्तावेज़ |
| `refactor:` | कोड पुनर्गठन (कोई व्यवहार परिवर्तन नहीं) |
| `i18n:`   | अनुवाद अपडेट |
| `plugin:` | प्लगइन से संबंधित परिवर्तन |

### PR चेकलिस्ट

- [ ] कोड बिना त��रुटियों के चलता है
- [ ] कोई हार्डकोडेड स्ट्रिंग नहीं (i18n कुंजी का उपयोग करें)
- [ ] उत्पादन कोड में कोई `console.log` नहीं छोड़ा गया
- [ ] मौजूदा परीक्षण अभी भी पास होते हैं

---

## 🌐 अनुवाद योगदान (254 भाषाएँ)

WIA SOOM **254 भाषाओं** का समर्थन करता है — अम्हारिक से लेकर ज़ुलु तक, ब्रेल और RTL भाषाओं सहित।

### अनुवाद कैसे काम करता है

- बेस भाषा फ़ाइल: `src/renderer/src/i18n/en.json`
- सभी 254 भाषा फ़ाइलें एक ही निर्देशिका में हैं
- अनुवाद `scripts/translate-patch.js` (GPT-4o-mini API) के माध्यम से किया जाता है

### अनुवादों में योगदान कैसे करें

#### विकल्प 1: एक विशिष्ट अनुवाद को ठीक करें

1. भाषा फ़ाइल खोजें: `src/renderer/src/i18n/{lang-code}.json`
2. गलत अनुवाद को ठीक करें
3. परिवर्तन के साथ एक PR सबमिट करें

#### विकल्प 2: गायब कुंजियाँ जोड़ें
§§§CHUNK_SEPARATOR§§§
#### विकल्प 3: मशीन अनुवादों की समीक्षा करें

हमारी 254 भाषाओं में से कई मशीन-अनुवादित हैं। मूल वक्ता की समीक्षाएँ बेहद मूल्यवान हैं!

1. अपनी भाषा फ़ाइल चुनें
2. अनुवादों की समीक्षा करें
3. किसी भी अजीब या गलत अनुवाद को ठीक करें
4. एक PR सबमिट करें

### भाषा कोड

हम मानक ISO 639-1 कोड (जैसे, `ko`, `en`, `ja`, `ar`, `hi`) का उपयोग करते हैं, जहां आवश्यक हो क्षेत्रीय भिन्नताओं के साथ (जैसे, `zh-CN`, `pt-BR`)।

---

## 🛠 विकास ��ेटअप

### पूर्वापेक्षाएँ

- Node.js 18+
- npm 9+
- Git

### सेटअप
§§§CHUNK_SEPARATOR§§§
### निर्माण
§§§CHUNK_SEPARATOR§§§
> नोट: डिफ़ॉल्ट 2GB हीप 254 भाषा फ़ाइलों + मोंको एडिटर बंडल (~38MB रेंडरर) के कारण पर्याप्त नहीं है।

### प्रोजेक्ट संरचना
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 धन्यवाद

हर योगदान WIA SOOM को दुनिया भर के डेवलपर्स के लिए बेहतर बनाता है।

चाहे आप एक टाइपो ठीक करें, एक स्ट्रिंग का अनुवाद करें, एक प्लगइन बनाएं, या एक प्रमुख फीचर जोड़ें — **आप इस कहानी का हिस्सा हैं।**

---

<p align="center"><em>❤️ के साथ बनाया गया SmileStory Inc. और दुनिया भर के योगदानकर्ताओं द्वारा।</em></p>
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
