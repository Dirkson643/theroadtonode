# Tor aanpassen

{% hint style="info" %}
Tijd: 5 minuten
{% endhint %}

Tor heeft een paar aanpassingen nodig zodat LND hierover kan communiceren. Open torrc.

```bash
sudo nano /etc/tor/torrc
```

In het torrc bestand zijn we [eerder geweest](https://node.bitdeal.nl/raspberry-pi/tor). Voeg de volgende regels toe aan hetgeen dat er al staat.

```text
SOCKSPort 9050

HiddenServiceDir /var/lib/tor/lightning/lnd
HiddenServiceVersion 3
HiddenServicePort 8080 127.0.0.1:8080
HiddenServicePort 9735 127.0.0.1:9735
HiddenServicePort 10009 127.0.0.1:10009
```

Sla het bestand op met de toestencombinatie Control + X. Geef "Y" als antwoord op de vraag of je op wil slaan.

Maak mappen aan met:

```bash
sudo mkdir /var/lib/tor/lightning
```

```bash
sudo mkdir /var/lib/tor/lightning/lnd
```

Geef de juiste rechten met:

```bash
sudo chown -R debian-tor:debian-tor /var/lib/tor/lightning/lnd
```

```bash
sudo chmod 700 /var/lib/tor/lightning/lnd
```

Tor moet nu opnieuw opgestart worden.

```text
sudo systemctl restart tor
```

Het onion-adres krijg je met:

```bash
sudo cat /var/lib/tor/lightning/lnd/hostname
```
