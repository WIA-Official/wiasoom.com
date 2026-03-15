# Changelog

## v0.52.0 (2026-03-16)

### Highlights
- **The World's First Multimodal AI Terminal** — category rebrand
- **AI Arena** — 2~4 agents compete simultaneously with per-slot control
- **254 languages** fully translated (including 55 indigenous/minority languages)
- **WIA Braille** — 7,000-language universal Braille (IPA-based)
- **YouTube Demo** — [Watch 58-second demo](https://youtu.be/Rv_bSDcHVJo)

### Features
- AI waveform animation on dashboard and Arena progress bar
- Terminal auto-follow with 1-second "New output" preview
- Per-slot quick reply buttons in Arena (individual agent control)
- Voice AI promotion card in Settings
- Plugin marketplace 5-column grid (517 plugins)
- Plan benefits dashboard with 56 features across 9 tiers
- Voice/Relay addon usage cards
- Language modal: English + native name 2-line display
- Command bar default hidden (Ctrl+` toggle)

### Security
- 10 CRITICAL fixes (hardcoded secrets, shell injection, nodeIntegration)
- 25 HIGH fixes (API key exposure, fail-open browsing, path traversal)
- Full security audit by 4 independent agents

### Bug Fixes
- Server card drag-and-drop timing and bounce fix
- Tab duplicate creating new session (force parameter)
- Welcome screen flash for logged-in users
- Arena session preservation on tab switch
- Terminal scroll restoration after tab switch
- Arena anti-paste chunked sending (prevents [Pasted text] freeze)

### i18n
- 254 languages: app.subtitle fully translated
- 55 manual translations for indigenous/minority languages
- Upload progress messages i18n
