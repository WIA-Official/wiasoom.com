<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM मध्ये योगदान देणे</h1>
<p align="center"><strong>आम्हाला तुमच्या योगदानाची अपेक्षा आहे!</strong></p>
<p align="center">ते बग फिक्स असो, नवीन वैशिष्ट्य, प्लगइन, किंवा भाषांतर — प्रत्येक योगदान महत्त्वाचे आहे.</p>

---

## सामग्रीची यादी

- [आचारसंहिता](#code-of-conduct)
- [बग कसे रिपोर्ट करावे](#-how-to-report-bugs)
- [वैशिष्ट्ये कशा सुचवायच्या](#-how-to-suggest-features)
- [प्लगइन कसे सबमिट करावे](#-how-to-submit-a-plugin)
- [पुल विनंती कशी सबमिट करावी](#-how-to-submit-a-pull-request)
- [भा��ांतर योगदान (254 भाषांमध्ये)](#-translation-contributions-254-languages)
- [विकास सेटअप](#-development-setup)

---

## आचारसंहिता

आम्ही सर्वांसाठी एक स्वागतार्ह आणि समावेशक अनुभव प्रदान करण्यास वचनबद्ध आहोत.

- **आदर करा.** प्रत्येकाला प्रतिष्ठा द्या.
- **उपयुक्त रहा.** नकारात्मक टीका न करता उपयुक्त अभिप्राय द्या.
- **समावेशक रहा.** आम्ही 254 भाषांना समर्थन देतो आणि पृथ्वीवरील प्रत्येक देशातील योगदानकर्त्यांचे स्वागत करतो.
- **छेडछाड नाही.** कोणत्याही प्रकारच्या भेदभावासाठी शून्य सहिष्णुता.

---

## 🐛 बग कसे रिपोर्ट करावे

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) वर जा
2. **"नवीन समस्या"** वर क्लिक करा
3. **"बग रिपोर्ट"** टेम्पलेट निवडा
4. समाविष्ट करा:
   - WIA SOOM आवृत्ती (सेटिंग्ज → माहिती)
   - OS आणि आवृत्ती (Windows/macOS/Linux)
   - पुनरुत्पादनाचे पायऱ्या
   - अपेक्षित व वास्तविक वर्तन
   - शक्य असल्यास स्क्रीनशॉट किंवा टर्मिनल आउटपुट

---

## 💡 वैशिष्ट्ये कशा सुचवायच्या

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues) वर जा
2. **"नवीन समस्या"** वर क्लिक करा
3. **"वैशिष्ट्य विनंती"** टेम्पलेट निवडा
4. वर्णन करा:
   - तुम्ही कोणता समस्या सोडवत आहात
   - तुम्ही ते कसे कार्यरत असेल असे विचारता
   - तुम्ही विचारलेले कोणतेही पर्याय

---

## 🔌 प्लगइ�� कसे सबमिट करावे

WIA SOOM मध्ये एक शक्तिशाली प्लगइन प्रणाली आहे — तुम्ही 5 मिनिटांत तुमचा स्वतःचा प्लगइन तयार करू शकता.

### जलद प्रारंभ
§§§CHUNK_SEPARATOR§§§
### संपूर्ण मार्गदर्शक

**[प्लगइन डेव्हलपर मार्गदर्शक](docs/PLUGIN_DEVELOPER_GUIDE.md)** वाचा:
- संपूर्ण API संदर्भ
- कार्यरत उदाहरणे
- चरण-दर-चरण ट्यूटोरियल
- सर्वोत्तम पद्धती आणि सुरक्षा नियम

### तुमचा प्लगइन सबमिट करा

1. [Plugin Store](https://wiasoom.com) फोर्क करा
2. तुमचा प्लगइन `plugins/{your-plugin-name}/` मध्ये जोडा
3. पुल विनंती सबमिट करा
4. पुनरावलोकनानंतर, तुमचा प्लगइन सर्व वापरकर्त्यांसाठी प्लगइन स्टोअरम��्ये दिसतो!

---

## 🔀 पुल विनंती कशी सबमिट करावी

### मुख्य अॅपसाठी (wia-soom)

1. रेपॉजिटरी फोर्क करा
2. एक वैशिष्ट्य शाखा तयार करा: `git checkout -b feat/my-feature`
3. तुमच्या बदलांची अंमलबजावणी करा
4. स्थानिकपणे चाचणी करा:
   ```bash
   ```
5. स्पष्ट संदेशासह कमिट करा:
   ```
   feat: सेटिंग्जमध्ये डार्क मोड टॉगल जोडा
   ```
6. `main` विरुद्ध PR ओपन करा

### कमिट संदेशाची पद्धत

| प्रीफिक्स | वापरासाठी |
|-----------|-----------|
| `feat:`   | नवीन वैशिष्ट्य |
| `fix:`    | बग फिक्स |
| `docs:`   | फक्त दस्तऐवजीकरण |
| `refactor:` | कोड पुनर्रचना (कोणतेही वर्तन बदल नाही) |
| `i18n:`   | भाषांतर अद्यतने |
| `plugin:` | प���लगइन-संबंधित बदल |

### PR चेकलिस्ट

- [ ] कोड त्रुटीशिवाय चालतो
- [ ] हार्डकोडेड स्ट्रिंग्ज नाहीत (i18n की वापरा)
- [ ] उत्पादन कोडमध्ये `console.log` नाही
- [ ] विद्यमान चाचण्या अजूनही पास होतात

---

## 🌐 भाषांतर योगदान (254 भाषांमध्ये)

WIA SOOM **254 भाषांना** समर्थन देते — अम्हारिकपासून झुलूपर्यंत, ब्रेल आणि RTL भाषांचा समावेश आहे.

### भाषांतर कसे कार्य करते

- मूलभूत भाषा फाइल: `src/renderer/src/i18n/en.json`
- सर्व 254 भाषा फाइल्स एकाच निर्देशिकेत आहेत
- भाषांतर `scripts/translate-patch.js` (GPT-4o-mini API) द्वारे केले जाते

### भाषांतर कसे योगदान द्यावे

#### पर्याय 1: विशिष्ट भाषांतर दुरुस���त करा

1. भाषा फाइल शोधा: `src/renderer/src/i18n/{lang-code}.json`
2. चुकीचे भाषांतर दुरुस्त करा
3. बदलासह PR सबमिट करा

#### पर्याय 2: गहाळ कीज जोडा
§§§CHUNK_SEPARATOR§§§
#### पर्याय 3: मशीन भाषांतरांचे पुनरावलोकन करा

आमच्या 254 भाषांपैकी अनेक मशीनद्वारे भाषांतरित केल्या गेल्या आहेत. स्थानिक भाषिक पुनरावलोकन अत्यंत मौल्यवान आहे!

1. तुमची भाषा फाइल निवडा
2. भाषांतरांचे पुनरावलोकन करा
3. कोणतीही अडचण किंवा चुकीची भाषांतर दुरुस्त करा
4. PR सबमिट करा

### भाषा कोड

आम्ही मानक ISO 639-1 कोड (उदा., `ko`, `en`, `ja`, `ar`, `hi`) वापरतो, आवश्यकतेनुसार प्रादेशिक भिन्नता (उदा., `zh-CN`, `pt-BR`) सह.

---

## 🛠 विकास सेटअप

### पूर्वअट

- Node.js 18+
- npm 9+
- Git

### सेटअप
§§§CHUNK_SEPARATOR§§§
### बिल्ड
§§§CHUNK_SEPARATOR§§§
> नोट: 254 भाषा फाइल्स + Monaco संपादक बंडल (~38MB) मुळे डिफॉल्ट 2GB ढेर पुरेसे नाही.

### प्रोजेक्ट संरचना
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 धन्यवाद

प्रत्येक योगदान WIA SOOM ला जगभरातील विकासकांसाठी चांगले बनवते.

तुम्ही एक टायपो दुरुस्त केला, एक स्ट्रिंगचा भाषांतर केला, एक प्लगइन तयार केला, किंवा एक मोठी वैशिष्ट्य जोडली — **तुम्ही या कथेत भाग आहात.**

---

<p align="center"><em>❤️ ने तयार केलेले SmileStory Inc. आणि जगभरातील योगदानकर्त्यांनी.</em></p>
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
