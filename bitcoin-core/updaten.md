# Updaten

Ga naar de applicatie directory.

```bash
cd ~/bitcoin
```

Update de repository met de laatste wijzigingen via Git.

```bash
git fetch --all
```

Toon de laatste versie / tag.

```text
git describe --tags `git rev-list --tags --max-count=1`
```

Haal de wijzigingen op van de laatste versie.

```bash
git checkout <OUTPUT VAN DE VORIGE STAP> #voorbeeld: v0.21.0
```

Stel bitcoin core opnieuw samen met de laatste wijzigingen.

```bash
make
```

Dit zal even duren, mooi moment voor een biertje of kopje koffie!

Stop de lnd service.

```bash
sudo systemctl stop lnd
```

Stop de bitcoind service.

```bash
sudo systemctl stop bitcoind
```

Installeer nu de software.

```bash
sudo make install
```

Start de service bitcoin.

```bash
sudo systemctl start bitcoind
```

Start de lnd service.

```bash
sudo systemctl start lnd
```

Check de huidige versie van bitcoin core.

```bash
bitcoin-cli --version
```

De output zal lijken op `Bitcoin Core RPC client v0.21.0`

Bitcoin core is nu bijgewerkt!

