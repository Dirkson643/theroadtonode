# Starten en CLI

{% hint style="info" %}
Tijd: 5 minuten
{% endhint %}

Het is tijd om LND te starten en te initialiseren. Hier heb je twee terminals nodig en met allebei ben je met SSH in de Pi. Op terminal 1 start je LND met het "lnd" commando.

```bash
lnd
```

Op de tweede terminal gaan we een wallet aanmaken. Typ:

```bash
lncli create
```

Je komt een proces in waarbij je een wachtwoord moet opgeven van minimaal 8 karakters lang. Je krijgt de vraag of je een bestaande seed hebt. Kies N. Hierna krijg je de vraag of je de seed wil versleutelen met een wachtwoord. Ik zou het wel doen. Tot slot krijg je de seed te zien. Sla hem goed op, maar dat wordt ook duidelijk vermeld in de terminal.

Nadat je een wallet geïnitialiseerd hebt, zal op het eerste terminal scherm meer tekst verschijnen.

Druk in het eerste terminal scherm op Control + C om LND af te sluiten.

## Opnieuw opstarten

In het eerste terminal scherm start je LND.

```bash
lnd
```

Op het tweede terminal scherm unlock je je wallet.

```bash
lncli unlock
```

Vul je wachtwoord in.

## Informatie over jouw node

Om een overzicht te krijgen van jouw node met onder andere de public key en de URL naar de buitenwereld, typ je het volgende commando.

```bash
lncli getinfo
```
