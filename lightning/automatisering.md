# Automatisering

{% hint style="info" %}
Tijd: 5 minuten
{% endhint %}

Het open hebben van twee terminals is irritant. LND wil je in de achtergrond hebben draaien, net als bitcoind.

```bash
sudo nano /etc/systemd/system/lnd.service
```

Plak er dit in.

```bash
[Unit]
Description=Lightning Network Daemon
Requires=bitcoind.service
After=bitcoind.service

[Service]
User=pi
ExecStart=/home/pi/go/bin/lnd
PIDFile=/home/pi/.lnd/lnd.pid
Restart=always
TimeoutSec=180
RestartSec=60

[Install]
WantedBy=multi-user.target
```

Sla het weer op met `Ctrl + X` en bevestig met `Y`.

Het systeem moet op de hoogte gesteld worden van de nieuwe service en kan daarna gestart worden.

```bash
sudo systemctl enable lnd
```

```bash
sudo systemctl start lnd
```

Wil je zien of alles goed is opgestart, voer dan dit uit:

```bash
systemctl status lnd
```

De output zou er ongeveer zo uit moeten zien:

```bash
● lnd.service - Lightning Network Daemon
   Loaded: loaded (/etc/systemd/system/lnd.service; enabled; vendor preset: enabled)
   Active: active (running) since Wed 2020-11-25 17:45:10 GMT; 6s ago
 Main PID: 2980 (lnd)
    Tasks: 9 (limit: 4915)
   CGroup: /system.slice/lnd.service
           └─2980 /home/pi/go/bin/lnd

Nov 25 17:45:10 raspberrypi systemd[1]: Started Lightning Network Daemon.
```

Met `q` ga je weer uit het scherm.

Wil je een overzicht van de status over meerdere sessie, gebruik dan dit:

```bash
sudo journalctl -f -u lnd
```

