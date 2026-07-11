# Logging System

SimpleEconomy provides two logging channels:

1. Transaction Logger (local DB)
2. Discord Webhook

---

## Transaction Logger

- File: `plugins/SimpleEconomy/transactions.db`
- Rotates when the size limit in `transaction-logger.file-size-limit-mb` is exceeded
- Table includes the `currency` column

```sql
CREATE TABLE IF NOT EXISTS transactions (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  timestamp INTEGER NOT NULL,
  player_uuid VARCHAR(36) NOT NULL,
  currency VARCHAR(64) NOT NULL DEFAULT 'money',
  type VARCHAR(32) NOT NULL,
  amount REAL NOT NULL,
  balance_before REAL NOT NULL,
  balance_after REAL NOT NULL,
  target_uuid VARCHAR(36) NOT NULL
);
```

Logged transaction types are the values of `TransactionTypes`:

- `PAY`
- `WITHDRAW`
- `DEPOSIT`
- `ADMIN_ADJUSTMENT`

!!! tip
    Use `/ehistory <player> [limit] [currency]` to fetch audit entries.

---

## Discord Webhook

- Enable: `settings.enable-discord-logging: true`
- Configure: `webhook-settings.*`
- The webhook URL must not contain `your_webhook_url`

Tracked log templates:

- `pay`
- `withdraw`
- `give`
- `set`
- `remove`

Embed templates live under `webhook-settings.embed-templates.*` and the footer text uses `webhook-settings.embed-templates.footer`.

Useful placeholders inside templates:

- `%sender%`
- `%receiver%`
- `%executor%`
- `%target%`
- `%player%`
- `%amount%`
- `%timestamp%` in the footer

!!! warning
    If the webhook URL is empty or a placeholder, no payloads are sent.

## Event Flow

1. Economy operations fire `PreTransactionEvent`.
2. If the event is not cancelled, the provider updates the balances.
3. The provider then fires `PostTransactionEvent`.
4. The transaction logger persists the action asynchronously.
