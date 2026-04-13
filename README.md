# Steam Manager

Desktop application for managing multiple Steam accounts. Windows only.

## Download

Download the latest installer from the [Releases](https://github.com/Soska512/Steam-Manager-Updates/releases/latest) page.

Run `SteamManager-Setup-<version>-x64.exe` and follow the installer steps.

## Features

**Account Management**
- Import accounts from txt / maFile format
- Automatic session management with re-login and warmup
- Per-account proxy support (HTTP / SOCKS5)
- Session health monitoring and auto-recovery
- Active hours scheduling (accounts "sleep" outside configured window)
- Batch operations across all accounts

**Inventory**
- View inventory across all accounts in one place
- Market price tracking with auto-refresh
- Filter by type, rarity, quality, tradability, marketability
- Sort by name, price, or other attributes

**Market — Sell**
- List items on Steam Community Market from any account
- Automatic currency conversion (USD price → account's wallet currency)
- Mobile confirmation handled automatically (no phone needed)
- Bulk listing with human-like delays between operations
- Recovery worker for stuck confirmations

**Market — Buy**
- Search Steam Market for items
- Place buy orders at specified price and quantity
- Automatic mobile confirmation for buy orders
- View and cancel active buy orders
- Instant fill when price matches existing sellers

**Trading**
- Send trade offers to any trade URL
- Counter-item support (two-sided trades)
- Automatic mobile confirmation for outgoing trades
- Auto-accept incoming trades from managed accounts
- Trade cooldown tracking and daily budget limits

**Ban Checker**
- Check VAC, game, community, and economy bans via Steam Web API
- Batch check across all accounts

**Updates**
- Built-in update checker (checks on startup + manual)
- Download and install updates from within the app

## System Requirements

- Windows 10 / 11 (x64)
- Internet connection
- ~200 MB disk space

## Data Storage

All data is stored locally in `%APPDATA%/steam-manager/`:
- `data/` — encrypted database, master key, settings
- `logs/` — application logs

Your Steam passwords and tokens are encrypted with AES-256-GCM. The encryption key is protected by Windows DPAPI and tied to your Windows user account.

Uninstalling the application does not delete your data. To remove everything, manually delete the `%APPDATA%/steam-manager/` folder.

## Troubleshooting

- **App won't start**: Check `%APPDATA%/steam-manager/logs/backend.log`
- **Accounts fail to login**: Session cookies may have expired — the app will auto-retry
- **Market operations fail**: Check that the account is online and market-enabled
- **Update check fails**: GitHub API rate limit (60 req/hour) — wait and retry

## Privacy

- No telemetry or analytics
- No data sent to third parties
- Update checks go to GitHub API only (this repository)
- All Steam communication goes directly to Steam servers
