<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">为 WIA SOOM 贡献</h1>
<p align="center"><strong>我们期待你的贡献!</strong></p>
<p align="center">无论是修复bug、新功能、插件还是翻译——每一份贡献都很重要。</p>

---

## 目录

- [行为规范](#code-of-conduct)
- [如何报告bug](#-how-to-report-bugs)
- [如何建议功能](#-how-to-suggest-features)
- [如何提交插件](#-how-to-submit-a-plugin)
- [如何提交拉取请求](#-how-to-submit-a-pull-request)
- [翻译贡献（254种语言）](#-translation-contributions-254-languages)
- [开发环境设置](#-development-setup)

---

## 行为规范

我们致力于为每个人提供一个热情和包容的体验。

- **尊重他人。** 以尊严对待每一个人。
- **建设性。** 提供有帮助的反馈，而不是破坏性的批评。
- **包容性。** 我们支持254种语言，欢迎来自世界各国的贡献者。
- **无骚扰。** 对任何形式的歧视零容忍。

---

## 🐛 如何报告bug

1. 前往 [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. 点击 **"新建问题"**
3. 选择 **"Bug报告"** 模板
4. 包括:
   - WIA SOOM 版本 (设置 → 关于)
   - 操作系统及版本 (Windows/macOS/Linux)
   - 重现步骤
   - 预期行为与实际行为
   - 如果可能，附上截图或终端输出

---

## 💡 如何建议功能

1. 前往 [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. 点击 **"新建问题"**
3. 选择 **"功能请求"** 模板
4. 描述:
   - 你要解决的问题
   - 你想象它的工作方式
   - 你考虑过的任何替代方案

---

## 🔌 如何提交插件

WIA SOOM 有一个强大的插件系统——你可以在5分钟内构建自己的插件。

### 快速开始
§§§CHUNK_SEPARATOR§§§
### 完整指南

阅读 **[插件开发者指南](docs/PLUGIN_DEVELOPER_GUIDE.md)** 以获取:
- 完整的API参考
- 工作示例
- 逐步教程
- 最佳实践和安全规则

### 提交你的插件

1. Fork [Plugin Store](https://wiasoom.com)
2. 将你的插件添加到 `plugins/{your-plugin-name}/`
3. 提交拉取请求
4. 审核后，你的插件将出现在插件商店中供所有用户��用！

---

## 🔀 如何提交拉取请求

### 对于主应用 (wia-soom)

1. Fork 仓库
2. 创建一个功能分支: `git checkout -b feat/my-feature`
3. 进行更改
4. 本地测试:
   ```bash
   ```
5. 提交时写明清晰的消息:
   ```
   feat: 在设置中添加暗黑模式切换
   ```
6. 推送并打开一个针对 `main` 的PR

### 提交消息规范

| 前缀 | 用途 |
|--------|---------|
| `feat:` | 新功能 |
| `fix:` | 修复bug |
| `docs:` | 仅文档 |
| `refactor:` | 代码重构（不改变行为） |
| `i18n:` | 翻译更新 |
| `plugin:` | 与插件相关的更改 |

### PR 检查清单

- [ ] 代码运行无错误
- [ ] 无硬编码字符串（使用i18n键）
- [ ] 生产代码中没有留下 `console.log`
- [ ] 现有测试仍然通过

---

## 🌐 翻译贡献（254种语言）

WIA SOOM 支持 **254种语言** — 从阿姆哈拉语到祖鲁语，包括盲文和RTL语言。

### 翻译工作原理

- 基础语言文件: `src/renderer/src/i18n/en.json`
- 所有254种语言文件都在同一目录下
- 翻译通过 `scripts/translate-patch.js` 完成 (GPT-4o-mini API)

### 如何贡献翻译

#### 选项1: 修复特定翻译

1. 找到语言文件: `src/renderer/src/i18n/{lang-code}.json`
2. 修复不正确的翻译
3. 提交包含更改的PR

#### 选项2: 添加缺失的键
§§§CHUNK_SEPARATOR§§§
#### 选项3: 审核机器翻译

我们254种语言中的许多是机器翻译的。母语者的审核非常宝贵！

1. 选择你的语言文件
2. 审核翻译
3. 修复任何尴尬或不正确的翻译
4. 提交PR

### 语言代码

我们使用标准的ISO 639-1代码（例如，`ko`，`en`，`ja`，`ar`，`hi`），并在需要时使用地区变体（例如，`zh-CN`，`pt-BR`）。

---

## 🛠 开发环境设置

### 前提条件

- Node.js 18+
- npm 9+
- Git

### 设置
§§§CHUNK_SEPARATOR§§§
### 构建
§§§CHUNK_SEPARATOR§§§
> 注意: 默认的2GB堆内存由于254种语言文件 + Monaco编辑器包（约38MB渲染器）不够用。

### 项目结构
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
---

## 🙏 谢谢你

每一个贡献都让 WIA SOOM 对全球的开发者变得更好。

无论你是修复一个错别字、翻译一个字符串、构建一个插件，还是添加一个重大功能 — **你都是这个故事的一部分。**

---

<p align="center"><em>由 SmileStory Inc. 和全球的贡献者们用 ❤️ 构建。</em></p>
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
