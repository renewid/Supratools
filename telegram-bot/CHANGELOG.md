# Changelog

All notable changes to the SupraTools Telegram Bot are documented here.

Format inspired by [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

> Note: this changelog was introduced on 2026-07-02 and starts at the feature set current at that time (`v0.5 beta`). Changes before this date are not tracked retroactively.

## [v0.69 beta] - 2026-08-19

### Added
- `/contract` - official contract data for SUPRA: on Supra L1 (native/legacy coin, its paired Fungible Asset object, and iSUPRA - a separate SupraNova asset, clearly labeled as not being the same token) and on Ethereum (the SupraNova Bridge's ERC-20, live since 2026-08-18), including the live bridged total
- `/supply` - SUPRA's total and circulating supply on Supra L1, plus how much has been bridged to Ethereum (live, and as a percentage of circulating supply)

## [v0.68 beta] - 2026-08-16

### Added
- `/cashexploit <0x-address | name.supra>` (DMs only) - checks whether a wallet bought CASH during the Solido CASH stablecoin exploit window and thus qualifies for Solido's redeem program. Analysis only, no guarantee - the final decision rests solely with the Solido team
- `/leoex [1d|7d|1m|1y]` - top 5 most profitable trades by LeoEx, an arbitrage system on Supra, ranked by gross profit (before its trader/treasury profit split) in SUPRA and USD. Tracks every trader wallet the system routes through, not a single fixed address. History starts 2026-08-01

## [v0.67 beta] - 2026-08-12

### Added
- `/suprafx` - platform status for SupraFX (suprafx.ai), a separate cross-chain swap/settlement platform built on Supra, unrelated to Atmos: chain health (validator quorum, batch height), lifetime settled volume, supported assets, per-asset proof-of-reserves backing, the most recent public trade requests, and a top-5-by-volume ranking of matched trades (tracked over time, not just a live snapshot)

## [v0.66 beta] - 2026-08-12

### Changed
- `/sprice`/`/preis`: for coins not tradeable on Atmos at all, the price lookup now walks a fallback chain - Supra Oracle, then CoinGecko, then CoinMarketCap - instead of stopping at CoinGecko. The reply always states which source the number came from (e.g. "via CoinGecko, live")

## [v0.65 beta] - 2026-08-07

### Added
- `/galaxy <address or name.supra>` - renders a wallet's holdings as a solar system (SUPRA as the central star, every other held coin orbiting as a planet sized by USD value), priced live via Atmos

## [v0.64 beta] - 2026-07-31

### Changed
- `/sprice`/`/preis` now shows a coin's live on-chain supply next to its market cap, and market cap is now also shown as a SUPRA-equivalent figure alongside the USD value

## [v0.63 beta] - 2026-07-30

### Added
- New tracked coins with full chart/market cap/history support: `ROBBIE` (2026-07-15), `SUPRABAG` (2026-07-16), `LEO` (2026-07-30)
- `/tracked` - lists all coins with full price history support
- `/calculate <coin> <new SUPRA price>` - a coin's theoretical market cap at some hypothetical new SUPRA price, same coin resolution as `/compare`

## [v0.62 beta] - 2026-07-05

### Added
- `/stats` or `/stats <week|month|year>` - pre-generated weekly/monthly/yearly trade recap per subscribed coin, always in USD
- `/stats auto` - toggle automatic posting of the recap into the current topic (group admins only)
- `/top10`/`/top5` entries now also show a buy/sell direction arrow, matching the arrow already used in live trade alerts

### Fixed
- Trade-volume alerts and `/top10`/`/top5` no longer count multi-hop swaps where the subscribed coin was merely a pass-through pool leg (e.g. LEO → SUPRA → GLOOPO alerting a SUPRA subscription even though SUPRA was never actually bought or sold)
- The legacy on-chain SUPRA coin type was sometimes displayed/matched as `SupraCoin` instead of `SUPRA` in swap detection, which had been silently breaking the pass-through filter above for genuine direct SUPRA trades

## [v0.61 beta] - 2026-07-04

### Added
- `/top5` – same as `/top10`, limited to the top 5 trades

### Changed
- `/subscribe` now requires `<limit>` to be greater than 5 (in the group's display currency); smaller values are rejected with an error
- `/top10`/`/top5` output redesigned: each entry now shows a relative volume bar, a rank number paired with a medal (top 3) or a themed size icon (ranks 4-10), and a timestamp formatted for the bot's language instead of a fixed format
- Trade alerts now show the bought/sold amounts on two separate lines instead of one line joined by an arrow

## [v0.6 beta] - 2026-07-04

### Added
- `/subscribe` / `/unsubscribe` – trade-volume alerts pushed into a forum topic whenever an Atmos swap trades a coin above a given volume
  - Alerts include which coin was bought/sold and the traded amount on each side of the swap, resolved from the transaction's on-chain swap event
  - Displayed amounts follow the group's `/currency` setting; the topic target is shown by name where cached
  - Only one alert allowed per forum topic - subscribing a new coin to a topic replaces the previous one
  - Topic argument is optional, defaulting to the current topic or the main group for chats without topics
- `/viewsub` – view a group's active trade-volume alerts from anywhere in the group, without needing to be inside the target topic
- `/top10` – biggest recorded trades per subscribed coin, ranked over a rolling window (`1d`/`7d`/`1m`/`1y`, plus `24h`/`1w`/`4w`/`12m` aliases; defaults to `7d`), always in USD. Rankings are recorded once per coin and shared across every topic/chat subscribed to it, rather than duplicated per topic

## [v0.5 beta] - 2026-07-02

### Included
- `/sprice` (alias `/preis`) – price lookup for tracked coins (chart, market cap, 1h/24h/7d) plus live lookup for other Atmos-listed coins
- `/address` – contract address & Suprascan link
- `/sns` – Supra Name Service resolution (name ↔ address)
- `/alert` – price alerts
- `/compare` – market cap comparison of two coins, with CoinGecko fallback for coins not listed on Atmos
- `/currency` – switchable display currency (USD/EUR)
- `/dapps` – overview of active Supra dApps
- `/lang` – switchable language (DE/EN)
- `/settopic` – restriction to a forum topic
- `/singlemode` – restriction of `/sprice`/`/preis` to a single coin
- `/help` – full command overview
- Tracked coins: SUPRA, SUPRAWR, JOSH, RAGE, LUCKY, IWBTC, IETH, SOLID, TSUPRA
