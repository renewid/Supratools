# SupraTools Telegram Bot

Telegram bot for prices, market data, and tools around the Supra ecosystem (SUPRA and select Supra-native coins).

> This directory contains documentation only (README + Changelog). The bot's source code is not made public.

## Language

The bot is fully bilingual (German/English), toggleable via `/lang`. In groups the setting applies to everyone and can only be changed by admins.

## Commands

| Command | Description |
|---------|--------------|
| `/sprice` or `/sprice <symbol\|name\|address>` (alias: `/preis`) | Show SUPRA or altcoin price. For tracked coins includes chart, market cap, and 1h/24h/7d change; for other Atmos-tradeable coins, a live price via Atmos lookup |
| `/address <symbol\|name>` | Contract address & Suprascan link for a coin |
| `/sns <name.supra \| 0x-address>` | Supra Name Service resolution: name → address and address → name |
| `/alert <price>` or `/alert <symbol> <price>` | Set a price alert |
| `/compare <coin1> <coin2>` | Market cap comparison of two coins, including coin1's extrapolated price based on coin2's market cap. If a coin isn't listed on Atmos, price/market cap is looked up via CoinGecko |
| `/currency USD\|EUR` | Change the display currency for prices & chart (group admins only) |
| `/dapps` | Show dApps live on Supra |
| `/lang` | Change language (DE/EN) (group admins only) |
| `/settopic` or `/settopic <name\|ID\|all>` | Restrict the bot to a forum topic (admins only) |
| `/singlemode ON <COIN>` or `/singlemode OFF` | Lock `/sprice`/`/preis` to a single coin (group admins only) |
| `/subscribe` or `/subscribe <coin> <limit> [<topic>]` | Trade-volume alert for a coin: pushes a message whenever an Atmos swap trades it above the given volume. `<limit>` must be greater than 5 (in the group's display currency). `<topic>` is optional - defaults to the current topic, or the main group for chats with no topics. Only one alert per topic; subscribing a new coin there replaces the previous one (group admins only) |
| `/unsubscribe <coin>` | Remove a trade-volume alert (group admins only) |
| `/viewsub` | List a group's active trade-volume alerts, from anywhere in the group (group admins only) |
| `/top10 [1d\|7d\|1m\|1y]` | Biggest recorded trades per subscribed coin/topic for one rolling window (aliases: 24h/1w/4w/12m); defaults to 7d if omitted, always shown in USD. Each entry shows a relative size bar, a rank with medal/creature icon, and a timestamp formatted for the bot's language |
| `/top5 [1d\|7d\|1m\|1y]` | Same as `/top10`, limited to the top 5 |
| `/help` | Show this command overview |

## Tracked coins

With full chart, market cap, and historical support: `SUPRA`, `SUPRAWR`, `JOSH`, `RAGE`, `LUCKY`, `IWBTC` (bridged BTC), `IETH` (bridged ETH), `SOLID`, `TSUPRA`.

Other coins listed on Atmos can be queried via `/sprice` through a live lookup, without historical chart data.

## Price source

Prices are primarily derived from the Supra DEX Atmos (swap-based price derivation). If a coin isn't listed there, `/compare` falls back to the CoinGecko API.

## Trade-volume alerts (`/subscribe`)

- Alerts show which coin was bought and which was sold, each on its own line, with the traded amount on each side of the swap (resolved from the transaction's on-chain swap event), plus the total volume and a Suprascan link.
- Displayed amounts follow the group's `/currency` setting (USD/EUR); `/top10`/`/top5` amounts are always shown in USD regardless of that setting. The alert `<limit>` itself must be greater than 5 in that currency.
- Only one active alert per forum topic - subscribing a new coin to a topic replaces whatever was subscribed there before.
- `/top10` and `/top5` rank the biggest recorded trades per coin over 24h/7d/30d/365d. Rankings are shared across every topic/chat subscribed to the same coin rather than duplicated per topic, and only cover trades recorded since this feature shipped (2026-07-04) - there is no historical backfill. Each entry is shown with a relative volume bar, a rank number with a medal (top 3) or a themed size icon (🦈🐬🐟🐠🦑🦀🦐 for ranks 4-10), and a timestamp formatted for the bot's language (DE `dd.mm.yyyy hh:mm`, EN `yyyy-mm-dd hh:mm`).

## Operation

The bot runs as a persistent systemd service on the SupraTools infrastructure and restarts automatically.
