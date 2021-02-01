# BTC RPC Explorer

{% hint style="info" %}
Tijd: 20 minuten
{% endhint %}

Een eigen block explorer helpt je privacy omdat je nu geen publieke block explorer meer hoeft te raadplegen om te zien of jouw transacties nog in de mempool zitten of al gevalideerd zijn.

De Block Explorer die we gaan gebruiken is [BTC RPC Explorer](https://github.com/janoside/btc-rpc-explorer) van Dan Janosik.

{% hint style="info" %}
Let op: dit onderdeel is afhankelijk van de [NodeJS installatie](https://node.bitdeal.nl/raspberry-pi/algemene-dependencies-installeren#nodejs). Je kunt niet verder als je NodeJS niet geinstalleerd hebt op de Raspberry Pi.
{% endhint %}

## Voorbereiding

Er zijn een aantal voorwaarden waaraan je moet voldoen om deze block explorer te kunnen draaien:

1. Je moet een full node hebben draaien waarvan de blockchain compleet is.
2. Je node moet ook een index hebben van alle transacties.
3. Je moet een recente versie van Node.js hebben draaien.

### Full node

Je bent bezig met de road to node. Als je het in de juiste volgorde aan het doen bent dan heb je [Bitcoin Core](https://node.bitdeal.nl/bitcoin-core/installatie) inmiddels geinstalleerd. Zo niet doe dat dan eerst.

### Transactieindex

Login op je Pi en open het bitcoin configuratiebestand.

```bash
nano /home/pi/.bitcoin/bitcoin.conf
```

Controleer of er een regel `txindex = 1` in voorkomt. Zo niet, voeg deze dan toe en sla je wijzigingen op met `Ctrl + X` gevolgd door `Y`.

Herstart vervolgens `bitcoind`.

```bash
sudo systemctl restart bitcoind
```

## Installatie

Zorg dat je in de home directory bent.

```bash
cd ~
```

Haal de broncode binnen.

```bash
git clone https://github.com/janoside/btc-rpc-explorer.git
```

Ga de BTC RPC Explorer map in.

```bash
cd btc-rpc-explorer
```

Installeer BTC RPC Explorer, maak het configuratie bestand `.env` en pas deze aan.

```bash
npm install
nano .env
```

Plak daar in de volgende regels.

{% hint style="info" %}
Open de file .env-sample om te zien welke opties er nog meer zijn.
{% endhint %}

```bash
BTCEXP_HOST=IP-ADRES VAN PI
BTCEXP_PORT=3002

BTCEXP_BITCOIND_HOST=127.0.0.1
BTCEXP_BITCOIND_PORT=8332
BTCEXP_BITCOIND_COOKIE=/home/pi/.bitcoin/.cookie
BTCEXP_BITCOIND_RPC_TIMEOUT=5000

BTCEXP_PRIVACY_MODE=true
```

Pas de tekst `IP-ADRES VAN PI` aan naar wat voor jou van toepassing is. Vervang het dus met iets dat lijkt op `192.168.1.6`. Sla het bestand op met `Ctrl + X` gevolgd door `Y`.

## Service

Zorg er nu voor dat de BTC-RPC-Explorer automatisch start en draait als een service wanneer je Pi opnieuw opstart.

```bash
sudo nano /etc/systemd/system/btc-rpc-explorer.service
```

De inhoud van het bestand moet er zo uit zien. Met name het pad naar de workingdirectory is belangrijk.

```bash
[Unit]
Description=BTC-RPC-Explorer
Wants=bitcoind.service
After=bitcoind.service

[Service]
WorkingDirectory=/home/pi/btc-rpc-explorer
ExecStart=npm run start
User=pi
Group=pi
Type=simple
Restart=on-failure
TimeoutSec=120
RestartSec=30

[Install]
WantedBy=multi-user.target
```

Sla het bestand op met `Ctrl + X` gevolgd door `Y`.

```bash
sudo systemctl enable btc-rpc-explorer
sudo systemctl start btc-rpc-explorer
```

Open nu in Firefox op je PC een tabblad naar `http://IP-ADRES VAN PI:3002` om te zien of het werkt. Bijvoorbeeld `http://192.168.1.6:3002`.

## Updaten

Als er een nieuwe versie beschikbaar is voor BTC RPC Explorer, kun je eenvoudig updaten door de nieuwe source uit git op te halen en deze te installeren. Wel eerst even de service stoppen en naderhand weer starten zoals hieronder staat aangegeven.

```text
sudo systemctl stop btc-rpc-explorer
cd ~/btc-rpc-explorer
git pull
npm install
sudo systemctl start btc-rpc-explorer
```

## Tor

De service kun je ook beschikbaar maken via tor. Alleerst passen we de tor configuratie aan om een nieuwe hidden service te maken.

```bash
sudo nano /etc/tor/torrc
```

In het bestand dat zich opent voeg je onderaan de volgende drie regels toe.

```bash
HiddenServiceDir /var/lib/tor/btc-rpc-explorer
HiddenServiceVersion 3
HiddenServicePort 80 IP-ADRES VAN JE PI:3002
```

In de bovenstaande tekst zie je IP-ADRES VAN JE PI staan. Dit is iets wat je zelf moet invullen. De regel zou op `HiddenServicePort 80 192.168.1.6:3002` moeten lijken.

Nadat tor is geconfigureerd moeten we de juiste mappen aanmaken.

```bash
sudo mkdir /var/lib/tor/btc-rpc-explorer
sudo chown -R debian-tor:debian-tor /var/lib/tor/btc-rpc-explorer
sudo chmod 700 /var/lib/tor/btc-rpc-explorer
```

Herstart tor met:

```bash
sudo systemctl restart tor
```

Het onion-adres vind je met het volgende commando:

```bash
sudo cat /var/lib/tor/btc-rpc-explorer/hostname
```

Vul deze \(zonder portnummer\) in in je tor browser. De BTC RPC Explorer homepage zou moeten verschijnen.

## Koppeling met Electrum X

Als je de [Electrum X](https://node.bitdeal.nl/bitcoin-core-extensies/electrum-x) guide gevolgd hebt, kun je BTC RPC Explorer meteen hierop aansluiten voor verbeterde privacy. Pas het configuratie bestand van BTC RPC Explorer aan.

```bash
nano ~/btc-rpc-explorer/.env
```

Voeg onderaan de volgende twee regels toe:

```bash
BTCEXP_ADDRESS_API=electrumx
BTCEXP_ELECTRUMX_SERVERS=tcp://127.0.0.1:50001,ssl://127.0.0.1:50002,wss://127.0.0.1:50004,rpc://127.0.0.1:8000
```

Herstart de service om de nieuwe configuratie van kracht te laten zijn.

```bash
sudo systemctl restart btc-rpc-explorer
```

