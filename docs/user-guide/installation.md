# Installation

## Requirements

- Spigot or Paper 1.21+
- Vault (required)
- PlaceholderAPI (optional)
- Java 21

!!! note
    The plugin declares `api-version: 1.21` and depends on Vault at startup. Paper is recommended, but the plugin falls back to Bukkit-safe messaging when Paper-only adventure audiences are unavailable.

## Install Steps

1. Place the plugin JAR in `plugins/`.
2. Start the server once so SimpleEconomy generates its data folder and default resources.
3. Edit `plugins/SimpleEconomy/config.yml`.
4. Optionally edit `plugins/SimpleEconomy/currencies.yml` to define custom currencies. Or use the in game command.
5. Edit `plugins/SimpleEconomy/languages/lang_en.yml` or `lang_it.yml` if you want to customize messages.
6. Restart the server or run `/se reload` after changing configuration files.

!!! warning
    If you select MySQL, change `database.password` from `CHANGEME` before starting.

!!! note
    Default language files are created under `plugins/SimpleEconomy/languages/` as `lang_en.yml` and `lang_it.yml`.

## Generated Files and Folders

- `plugins/SimpleEconomy/config.yml`
- `plugins/SimpleEconomy/currencies.yml`
- `plugins/SimpleEconomy/languages/lang_en.yml`
- `plugins/SimpleEconomy/languages/lang_it.yml`
- `plugins/SimpleEconomy/transactions.db` when transaction logging is enabled
- `plugins/SimpleEconomy/player_data/` when `storage-system` is set to `FILE`
- `plugins/SimpleEconomy/modules/` for external module JARs
