<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM에 기여하기</h1>
<p align="center"><strong>여러분의 기여를 기다립니다!</strong></p>
<p align="center">버그 수정, 새로운 기능, 플러그인 또는 번역 등 모든 기여가 중요합니다.</p>

---

## 목차

- [행동 강령](#code-of-conduct)
- [버그 보고 방법](#-how-to-report-bugs)
- [기능 제안 방법](#-how-to-suggest-features)
- [플러그인 제출 방법](#-how-to-submit-a-plugin)
- [풀 리퀘스트 제출 방법](#-how-to-submit-a-pull-request)
- [번역 기여 (254개 언어)](#-translation-contributions-254-languages)
- [개발 환경 설정](#-development-setup)

---

## 행동 강령

모든 사람에게 환영하고 포용적인 경험을 제공하기 위해 최선을 다하고 있습니다.

- **존중하세요.** 모든 사람을 존엄하게 대하세요.
- **건설적이세요.** 파괴적인 비판이 아닌 유용한 피드백을 제공하세요.
- **포용적이세요.** 우리는 254개 언어를 지원하며, 지구의 모든 국가에서 기여자를 환영합니다.
- **괴롭힘 금지.** 어떤 종류의 차별도 제로 톨러런스입니다.

---

## 🐛 버그 보고 방법

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)로 이동합니다.
2. **"New Issue"**를 클릭합니다.
3. **"Bug Report"** 템플릿을 선택합니다.
4. 다음을 포함합니다:
   - WIA SOOM 버전 (설정 → 정보)
   - OS 및 버전 (Windows/macOS/Linux)
   - 재현 단계
   - 예상 동작 vs. 실제 동작
   - 가능하면 스크린샷이나 터미널 출력

---

## 💡 기능 제안 방법

1. [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)로 이동합니다.
2. **"New Issue"**를 클릭합니다.
3. **"Feature Request"** 템플릿을 선택합니다.
4. 다음을 설명합니다:
   - 해결하려는 문제
   - 어떻게 작동할 것이라고 생각하는지
   - 고려한 대안

---

## 🔌 플러그인 제출 방법

WIA SOOM은 강력한 플러그인 시스템을 가지고 있습니다 — 5분 안에 자신의 플러그인을 만들 수 있습니다.

### 빠른 시작
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### 전체 가이드

**[플러그인 개발자 가이드](docs/PLUGIN_DEVELOPER_GUIDE.md)**를 읽어보세요:
- 전체 API 참조
- 작동 예제
- 단계별 튜토리얼
- 모범 사례 및 보안 규칙

### 플러그인 제출

1. [Plugin Store](https://wiasoom.com)를 포크합니다.
2. `plugins/{your-plugin-name}/`에 플러그인을 추가합니다.
3. 풀 리퀘스트를 제출합니다.
4. 검토 후, 플러그인이 모든 사용자에게 플러그인 스토어에 나타납니다!

---

## 🔀 풀 리퀘스트 제출 방법

### 메인 앱 (wia-soom) 용

1. 저장소를 포크합니다.
2. 기능 브랜치를 생성합니다: `git checkout -b feat/my-feature`
3. 변경 사항을 만듭니다.
4. 로컬에서 테스트합니다:
   ```bash
   ```
5. 명확한 메시지로 커밋합니다:
   ```
   feat: 설정에 다크 모드 토글 추가
   ```
6. `main`에 대해 푸시하고 PR을 엽니다.

### 커밋 메시지 규칙

| 접두사 | 사용 용도 |
|--------|---------|
| `feat:` | 새로운 기능 |
| `fix:` | 버그 수정 |
| `docs:` | 문서 전용 |
| `refactor:` | 코드 구조 조정 (동작 변화 없음) |
| `i18n:` | 번역 업데이트 |
| `plugin:` | 플러그인 관련 변경 사항 |

### PR 체크리스트

- [ ] 코드가 오류 없이 실행됩니다.
- [ ] 하드코딩된 문자열이 없습니다 (i18n 키 사용).
- [ ] 프로덕션 코드에 `console.log`가 남아있지 않습니다.
- [ ] 기존 테스트가 여전히 통과합니다.

---

## 🌐 번역 기여 (254개 언어)

WIA SOOM은 **254개 언어**를 지원합니다 — 암하라어에서 줄루어까지, 점자 및 RTL 언어 포함.

### 번역 작동 방식

- 기본 언어 파일: `src/renderer/src/i18n/en.json`
- 모든 254개 언어 파일이 동일한 디렉토리에 있습니다.
- 번역은 `scripts/translate-patch.js`를 통해 수행됩니다 (GPT-4o-mini API 사용).

### 번역 기여 방법

#### 옵션 1: 특정 번역 수정

1. 언어 파일을 찾습니다: `src/renderer/src/i18n/{lang-code}.json`
2. 잘못된 번역을 수정합니다.
3. 변경 사항으로 PR을 제출합니다.

#### 옵션 2: 누락된 키 추가
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### 옵션 3: 기계 번역 검토

우리의 254개 언어 중 많은 부분이 기계 번역되었습니다. 원어민의 검토는 매우 소중합니다!

1. 언어 파일을 선택합니다.
2. 번역을 검토합���다.
3. 어색하거나 잘못된 번역을 수정합니다.
4. PR을 제출합니다.

### 언어 코드

우리는 표준 ISO 639-1 코드를 사용합니다 (예: `ko`, `en`, `ja`, `ar`, `hi`) 필요에 따라 지역 변형을 포함합니다 (예: `zh-CN`, `pt-BR`).

---

## 🛠 개발 환경 설정

### 필수 조건

- Node.js 18+
- npm 9+
- Git

### 설정
```bash
```
### 빌드
```bash
```
> 참고: 기본 2GB 힙은 254개 언어 파일 + Monaco 에디터 번들 (~38MB 렌더러)로 인해 충분하지 않습니다.

### 프로젝트 구조
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

## 🙏 감사합니다

모든 기여는 전 세계 개발자들을 위해 WIA SOOM을 더 좋게 만듭니다.

오타를 수정하든, 문자열을 번역하든, 플러그인을 만들든, 주요 기능을 추가하든 — **당신은 이 이야기의 일부입니다.**

---

<p align="center"><em>❤️로 만들어진 SmileStory Inc.와 전 세계 기여자들에 의해.</em></p>