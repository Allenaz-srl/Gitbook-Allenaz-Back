# SpiComm

## **Requirements**

Il componente "SpaceMouse" deve:

1. **Inizializzazione della comunicazione SPI**\
   Il componente deve inizializzare la comunicazione SPI utilizzando i parametri configurati:
   * Velocità di comunicazione (`spiSpeed`): 500 kHz.
   * Modalità SPI (`mode`): 0.
   * Ordine dei bit: MSB (Most Significant Bit) per primo.
   * Lunghezza di parola: 8 bit.
2. **Gestione degli eventi**
   * Deve ricevere il numero di assi (`axisNumber`) tramite un evento emesso da `serialEventEmitter` e memorizzarlo.
   * Deve ricevere i dati mappati tramite `playModeEventEmitter` e preparare i dati per l'invio.
3. **Lettura e scrittura GPIO**
   * Deve monitorare i pin GPIO per verificare la disponibilità della comunicazione SPI (`gpioPinSpi`) e segnalare errori quando rilevati (`gpioPinComm`).
   * Deve gestire il segnale di "pronto per ricevere" sul pin GPIO (`gpioReadytoReceive`).
4. **Gestione della sensibilità del joystick**
   * Deve leggere i dati di sensibilità del joystick da un file JSON (`json/joystickSensitivity.json`).
   * Deve applicare i moltiplicatori di sensibilità corretti ai dati del joystick prima dell'invio.
5. **Preparazione e invio dei dati tramite SPI**
   * Deve formattare i dati del joystick per il buffer SPI, limitandoli al range \[-100, 100].
   * Deve inviare i dati tramite SPI al master e successivamente ad ogni asse configurato.
   * Deve garantire che la lunghezza del buffer trasmesso sia corretta:
     * Master: 24 byte.
     * Assi: 20 byte per asse.
6. **Ricezione e gestione dei dati SPI**
   * Deve gestire i dati ricevuti dal master e dagli assi, includendo:
     * Decodifica degli stati (`driverState`, `workingMode`).
     * Lettura dei valori di posizione, coppia, e frequenza.
     * Gestione di errori, avvisi, e parità.
7. **Notifica errori**
   * Deve segnalare errori di comunicazione o trasferimento utilizzando la funzione `notifyError` e l'emettitore `errorEmitter`.
8. **Interfaccia pubblica**\
   Deve esportare:
   * Funzioni per inviare i dati (`sendData`) e accedere a parametri specifici (e.g., `getAxesX`, `getMouseData`).
   *   Variabili per monitorare stati, errori, avvisi, e dati di posizionamento (e.g., `parity`, `driverState`, `actualPosition`).**Requirements**

       Il componente "SpaceMouse" deve:**Requirements**

       Il componente "SpaceMouse" deve:

## **Test**

***

**Obiettivo dei test**

Verificare che il componente `SpiComm.js` soddisfi i requisiti funzionali, garantendo la corretta comunicazione SPI, la gestione dei dati, e l'integrità della trasmissione.

***

**Ambiente di test**

* **Hardware richiesto:**
  * Raspberry Pi o un dispositivo con supporto GPIO.
  * Dispositivo Arduino compatibile con SPI.
  * Joystick compatibile con SpaceMouse.
  * Modulo SPI configurato con master e slave.
* **Software richiesto:**
  * Node.js versione >=14.
  * Moduli `spi-device`, `onoff`, e altre dipendenze necessarie.
  * File JSON con sensibilità joystick (`json/joystickSensitivity.json`).
  * Logger configurato.

***

**Test Case**

**TC1 - Inizializzazione SPI**

**Descrizione:** Verificare che la comunicazione SPI venga inizializzata correttamente.\
**Passi:**

1. Avviare il componente.
2. Controllare i log per il messaggio di successo ("SPI communication initialized successfully").\
   **Risultato atteso:** La comunicazione SPI viene inizializzata senza errori.

***

**TC2 - Gestione degli eventi**

**Descrizione:** Verificare che il componente reagisca correttamente agli eventi emessi.\
**Passi:**

1. Emittere un evento `axisNumber` con un numero di assi.
2. Emittere un evento `mappedDataSend` con dati simulati.\
   **Risultato atteso:**

* `axisNumberPrototype` viene aggiornato correttamente.
* I dati mappati vengono salvati nella variabile `mappedData`.

