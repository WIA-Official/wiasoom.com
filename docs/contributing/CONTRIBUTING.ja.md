<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOMへの貢献</h1>
<p align="center"><strong>あなたの貢献をお待ちしています！</strong></p>
<p align="center">バグ修正、新機能、プラグイン、翻訳など、すべての貢献が重要です。</p>

---

## 目次

- [行動規範](#code-of-conduct)
- [バグの報告方法](#-how-to-report-bugs)
- [機能提案の方法](#-how-to-suggest-features)
- [プラグインの提出方法](#-how-to-submit-a-plugin)
- [プルリクエストの提出方法](#-how-to-submit-a-pull-request)
- [翻訳貢献 (254言語)](#-translation-contributions-254-languages)
- [開発セットアップ](#-development-setup)

---

## 行動規範

私たちは、すべての人にとって歓迎され、包括的な体験を提供することを約束します。

- **敬意を持って接する。** すべての人を尊厳を持って扱いましょう。
- **建設的である。** 役立つフィードバックを提供し、破壊的な批判は避けましょう。
- **包括的である。** 私たちは254言語をサポートし、地球上のすべての国からの貢献者を歓迎します。
- **嫌がらせはしない。** いかなる種類の差別にもゼロトレランスです。

---

## 🐛 バグの報告方法

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)に移動します
2. **「New Issue」**をクリックします
3. **「Bug Report」**テンプレートを選択します
4. 次の情報を含めます：
   - WIA SOOMのバージョン (設定 → アバウト)
   - OSとバージョン (Windows/macOS/Linux)
   - 再現手順
   - 期待される動作と実際の動作
   - 可能であればスクリーンショットやターミナル出力

---

## 💡 機能提案の方法

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)に移動します
2. **「New Issue」**をクリックします
3. **「Feature Request」**テンプレートを選択します
4. 次のことを説明します：
   - 解決しようとしている問題
   - どのように機能することを想像しているか
   - 考慮した代替案

---

## 🔌 プラグインの提出方法

WIA SOOMには強力なプラグインシステムがあります — 5分で自分のプラグインを作成できます。

### クイックスタート
§§§CHUNK_SEPARATOR§§��
### 完全ガイド

**[プラグイン開発者ガイド](docs/PLUGIN_DEVELOPER_GUIDE.md)**を読んでください：
- 完全なAPIリファレンス
- 動作例
- ステップバイステップのチュートリアル
- ベストプラクティスとセキュリティルール

### プラグインを提出する

1. [Plugin Store](https://wiasoom.com)をフォークします
2. `plugins/{your-plugin-name}/`にプラグインを追加します
3. プルリクエストを提出します
4. レビュー後、あなたのプラグインがすべてのユーザーのためにプラグインストアに表示されます！

---

## 🔀 プルリクエストの提出方法

### メインアプリ (wia-soom) の場合

1. リポジトリをフォークします
2. フィーチャーブランチを作成します： `git checkout -b feat/my-feature`
3. 変更を加えます
4. ローカルでテストします：
   ```bash
   ```
5. 明確なメッセージでコミットします：
   ```
   feat: 設定にダークモードの切り替えを追加
   ```
6. `main`に対してプッシュし、PRを開きます

### コミットメッセージの規約

| プレフィックス | 用途 |
|----------------|------|
| `feat:`        | 新機能 |
| `fix:`         | バグ修正 |
| `docs:`        | ドキュメントのみ |
| `refactor:`    | コードのリファクタリング（動作変更なし） |
| `i18n:`        | 翻訳の更新 |
| `plugin:`      | プラグイン関連の変更 |

### PRチェックリスト

- [ ] コードがエラーなく実行される
- [ ] ハードコーディングされた文字列がない（i18nキーを使用）
- [ ] 本番コードに`console.log`が残っていない
- [ ] 既存のテストがすべて通過する

---

## 🌐 翻訳貢献 (254言語)

WIA SOOMは**254言語**をサポートしています — アムハラ語からズールー語まで、点字やRTL言語も含まれます。

### 翻訳の仕組み

- 基本言語ファイル： `src/renderer/src/i18n/en.json`
- すべての254言語ファイルは同じディレクトリにあります
- 翻訳は`scripts/translate-patch.js`（GPT-4o-mini API）を通じて行われます

### 翻訳に貢献する方法

#### オプション1：特定の翻訳を修正する

1. 言語ファイルを見つけます： `src/renderer/src/i18n/{lang-code}.json`
2. 不正確な翻訳を修正します
3. 変更を含むPRを提出します

#### オプション2：欠落しているキーを追加する
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
#### オプション3：��械翻訳をレビューする

私たちの254言語の多くは機械翻訳されています。ネイティブスピーカーによるレビューは非常に貴重です！

1. あなたの言語ファイルを選びます
2. 翻訳をレビューします
3. 不自然または不正確な翻訳を修正します
4. PRを提出します

### 言語コード

標準のISO 639-1コード（例： `ko`, `en`, `ja`, `ar`, `hi`）を使用し、必要に応じて地域のバリアント（例： `zh-CN`, `pt-BR`）を使用します。

---

## 🛠 開発セットアップ

### 前提条件

- Node.js 18+
- npm 9+
- Git

### セットアップ
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
### ビルド
```bash
```
> 注：デフォルトの2GBヒープは、254の言語ファイルとMonacoエディタバンドル（約38MBレンダラー）のために不十分です。

### プロジェクト構造
```bash
```
---

## 🙏 ありがとう

すべての貢献が、世界中の開発者にとって WIA SOOM をより良くします。

タイプミスを修正したり、文字列を翻訳したり、プラグインを作成したり、大きな機能を追加したりするかどうかにかかわらず — **あなたはこの物語の一部です。**

---

<p align="center"><em>❤️ で構築された SmileStory Inc. と世界中の貢献者による。</em></p>
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
