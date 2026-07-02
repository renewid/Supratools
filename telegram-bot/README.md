# SupraTools Telegram Bot

Telegram-Bot für Preise, Marktdaten und Tools rund um das Supra-Ökosystem (SUPRA und ausgewählte Supra-native Coins).

> Dieses Verzeichnis enthält ausschließlich Dokumentation (README + Changelog). Der Quellcode des Bots wird nicht öffentlich bereitgestellt.

## Sprache

Der Bot ist vollständig zweisprachig (Deutsch/Englisch), umschaltbar per `/lang`. In Gruppen gilt die Einstellung für alle Mitglieder und kann nur von Admins geändert werden.

## Befehle

| Befehl | Beschreibung |
|--------|--------------|
| `/sprice` oder `/sprice <Symbol\|Name\|Adresse>` (Alias: `/preis`) | SUPRA- bzw. Altcoin-Preis anzeigen. Für getrackte Coins inkl. Chart, Marktkapitalisierung und 1h/24h/7d-Veränderung; für weitere Atmos-handelbare Coins Live-Preis über Atmos-Suche |
| `/address <Symbol\|Name>` | Contract-Adresse & Suprascan-Link eines Coins |
| `/sns <Name.supra \| 0x-Adresse>` | Supra Name Service: Auflösung Name → Adresse und Adresse → Name |
| `/alert <Preis>` oder `/alert <Symbol> <Preis>` | Preis-Alarm setzen |
| `/compare <Coin1> <Coin2>` | Market-Cap-Vergleich zweier Coins, inkl. hochgerechnetem Preis von Coin1 auf Basis der Marktkapitalisierung von Coin2. Ist ein Coin nicht auf Atmos gelistet, wird der Preis/die Marktkapitalisierung über CoinGecko nachgeschlagen |
| `/currency USD\|EUR` | Anzeige-Währung für Preise & Chart ändern (nur Gruppen-Admins) |
| `/dapps` | Live-dApps im Supra-Ökosystem anzeigen |
| `/lang` | Sprache ändern (DE/EN) (nur Gruppen-Admins) |
| `/settopic` oder `/settopic <Name\|ID\|all>` | Bot auf ein Forum-Topic beschränken (nur Admins) |
| `/singlemode ON <COIN>` oder `/singlemode OFF` | Bot auf einen einzigen Coin für `/sprice`/`/preis` festlegen (nur Gruppen-Admins) |
| `/help` | Diese Befehlsübersicht anzeigen |

## Getrackte Coins

Mit vollem Chart-, Marktkapitalisierungs- und Zeitverlauf-Support: `SUPRA`, `SUPRAWR`, `JOSH`, `RAGE`, `LUCKY`, `IWBTC` (bridged BTC), `IETH` (bridged ETH), `SOLID`, `TSUPRA`.

Weitere auf Atmos gelistete Coins können über `/sprice` per Live-Lookup abgefragt werden, auch ohne historischen Chart.

## Preisquelle

Preise werden primär über den Supra-DEX Atmos (swap-basierte Kursableitung) ermittelt. Ist ein Coin dort nicht gelistet, greift `/compare` als Fallback auf die CoinGecko-API zurück.

## Betrieb

Der Bot läuft als dauerhafter systemd-Service auf der SupraTools-Infrastruktur und wird automatisch neu gestartet.
