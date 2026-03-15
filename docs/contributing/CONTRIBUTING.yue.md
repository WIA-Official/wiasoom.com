<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">為 WIA SOOM 作出貢獻</h1>
<p align="center"><strong>我們期待你的貢獻！</strong></p>
<p align="center">無論是修復錯誤、新功能、插件還是翻譯 — 每一份貢獻都很重要。</p>

---

## 目錄

- [行為準則](#code-of-conduct)
- [如何報告錯誤](#-how-to-report-bugs)
- [如何建議功能](#-how-to-suggest-features)
- [如何提交插件](#-how-to-submit-a-plugin)
- [如何提交拉取請求](#-how-to-submit-a-pull-request)
- [翻譯貢獻 (254 種語言)](#-translation-contributions-254-languages)
- [開發設置](#-development-setup)

---

## 行為準則

我們致力於為每個人提供一個友好和包容的體驗。

- **尊重他人。** 以尊嚴對待每個人。
- **建設性。** 提供有幫助的反饋，而不是破壞性的批評。
- **包容性。** 我們支持 254 種語言，歡迎來自地球上每個國家的貢獻者。
- **無騷擾。** 對任何形式的歧視零容忍。

---

## 🐛 如何報告錯誤

1. 前往 [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. 點擊 **"新問題"**
3. 選擇 **"錯誤報告"** 模板
4. 包括：
   - WIA SOOM 版�� (設置 → 關於)
   - 操作系統及版本 (Windows/macOS/Linux)
   - 重現步驟
   - 預期行為與實際行為
   - 如果可能，附上截圖或終端輸出

---

## 💡 如何建議功能

1. 前往 [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. 點擊 **"新問題"**
3. 選擇 **"功能請求"** 模板
4. 描述：
   - 你要解決的問題
   - 你想像的運作方式
   - 你考慮過的任何替代方案

---

## 🔌 如何提交插件

WIA SOOM 擁有強大的插件系統 — 你可以在 5 分鐘內建立自己的插件。

### 快速入門
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### 完整指南

閱讀 **[插件開發者指南](docs/PLUGIN_DEVELOPER_GUIDE.md)** 以獲取：
- 完整的 API 參考
- 工作示例
- 步驟教程
- 最佳實踐和安全規則

### 提交你的插件

1. Fork [Plugin Store](https://wiasoom.com)
2. 將你的插件添加到 `plugins/{your-plugin-name}/`
3. 提交拉取請求
4. 經過審核後，你的插件將出現在所有用戶的插件商店中！

---

## 🔀 如何提交拉取請求

### 對於主應用程式 (wia-soom)

1. Fork 該倉庫
2. 創建一個功能分支：`git checkout -b feat/my-feature`
3. 進行更改
4. 本地測試：
   ```bash
   ```
5. ��清晰的訊息提交：
   ```
   feat: 在設置中添加深色模式切換
   ```
6. 推送並對 `main` 開啟 PR

### 提交訊息約定

| 前綴 | 用途 |
|------|------|
| `feat:` | 新功能 |
| `fix:` | 錯誤修復 |
| `docs:` | 僅文檔 |
| `refactor:` | 代碼重構（無行為變更） |
| `i18n:` | 翻譯更新 |
| `plugin:` | 與插件相關的更改 |

### PR 檢查清單

- [ ] 代碼無錯誤運行
- [ ] 無硬編碼字符串（使用 i18n 鍵）
- [ ] 生產代碼中無 `console.log`
- [ ] 現有測試仍然通過

---

## 🌐 翻譯貢獻 (254 種語言)

WIA SOOM 支持 **254 種語言** — 從阿姆哈拉語到祖魯語，包括盲文和 RTL 語言。

### 翻譯工作原理

- 基本語言文件：`src/renderer/src/i18n/en.json`
- 所有 254 種語言文件都在同一目錄中
- 翻譯通過 `scripts/translate-patch.js` 完成（GPT-4o-mini API）

### 如何貢獻翻譯

#### 選項 1：修正特定翻譯

1. 找到語言文件：`src/renderer/src/i18n/{lang-code}.json`
2. 修正不正確的翻譯
3. 提交一個 PR 以更改

#### 選項 2：添加���失的鍵
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### 選項 3：審核機器翻譯

我們的 254 種語言中有許多是機器翻譯的。母語者的審核非常有價值！

1. 選擇你的語言文件
2. 審核翻譯
3. 修正任何尷尬或不正確的翻譯
4. 提交一個 PR

### 語言代碼

我們使用標準的 ISO 639-1 代碼（例如，`ko`、`en`、`ja`、`ar`、`hi`），必要時使用地區變體（例如，`zh-CN`、`pt-BR`）。

---

## 🛠 開發設置

### 前提條件

- Node.js 18+
- npm 9+
- Git

### 設置
```bash
```
### 構建
```bash
```
> 注意：由於 254 種語言文件 + Monaco 編輯器包（約 38MB 渲染），默認的 2GB 堆棧不夠用。

### 項目結構
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

## 🙏 多謝你

每一份貢獻都令 WIA SOOM 對全球開發者變得更好。

無論你是修正錯字、翻譯字符串、建立插件，還是添加一個主要功能 — **你都是這個故事的一部分。**

---

<p align="center"><em>由 SmileStory Inc. 及全球貢獻者用 ❤️ 建造。</em></p>