***

**TC3 - Lettura GPIO**

**Descrizione:** Verificare la gestione dei pin GPIO per la comunicazione SPI.\
**Passi:**

1. Simulare il valore del pin `gpioPinSpi` (ad esempio, impostandolo su 1).
2. Simulare un errore sul pin `gpioPinComm` (valore 1).\
   **Risultato atteso:**

* Quando `gpioPinSpi` è 1, il componente tenta di inviare dati.
* Quando `gpioPinComm` è 1, viene emesso un errore di comunicazione tramite `notifyError`.

***

**TC4 - Applicazione sensibilità joystick**

**Descrizione:** Verificare che i moltiplicatori di sensibilità del joystick vengano applicati correttamente.\
**Passi:**

1. Modificare il file `joystickSensitivity.json` con valori noti.
2. Simulare dati del joystick (`spaceMouse`) e inviare tramite SPI.\
   **Risultato atteso:**\
   I dati inviati vengono moltiplicati correttamente dai valori del file JSON e limitati a \[-100, 100].

***

**TC5 - Trasmissione dati SPI**

**Descrizione:** Verificare l'invio di dati tramite SPI al master e agli assi.\
**Passi:**

1. Simulare un dispositivo SPI master e assi slave.
2. Chiamare la funzione `sendData`.
3. Monitorare i dati inviati e ricevuti tramite SPI.\
   **Risultato atteso:**

* I buffer inviati hanno le lunghezze corrette (24 byte per il master, 20 per ogni asse).
* I dati ricevuti sono coerenti e vengono elaborati senza errori.

***

**TC6 - Gestione errori**

**Descrizione:** Verificare che gli errori vengano gestiti e notificati correttamente.\
**Passi:**

1. Simulare errori nella trasmissione SPI o nei GPIO.
2. Controllare i log e le notifiche emesse.\
   **Risultato atteso:**

* Viene chiamata la funzione `notifyError` con il codice e il messaggio appropriati.
* L'errore è identificabile nei log.

***

**TC7 - Interfaccia pubblica**

**Descrizione:** Verificare che le funzioni e le variabili esportate forniscano i dati corretti.\
**Passi:**

1. Eseguire la funzione `sendData`.
2. Chiamare i metodi `getAxesX`, `getAxesY`, ecc.
3. Controllare il valore di variabili come `parity`, `actualPosition`, ecc.\
   **Risultato atteso:**

* I metodi restituiscono i valori aggiornati dopo la trasmissione.
* Le variabili riflettono lo stato corrente del sistema.

***

**TC8 - Prestazioni**

**Descrizione:** Misurare il tempo di esecuzione della funzione `sendData` per garantire che rientri nei limiti richiesti.\
**Passi:**

1. Attivare il monitoraggio del tempo con strumenti di debug.
2. Eseguire `sendData` in un loop continuo.\
   **Risultato atteso:**\
   Il tempo di esecuzione rimane costante e non introduce ritardi significativi (>100ms per iterazione).

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>19/11/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>19/11/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>19/11/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>19/11/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>19/11/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>19/11/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC7</td><td>19/11/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC8</td><td>19/11/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests SpiComm

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Inizializzazione SPI</td><td>Verificare che la configurazione SPI sia stabile e conforme ai parametri richiesti.</td></tr><tr><td align="center">RT2</td><td>Gestione degli eventi</td><td>Assicurarsi che il componente gestisca correttamente eventi esterni tramite gli emitter configurati.</td></tr><tr><td align="center">RT3</td><td>Lettura GPIO</td><td>Garantire che il monitoraggio dei pin GPIO rilevi correttamente gli stati e segnali eventuali errori.</td></tr><tr><td align="center">RT4</td><td>Applicazione sensibilita joystick</td><td>Confermare che i moltiplicatori di sensibilita siano applicati e limitati ai valori accettabili.</td></tr><tr><td align="center">RT5</td><td>Trasmissione dati SPI</td><td>Testare la correttezza della preparazione, formattazione e invio dei buffer dati tramite SPI.</td></tr><tr><td align="center">RT6</td><td>Prestazioni</td><td>Assicurarsi che il componente mantenga prestazione elevate, senza introdurre ritardi.</td></tr></tbody></table>
