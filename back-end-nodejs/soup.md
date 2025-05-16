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

# SOUP

{% hint style="info" %}
**SOUP** (Software of Unknown Provenance) si riferisce a software di origine sconosciuta o non certificata, che non è stato sviluppato specificamente per essere integrato in dispositivi medici. Questo può includere librerie, framework grafici, driver o componenti di sistema che non sono stati creati seguendo processi medici, ma che possono essere utilizzati nei dispositivi medici seguendo regole precise.
{% endhint %}

## `fs` (libreria integrata Node.js): utilizzata per leggere e scrivere file (JSON con dati)

* **Requisiti:**
  * Gestire la lettura e la scrittura dei dati utilizzando il file system.
  * Elaborare file in formato JSON, garantendo l’integrità dei dati.
  * Gestire gli errori in caso di impossibilità di leggere/scrivere file.
* **Test:**
  * **Test 1:** Lettura di un file esistente.
    * **Passaggi:** Creare un file `test.json` con contenuto valido. Eseguire la lettura.
    * **Risultato atteso:** Dati ricevuti correttamente.
  * **Test 2:** Lettura di un file inesistente.
    * **Passaggi:** Assicurarsi che il file non esista ed eseguire la lettura.
    * **Risultato atteso:** Ritorno di errore `ENOENT`.
  * **Test 3:** Scrittura su un file.
    * **Passaggi:** Creare un nuovo file e scrivere dei dati.
    * **Risultato atteso:** I dati sono stati scritti correttamente.
*   **Result**:

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>09/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>09/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>09/10/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

***

## `express`: framework server, fornisce il routing e l'elaborazione delle richieste HTTP

* **Requisiti:**
  * Fornire il routing delle richieste HTTP e restituire risposte appropriate.
  * Supportare middleware per l’elaborazione del corpo delle richieste (ad esempio, `body-parser`).
* **Test:**
  * **Test 1:** Elaborazione di una richiesta valida.
    * **Passaggi:** Inviare una richiesta GET o POST ai percorsi del server.
    * **Risultato atteso:** Il server restituisce la risposta corretta.
  * **Test 2:** Richiesta a un percorso inesistente.
    * **Passaggi:** Inviare una richiesta a un percorso non configurato.
    * **Risultato atteso:** Risposta con stato HTTP 404.
*   **Result**:

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>10/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>10/10/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

***

## `socket.io/socket.io-client`: per la comunicazione(front-back) in tempo reale e WebSocket

* **Requisiti:**
  * Garantire la trasmissione affidabile di dati tramite WebSocket.
  * Supportare la sincronizzazione degli eventi tra cliente e server.
* **Test:**
  * **Test 1:** Connessione iniziale.
    * **Passaggi:** Connettere un cliente al server tramite WebSocket.
    * **Risultato atteso:** La connessione è stabilita con successo.
  * **Test 2:** Trasferimento dei dati.
    * **Passaggi:** Inviare un messaggio dal cliente al server e viceversa.
    * **Risultato atteso:** I messaggi vengono trasmessi senza errori.
*   **Result**:

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>10/04/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>10/04/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

***

## `bcrypt`: hashing dei dati (ad esempio password)

* **Requisiti:**
  * Fornire un hashing sicuro dei dati.
  * Supportare la verifica della corrispondenza tra dati hashati e originali.
* **Test:**
  * **Test 1:** Hashing di una stringa.
    * **Passaggi:** Hashare una stringa usando `bcrypt.`
    * **Risultato attesso:** Hash generato correttamente.
  * **Test 2:** Confronto tra stringa e hash.
    * **Passagi:** Confrontare una stringa originale con il rispettivo hash.
    * **Risultato atteso:** Confronto riuscito senza errori.
*   **Result:**

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>25/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>25/09/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

***

## `serialport`: comunicazione con dispositivi esterni tramite porta seriale

* **Requisiti:**
  * Gestire la comunicazione tramite porta seriale con dispositivi esterni(es. arduino-master)
  * Supportare la gestione degli errori in caso di perdita di connessione o dati non validi.
* **Test:**
  * **Test 1:** Connessione riuscit&#x61;**.**
    * **Passaggi:** Connettere un dispositivo tramite porta seriale.
    * **Risultato atteso:** La connessione è stabilita correttamente.
  * **Test 2:** Perdita della connessione.
    * **Passaggi:** Disconnettere il dispositivo durante il trasferimento dati.
    * **Risultato atteso:** Generazione di un errore appropriato.
*   **Result:**

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>13/04/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>13/04/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

