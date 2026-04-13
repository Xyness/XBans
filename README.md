# XBans

Advanced sanctions, security, and AI-powered moderation addon for XCore.

## Features

### Sanctions
- **Ban** / **IP Ban** / **IP Range Ban** -- with optional duration, silent flag, server targeting, templates
- **Mute** / **IP Mute** -- blocks chat and configurable commands (`/msg`, `/r`, `/tell`, `/w`, `/me`)
- **Warn** -- with configurable expiration and auto-escalation at thresholds
- **Kick** -- immediate expulsion with reason display
- **Jail** -- teleport to a predefined jail location with configurable restrictions (movement radius, chat, commands, interact, PvP)
- **Report** -- with AI classification, chat evidence extraction, and GUI resolution workflow
- **Templates** -- shortcuts for common sanctions (e.g. `/ban player @cheat`)
- **Shadow ban** -- banned player can join but only sees their own messages

### Jail System
- **Multi-jail** -- create named jail locations at any position (`/jail-create <name>`)
- **Configurable restrictions** -- movement radius, chat, commands (with whitelist), interaction, item drop, PvP
- **Auto-expiration** -- configurable default duration, auto-release when time is up
- **Previous location restore** -- player teleported back to their original position on unjail
- **Join enforcement** -- jailed players are teleported to jail on login
- **Cross-server sync** -- jail status propagates across all servers
- **GUI** -- `/jaillist` shows all jailed players with unjail/profile buttons

### AI Moderation (Bayesian Classifier)
- **Pre-trained out of the box** -- works immediately in EN, FR, ES, PT, DE
- **Self-learning** -- improves from confirmed/dismissed reports and moderator actions
- **Real-time chat analysis** -- classifies messages (toxicity, spam, scam) with escalating actions
- **Report classification** -- categorizes reports with confidence percentage
- **Chat evidence** -- analyzes target's recent messages when a report comes in, shows flagged ones to staff
- **Cross-server sync** -- training data synchronized via XCore SyncManager

### IP Security
- **VPN/Proxy blocking** -- detects VPNs, proxies, Tor
- **Datacenter/Hosting blocking** -- blocks non-residential IPs
- **Mobile network blocking** -- blocks 4G/5G connections
- **ISP blocking** -- block specific providers by name or AS number
- **CIDR range blocking** -- supports `/notation` and wildcards (`192.168.1.*`)
- **GeoIP** -- country whitelist/blacklist (uses ip-api.com, no API key)

### Client Security
- **Client brand detection** -- identify and log player client types
- **Brand blocking** -- block specific modded clients (wurst, meteor, etc.)
- **Whitelist mode** -- only allow specific clients
- **Protocol version control** -- enforce min/max Minecraft protocol versions

### Alt-Account Detection (10-factor scoring)
- Shared IP, multiple shared IPs, same client brand, same protocol version
- Same country/ISP, login time proximity, name similarity (Levenshtein)
- Fingerprint matching, sanction history correlation
- Risk levels: LOW / MEDIUM / HIGH / CRITICAL
- Configurable auto-action on score threshold (kick or ban)

### Moderation Tools
- **Freeze** (`/freeze`) -- immobilize a player during investigation, configurable action on disconnect
- **Watchlist** (`/watchlist`) -- monitor players, Warden notifies on join
- **Staff Notes** (`/note`) -- private notes on players, visible in profile GUI
- **Warden** (`/warden`) -- real-time notification system for staff (sanctions, reports, AI alerts, alt detection)

### GUIs (14 screens, fully YAML-configurable)
- **Ban/Mute/Warn/Report/Jail lists** -- paginated with left-click to remove, right-click for profile
- **Players** -- browse all tracked players with sanction status indicators
- **Profile** -- player head, all sanctions, alt accounts, notes, watchlist, 5 history sub-GUIs (ban, mute, warn, report, jail)
- **Report resolution** -- left-click to resolve (trains AI), shift+left to dismiss, right-click for profile
- **Duplicate accounts** -- players sharing IPs
- **Notes** -- view and delete staff notes

