# Changelog

All notable changes to WIA SOOM are documented here.

---

## v1.0.0 — 2026-03-16

The first public release. 411 commits, 80,000+ lines of code, built in 7 days.

### New Features

#### AI Arena — Multi-Agent Battle
- 2–4 AIs solve the same problem simultaneously on isolated Git branches
- Cross-review: each AI scores every other AI's solution
- AI wave progress bar animation during processing
- Quick reply input with auto-focus
- Per-slot reply and session preserve across tab switches
- Scroll position restore after tab switch

#### AI Agent
- 14 tools: read/write/edit files, execute commands, git, grep, process management
- 3 providers: GPT-4o, Gemini, Claude — with streaming responses
- Soomy independent agent with session memory
- AI Council: multiple AIs discuss, then synthesize the best answer

#### Multimodal Upload (World's First in CLI)
- **Ctrl+Shift+V** — paste screenshots directly into SSH terminal
- Drag & drop images, files, videos into terminal
- Video keyframe extraction via ffmpeg
- All 3 AI providers support vision analysis

#### Soom Shield (Secret Redaction)
- Automatic detection and masking of API keys, passwords, tokens, private keys
- Secrets masked before sending to AI and in shared sessions
- Per-category toggles, enabled by default

#### Soom Summon (Quake Mode)
- `Ctrl+\`` to summon terminal from any screen edge
- 4-direction support, multi-monitor aware, configurable size and opacity

#### Soom Block (Block Output)
- Terminal output grouped into blocks per command
- Each block shows execution time, exit code, status
- Copy output, filter within blocks, send to AI for analysis

#### Soom Share (Block Permalink)
- Share any terminal block as a permalink
- Configurable expiry, optional password, view count limits, automatic secret redaction

#### Soom Drive
- Cloud file sync and management

#### Soom Guardian
- Server health monitoring and auto-recovery

#### Local Terminal
- Native local terminal support (no SSH required)

#### Serial Terminal
- Serial port connection support for IoT/embedded devices

#### Voice Interface
- Speak commands, hear responses
- Voice Settings UI with STT/TTS configuration

#### Code Editor
- Monaco editor with AI-powered completion
- Bug detection and one-click deployment to server

#### Infrastructure Intelligence
- Topology Map — visualize servers and connections, auto-discovery via SSH
- Real-time Observability — CPU, memory, disk, network sparkline graphs + AI prediction
- AI Log Analysis — real-time streaming, anomaly detection, cross-server correlation

#### Security
- Security Command Center — 7-area scan, security score, one-click fix
- Command security gate — real-time risk analysis
- SQL injection, XSS, CSP hardening

#### DevOps Tools
- Docker Manager — full Docker management via SSH, zero agent
- Runbook Automation — AI-powered, natural language creation, auto-rollback
- Cron & Env Manager — visual crontab editor, AI natural language to cron
- API Client — built-in REST/GraphQL/WebSocket client
- Status Page monitoring

#### Plugin System
- 517 plugins in the Plugin Store
- Plugin marketplace with one-click install
- Plugin Developer Guide with full API reference
- Built-in plugin creator (Settings → Plugins → Create New)

#### Collaboration
- Multiplayer terminal — real-time session sharing
- Team Workspace with RBAC and audit logs

#### Accessibility & Internationalization
- 254 languages — every language, every script, RTL support
- WIA Braille — 7,000-language universal Braille (IPA-based)
- Screen reader (ARIA), high contrast mode, keyboard navigation
- Full font size adjustment

#### Productivity
- Floating command bar with keyboard shortcut
- Dashboard with drag-and-drop server cards
- Tab management with duplicate naming fix
- Split pane with swap button
- Terminal auto-follow (smooth output tracking)
- Markdown Runner — execute markdown code blocks directly

#### Pricing
- Free tier with 33 trials for all premium features
- BYOK (Bring Your Own Key) — unlimited AI with your own API keys
- Subscription plans from $3/mo

### Platform Support
- Windows (.exe, .portable)
- macOS (Apple Silicon, Intel) (.dmg)
- Linux (.AppImage, .deb)

---

<p align="center"><em>"Terminal" means the end of the line. "SOOM(숨)" is Korean for "breath" — the first breath, a new beginning.</em></p>