***

## `spi-device`: comunicazione con dispositivi esterni tramite SPI (Serial Peripheral Interfce).

* **Requisiti**:
  * Fornire comunicazione affidabile tramite il protocollo SPI.
  * Consentire la configurazione di dispositivi esterni(es. arduino-master) collegati tramite SPI.
  * Supportare la gestione defli errori in caso di mancata comunicazione o configurazione errata.
* **Test:**
  * **Test 1**: Comunicazione SPI corretta.
    * **Passaggi:** Configurare un dispositivio esterno e avviare la comunicazione tramite SPI.
    * **Risultato atteso:** I dati sono trasmessi e ricevuti correttamente.
  * **Test 2**: Dispositivo non connesso.
    * **Passaggi**: Esseguire una richiesta SPI senza connettere il dispositivo.
    * **Risultato atteso:** Ritorno di un errore di connessione.
  * **Test 3**: Configurazione errata.
    * **Passaggi:** Configurare parametri errati per il dispositivo SPI.
    * **Risultato atteso:** Generazione di in errore con messaggio esplicativo.
*   **Result:**

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>17/04/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>17/04/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>17/04/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

***

## `onoff`: controllo GPIO (Raspberry Pi).

* **Requisiti:**
  * Interagire con i GPIO (General Purpose Input/Output) di Raspberry Pi.
  * Fornire lettura e scrittura affidabile degli stati GPIO.
  * Abilitare controlli critici che influenzano il funzionamento del componente _SpiComm_.
* **Test:**
  * **Test 1:** Lettura degli stati GPIO.
    * **Passaggi:** Leggere lo stato di un pin GPIO configurato come input.
    * **Risultato atteso:** Valore corretto dello stato del pin (HIGH o LOW).
  * **Test 2:** Scrittura su un pin GPIO.
    * **Passaggi:** Configurare un pin GPIO come output e modificare il suo stato.
    * **Risultato atteso:** Lo stato del pin cambia correttamente.
  * **Test 3:** Controlli nel componente _SpiComm_.
    * **Passaggi:** Avviare il processo SPI e verificare che le condizioni GPIO siano rispettate.
    * **Risultato atteso:** Se i controlli GPIO falliscono, la comunicazione SPI non inizia e viene generato un errore.
*   **Result:**

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>11/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>11/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>11/01/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

***

## `jsonwebtoken`: genera e convalida token di autenticazione.

* **Requisiti:**
  * Fornire un meccanismo sicuro per la creazione e la verifica dei token JWT.
  * Supportare l'autenticazione degli utenti tramite token.
  * Garantire la validità dei token generati e la gestione di eventuali token scaduti o non validi.
* **Test:**
  * **Test 1:** Creazione di un token valido.
    * **Passaggi:** Generare un token JWT per un utente con una chiave segreta.
    * **Risultato atteso:** Token generato correttamente e firmato con la chiave.
  * **Test 2:** Verifica di un token valido.
    * **Passaggi:** Verificare un token precedentemente generato.
    * **Risultato atteso:** Il token è verificato con successo.
  * **Test 3:** Verifica di un token scaduto o non valido.
    * **Passaggi:** Verificare un token modificato o scaduto.
    * **Risultato atteso:** Ritorno di un errore di autenticazione.
*   **Result:**

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>25/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>25/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>25/09/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

***

## `cors`: middleware per abilitare la condivisione delle risorse tra origini (CORS) nelle applicazioni Node.js

* **Requisiti:**
  * Consentire richieste HTTP cross-origin (CORS) tra client e server.
  * Supportare la configurazione di origini specifiche, metodi e intestazioni HTTP autorizzate.
  * Garantire la sicurezza del server limitando le richieste non autorizzate.
* **Test:**
  * **Test 1:** Richiesta autorizzata da un'origine valida.
    * **Passaggi:** Configurare un'origine autorizzata ed eseguire una richiesta.
    * **Risultato atteso:** La richiesta viene accettata.
  * **Test 2:** Richiesta non autorizzata da un'origine non valida.
    * **Passaggi:** Inviare una richiesta da un'origine non configurata.
    * **Risultato atteso:** Il server blocca la richiesta.
  * **Test 3:** Configurazione delle intestazioni personalizzate.
    * **Passaggi:** Configurare il server per accettare intestazioni specifiche e inviare una richiesta con tali intestazioni.
    * **Risultato atteso:** La richiesta viene accettata correttamente.
*   **Result:**

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>10/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>10/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>10/10/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>
