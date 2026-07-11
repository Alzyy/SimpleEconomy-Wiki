# Configuration

## Main Files

- `config.yml` for core settings
- `currencies.yml` for custom virtual currencies
- `languages/lang_*.yml` for translated messages

## config.yml

The current default configuration contains the following sections and keys.

### `settings`

| Key | Default | Purpose |
| --- | --- | --- |
| `locale` | `en` | Active language code for generated language files. |
| `starting-balance` | `1000` | Initial balance for new accounts. |
| `currency-symbol` | `$` | Symbol shown by format helpers. |
| `storage-system` | `SQLITE` | Storage backend: `SQLITE`, `FILE`, or `MYSQL`. |
| `auto-save-time` | `30` | Auto-save interval in minutes. |
| `max-thread-pool-size` | `2` | Thread pool size for async work. |
| `enable-vouchers` | `true` | Enables `/voucher` and `/withdraw`. |
| `baltop-refresh-interval` | `20` | Leaderboard refresh interval in minutes. |
| `baltop-limit` | `10` | Number of players shown per `/baltop` page. |
| `check-for-updates` | `true` | Enables startup update checks. |
| `update-check-interval` | `1440` | Update check interval in minutes. |
| `use-placeholderapi` | `true` | Registers the PlaceholderAPI expansion when PAPI is installed. |
| `enable-balance-formatting` | `true` | Enables short suffix formatting such as `k`, `M`, `B`, and `T`. |
| `max-transaction-amount` | `0` | Maximum amount for `/pay`; `0` disables the limit. |
| `enable-action-bar-messages` | `false` | Sends Vault deposit/withdraw action bar feedback to online players. |
| `enable-discord-logging` | `false` | Enables Discord webhook logging. |
| `enable-metrics` | `true` | Enables bStats metrics. |
| `decimal-places` | `2` | Decimal precision used by formatting and rounding helpers. |

### `storage`

| Key | Default | Purpose |
| --- | --- | --- |
| `enable-auto-purge` | `true` | Enables periodic cleanup of inactive accounts. |
| `auto-purge-days` | `30` | Inactivity threshold before purge removes an account. |

### `transaction-logger`

| Key | Default | Purpose |
| --- | --- | --- |
| `enable-logger` | `true` | Enables the local transaction history database. |
| `file-size-limit-mb` | `10` | Rotates `transactions.db` when the file exceeds this size. |

### `database`

| Key | Default | Purpose |
| --- | --- | --- |
| `host` | `localhost` | MySQL host name. |
| `username` | `root` | MySQL user name. |
| `password` | `CHANGEME` | MySQL password. |
| `database` | `simpleeconomy` | MySQL schema name. |
| `table-prefix` | `se_` | Prefix used when building the MySQL players table name. |
| `max-connection-pool` | `10` | Hikari pool size for MySQL. |
| `port` | `3306` | MySQL port. |

### `vouchers`

| Key | Default | Purpose |
| --- | --- | --- |
| `item` | `PAPER` | Material used for generated voucher items. |
| `name` | `&cCHECK &7(%playerName%)` | Voucher display name. |
| `max-check-amount` | `1000000` | Maximum voucher amount; `0` disables the cap. |
| `lore` | See file | Lore lines for voucher items. Supports `%amount%` and `%creationDate%`. |

### `format`

| Key | Default | Purpose |
| --- | --- | --- |
| `thousand` | `k` | Suffix used for thousands. |
| `million` | `M` | Suffix used for millions. |
| `billion` | `B` | Suffix used for billions. |
| `trillion` | `T` | Suffix used for trillions. |

### `webhook-settings`

| Key | Default | Purpose |
| --- | --- | --- |
| `url` | `https://discord.com/api/webhooks/your_webhook_url` | Discord webhook URL. |
| `username` | `SimpleEconomy Logger` | Webhook username. |
| `avatar-url` | `https://example.com/avatar.png` | Webhook avatar URL. |
| `log-payments` | `true` | Logs `/pay` transactions. |
| `log-withdrawals` | `true` | Logs withdrawals from Vault or vouchers. |
| `log-admin` | `true` | Logs admin balance changes. |
| `log-voucher-creations` | `true` | Logs voucher creation events. |
| `embed-templates.pay.title` | `💸 Player Payment` | Template title for payment logs. |
| `embed-templates.give.title` | `📈 Admin Action: Give` | Template title for admin give logs. |
| `embed-templates.set.title` | `⚙️ Admin Action: Set` | Template title for admin set logs. |
| `embed-templates.withdraw.title` | `🎫 Voucher Creation` | Template title for voucher logs. |
| `embed-templates.remove.title` | `📉 Admin Action: Remove` | Template title for admin remove logs. |
| `embed-templates.footer` | `SimpleEconomy Audit Log • %timestamp%` | Footer template used by all webhook embeds. |

### `interests`

| Key | Default | Purpose |
| --- | --- | --- |
| `enabled` | `false` | Enables periodic interest payouts for online players. |
| `rate` | `0.05` | Interest rate applied to the current balance. |
| `max-interest` | `2000` | Maximum interest payout per cycle. |
| `interval` | `60` | Interest interval in minutes. |
| `min-balance` | `100` | Minimum balance required to earn interest. |

!!! tip
    `max-transaction-amount: 0` disables the limit. The `locale` value maps to `languages/lang_<locale>.yml`.

## currencies.yml

The default file is empty. Add currencies like this:

```yaml
gems:
  name: "Gems"
  symbol: "<>"
tokens:
  name: "Tokens"
  symbol: "T"
```

!!! note
    The YAML key is the currency key used in storage and commands. The implementation normalizes names to lowercase and stores them using a lowercased key.

## Language Files

Default language files are created under `languages/` as `lang_en.yml` and `lang_it.yml`.

The `LanguageManager` keeps language keys in sync on load and also auto-copies generic currency messages into `messages.currencies.<currencyName>.*` when a new currency is registered.
