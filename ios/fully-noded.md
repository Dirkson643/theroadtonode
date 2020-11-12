# Fully Noded

{% hint style="info" %}
Tijd: 5 minuten
{% endhint %}

[Fully Noded](https://github.com/Fonta1n3/FullyNoded#why-fully-noded%EF%B8%8F) is een wallet app die je eigen node als backend gebruikt. Het kan gebruikt worden over Tor voor zowel Bitcoin als Lightning. Vanaf iedere plek in de wereld. Het biedt ook de mogelijkheid om je hardware wallet, zoals een Coldcard, aan te sluiten en zo gebruik te maken van PSBT.

Allereerst download je Fully Noded uit de [App Store](https://apps.apple.com/us/app/fully-noded/id1436425586). Open de app en druk rechtsonder op het tandwiel om naar de instellingen te gaan. Bovenaan zie je de "Node Manager". In dit gedeelte van de app kun je jouw node toevoegen.

![Het instellen van jouw eigen node in Fully Noded](../.gitbook/assets/instel.png)

Deze gegevens heb je nodig:

* **RPCUSER + RPCPASSWORD**, dat is hetzelfde als [hier](https://node.bitdeal.nl/bitcoin-core/configuratie-en-starten#configuratie) op regel 24 en 25 van bitcoin.conf
* **Onion adres voor main inclusief :8332**, zoals [eerder](https://node.bitdeal.nl/raspberry-pi/tor#onion-adressen) besproken

Nadat je alles goed hebt ingevuld druk je op "Save". Met een beetje geluk werkt alles in 1 keer. Zo niet, wil het meestal wel lukken nadat je de app hebt afgesloten.