### Integrations
- **Discord** -- webhook notifications for sanctions (configurable per action type)
- **Web Dashboard** -- REST API with stats, sanctions, audit log (search/filter), unban/unmute endpoints
- **Cross-server sync** -- sanctions and AI training propagate instantly via XCore
- **Audit log** -- complete record of all moderation actions with moderator attribution

## Commands

### Sanctions

| Command | Permission |
|---------|-----------|
| `/ban [-s] [-server <name>] <player> [time] [reason]` | `xbans.ban` |
| `/unban [-s] <player>` | `xbans.unban` |
| `/ban-ip [-s] <player> [time] [reason]` | `xbans.banip` |
| `/unban-ip [-s] <player>` | `xbans.unbanip` |
| `/ban-ip-range [-s] <range> [reason]` | `xbans.banip` |
| `/mute [-s] <player> [time] [reason]` | `xbans.mute` |
| `/unmute [-s] <player>` | `xbans.unmute` |
| `/mute-ip [-s] <player> [time] [reason]` | `xbans.muteip` |
| `/unmute-ip [-s] <player>` | `xbans.unmuteip` |
| `/warn [-s] <player> [reason]` | `xbans.warn` |
| `/unwarn [-s] <player> [id]` | `xbans.unwarn` |
| `/kick [-s] <player> [reason]` | `xbans.kick` |
| `/jail [-s] <player> [jail] [time] [reason]` | `xbans.jail` |
| `/unjail [-s] <player>` | `xbans.unjail` |
| `/report <player> <reason>` | `xbans.report` |
| `/unreport <player> [id]` | `xbans.unreport` |
| `/unban-all` | `xbans.unban-all` |
| `/unmute-all` | `xbans.unmute-all` |

### Jail Management

| Command | Permission |
|---------|-----------|
| `/jail-create <name>` | `xbans.jail.manage` |
| `/jail-delete <name>` | `xbans.jail.manage` |
| `/jail-locations` | `xbans.jail` |
| `/jaillist` | `xbans.jail` |

### Moderation

| Command | Permission |
|---------|-----------|
| `/freeze <player>` | `xbans.freeze` |
| `/watchlist <add\|remove\|list> [player] [reason]` | `xbans.watchlist` |
| `/note <add\|list\|delete> <player\|id> [note]` | `xbans.note` |
| `/warden` | `xbans.warden` |

### Information

| Command | Permission |
|---------|-----------|
| `/profil <player>` | `xbans.profil` |
| `/whois <player\|ip>` | `xbans.whois` |
| `/banlist` | `xbans.banlist` |
| `/baniplist` | `xbans.baniplist` |
| `/mutelist` | `xbans.mutelist` |
| `/muteiplist` | `xbans.muteiplist` |
| `/warnlist` | `xbans.warnlist` |
| `/reportlist` | `xbans.reportlist` |
| `/players` | `xbans.players` |

### Admin

| Command | Permission |
|---------|-----------|
| `/xbans reload` | `xbans.admin` |
| `/xbans stats` | `xbans.admin` |
| `/xbans export <bans\|mutes\|warns\|reports\|all>` | `xbans.admin` |
| `/xbans import <file>` | `xbans.admin` |

## Permissions

