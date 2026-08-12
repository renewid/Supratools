# SupraTools Telegram Bot

Telegram bot for prices, market data, and tools around the Supra ecosystem (SUPRA and select Supra-native coins).

> This directory contains documentation only (README + Changelog). The bot's source code is not made public.

## Language

The bot is fully bilingual (German/English), toggleable via `/lang`. In groups the setting applies to everyone and can only be changed by admins.

## Commands

| Command | Description |
|---------|--------------|
| `/sprice` or `/sprice <symbol\|name\|address>` (alias: `/preis`) | Show SUPRA or altcoin price. For tracked coins includes chart, market cap (also shown as a SUPRA-equivalent figure), live on-chain supply, and 1h/24h/7d change; for other Atmos-tradeable coins, a live price via Atmos lookup. For coins not on Atmos at all, falls back through Supra Oracle → CoinGecko → CoinMarketCap - the reply always states which of these the price came from |
| `/address <symbol\|name>` | Contract address & Suprascan link for a coin |
| `/sns <name.supra \| 0x-address>` | Supra Name Service resolution: name → address and address → name |
| `/alert <price>` or `/alert <symbol> <price>` | Set a price alert |
| `/calculate <coin> <new SUPRA price>` | A coin's theoretical market cap at some hypothetical new SUPRA price, assuming its price relative to SUPRA stays constant. Same coin resolution as `/compare` |
| `/compare <coin1> <coin2>` | Market cap comparison of two coins, including coin1's extrapolated price based on coin2's market cap. If a coin isn't listed on Atmos, price/market cap is looked up via CoinGecko |
| `/currency USD\|EUR` | Change the display currency for prices & chart (group admins only) |
| `/dapps` | Show dApps live on Supra |
| `/galaxy <0x-address \| name.supra>` | Render a wallet's holdings as a solar system - SUPRA as the central star, every other held coin orbiting as a planet sized by USD value, priced live via Atmos |
| `/lang` | Change language (DE/EN) (group admins only) |
| `/settopic` or `/settopic <name\|ID\|all>` | Restrict the bot to a forum topic (admins only) |
| `/singlemode ON <COIN>` or `/singlemode OFF` | Lock `/sprice`/`/preis` to a single coin (group admins only) |
| `/stats` or `/stats <week\|month\|year>` | Pre-generated trade recap per subscribed coin, always in USD |
| `/stats auto` | Toggle automatic posting of the recap into the current topic (group admins only) |
| `/subscribe` or `/subscribe <coin> <limit> [<topic>]` | Trade-volume alert for a coin: pushes a message whenever an Atmos swap trades it above the given volume. `<limit>` must be greater than 5 (in the group's display currency). `<topic>` is optional - defaults to the current topic, or the main group for chats with no topics. Only one alert per topic; subscribing a new coin there replaces the previous one (group admins only) |
| `/suprafx` | Platform status for SupraFX (suprafx.ai) - a separate cross-chain swap/settlement platform built on Supra, unrelated to Atmos. Shows chain health, lifetime volume, supported assets, reserve backing, recent trade requests, and a top-5-by-volume ranking |
| `/unsubscribe <coin>` | Remove a trade-volume alert (group admins only) |
| `/viewsub` | List a group's active trade-volume alerts, from anywhere in the group (group admins only) |
| `/top10 [1d\|7d\|1m\|1y]` | Biggest recorded trades per subscribed coin/topic for one rolling window (aliases: 24h/1w/4w/12m); defaults to 7d if omitted, always shown in USD. Each entry shows a relative size bar, a buy/sell direction arrow, a rank with medal/creature icon, and a timestamp formatted for the bot's language |
| `/top5 [1d\|7d\|1m\|1y]` | Same as `/top10`, limited to the top 5 |
| `/tracked` | List all coins with full price history support (chart, market cap, 1h/24h/7d) |
| `/help` | Show this command overview |

## Tracked coins

With full chart, market cap, and historical support: `SUPRA`, `SUPRAWR`, `JOSH`, `RAGE`, `LUCKY`, `IWBTC` (bridged BTC), `IETH` (bridged ETH), `SOLID`, `TSUPRA`, `ROBBIE`, `SUPRABAG`, `LEO`.

Other coins listed on Atmos can be queried via `/sprice` through a live lookup, without historical chart data.

## Price source

Prices are primarily derived from the Supra DEX Atmos (swap-based price derivation). For a coin not listed there at all:

- `/compare` falls back to the CoinGecko API
- `/sprice`/`/preis` falls back through a chain - Supra Oracle, then CoinGecko, then CoinMarketCap - stopping at the first one with a price. The reply always names the source that actually answered (e.g. "via CoinGecko, live"), so it's never ambiguous where a number came from

## Trade-volume alerts (`/subscribe`)

- Alerts show which coin was bought and which was sold, each on its own line, with the traded amount on each side of the swap (resolved from the transaction's on-chain swap event), plus the total volume, a buy/sell direction indicator, and a Suprascan link.
- Multi-hop swaps that only route *through* the subscribed coin's pool - without the trader actually buying or selling that coin - are filtered out and never alert or count towards `/top10`/`/top5`.
- Displayed amounts follow the group's `/currency` setting (USD/EUR); `/top10`/`/top5` amounts are always shown in USD regardless of that setting. The alert `<limit>` itself must be greater than 5 in that currency.
- Only one active alert per forum topic - subscribing a new coin to a topic replaces whatever was subscribed there before.
- `/top10` and `/top5` rank the biggest recorded trades per coin over 24h/7d/30d/365d. Rankings are shared across every topic/chat subscribed to the same coin rather than duplicated per topic, and only cover trades recorded since this feature shipped (2026-07-04) - there is no historical backfill. Each entry is shown with a relative volume bar, a rank number with a medal (top 3) or a themed size icon (🦈🐬🐟🐠🦑🦀🦐 for ranks 4-10), and a timestamp formatted for the bot's language (DE `dd.mm.yyyy hh:mm`, EN `yyyy-mm-dd hh:mm`).

## SupraFX (`/suprafx`)

SupraFX (suprafx.ai) is a separate cross-chain swap/settlement platform built on Supra (RFQ orderbook + BFT-consensus "Settlement Council") - unrelated to Atmos, and not used as a price source anywhere else in the bot, since its tradeable assets (SUPRA, ETH, USDT, USDC and their i-wrapped Supra versions) are already covered elsewhere.

- Matched (executed) trades are polled and recorded continuously, so the top-5-by-volume ranking reflects real trading history, not just whatever fits in a single live API call.
- USD volume per trade is computed from whichever leg of the pair is priceable: a USD-pegged stable asset's own amount, or the SUPRA leg converted via the bot's own tracked SUPRA price.

## Operation

The bot runs as a persistent systemd service on the SupraTools infrastructure and restarts automatically.
