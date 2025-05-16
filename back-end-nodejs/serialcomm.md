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

# SerialComm

## **Requisiti per il componente `SerialComm`**

1. **Comunicazione seriale**
   * Il componente deve inizializzare correttamente una connessione seriale utilizzando il path `/dev/ttyS0` con una velocità di trasmissione di 38400 baud.
   * Deve supportare la ricezione di dati tramite un parser `ReadlineParser` e la gestione di eventi tramite `EventEmitter`.
2. **Invio di dati tramite SerialComm**
   * Deve fornire una funzione `sendViaSerialComm` che invia dati in formato specifico al dispositivo, differenziando tra indirizzi compresi nei seguenti intervalli:
     * Da 2 a 198: messaggi con una lunghezza fissa di 7 byte.
     * Da 200 a 399: messaggi con una lunghezza fissa di 9 byte.
   * Deve verificare che l'indirizzo sia valido e, in caso contrario, restituire un errore (`Wrong Address`).
   * Deve gestire errori durante la scrittura sulla porta seriale, notificandoli tramite la funzione `notifyError`.
   * Deve inviare i dati nel formato:

{% tabs %}
{% tab title="JavaScript" %}
```javascript
[#][indirizzo in 2 byte][":" carattere][valore in byte(s)][*]
```
{% endtab %}
{% endtabs %}

3. **Ricezione e validazione dei dati**
   * Deve validare i messaggi ricevuti, assicurandosi che:
     * Inizino con il carattere `#`.
     * Contengano il carattere `:`.
     * Finiscano con il carattere `*`.
   * Deve notificare errori se i messaggi ricevuti non rispettano questo formato.
   * Deve decodificare i dati ricevuti e aggiornarne lo stato in base all'indirizzo (es. `SerialNumber`, `NumberOfAxis`, `VersionSoftware`).
4. **Gestione dei dati ROM**
   * Deve leggere dati da file JSON specificati, come `ROM.json`, e inviarli tramite la funzione `sendViaSerialComm`, limitandosi a un numero specifico di assi (`numberOfAxis`).
   * Deve gestire eventuali errori di lettura o parsing dei file, notificandoli tramite `notifyError`.
5. **Gestione della posizione**
   * Deve includere funzioni per convertire la posizione tra due formati:
     * `updateActualPositionDeg`: da valore in bit a gradi.
     * `updateActualPositionEx`: da valore in gradi a bit.
   * Deve utilizzare parametri di configurazione come `position_sensor_resolution`, `position_sensor_stroke`, e `position_sensor_offset_kinematics` per i calcoli.
   * Deve gestire errori durante il calcolo e notificarli tramite `notifyError`.
6. **Inizializzazione del sistema**
   * Deve richiedere al dispositivo informazioni iniziali, come:
     * Numero di serie.
     * Numero di assi.
     * Versione del software.
     * Stato iniziale dell’asse.
   * Deve monitorare la ricezione di questi dati e notificare eventuali mancanze o errori.
7. **Gestione degli eventi**
   * Deve utilizzare `EventEmitter` per notificare eventi come `serialNumber`, `axisNumber`, `versionSoftware`, `actualAxisZero`, e altre configurazioni ricevute.
8. **Gestione degli errori**
   * Deve notificare errori tramite la funzione `notifyError` con un codice identificativo e un messaggio descrittivo.

## **Test Case**

**TC1 - Inizializzazione della comunicazione seriale**

**Descrizione:** Verificare che la comunicazione seriale venga inizializzata correttamente.\
**Passi:**

1. Avviare il componente `SerialComm`.
2. Controllare i log per il messaggio di successo:
   * "Serial port communication initialized successfully".
3. Verificare che non siano stati generati errori (`notifyError` non chiamato).\
   **Risultato atteso:**

* La comunicazione seriale viene inizializzata senza errori.

***

**TC2 - Invio di dati via seriale**

**Descrizione:** Verificare che i dati vengano inviati correttamente tramite la funzione `sendViaSerialComm`.\
**Passi:**

1. Chiamare `sendViaSerialComm` con un indirizzo valido (es. 50) e un valore valido (es. 12345).
2. Controllare i log per il messaggio:
   * "Data send successful to the port".
3. Chiamare `sendViaSerialComm` con un indirizzo non valido (es. 500) e verificare che venga generato un errore (`notifyError`).\
   **Risultato atteso:**

