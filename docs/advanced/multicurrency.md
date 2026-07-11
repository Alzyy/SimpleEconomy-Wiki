# Multicurrency

SimpleEconomy stores balances per `(uuid, currency)`. Every operation depends on a currency key.

## Currency Keys

- Default: `money`
- Custom: defined in `currencies.yml`
- Normalization: lowercase, spaces to underscore

!!! note
    Storage always uses the currency key. Keep keys stable to avoid orphaned data.

`VirtualCurrency` computes `columnOrKey` by lowercasing the currency name and replacing whitespace with underscores.

## Storage Layout

- SQLite: table `users` in `players.db`, primary key `(uuid, currency)`
- MySQL: table `<table-prefix>players`, primary key `(uuid, currency)`
- File: `player_data/<uuid>.yml` with `balances.<currency>` entries

## Effects on the API

All API calls include a currency key:

```java
provider.deposit(playerId, "gems", 100.0);
provider.getBalance(playerId, "gems").thenAccept(...);
```

## Effects on Commands

- `/pay` and `/eco` operate on `money`.
- Custom currencies expose their own command alias (currency name).
- `/currencies create|add` registers a new currency at runtime and persists it to `currencies.yml`.
- `/currencies symbol` updates the stored symbol for an existing currency.

## Runtime Currency Registration

The `CurrencyManager` loads every entry in `currencies.yml`, registers a command for that currency, and adds language translations for currency-specific messages.

When a new currency is added:

1. The currency is stored in memory.
2. The command is registered.
3. A dynamic command tree is created for the currency name.
4. Currency-specific language keys are copied into the language files.
