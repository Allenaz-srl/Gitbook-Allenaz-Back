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

# SerialFunc

## **Requirements**

1. **Funzione `sendSerialMessage(address, value)`**
   * **Descrizione:** Questa funzione è progettata per inviare un messaggio tramite comunicazione seriale a uno specifico indirizzo con un valore associato.
   * **Comportamento:**
     * Deve generare un messaggio contenente il timestamp attuale, un contatore incrementale per i messaggi inviati nello stesso millisecondo, l'indirizzo del destinatario e il valore da inviare.
     * Deve registrare il messaggio utilizzando il logger `longLogger` con il livello di log "info".
     * Deve inviare il messaggio tramite la funzione `sendViaSerialComm` del modulo `serialPort`.
2. **Funzione `sendDataViaSerialComm(filePath)`**
   * **Descrizione:** Questa funzione legge un file JSON contenente dati mappati ad indirizzi e li invia tramite comunicazione seriale.
   * **Comportamento:**
     * Deve leggere il file specificato da `filePath` utilizzando il modulo `fs`.
     * Deve analizzare il file JSON per ottenere una mappatura di indirizzi e valori.
     * Deve filtrare gli indirizzi, includendo solo quelli validi (numerici e inferiori a 6, se applicabile).
     * Deve inviare i valori associati agli indirizzi validi utilizzando la funzione `sendSerialMessage`.
     * Deve gestire eventuali errori durante la lettura del file o durante l'invio dei dati, notificandoli tramite la funzione `notifyError` con i codici di errore appropriati (57 per errori durante la scrittura alla porta, 58 per errori generali di invio).
3. **Sistema di Notifica degli Errori**
   * La funzione deve notificare gli errori in caso di:
     * Impossibilità di leggere il file specificato.
     * Fallimento nella comunicazione seriale.
   * I messaggi di errore devono essere registrati con codici identificativi specifici (ad esempio, 57 e 58).

***

## **Test**

**Test Case**

1. **TC1 - Invio di un messaggio seriale valido**
   * **Descrizione:** Verificare che la funzione `sendSerialMessage` invii correttamente un messaggio tramite comunicazione seriale.
   * **Passi:**
     1. Chiamare `sendSerialMessage(3, "value123")`.
     2. Verificare che il messaggio sia stato registrato dal logger `longLogger`.
     3. Verificare che `serialPort.sendViaSerialComm` sia stata chiamata con i parametri corretti.
   * **Risultato atteso:** Il messaggio viene inviato correttamente e il logger registra l'evento.
2. **TC2 - Invio di dati validi tramite file JSON**
   * **Descrizione:** Verificare che la funzione `sendDataViaSerialComm` invii correttamente i dati estratti da un file JSON.
   * **Passi:**
     1. Creare un file JSON con dati di esempio (ad esempio: `{ "1": "val1", "2": "val2" }`).
     2. Chiamare `sendDataViaSerialComm("path/al/file.json")`.
     3. Verificare che ogni indirizzo e valore nel file sia stato inviato correttamente tramite `sendSerialMessage`.
   * **Risultato atteso:** Tutti i dati vengono inviati correttamente e il logger registra gli eventi.
3. **TC3 - Gestione degli errori durante la lettura del file**
   * **Descrizione:** Verificare che la funzione notifichi un errore se il file specificato non esiste o non può essere letto.
   * **Passi:**
     1. Chiamare `sendDataViaSerialComm("file/invalido.json")`.
     2. Verificare che venga chiamata `notifyError` con il codice di errore 58.
   * **Risultato atteso:** L'errore viene notificato correttamente.
4. **TC4 - Gestione degli errori durante l'invio tramite porta seriale**
   * **Descrizione:** Verificare che la funzione notifichi un errore se si verifica un problema durante l'invio tramite la porta seriale.
   * **Passi:**
     1. Simulare un errore nella funzione `serialPort.sendViaSerialComm`.
     2. Chiamare `sendDataViaSerialComm` con un file JSON valido.
     3. Verificare che venga chiamata `notifyError` con il codice di errore 57.
   * **Risultato atteso:** L'errore viene notificato correttamente.
5. **TC5 - Filtraggio degli indirizzi non validi**
   * **Descrizione:** Verificare che gli indirizzi non numerici o superiori a 5 vengano esclusi.
   * **Passi:**
     1. Creare un file JSON con dati misti (ad esempio: `{ "1": "val1", "6": "val2", "abc": "val3" }`).
     2. Chiamare `sendDataViaSerialComm("path/al/file.json")`.
     3. Verificare che solo gli indirizzi validi siano inviati tramite `sendSerialMessage`.
   * **Risultato atteso:** Gli indirizzi non validi vengono esclusi.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>04/06/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>04/06/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>04/06/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>04/06/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>04/06/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests SerialFunc

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Passi</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Verificare l'invio di un messaggio valido con <code>sendSerialMessage</code>.</td><td>-Chiamare <code>sendSerialMessage</code>(3, "value123").<br>-Verificare che il logger <code>longLogger</code> registri l'evento.<br>-Verificare che <code>sendViaSerialComm</code> venga chiamata con i parametri corretti.</td></tr><tr><td align="center">RT2</td><td>Verificare la gestione degli errori durante la lettura del file JSON.</td><td>-Chiamare <code>sendDataViaSerialComm</code> ("file/invalido.json").<br>-Verificare che notifyError venga chiamata con il codice di errore 58.</td></tr><tr><td align="center">RT3</td><td>Validare il logging coretto di tutti i messaggi inviati.</td><td>-Inviare un messaggio con <code>sendSerialMessage</code> e uno tramite <code>sendDataViaSerialComm</code>.<br>-Verificare che <code>longLogger</code> registri ogni evento correttamente.</td></tr><tr><td align="center">RT4</td><td>Validare la corretta generazione del timestamp e del contatore nei messaggi inviati.</td><td>-Inviare piu messaggi nello stesso millisecondo con <code>sendSerialMessage</code>.<br>-Verificare che ogni messaggio abbia un timestamp e un contatore univoci.</td></tr></tbody></table>
