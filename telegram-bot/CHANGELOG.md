# Changelog

All notable changes to the SupraTools Telegram Bot are documented here.

Format inspired by [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

> Note: this changelog was introduced on 2026-07-02 and starts at the feature set current at that time (`v0.8 beta`). Changes before this date are not tracked retroactively.

## [v0.91 beta] - 2026-07-04

### Added
- `/top5` – same as `/top10`, limited to the top 5 trades

### Changed
- `/subscribe` now requires `<limit>` to be greater than 5 (in the group's display currency); smaller values are rejected with an error
- `/top10`/`/top5` output redesigned: each entry now shows a relative volume bar, a rank number paired with a medal (top 3) or a themed size icon (ranks 4-10), and a timestamp formatted for the bot's language instead of a fixed format
- Trade alerts now show the bought/sold amounts on two separate lines instead of one line joined by an arrow

## [v0.9 beta] - 2026-07-04

### Added
- `/subscribe` / `/unsubscribe` – trade-volume alerts pushed into a forum topic whenever an Atmos swap trades a coin above a given volume
  - Alerts include which coin was bought/sold and the traded amount on each side of the swap, resolved from the transaction's on-chain swap event
  - Displayed amounts follow the group's `/currency` setting; the topic target is shown by name where cached
  - Only one alert allowed per forum topic - subscribing a new coin to a topic replaces the previous one
  - Topic argument is optional, defaulting to the current topic or the main group for chats without topics
- `/viewsub` – view a group's active trade-volume alerts from anywhere in the group, without needing to be inside the target topic
- `/top10` – biggest recorded trades per subscribed coin, ranked over a rolling window (`1d`/`7d`/`1m`/`1y`, plus `24h`/`1w`/`4w`/`12m` aliases; defaults to `7d`), always in USD. Rankings are recorded once per coin and shared across every topic/chat subscribed to it, rather than duplicated per topic

## [v0.8 beta] - 2026-07-02

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
