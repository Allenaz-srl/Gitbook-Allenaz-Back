---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Libraries

{% hint style="info" %}
**Librerie utilizzate nel backend.**
{% endhint %}

***

## Librerie per la gestione del server e middleware

* **`express-rate-limit`** (v6.8.1): Limita il numero di richieste al server per prevenire abusi o attacchi.
* **`cors`** (v2.8.5): Gestisce le richieste Cross-Origin Resource Sharing (CORS).
* **`body-parser`** (v1.20.2): Analizza il corpo delle richieste HTTP, supportando formati JSON e URL-encoded.
* **`nodemon`** (v2.0.20): Strumento per lo sviluppo che riavvia automaticamente il server quando il codice cambia.

***

## Librerie per la comunicazione e networking

* **`axios`** (v1.3.4): Cliente HTTP per effettuare richieste verso API esterne.
* **`socket.io`** (v4.7.2): Implementazione del WebSocket per la comunicazione in tempo reale tra cliente e server.
* **`socket.io-cliente`** (v4.7.2): Cliente WebSocket per connettersi al server Socket.io.
* **`socket.io-stream`** (v0.9.1): Supporto per lo streaming di dati binari con Socket.io.

***

## Librerie per la gestione di file e hardware

* **`archiver`** (v7.0.1): Crea archivi compressi in formato ZIP o altri formati.
* **`serialport`** (v11.0.0): Gestisce la comunicazione seriale con dispositivi hardware.
* **`@serialport/parser-readline`** (v10.5.0): Parser per leggere dati seriali in formato readline.
* **`spi-device`** (v3.1.2): Interfaccia per la comunicazione con dispositivi SPI.
* **`node-hid`** (v2.1.2): Libreria per l'accesso a dispositivi HID (Human Interface Device).
* **`onoff`** (v6.0.3): Gestisce i pin GPIO su dispositivi come Raspberry Pi.

***

## Librerie per la sicurezza e autenticazione

* **`bcrypt`** (v5.1.0): Per la crittografia della password.
* **`jsonwebtoken`** (v9.0.1): Implementa la gestione dei token JWT per l'autenticazione.

***

## Librerie per la gestione dei dati

* **`mariadb`** (v3.2.0): Driver per connettersi a database MariaDB.
* **`uuid`** (v9.0.0): Generatore di identificativi univoci (UUID).
* **`moment-timezone`** (v0.5.43): Gestisce le date e i fusi orari.

***

## **Librerie per logging e monitoraggio**

* **`pino`** (v8.14.1): Logger ad alte prestazioni per Node.js.
* **`winston`** (v3.8.2): Libreria di logging configurabile e versatile.
* **`hot-shots`** (v10.0.0): Client StatsD per raccogliere metriche e monitorare il sistema.

***

## **Librerie per analisi e calcoli**

* **`chartjs-plugin-streaming`** (v2.0.0): Estensione di Chart.js per rappresentare dati in streaming.
* **`r-script`** (v0.0.4): Permette di eseguire script R da Node.js.

***

## **Librerie per notifiche**

* **`node-notifier`** (v10.0.1): Per inviare notifiche desktop native.
* **`toastr`** (v2.1.4): Per notifiche grafiche nei client web.

***

## **Librerie per testing**

* **`@testing-library/jest-dom`** (v5.16.3): Estende Jest con matcher DOM utili per il testing.
* **`@testing-library/react`** (v12.1.4): Libreria per testare componenti React.
* **`@testing-library/user-event`** (v13.5.0): Simula eventi utente per il testing.

***

## **Librerie per funzionalità speciali**

* **`vosk`** (v0.3.39): Riconoscimento vocale offline con Vosk API.
* **`react`** (v18.1.0): Libreria JavaScript per la costruzione di interfacce utente.
* **`react-dom`** (v18.1.0): Supporta il rendering di elementi React nel DOM.
* **`react-scripts`** (v5.0.1): Script predefiniti per configurare progetti React.
* **`set-interval-async`** (v3.0.3): Implementa intervalli asincroni con gestione delle promesse.
* **`open`** (v9.1.0): Apre URL, file o applicazioni con un semplice comando.

***

## **Librerie per il networking wireless**

* **`node-wifi`** (v2.0.16): Gestisce connessioni WiFi su dispositivi compatibili.
* **`node-wifi-scanner`** (v1.1.3): Permette di effettuare scansioni di reti WiFi vicine.