* I dati vengono inviati correttamente per indirizzi validi.
* Per indirizzi non validi, viene generato un errore e i dati non vengono inviati.

***

**TC3 - Validazione dei dati ricevuti**

**Descrizione:** Verificare che i dati ricevuti tramite `parser.on('data')` vengano validati correttamente.\
**Passi:**

1. Simulare la ricezione di un messaggio corretto, come `#1:123*`.
2. Verificare che il messaggio venga elaborato correttamente e che l'evento associato (es. `serialNumber`) venga emesso.
3. Simulare la ricezione di un messaggio non valido, come `1:123`.
4. Controllare che venga generato un errore tramite `notifyError`.\
   **Risultato atteso:**

* I messaggi validi vengono elaborati e gli eventi appropriati vengono emessi.
* I messaggi non validi generano errori senza interrompere il processo.

***

**TC4 - Invio di dati ROM**

**Descrizione:** Verificare che i dati ROM vengano inviati correttamente tramite `sendROMDataViaSerialComm`.\
**Passi:**

1. Creare un file JSON di test (`ROM.json`) con dati validi per diversi indirizzi.
2. Chiamare `sendROMDataViaSerialComm` con il path del file e un numero di assi (es. 3).
3. Controllare i log per confermare che tutti i dati siano stati inviati correttamente.
4. Rimuovere il file o modificarlo per introdurre un errore e verificare che venga generato un errore tramite `notifyError`.\
   **Risultato atteso:**

* I dati ROM vengono inviati correttamente per file validi.
* Viene generato un errore in caso di file non validi o assenti.

***

**TC5 - Conversione della posizione**

**Descrizione:** Verificare la correttezza delle funzioni `updateActualPositionDeg` e `updateActualPositionEx`.\
**Passi:**

1. Impostare i parametri di configurazione (`position_sensor_*`) con valori noti.
2. Chiamare `updateActualPositionDeg` con una posizione valida in bit e verificare che la conversione in gradi sia corretta.
3. Chiamare `updateActualPositionEx` con una posizione valida in gradi e verificare che la conversione in bit sia corretta.
4. Introdurre parametri non validi e verificare che venga generato un errore tramite `notifyError`.\
   **Risultato atteso:**

* Le conversioni avvengono correttamente per valori validi.
* Gli errori vengono gestiti correttamente per valori non validi.

***

**TC6 - Gestione degli eventi**

**Descrizione:** Verificare che il componente reagisca correttamente agli eventi emessi.\
**Passi:**

1. Emittere un evento `serialNumber` con un numero di serie simulato.
2. Emittere un evento `axisNumber` con un numero di assi simulato.
3. Verificare che le variabili associate (`receivedSerialNumber`, `receivedNumberOfAxis`) vengano aggiornate correttamente.\
   **Risultato atteso:**

* Gli eventi vengono gestiti correttamente e aggiornano lo stato del componente.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>12/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>12/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>12/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>12/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>12/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>12/01/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests SerialComm

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Passi</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Verificare l'inizializzazione della comunicazione seriale dopo l'implementazione di nuovi cambiamenti.</td><td>-Avviare SerialComm.<br>-Verificare i log per: "Serial port communication initialized successfully".<br>-Confermare che notifyError non viene chiamato.</td></tr><tr><td align="center">RT2</td><td>Testare l'invio di dati via seriale per validare i cambiamenti nel metodo <code>sendViaSerialComm</code>.</td><td>-Chiamare <code>sendViaSerialComm</code> con:<br>1) Indirizzovalido (es. 50) e valore (12345).<br>2)Indirizzo non valido (es. 500)</td></tr><tr><td align="center">RT3</td><td>Verificare la gestione dei file ROM con modifiche alle funzionalita di parsing.</td><td>-Utilizzare un file JSON valido e inviarlo con <code>sendROMDataViaSerialComm.</code><br>-Introdurre errori nel file e verificare notifyError.</td></tr><tr><td align="center">RT4</td><td>Assicurarsi che le modifiche agli EventEmitter non alterino la gestione degli eventi.</td><td>-Simulare eventi serialNumber e axisNumber.<br>-Verificare che i valori associati vengano aggiornati.</td></tr></tbody></table>
