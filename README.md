# Steam Manager

Desktop application for managing multiple Steam accounts. Windows only.

## Download

Download the latest installer from the [Releases](https://github.com/Soska512/Steam-Manager-Updates/releases/latest) page.

Run `SteamManager-Setup-<version>-x64.exe` and follow the installer.

---

## Account Management

- **Import accounts** from `login:password` text format or SDA maFiles (including encrypted maFiles with password)
- **Automatic session management** — login, re-login on session expiry, cookie persistence across restarts
- **Session health monitoring** — periodic probes detect expired sessions and trigger automatic re-login with exponential backoff cooldown
- **Account status tracking** — online, offline, error, need_mafile, session_expired, quarantined (banned)
- **Capability badges** — visual indicators for trade/list/confirm/sync readiness per account
- **Attention levels** — critical/warn/none based on relogin failures, bans, session state
- **Batch ID grouping** — assign batch labels to accounts (e.g. "batch-001") for organization
- **Per-account notes** — free-text annotations visible in sidebar

## Proxy Support

- **Per-account proxy assignment** — each account can use its own proxy
- **HTTP and SOCKS5** — both protocols supported with auto-detection
- **Bulk proxy assignment** — paste a list of proxies, auto-distribute across accounts
- **Proxy health monitoring** — probes detect dead proxies (unknown → suspect → dead lifecycle)
- **Per-proxy rate limiting** — accounts on the same proxy share a queue; different proxies get independent queues
- **Round-robin for market pricing** — distributes price-check requests across all available proxies

## Anti-Detection & Fingerprinting

- **Per-account browser fingerprint** — generated on first login, persistent across sessions
- **User-Agent rotation** — Chrome 120-131 with realistic full version strings
- **OS-matched headers** — `Sec-CH-UA`, `Sec-CH-UA-Platform`, `Sec-CH-UA-Mobile` consistent with User-Agent OS
- **Regional language matching** — `Accept-Language` and Steam language cookie match proxy region (RU, UA, EU, US variants)
- **Timezone matching** — fingerprint timezone offset matches proxy region geography
- **Self-consistency guarantee** — all fingerprint fields are guaranteed to match each other
- **Custom User-Agent per account** — optionally override with a specific UA string

## Active Hours & Scheduling

- **Per-account active window** — configurable start/end hours (e.g. 9:00–23:00)
- **Timezone-aware** — hours calculated using account's fingerprint timezone, not system clock
- **Background operations respect sleep** — sync, health probes, auto-accept trades pause outside active hours
- **Manual operations bypass sleep** — UI-triggered login, sell, trade, confirm always go through
- **Wrap-around support** — works for night-shift windows like 22:00–06:00

## Daily Budget & Rate Limiting

- **Trades per day limit** — max 5 outgoing trades per account per calendar day
- **Market sells per day limit** — max 50 sell operations per account per calendar day
- **Calendar day uses account timezone** — not UTC, matching regional fingerprint
- **Trade cooldown per receiver** — 30-60 minute random cooldown between trades to the same partner
- **Persisted across restarts** — budget counters stored in database
- **Human-like timing** — configurable delays between all operations (listing: 3-8s, confirmation: 15-40s, batch: 10-45s)

## Inventory

- **Unified view** — browse inventory across all accounts in one grid
- **Manual and batch refresh** — refresh single account or all at once
- **Auto-refresh after trade** — inventory updates automatically when incoming trade is accepted
- **Market price tracking** — fetch current lowest prices from Steam Market with 15-min cache
- **Proxy-distributed pricing** — price requests spread across proxy pool via round-robin
- **Filters** — search by name, filter by type/rarity/quality/tradable/marketable
- **Sorting** — by name, price, type (ascending/descending)
- **Pagination** — configurable page size

## Market — Sell

- **List items on Steam Community Market** from any managed account
- **Automatic currency conversion** — set price in USD, backend converts to account's wallet currency using live exchange rates
- **Fee calculation** — Steam 5% + game fee 10%, minimum 1 cent each, computed automatically
- **Pre-flight browsing** — simulates viewing item's market page before listing (anti-detection)
- **Automatic mobile confirmation** — no phone needed, confirmations handled programmatically
- **Confirmation recovery** — stuck confirmations retried by background worker every 5 minutes
- **Bulk listing** — sell multiple items with human-like delays between each
- **Market restriction detection** — auto-detects "unable to use Community Market" and updates account status

## Market — Buy

- **Search Steam Market** — find items by name with icons and current prices
- **Place buy orders** — specify max price and quantity; instant fill if sellers exist at or below your price
- **Automatic mobile confirmation** — 3-step flow (create → confirm → re-submit) handled automatically
- **View active buy orders** — see pending orders with price, quantity, and item icon
- **Cancel buy orders** — one-click cancellation
- **Retry on transient errors** — automatic retry (up to 3 attempts) on Steam server hiccups

## Trading

- **Send trade offers** — to any Steam trade URL
- **Counter-item support** — add items from receiver's side for two-sided (masked) trades
- **Automatic mobile confirmation** — outgoing trades confirmed programmatically with retries
- **Auto-accept incoming trades** — trades from other managed accounts accepted automatically during active hours
- **Safety checks** — only auto-accepts one-sided incoming (receiver gives nothing), from known accounts, created within 2 hours
- **Trade cooldown tracking** — per-partner cooldown prevents rapid repeated trades
- **Daily budget enforcement** — max 5 trades per day per account
- **Inventory auto-refresh** — receiver's inventory updates after accepting incoming trade

## Session Warmup

- **Multi-phase warmup** — simulates natural browsing after login:
  1. Idle pause (30-90s)
  2. Load Steam community home page
  3. Pause (10-25s)
  4. Load profile page
  5. Pause (5-15s)
  6. Check market eligibility
- **Market restriction detection** — parses market page HTML for restriction banners
- **Non-blocking** — runs in background, doesn't delay other operations

## Ban Checker

- **Check bans via Steam Web API** — VAC, game, community, and economy bans
- **Batch checking** — up to 100 accounts per API call
- **Tracked fields** — VAC ban count, game ban count, days since last ban, economy ban status
- **Auto-quarantine** — accounts with detected bans automatically quarantined with reason
- **Requires Steam Web API key** — configurable in Settings

## Settings & About

- **Steam Web API key management** — input, test, mask, clear
- **About section** — shows current version, platform, data/logs directory paths
- **Open folders** — one-click buttons to open data and logs folders in file explorer
- **Check for updates** — manual check from Settings, or automatic check 60s after startup

## Updates

- **Built-in update checker** — queries GitHub Releases API
- **Auto-check on startup** — silent check 60 seconds after launch
- **Update available badge** — green indicator in header when new version found
- **Changelog display** — shows release notes from GitHub
- **Download with progress** — progress bar, cancel button, retry on failure
- **Install and restart** — downloads installer, launches it, quits app; NSIS replaces files and auto-relaunches

## Encryption & Security

- **AES-256-GCM encryption** — passwords, secrets, tokens encrypted at rest
- **Windows DPAPI protection** — master encryption key stored in DPAPI-protected blob, tied to Windows user account
- **No plaintext secrets on disk** — master key never written to settings or logs
- **Context isolation** — Electron renderer has no access to Node.js or filesystem
- **Whitelisted IPC** — renderer can only request pre-defined operations (open folder, install update)
- **No telemetry** — no data sent to third parties; update checks go to GitHub API only

## Audit Log

- **Comprehensive action tracking** — login, trade, sell, buy, sync, import, ban check, warmup, errors
- **Structured details** — JSON payload per action with relevant IDs, prices, counts, errors
- **Severity levels** — info, warn, error
- **Source categories** — auth, sync, market, trade, health, import
- **Enriched context** — batch_id and proxy_region auto-attached from account
- **Queryable** — filter by account, action type, severity, source, with pagination

## Data Storage

All data is stored locally in `%APPDATA%/steam-manager/`:

| Folder/File | Contents |
|---|---|
| `data/steam-manager.db` | SQLite database (accounts, inventory, trades, listings, audit log) |
| `data/master-key.bin` | DPAPI-encrypted master encryption key |
| `data/settings.json` | User settings (Steam API key) |
| `logs/backend.log` | Application logs |
| `updates/` | Downloaded installer (temporary, cleaned on next update) |

Uninstalling the application does **not** delete your data. To remove everything, manually delete `%APPDATA%/steam-manager/`.

## System Requirements

- Windows 10 / 11 (x64)
- Internet connection
- ~200 MB disk space

## Troubleshooting

| Problem | Solution |
|---|---|
| App won't start | Check `%APPDATA%/steam-manager/logs/backend.log` |
| Accounts fail to login | Cookies expired — app auto-retries; check proxy health |
| Market operations fail | Verify account is online and market-enabled (no restrictions) |
| Update check returns error | GitHub API rate limit (60 req/hour unauthenticated) — wait and retry |
| "Master key unrecoverable" on startup | Windows profile changed or file copied from another machine — see dialog options |
