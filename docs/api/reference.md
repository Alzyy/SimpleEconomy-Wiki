# API Overview

## SimpleEconomyAPI

Central access point for the API.

- `SimpleEconomyAPI.setProvider(EconomyProvider)`
  Registers the provider (single-use).

- `SimpleEconomyAPI.getProvider()`
  Returns the registered provider.

- `SimpleEconomyAPI.isProviderSet()`
    Returns whether a provider has already been registered.

!!! warning
    `setProvider` can only be called once, otherwise an `IllegalStateException` is thrown.

---

## EconomyCore

`EconomyCore` is the contract exposed by the main plugin to external modules.

- `Logger getCoreLogger()`
- `File getCoreDataFolder()`
- `EconomyModule getModule(String name)`

Modules should depend on this interface instead of the concrete plugin class.

---

## EconomyProvider

All operations are asynchronous and return `CompletableFuture`.

### Key Methods

- `getBalance(UUID, String currency)`
- `setBalance(UUID, String currency, double amount)`
- `deposit(UUID, String currency, double amount)`
- `detract(UUID, String currency, double amount)` -> `CompletableFuture<Boolean>`
- `hasAccount(UUID)`
- `hasEnough(UUID, String currency, double amount)`
- `transfer(UUID from, UUID to, String currency, double amount)` -> `CompletableFuture<TransactionResult>`
- `getAvailableVirtualCurrencies()`

### Basic Usage

```java
UUID playerId = player.getUniqueId();
EconomyProvider provider = SimpleEconomyAPI.getProvider();

provider.getBalance(playerId, "money")
    .thenAccept(balance -> {
        // Only non-Bukkit logic here
        System.out.println("Balance: " + balance);
    })
    .exceptionally(ex -> {
        ex.printStackTrace();
        return null;
    });
```

### Return to Main Thread

```java
provider.getBalance(playerId, "money")
    .thenAccept(balance -> {
        Bukkit.getScheduler().runTask(plugin, () -> {
            player.sendMessage("Balance: " + balance);
        });
    });
```

---

## TransactionTypes

The transaction type enum uses these values and serialized identifiers:

- `PAY` -> `pay`
- `WITHDRAW` -> `withdraw`
- `DEPOSIT` -> `deposit`
- `ADMIN_ADJUSTMENT` -> `admin_adjustment`

---

## TransactionResult

- `SUCCESS`
- `INSUFFICIENT_FUNDS`
- `ERROR`

---

## EconomyModule

Modules implement the `EconomyModule` interface:

```java
void onEnable(EconomyCore core, File moduleFolder);
void onDisable();
String getName();
```

---

## Transaction Events

SimpleEconomy exposes two asynchronous Bukkit events in `it.alzy.simpleeconomy.api.events`.

### PreTransactionEvent

- Cancellable
- Fields: `sender`, `receiver`, `currency`, `amount`, `type`
- `setCancelReason(String)` stores the reason reported by `getCancelReason()`

### PostTransactionEvent

- Fired after the balance update completes
- Fields: `sender`, `receiver`, `currency`, `amount`, `type`

---

## Multicurrency in the API

Every call must include a currency key:

```java
provider.deposit(playerId, "gems", 100.0);
provider.getBalance(playerId, "gems").thenAccept(...);
```

!!! tip
    Store currency keys in config or use `getAvailableVirtualCurrencies()`.

---

## PlaceholderAPI

If PlaceholderAPI is present, the registered expansion identifier is `seco`.

Available placeholders:

- `%seco_balance_formatted%`
- `%seco_balance_normal%`
- `%seco_top_position%`
- `%seco_baltop_<position>%`
- `%seco_baltop_<position>_balance%`
- `%seco_currency_<currencyKey>%`

Examples:

- `%seco_currency_money%`
- `%seco_currency_gems%`
- `%seco_baltop_1%`
- `%seco_baltop_1_balance%`

!!! note
    The `baltop` placeholders refresh on a short cache interval and currently read from the `money` leaderboard.
