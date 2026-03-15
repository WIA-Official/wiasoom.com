<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">WIA SOOM 플러그인 개발자 가이드</h1>
<p align="center"><strong>5분 안에 나만의 플러그인을 만들어보세요.</strong></p>
<p align="center">WIA SOOM 내부에서 강력한 서버 도구, 대시보드 및 자동화를 생성하세요.</p>

---

## 목차

- [1부: 빠른 시작 — 5분 안에 첫 번째 플러그인 만들기](#1부-빠른-시작--5분-안에-첫-번째-플러그인-만들기)
- [2부: 플러그인 컨텍스트 API 참조](#2부-플러그인-컨텍스트-api-참조)
  - [ctx.terminal](#ctxterminal--원격-서버에서-명령-실행)
  - [ctx.sftp](#ctxsftp--파일-전송)
  - [ctx.ui](#ctxui--사용자-인터페이스)
  - [ctx.settings](#ctxsettings--지속적인-저장소)
  - [ctx.ai](#ctxai--ai-통합)
- [3부: 웹뷰로 사용자 정의 UI 만들기](#3부-웹뷰로-사용자-정의-ui-만들기)
- [4부: 플러그인 게시하기](#4부-플러그인-게시하기)
- [5부: 모범 사례](#5부-모범-사례)
- [6부: 실제 사례](#6부-실제-사례)
- [부록: 카테고리 및 아이콘](#부록-카테고리-및-아이콘)

---

## 1부: 빠른 시작 — 5분 안에 첫 번째 플러그인 만들기

### 만들 내용

사이드바에 버튼을 추가하는 "Hello World" 플러그인입니다. 클릭하면 알림이 표시됩니다.

### 1단계: 플러그인 폴더 만들기
```bash
mkdir -p ~/.wia-soom/plugins/hello-world
cd ~/.wia-soom/plugins/hello-world
```
### 2단계: package.json 만들기
```json
{
  "name": "hello-world",
  "version": "1.0.0",
  "description": "My first WIA SOOM plugin — says hello!",
  "author": "Your Name",
  "main": "index.js",
  "license": "MIT",
  "keywords": ["hello", "example"],
  "soom": {
    "minVersion": "0.50.0"
  }
}
```
**필수 필드:** `name`, `version`, `description`, `author`, `main`

### 3단계: index.js 만들기
```javascript
'use strict';

/**
 * Hello World — WIA SOOM Plugin
 *
 * This is the simplest possible plugin.
 * It adds a sidebar button and shows a notification when clicked.
 */

exports.activate = function activate(context) {
  // Add a button to the sidebar
  context.ui.addSidebarButton({
    icon: 'hand-metal',      // Lucide icon name (see: lucide.dev/icons)
    label: 'Hello World',
    onClick: function() {
      context.ui.showNotification('success', 'Hello from WIA SOOM! Your first plugin works!');
    }
  });
};

// Optional: cleanup when plugin is disabled or app closes
exports.deactivate = function deactivate() {
  // Nothing to clean up in this example
};
```
### 4단계: WIA SOOM 재시작하기

앱을 재시작하거나 설정 → 플러그인에서 플러그인을 껐다 켜세요.

사이드바에 **"Hello World"** 버튼이 표시됩니다. 클릭해보세요 — 성공 알림이 표시됩니다!

### 작동 방식
```
1. App starts → scans ~/.wia-soom/plugins/
2. Finds hello-world/package.json → reads manifest
3. If enabled: require('hello-world/index.js')
4. Calls activate(context) → your code runs
5. Your code registers a sidebar button
6. User clicks button → onClick fires
7. App closes → calls deactivate() for cleanup
```
---

## 2부: 플러그인 컨텍스트 API 참조

`activate(context)` 함수가 호출되면 `context` (또는 `ctx`)는 다음 API를 제공합니다:
```typescript
interface PluginContext {
  terminal: { ... }   // Run commands on remote servers
  sftp:     { ... }   // Upload/download files
  ui:       { ... }   // Sidebar buttons, webviews, notifications, status bar
  settings: { ... }   // Persistent key-value storage
  ai:       { ... }   // AI chat (Soomy integration)
}
```
---

### `ctx.terminal` — 원격 서버에서 명령 실행

#### `terminal.send(sessionId, data)`

활성 터미널 세션에 명령(또는 데이터를) 전송합니다.

| 매개변수 | 유형 | 설명 |
|-----------|------|-------------|
| `sessionId` | `string` | 전송할 터미널 세션 |
| `data` | `string` | 전송할 명령 또는 데이터 |
```javascript
// Send a command to the terminal
// Don't forget the \n (newline) to execute it!
context.terminal.send(sessionId, 'df -h\n');
```
#### `terminal.onOutput(sessionId, callback)`

터미널 세���의 모든 출력에 구독합니다. **구독 해제 함수**를 반환합니다.

| 매개변수 | 유형 | 설명 |
|-----------|------|-------------|
| `sessionId` | `string` | 감시할 터미널 세션 |
| `callback` | `(data: string) => void` | 각 출력 청크와 함께 호출됩니다 |
| **반환** | `() => void` | 청취를 중지하려면 이 함수를 호출하세요 |
```javascript
// Watch terminal output for errors
var unsubscribe = context.terminal.onOutput(sessionId, function(data) {
  if (data.includes('ERROR') || data.includes('error')) {
    context.ui.showNotification('error', 'Error detected in terminal output!');
  }
});

// Later: stop watching
unsubscribe();
```
**중요:** 항상 구독 해제 함수를 저장하고 `deactivate()`에서 호출하여 메모리 누수를 방지하세요.

---

### `ctx.sftp` — 파일 전송

> **상태: 곧 출시 예정** — SFTP API는 정의되었지만 아직 앱의 SFTP 엔진에 연결되지 않았습니다. `list()`는 현재 빈 배열을 반환하며, `upload()`/`download()`는 작동하지 않습니다. 이는 향후 릴리스에서 완전히 구현될 예정입니다. 현재는 `ctx.terminal.send()`를 사용하여 `scp` 또는 `rsync` 명령으로 우회하세요.

#### `sftp.list(sessionId, path)`

원격 디렉토리의 파일을 나열합니다.
```javascript
var files = await context.sftp.list(sessionId, '/var/log/');
// files = [{ name: 'syslog', size: 1024, ... }, ...]
```
#### `sftp.upload(sessionId, localPath, remotePath)`

��컬 머신에서 원격 서버로 파일을 업로드합니다.
```javascript
await context.sftp.upload(sessionId, '/tmp/config.json', '/etc/myapp/config.json');
context.ui.showNotification('success', 'Config uploaded!');
```
#### `sftp.download(sessionId, remotePath, localPath)`

원격 서버에서 로컬 머신으로 파일을 다운로드합니다.
```javascript
await context.sftp.download(sessionId, '/var/log/app.log', '/tmp/app.log');
```
**우회 방법 (SFTP API가 활성화될 때까지):**
```javascript
// Use terminal commands instead
context.terminal.send(sessionId, 'scp user@host:/var/log/app.log /tmp/\n');
```
---

### `ctx.ui` — 사용자 인터페이스

#### `ui.addSidebarButton(options)`

WIA SOOM 사이드바에 버튼을 추가합니다.

| 옵션 | 유형 | 필수 | 설명 |
|--------|------|----------|-------------|
| `id` | `string` | 아니오 | 고유 ID (기본값은 플러그인 이름) |
| `icon` | `string` | 예 | Lucide 아이콘 이름 (예: `'server'`, `'shield'`, `'database'`) |
| `label` | `string` | 예 | 사이드바에 표시될 버튼 텍스트 |
| `onClick` | `() => void` | 예 | 버튼 클릭 시 호출되는 함수 |
```javascript
context.ui.addSidebarButton({
  id: 'my-dashboard',
  icon: 'layout-dashboard',
  label: 'My Dashboard',
  onClick: function() {
    // Open a webview, run a command, show a notification — anything!
    context.ui.openWebview({
      title: 'My Dashboard',
      html: '<h1>Hello!</h1><p>This is my dashboard.</p>'
    });
  }
});
```
**아이콘 참조:** 사용 가능한 모든 아이콘은 [lucide.dev/icons](https://lucide.dev/icons)에서 확인하세요.

> **호환성 주의:** 일부 오래된 플러그인은 `addSidebarButton(id, icon, label, onClick)`와 같은 위치 인수를 사용합니다. 공식 API는 위에 문서화된 **옵션 객체**를 사용합니다. 새로운 플러그인에는 항상 객체 스타일을 사용하세요.

#### `ui.openWebview(options)`

사용자 정의 HTML 콘텐츠로 팝업 창을 엽니다. 이것이 풍부한 UI를 만드는 방법입니다.

| 옵션 | 유형 | 설명 |
|--------|------|-------------|
| `title` | `string` | 창 제목 |
| `html` | `string` | 렌더링할 전체 HTML 콘텐츠 |
```javascript
context.ui.openWebview({
  title: 'Server Status',
  html: `
    <html>
    <body style="font-family: sans-serif; padding: 20px; background: #1a1a2e; color: #e2e8f0;">
      <h1>Server Status</h1>
      <div id="status">Loading...</div>
      <script>
        document.getElementById('status').textContent = 'All systems operational';
      </script>
    </body>
    </html>
  `
});
```
> [3부](#part-3-building-custom-ui-with-webviews)를 참조하여 고급 웹뷰 패턴을 확인하세요.

#### `ui.showNotification(type, message)`

토스트 알림을 표시합니다.

| 매개변수 | 유형 | 설명 |
|-----------|------|-------------|
| `type` | `'success' \| 'error' \| 'info'` | 알림 스타일 |
| `message` | `string` | 표시할 텍스트 |
§§§CHUNK_SEPARATOR§§§
#### `ui.addStatusBarItem(id, text)`

하단 상태 표시줄에 지속적인 텍스트 항목을 추가합니다.

| 매개변수 | 유형 | 설명 |
|-----------|------|-------------|
| `id` | `string` | 이 상태 항목의 고유 ID |
| `text` | `string` | 표시할 텍스트 |
§§§CHUNK_SEPARATOR§§§
---

### `ctx.settings` — 지속적인 저장소

플러그인 설정은 `~/.wia-soom/plugins/{your-plugin}/.plugin-settings.json`에 영구적으로 저장됩니다.

#### `settings.get(key)`

저장된 값을 읽습니다.
§§§CHUNK_SEPARATOR§§§
키가 존재하지 않으면 `undefined`를 반환합니다.

#### `settings.set(key, value)`

값을 저장합니다. 문자열, 숫자, 불리언, 배열 및 객체를 지원합니다.
§§§CHUNK_SEPARATOR§§§
**예시: 사용자 선호도 기억하기**
§§§CHUNK_SEPARATOR§§§
---

### `ctx.ai` — AI 통합

> **상태: 곧 출시 예정** — AI API는 정의되었지만 아직 Soomy에 연결되지 않았습니다. 현재는 `{ response: 'AI not yet connected' }`를 반환합니다. 전체 AI 통합은 향후 릴리스를 계획하고 있습니다.

#### `ai.chat(messages, options?)`

AI 어시스턴트(Soomy)에게 메시지를 보냅니다.
§§§CHUNK_SEPARATOR§§§
---

## 3부: 웹뷰로 사용자 정의 UI 구축하기

`openWebview()` API를 사용하면 HTML, CSS 및 JavaScript로 대시보드 UI를 팝업 창 안에서 구축할 수 있습니다.

> **중요한 제한 사항:** 웹뷰는 **표시 전용**입니다. 플러그인 API(`ctx.settings`, `ctx.terminal` 등)를 호출할 수 없습니다. 모든 사용자 작업에 대해 사이드바 버튼을 사용하고, 현재 상태를 표시하기 위해 `openWebview()`를 사용하세요. 상호작용 기능이 필요하면 사이드바 버튼에서 트리거하고 웹뷰를 다시 열어 표시를 새로 고치세요.

### 패턴: 터미널 명령 → 출력 파싱 → HTML�� 표시

가장 일반적인 플러그인 패턴입니다. 명령을 실행하고 결과를 파싱하여 시각적으로 표시합니다.
§§§CHUNK_SEPARATOR§§§
### 패턴: 자동 새로 고침이 있는 대시보드
§§§CHUNK_SEPARATOR§§§
### 패턴: 웹뷰에서 설정 표시하기

> **참고:** 웹뷰는 표시 전용입니다 — 플러그인 API를 호출할 수 없습니다. 설정을 수정하기 위해 사이드바 버튼 핸들러에서 `ctx.settings`를 사용하고, 현재 상태를 표시하기 위해 `openWebview()`를 사용하세요.
§§§CHUNK_SEPARATOR§§§
---

## 4부: 플러그인 게시하기

### 1단계: 로컬에서 테스트하기

1. 플러그인을 `~/.wia-soom/plugins/{your-plugin}/`에 복사합니다.
2. WIA SOOM을 재시작합니다.
3. 작동하는지 확인합니다: 사이드바 버튼이 나타나고, 기능이 올바르게 작동합니다.
4. 엣지 케이스를 테스트합니다: 터미널이 연결되지 않으면 어떻게 됩니까?

### 2단계: 제출 준비하기

플러그인 폴더에는 다음이 포함되어야 합니다:
```javascript
context.ui.showNotification('success', 'Backup completed!');
context.ui.showNotification('error', 'Connection failed — check SSH settings');
context.ui.showNotification('info', 'Scanning 254 servers...');
```
**필수 `package.json` 필드:**

| 필드 | 설명 | 예시 |
|-------|-------------|---------|
| `name` | 고유한 케밥 케이스 ID | `"my-awesome-plugin"` |
| `version` | 의미론적 버전 | `"1.0.0"` |
| `description` | 한 줄 설명 | `"Monitors nginx access logs in real-time"` |
| `author` | 당신의 이름 | `"John Doe"` |
| `main` | 진입점 | `"index.js"` |

**선택적 필드:**

| 필드 | 설명 |
|-------|-------------|
| `license` | 라이선스 유형 (MIT 추천) |
| `keywords` | 검색 태그 배열 |
| `soom.minVersion` | 필요한 최소 WIA SOOM 버전 |

### 3단계: 플러그인 레지스트리에 제출

1. **포크** [Plugin Store](https://wiasoom.com)
2. **추가** 당신의 플러그인을 `plugins/{your-plugin-name}/`에
3. **제출** 풀 리퀘스트

### 4단계: 검토 및 승인

우리는 모든 플러그인을 다음과 같은 기준으로 검토합니다:

- **보안** — 위험한 API 없음 (자세한 내용은 [보안 규��](#security-rules) 참조)
- **품질** — 작동하는가? 코드가 깔끔한가?
- **유용성** — 실제 문제를 해결하는가?

승인 후:
1. 당신의 플러그인이 `registry.json`에 추가됩니다.
2. `dist/`에 ZIP 번들이 생성됩니다.
3. 당신의 플러그인이 모든 WIA SOOM 사용자에게 **플러그인 스토어**에 나타납니다!

---

## 5부: 모범 사례

### 보안 규칙

이 규칙은 **필수**입니다. 이를 위반하는 플러그인은 거부됩니다.

| 규칙 | 이유 |
|------|-----|
| **절대** `eval()` 또는 `new Function()` 사용 금지 | 코드 주입 위험 |
| **절대** `child_process`, `exec()`, `spawn()` 사용 금지 | 명령어에는 오직 `ctx.terminal.send()`만 사용 |
| **절대** 외부 URL을 가져오지 마세요 | 예외: `wiasoom.com` API 엔드포인트 |
| **절대** `process.env`에 접근하지 마세요 | 환경 변수에는 비밀이 포함될 수 있음 |
| **절대** `require('fs')`를 직접 사용하지 마세요 | 저장소에는 `ctx.settings`를, 파일 전송에는 `ctx.sftp`를 사용 |
| **절대** npm 외부 패키지를 사용하지 마세요 | 순수 JavaScript만 사용 — node_modules 없음 |
| **반드시** 모든 원격 명령어에 `ctx.terminal.send()`를 사용하세요 | 이는 안전한 SSH 채널을 통해 전달됩니다 |
| **반드시** `deactivate()`에서 정리하세요 | 리스너 제거, 인터벌 클리어 |

### 오류 처리

항상 위험한 작업을 try/catch로 감싸세요:
```javascript
// Show server count in status bar
context.ui.addStatusBarItem('server-count', '3 servers connected');

// Update it later
context.ui.addStatusBarItem('server-count', '5 servers connected');
```
### deactivate()에서 정리하기

당신의 플러그인이 인터벌, 리스너 또는 구독을 생성하는 경우 — 이를 정리하세요:
```javascript
var theme = context.settings.get('theme');       // 'dark'
var count = context.settings.get('refreshRate'); // 30
var items = context.settings.get('todoList');    // [{ text: '...', done: false }, ...]
```
### i18n 지원

WIA SOOM은 254개 언어를 지원합니다. 당신의 플러그인 레이블을 번역 가능하게 만들기 위해 간단한 접근 방식을 사용하세요:
```javascript
context.settings.set('theme', 'dark');
context.settings.set('refreshRate', 30);
context.settings.set('todoList', [
  { text: 'Deploy v2', done: false },
  { text: 'Update DNS', done: true }
]);
```
---

## 6부: 실제 예시

### 예시 1: 서버 디스크 검사기

원격 서버에서 `df -h`를 실행하고 상태 표시줄에 사용 중/사용 가능한 공간을 표시합니다.
```javascript
exports.activate = function(context) {
  // Load saved preference (or default to 60 seconds)
  var interval = context.settings.get('interval') || 60;

  context.ui.addSidebarButton({
    icon: 'settings',
    label: 'Configure',
    onClick: function() {
      // Toggle between 30s and 60s
      interval = (interval === 60) ? 30 : 60;
      context.settings.set('interval', interval);
      context.ui.showNotification('info', 'Refresh interval: ' + interval + 's');
    }
  });
};
```
---

### 예시 2: TODO 관리자

설정을 사용하여 지속적인 저장소를 관리하고 웹뷰를 통해 표시하는 TODO 목록을 관리하는 플러그인입니다.

> **디자인 패턴:** 웹뷰가 플러그인 API를 직접 호출할 수 없기 때문에, 이 플러그인은 "스냅샷" 접근 방식을 사용합니다 — 설정에서 TODO를 읽고, 읽기 전용 HTML로 렌더링하며, 항목 추가를 위한 사이드바 기반 작업을 제공합니다. 웹뷰는 **디스플레이** 레이어이며, 상호작용 양식이 아닙니다.
```javascript
var response = await context.ai.chat([
  { role: 'user', content: 'Explain this error: ECONNREFUSED 127.0.0.1:3306' }
]);

// Once implemented, response will contain Soomy's AI answer
context.ui.showNotification('info', 'AI says: ' + response.response);
```
---

### 예시 3: 오류 감시기

터미널 출력을 모니터링하고 특정 패턴이 감지되면 알림을 보냅니다.
```javascript
exports.activate = function(context) {
  context.ui.addSidebarButton({
    icon: 'hard-drive',
    label: 'Disk Usage',
    onClick: function() {
      // Collect terminal output
      var output = '';
      var unsub = context.terminal.onOutput('current', function(data) {
        output += data;
      });

      // Send the command
      context.terminal.send('current', 'df -h --output=target,pcent,size,used,avail 2>/dev/null\n');

      // Wait for output, then show it
      setTimeout(function() {
        unsub(); // Stop listening

        // Parse the output into HTML
        var rows = output.split('\n')
          .filter(function(line) { return line.includes('%'); })
          .map(function(line) {
            var parts = line.trim().split(/\s+/);
            return '<tr><td>' + parts.join('</td><td>') + '</td></tr>';
          })
          .join('');

        context.ui.openWebview({
          title: 'Disk Usage',
          html: '<html><body style="font-family:monospace;background:#1a1a2e;color:#e2e8f0;padding:20px;">' +
            '<h2>Disk Usage</h2>' +
            '<table style="width:100%;border-collapse:collapse;">' +
            '<tr style="border-bottom:1px solid #333;"><th>Mount</th><th>Used%</th><th>Size</th><th>Used</th><th>Avail</th></tr>' +
            rows +
            '</table></body></html>'
        });
      }, 1500); // Give the command time to complete
    }
  });
};
```
---

## 부록: 카테고리 및 아이콘

### 플러그인 카테고리 (29)

`package.json`의 `keywords` 또는 레지스트리에 제출할 때 사용하세요:

| 카테고리 | 설명 |
|----------|-------------|
| `server` | 일반 서버 관리 |
| `devtools` | 개발 도구 |
| `calculator` | 계산기 및 변환기 |
| `simulator` | 시뮬레이터 |
| `game` | 터미널 게임 |
| `business` | 비즈니스 도구 |
| `security` | 보안 및 감사 |
| `web` | 웹 서버 관리 |
| `education` | 교육 도구 |
| `health` | 건강 관련 도구 |
| `islamic` | 이슬람 도구 (기도 시간 등) |
| `science` | 과학 도구 |
| `quantum` | 양자 컴퓨팅 도구 |
| `ai` | AI 기반 도구 |
| `biotech` | 생명공학 도구 |
| `space` | 우주 및 천문학 도구 |
| `network` | 네트워크 도구 |
| `database` | 데이터베이스 관리 |
| `monitoring` | 서버 모니터링 |
| `devops` | DevOps 및 CI/CD |
| `utility` | 일반 유틸리티 |
| `design` | 디자인 도구 |
| `ecommerce` | 전자상거래 도구 |
| `automation` | 자동화 도구 |
| `kpop` | K-pop 관련 도구 |
| `accessibility` | 접근성 도구 |
| `analytics` | 분석 및 보고 |
| `wia` | WIA 생태계 도구 |
| `all` | 모든 카테고리에 나타남 |

### 추천 아이콘 (Lucide)

| 아이콘 이름 | 용도 |
|-----------|---------|
| `server` | 서버 관리 |
| `shield` | 보안 |
| `database` | 데이터베이스 |
| `activity` | 모니터링 |
| `terminal` | 터미널 도구 |
| `code` | 개발 |
| `hard-drive` | 디스크/저장소 |
| `network` | 네트워킹 |
| `lock` | 인증/암호화 |
| `eye` | 감시/모니터링 |
| `check-square` | 작업/TODO |
| `layout-dashboard` | 대시보드 |
| `settings` | 설정 |
| `zap` | 자동화 |
| `globe` | 웹/국제 |

모든 1,500개 이상의 아이콘을 탐색하세요: [lucide.dev/icons](https://lucide.dev/icons)

---

## 도움이 필요하신가요?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **플러그인 문제:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **예제 플러그인:** [Website](https://wiasoom.com)
- **웹사이트:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>놀라운 무언가를 만들어보세요. 세상과 공유하세요.</em></p>
<p align="center"><em>— WIA SOOM 팀</em></p>
```javascript
exports.activate = function(context) {
  context.ui.addSidebarButton({
    icon: 'activity',
    label: 'Live Monitor',
    onClick: function() {
      context.ui.openWebview({
        title: 'Live Server Monitor',
        html: [
          '<html><head><style>',
          '  body { font-family: -apple-system, sans-serif; background: #0f172a; color: #e2e8f0; padding: 24px; }',
          '  .card { background: #1e293b; border-radius: 12px; padding: 20px; margin: 12px 0; }',
          '  .card h3 { margin: 0 0 8px 0; color: #38bdf8; }',
          '  .metric { font-size: 2rem; font-weight: bold; }',
          '  .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; }',
          '  .bar { height: 8px; background: #334155; border-radius: 4px; overflow: hidden; }',
          '  .bar-fill { height: 100%; background: linear-gradient(90deg, #22c55e, #eab308, #ef4444); transition: width 0.5s; }',
          '</style></head><body>',
          '  <h1>Server Monitor</h1>',
          '  <div class="grid">',
          '    <div class="card"><h3>CPU</h3><div class="metric" id="cpu">--</div><div class="bar"><div class="bar-fill" id="cpu-bar" style="width:0%"></div></div></div>',
          '    <div class="card"><h3>Memory</h3><div class="metric" id="mem">--</div><div class="bar"><div class="bar-fill" id="mem-bar" style="width:0%"></div></div></div>',
          '    <div class="card"><h3>Disk</h3><div class="metric" id="disk">--</div><div class="bar"><div class="bar-fill" id="disk-bar" style="width:0%"></div></div></div>',
          '    <div class="card"><h3>Uptime</h3><div class="metric" id="uptime">--</div></div>',
          '  </div>',
          '  <p style="color:#64748b;margin-top:20px;">Refreshes every 5 seconds</p>',
          '</body></html>'
        ].join('\n')
      });
    }
  });
};
```

### Pattern: Displaying Settings in a Webview

> **Note:** Webviews are display-only — they cannot call back into plugin APIs. Use `ctx.settings` in your sidebar button handlers to modify settings, and use `openWebview()` to show the current state.

```javascript
function showCurrentSettings(context) {
  var url = context.settings.get('webhookUrl') || '(not set)';
  var interval = context.settings.get('interval') || 60;

  context.ui.openWebview({
    title: 'Current Settings',
    html: [
      '<html><body style="font-family:sans-serif;background:#0f172a;color:#e2e8f0;padding:24px;">',
      '<h2>Current Settings</h2>',
      '<div style="background:#1e293b;padding:16px;border-radius:8px;margin:12px 0;">',
      '  <p><strong>Webhook URL:</strong> ' + url + '</p>',
      '  <p><strong>Refresh Interval:</strong> ' + interval + 's</p>',
      '</div>',
      '<p style="color:#475569;font-size:0.8rem;">Use sidebar buttons to change settings.</p>',
      '</body></html>'
    ].join('\n')
  });
}

// Toggle interval via sidebar button
context.ui.addSidebarButton({
  icon: 'settings',
  label: 'Toggle Interval',
  onClick: function() {
    var current = context.settings.get('interval') || 60;
    var next = (current === 60) ? 30 : 60;
    context.settings.set('interval', next);
    context.ui.showNotification('info', 'Interval set to ' + next + 's');
  }
});
```

---

## Part 4: Publishing Your Plugin

### Step 1: Test locally

1. Copy your plugin to `~/.wia-soom/plugins/{your-plugin}/`
2. Restart WIA SOOM
3. Verify it works: sidebar button appears, features work correctly
4. Test edge cases: what happens if no terminal is connected?

### Step 2: Prepare for submission

Your plugin folder must contain:

```
your-plugin/
├── package.json    ← manifest (required fields below)
└── index.js        ← must export activate(context)
```

**Required `package.json` fields:**

| Field | Description | Example |
|-------|-------------|---------|
| `name` | Unique kebab-case ID | `"my-awesome-plugin"` |
| `version` | Semantic version | `"1.0.0"` |
| `description` | One-line description | `"Monitors nginx access logs in real-time"` |
| `author` | Your name | `"John Doe"` |
| `main` | Entry point | `"index.js"` |

**Optional fields:**

| Field | Description |
|-------|-------------|
| `license` | License type (MIT recommended) |
| `keywords` | Array of search tags |
| `soom.minVersion` | Minimum WIA SOOM version required |

### Step 3: Submit to the Plugin Registry

1. ****Package** your plugin as a ZIP file
2. **Add** your plugin to `plugins/{your-plugin-name}/`
3. **Submit** a Pull Request

### Step 4: Review and approval

We review every plugin for:

- **Security** — no dangerous APIs (see [Security Rules](#security-rules))
- **Quality** — does it work? Is the code clean?
- **Usefulness** — does it solve a real problem?

After approval:
1. Your plugin is added to `registry.json`
2. A ZIP bundle is created in `dist/`
3. Your plugin appears in the **Plugin Store** for all WIA SOOM users!

---

## Part 5: Best Practices

### Security Rules

These rules are **mandatory**. Plugins that violate them will be rejected.

| Rule | Why |
|------|-----|
| **NEVER** use `eval()` or `new Function()` | Code injection risk |
| **NEVER** use `child_process`, `exec()`, `spawn()` | Only use `ctx.terminal.send()` for commands |
| **NEVER** fetch external URLs | Exception: `wiasoom.com` API endpoints |
| **NEVER** access `process.env` | Environment variables may contain secrets |
| **NEVER** use `require('fs')` directly | Use `ctx.settings` for storage, `ctx.sftp` for file transfer |
| **NEVER** use npm external packages | Pure JavaScript only — no node_modules |
| **MUST** use `ctx.terminal.send()` for all remote commands | This goes through the secure SSH channel |
| **MUST** clean up in `deactivate()` | Remove listeners, clear intervals |

### Error Handling

Always wrap risky operations in try/catch:

```javascript
exports.activate = function(context) {
  try {
    // Your plugin logic
    context.ui.addSidebarButton({
      icon: 'server',
      label: 'My Tool',
      onClick: function() {
        try {
          // risky operation
        } catch (err) {
          context.ui.showNotification('error', 'Something went wrong: ' + err.message);
        }
      }
    });
  } catch (err) {
    console.error('[my-plugin] Failed to activate:', err);
  }
};
```

### Cleanup in deactivate()

If your plugin creates intervals, listeners, or subscriptions — clean them up:

```javascript
var intervals = [];
var unsubscribers = [];

exports.activate = function(context) {
  // Save references to things you need to clean up
  var unsub = context.terminal.onOutput('session1', function(data) { /* ... */ });
  unsubscribers.push(unsub);

  var timer = setInterval(function() { /* ... */ }, 5000);
  intervals.push(timer);
};

exports.deactivate = function() {
  // Clean up everything
  intervals.forEach(function(timer) { clearInterval(timer); });
  intervals = [];
  unsubscribers.forEach(function(unsub) { unsub(); });
  unsubscribers = [];
};
```

### i18n Support

WIA SOOM supports 254 languages. To make your plugin label translatable, use a simple approach:

```javascript
// Simple multi-language labels
// 간단한 다국어 라벨
var LABELS = {
  en: { name: 'Disk Checker', scanning: 'Scanning disks...' },
  ko: { name: '디스크 체커', scanning: '디스크 스캔 중...' },
  ja: { name: 'ディスクチェッカー', scanning: 'ディスクスキャン中...' },
  // Add more languages as needed
};

function t(key) {
  // Try to detect language from localStorage (set by WIA SOOM language modal)
  var lang = 'en'; // default
  try { lang = localStorage.getItem('soom_language') || 'en'; } catch(e) {}
  return (LABELS[lang] && LABELS[lang][key]) || LABELS.en[key] || key;
}

exports.activate = function(context) {
  context.ui.addSidebarButton({
    icon: 'hard-drive',
    label: t('name'),
    onClick: function() {
      context.ui.showNotification('info', t('scanning'));
    }
  });
};
```

---

## Part 6: Real-World Examples

### Example 1: Server Disk Checker

Runs `df -h` on the remote server and shows used/available space in the status bar.

```javascript
'use strict';

/**
 * Server Disk Checker — WIA SOOM Plugin
 * 서버 디스크 용량 체커
 *
 * Shows disk usage in the status bar.
 * Alerts when any partition exceeds 90%.
 */

var checkInterval = null;
var unsubscribers = [];

exports.activate = function activate(context) {
  // Add sidebar button to trigger manual check
  // 사이드바 버튼: 수동 체크 트리거
  context.ui.addSidebarButton({
    icon: 'hard-drive',
    label: 'Disk Check',
    onClick: function() {
      checkDisk(context);
    }
  });

  // Auto-check every 5 minutes
  // 5분마다 자동 체크
  var interval = context.settings.get('interval') || 300;
  checkInterval = setInterval(function() {
    checkDisk(context);
  }, interval * 1000);
};

function checkDisk(context) {
  var output = '';

  // Listen for terminal output
  // 터미널 출력 수신
  var unsub = context.terminal.onOutput('current', function(data) {
    output += data;
  });
  unsubscribers.push(unsub);

  // Send the command
  // 명령 전송
  context.terminal.send('current', "df -h / | tail -1 | awk '{print $5}'\n");

  // Parse after delay
  // 잠시 후 파싱
  setTimeout(function() {
    unsub();

    // Extract percentage (e.g., "73%")
    // 퍼센트 추출 (예: "73%")
    var match = output.match(/(\d+)%/);
    if (match) {
      var percent = parseInt(match[1]);
      context.ui.addStatusBarItem('disk-usage', 'Disk: ' + percent + '%');

      // Alert if over 90%
      // 90% 초과 시 경고
      if (percent > 90) {
        context.ui.showNotification('error', 'WARNING: Disk usage at ' + percent + '%! Free up space.');
      }
    }
  }, 2000);
}

exports.deactivate = function deactivate() {
  if (checkInterval) {
    clearInterval(checkInterval);
    checkInterval = null;
  }
  unsubscribers.forEach(function(unsub) { unsub(); });
  unsubscribers = [];
};
```

---

### Example 2: TODO Manager

A plugin that manages a TODO list using settings for persistent storage and a webview for display.

> **Design pattern:** Since webviews cannot directly call plugin APIs, this plugin uses a "snapshot" approach — it reads TODOs from settings, renders them as read-only HTML, and provides sidebar-based actions for adding items. The webview is a **display** layer, not an interactive form.

```javascript
'use strict';

/**
 * TODO Manager — WIA SOOM Plugin
 * 할일 관리자
 *
 * Pattern: settings-driven display (no webview↔plugin bridge needed)
 * 패턴: settings 기반 표시 (웹뷰↔플러그인 브릿지 불필요)
 */

exports.activate = function activate(context) {
  // Show current TODO count in status bar
  // 현재 TODO 수를 상태바에 표시
  updateStatusBar(context);

  // Button 1: View TODO list
  // 버튼 1: TODO 목록 보기
  context.ui.addSidebarButton({
    id: 'todo-view',
    icon: 'check-square',
    label: 'TODO List',
    onClick: function() {
      showTodoList(context);
    }
  });

  // Button 2: Quick-add a TODO via notification prompt
  // 버튼 2: 알림 프롬프트로 빠르게 TODO 추가
  context.ui.addSidebarButton({
    id: 'todo-add',
    icon: 'plus-square',
    label: 'Add TODO',
    onClick: function() {
      // Use terminal echo as a quick input method
      // 터미널 echo를 빠른 입력 방법으로 사용
      var todos = context.settings.get('todos') || [];
      var newItem = 'Task #' + (todos.length + 1) + ' — ' + new Date().toLocaleString();
      todos.push({ text: newItem, done: false, createdAt: new Date().toISOString() });
      context.settings.set('todos', todos);
      updateStatusBar(context);
      context.ui.showNotification('success', 'Added: ' + newItem);
    }
  });

  // Button 3: Clear completed TODOs
  // 버튼 3: 완료된 TODO 정리
  context.ui.addSidebarButton({
    id: 'todo-clear',
    icon: 'trash-2',
    label: 'Clear Done',
    onClick: function() {
      var todos = context.settings.get('todos') || [];
      var before = todos.length;
      todos = todos.filter(function(t) { return !t.done; });
      context.settings.set('todos', todos);
      updateStatusBar(context);
      context.ui.showNotification('info', 'Cleared ' + (before - todos.length) + ' completed tasks');
    }
  });
};

function updateStatusBar(context) {
  var todos = context.settings.get('todos') || [];
  var remaining = todos.filter(function(t) { return !t.done; }).length;
  context.ui.addStatusBarItem('todo-count', 'TODO: ' + remaining + '/' + todos.length);
}

function showTodoList(context) {
  var todos = context.settings.get('todos') || [];
  var remaining = todos.filter(function(t) { return !t.done; }).length;

  // Build read-only HTML display
  // 읽기 전용 HTML 표시 생성
  var rows = todos.map(function(todo, i) {
    var status = todo.done ? '✅' : '⬜';
    var style = todo.done ? 'text-decoration:line-through;color:#64748b;' : 'color:#e2e8f0;';
    var date = todo.createdAt ? new Date(todo.createdAt).toLocaleDateString() : '';
    return '<div style="display:flex;align-items:center;gap:12px;padding:12px 16px;background:#1e293b;border-radius:8px;margin:6px 0;">' +
      '<span style="font-size:1.2em;">' + status + '</span>' +
      '<span style="flex:1;' + style + '">' + escapeHtml(todo.text) + '</span>' +
      '<span style="color:#475569;font-size:0.75rem;">' + date + '</span>' +
      '</div>';
  }).join('');

  if (todos.length === 0) {
    rows = '<div style="text-align:center;padding:40px;color:#475569;">No tasks yet. Click "Add TODO" in the sidebar.</div>';
  }

  context.ui.openWebview({
    title: 'TODO List (' + remaining + ' remaining)',
    html: [
      '<html><body style="font-family:-apple-system,sans-serif;background:#0f172a;color:#e2e8f0;padding:24px;">',
      '<h1 style="margin-bottom:4px;">TODO List</h1>',
      '<p style="color:#64748b;margin-bottom:20px;">' + remaining + ' of ' + todos.length + ' remaining</p>',
      rows,
      '<p style="color:#334155;margin-top:24px;font-size:0.8rem;">Use sidebar buttons to add/clear tasks, then reopen this view.</p>',
      '</body></html>'
    ].join('\n')
  });
}

function escapeHtml(str) {
  return String(str).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}

exports.deactivate = function deactivate() {};
```

---

### Example 3: Error Watcher

Monitors terminal output and sends a notification when specific patterns are detected.

```javascript
'use strict';

/**
 * Error Watcher — WIA SOOM Plugin
 * 에러 감시자
 *
 * Watches terminal output for error patterns.
 * Shows notification when errors are detected.
 * Configurable patterns via settings.
 */

var watchers = [];
var errorCount = 0;

// Default patterns to watch for
// 기본 감시 패턴
var DEFAULT_PATTERNS = [
  'FATAL',
  'CRITICAL',
  'OutOfMemory',
  'Segmentation fault',
  'kill -9',
  'No space left on device',
  'Connection refused',
  'Permission denied'
];

exports.activate = function activate(context) {
  // Load custom patterns or use defaults
  // 커스텀 패턴 로드 또는 기본값 사용
  var patterns = context.settings.get('patterns') || DEFAULT_PATTERNS;

  context.ui.addSidebarButton({
    icon: 'eye',
    label: 'Error Watcher',
    onClick: function() {
      context.ui.showNotification('info',
        'Watching for ' + patterns.length + ' error patterns. ' +
        errorCount + ' errors detected so far.'
      );
    }
  });

  // Update status bar
  // 상태바 업데이트
  context.ui.addStatusBarItem('error-watcher', 'Errors: ' + errorCount);

  // Watch current terminal
  // 현재 터미널 감시
  var unsub = context.terminal.onOutput('current', function(data) {
    for (var i = 0; i < patterns.length; i++) {
      if (data.includes(patterns[i])) {
        errorCount++;
        context.ui.addStatusBarItem('error-watcher', 'Errors: ' + errorCount);
        context.ui.showNotification('error',
          'Error detected: "' + patterns[i] + '" found in terminal output'
        );
        // Save error log
        // 에러 로그 저장
        var log = context.settings.get('errorLog') || [];
        log.push({
          pattern: patterns[i],
          time: new Date().toISOString(),
          snippet: data.substring(0, 200)
        });
        // Keep last 100 errors
        // 최근 100개만 유지
        if (log.length > 100) log = log.slice(-100);
        context.settings.set('errorLog', log);
        break; // One notification per output chunk
      }
    }
  });
  watchers.push(unsub);
};

exports.deactivate = function deactivate() {
  watchers.forEach(function(unsub) { unsub(); });
  watchers = [];
};
```

---

## Appendix: Categories & Icons

### Plugin Categories (29)

Use these in your `package.json` `keywords` or when submitting to the registry:

| Category | Description |
|----------|-------------|
| `server` | General server management |
| `devtools` | Development tools |
| `calculator` | Calculators and converters |
| `simulator` | Simulators |
| `game` | Terminal games |
| `business` | Business tools |
| `security` | Security and auditing |
| `web` | Web server management |
| `education` | Educational tools |
| `health` | Health-related tools |
| `islamic` | Islamic tools (prayer times, etc.) |
| `science` | Scientific tools |
| `quantum` | Quantum computing tools |
| `ai` | AI-powered tools |
| `biotech` | Biotechnology tools |
| `space` | Space and astronomy tools |
| `network` | Network tools |
| `database` | Database management |
| `monitoring` | Server monitoring |
| `devops` | DevOps and CI/CD |
| `utility` | General utilities |
| `design` | Design tools |
| `ecommerce` | E-commerce tools |
| `automation` | Automation tools |
| `kpop` | K-pop related tools |
| `accessibility` | Accessibility tools |
| `analytics` | Analytics and reporting |
| `wia` | WIA ecosystem tools |
| `all` | Appears in all categories |

### Recommended Icons (Lucide)

| Icon Name | Use for |
|-----------|---------|
| `server` | Server management |
| `shield` | Security |
| `database` | Database |
| `activity` | Monitoring |
| `terminal` | Terminal tools |
| `code` | Development |
| `hard-drive` | Disk/storage |
| `network` | Networking |
| `lock` | Auth/encryption |
| `eye` | Watching/monitoring |
| `check-square` | Tasks/TODO |
| `layout-dashboard` | Dashboards |
| `settings` | Configuration |
| `zap` | Automation |
| `globe` | Web/international |

Browse all 1,500+ icons: [lucide.dev/icons](https://lucide.dev/icons)

---

## Need Help?

- **GitHub Issues:** [wia-soom/issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Plugin Issues:** [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
- **Example Plugins:** [Website](https://wiasoom.com)
- **Website:** [wiasoom.com](https://wiasoom.com)

---

<p align="center"><em>Build something amazing. Share it with the world.</em></p>
<p align="center"><em>— The WIA SOOM Team</em></p>
