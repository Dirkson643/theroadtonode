# Tor aanpassen

{% hint style="info" %}
Tijd: 5 minuten
{% endhint %}

Tor heeft een paar aanpassingen nodig zodat jij van buitenaf met LND kan communiceren. Op deze manier kun je een mobiele wallet aan je Lightning node koppelen en overal gebruiken. Open torrc.

```bash
sudo nano /etc/tor/torrc
```

In het torrc bestand zijn we [eerder geweest](https://node.bitdeal.nl/bitcoin-core/tor-aanpassen). Voeg de volgende regels toe aan hetgeen dat er al staat.

```text
SOCKSPort 9050

HiddenServiceDir /var/lib/tor/lightning/rest
HiddenServiceVersion 3
HiddenServicePort 8080 127.0.0.1:8080

HiddenServiceDir /var/lib/tor/lightning/rpc
HiddenServiceVersion 3
HiddenServicePort 10009 127.0.0.1:10009
```

Sla het bestand op met de toestencombinatie `Ctrl + X`. Geef `Y` als antwoord op de vraag of je op wil slaan.

Maak mappen aan met:

```bash
sudo mkdir /var/lib/tor/lightning
```

```bash
sudo mkdir /var/lib/tor/lightning/rest
```

```bash
sudo mkdir /var/lib/tor/lightning/rpc
```

Geef de juiste rechten met:

```bash
sudo chown -R debian-tor:debian-tor /var/lib/tor/lightning/rest
```

```bash
sudo chown -R debian-tor:debian-tor /var/lib/tor/lightning/rpc
```

```bash
sudo chmod 700 /var/lib/tor/lightning/rest
```

```bash
sudo chmod 700 /var/lib/tor/lightning/rpc
```

Tor moet nu opnieuw opgestart worden.

```bash
sudo systemctl restart tor
```

## Onion-adressen

De onion-adressen krijg je met:

```bash
sudo cat /var/lib/tor/lightning/rest/hostname
```

```bash
sudo cat /var/lib/tor/lightning/rpc/hostname
```