| Permission | Description |
|-----------|-------------|
| `xbans.ban` / `xbans.unban` | Ban/unban players |
| `xbans.banip` / `xbans.unbanip` | IP ban/unban |
| `xbans.mute` / `xbans.unmute` | Mute/unmute |
| `xbans.muteip` / `xbans.unmuteip` | IP mute/unmute |
| `xbans.warn` / `xbans.unwarn` | Warn/unwarn |
| `xbans.kick` | Kick |
| `xbans.jail` / `xbans.unjail` | Jail/unjail players |
| `xbans.jail.manage` | Create/delete jail locations |
| `xbans.report` / `xbans.unreport` | Report/unreport |
| `xbans.freeze` | Freeze players |
| `xbans.watchlist` | Manage watchlist |
| `xbans.note` | Manage staff notes |
| `xbans.warden` | Receive warden notifications |
| `xbans.banlist` / `xbans.mutelist` / `xbans.warnlist` / `xbans.reportlist` | View sanction GUIs |
| `xbans.baniplist` / `xbans.muteiplist` | View IP sanction GUIs |
| `xbans.players` | Browse players GUI |
| `xbans.profil` | View player profile |
| `xbans.whois` | Whois lookup |
| `xbans.admin` | Admin commands (reload, stats, export, import) |
| `xbans.unban-all` / `xbans.unmute-all` | Bulk removal |
| `xbans.exempt.ban` / `.mute` / `.kick` / `.warn` / `.report` / `.jail` | Exempt from sanctions |
| `xbans.bypass.vpn` / `.hosting` / `.mobile` | Bypass IP security checks |
| `xbans.bypass.chatai` | Bypass AI chat moderation |
| `xbans.notify.warn` / `.report` | Receive broadcast notifications |

## Configuration

Key sections in `config.yml`:

| Section | Description |
|---------|-------------|
| `cooldowns` | Chat and command cooldowns |
| `warn-duration` / `report-duration` | Default sanction durations |
| `require-reason` | Force moderators to provide reasons |
| `templates` | Sanction shortcuts (e.g. `@cheat`) |
| `reasons` / `durations` | Tab-complete suggestions |
| `auto-sanctions` | Warning threshold escalation (e.g. 3 warns = mute) |
| `reason-escalation` | Regex-based auto-escalation by reason |
| `auto-report-escalation` | Auto-action at report count thresholds |
| `jail` | Jail system (enabled, default-duration, move-radius, restrictions, allowed-commands) |
| `shared-ip-detection` | Shared IP detection and auto-ban |
| `broadcast-permissions` | Control who sees sanction broadcasts |
| `freeze` | Reminder interval and disconnect action |
| `discord` | Webhook URL and notification types |
| `geoip` | Country whitelist/blacklist |
| `vpn-blocking` / `hosting-blocking` / `mobile-blocking` | IP security |
| `isp-blocking` / `ip-range-blocking` | ISP and CIDR blocking |
| `client-security` | Client brand/version control |
| `alt-detection` | Risk scoring weights, thresholds, auto-actions |
| `ai-classifier` | Bayesian classifier settings |
| `chat-ai` | Real-time chat moderation with escalating actions |
| `exempt-permissions` | Bypass permissions per sanction type |
| `auto-purge-days` | Auto-delete old inactive sanctions |
| `anonymous-moderator` | Hide moderator name in broadcasts |
| `appeal-url` | URL shown on ban kick screen |

## Web API

Base path: `/api/xbans` (requires XCore web dashboard)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/stats` | GET | Sanction counts |
| `/bans` | GET | Active bans |
| `/ip-bans` | GET | Active IP bans |
| `/mutes` | GET | Active mutes |
| `/ip-mutes` | GET | Active IP mutes |
| `/warns` | GET | Active warnings |
| `/reports` | GET | Active reports |
| `/audit` | GET | Audit log (paginated, searchable) |
| `/unban` | POST | Remove a ban |
| `/unmute` | POST | Remove a mute |

## Database

| Table | Description |
|-------|-------------|
| `xbans_banlist` | Player bans |
| `xbans_ipbanlist` | IP bans |
| `xbans_mutelist` | Player mutes |
| `xbans_ipmutelist` | IP mutes |
| `xbans_warnlist` | Warnings |
| `xbans_reportlist` | Reports |
| `xbans_jaillist` | Jailed players |
| `xbans_jail_locations` | Named jail locations |
| `audit_log` | Moderation action history |
| `xbans_ai_training` / `xbans_ai_model` | AI classifier data |
| `xbans_alt_detection` | Alt-account fingerprints |
| `xbans_watchlist` | Watched players |
| `xbans_notes` | Staff notes |
