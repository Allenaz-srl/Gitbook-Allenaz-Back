# SpaceMouse

## **Requirements**

Il componente "SpaceMouse" deve:

1. **Riconoscimento dispositivo:**
   * Rilevare i dispositivi supportati ("SpaceMouse Compact", "3Dconnexion Universal Receiver").
   * Segnalare errori se il dispositivo non è collegato o non è supportato.
2. **Caricamento scenario precedente:**
   * Leggere e analizzare il file `lastScenario.json`.
   * Trasformare i dati dello scenario in un formato utilizzabile.
   * Gestire dati di inversione se presenti.
3. **Trasformazione dati:**
   * Elaborare i dati ricevuti dal dispositivo SpaceMouse, applicando eventuali trasformazioni basate sulle impostazioni di lavoro (`workingMode`) e inversione.
4. **Mappatura dei dati:**
   * Convertire i dati del dispositivo SpaceMouse in un formato leggibile (`x`, `y`, `z`, `a`, `b`, `c`, `btnR`, `btnL`).
5. **Gestione modalità operative:**
   * Supportare il passaggio tra modalità di lavoro configurate tramite il pulsante destro (`btnR`).
6. **Gestione degli eventi:**
   * Emulare eventi tramite `eventEmitter` per inviare i dati mappati agli altri moduli del sistema.
7. **Gestione errori:**
   * Notificare eventuali errori durante l'elaborazione dei dati, il rilevamento del dispositivo o il caricamento dei file.

## Test

#### **Test (Casi di test):**

**Caso 1:** Dispositivo non collegato.

* **Input:** Nessun dispositivo collegato.
* **Output atteso:** Notifica di errore "**Device not found**".

**Caso 2:** Dispositivo non supportato.

* **Input:** Dispositivo collegato non presente nella lista `supportedProducts`.
* **Output atteso:** Notifica di errore "Device not found".

**Caso 3:** `lastScenario.json` mancante o corrotto.

* **Input:** File inesistente o con dati non validi.
* **Output atteso:** Notifica di errore "Failed to load lastScenario.json".

**Caso 4:** Valori negativi dal dispositivo.

* **Input:** SpaceMouse invia valori negativi per `x`, `y`, `z`, `a`, `b`, `c`.
* **Output atteso:** I dati mappati riflettono correttamente i valori negativi, con inversione applicata se configurata.

**Caso 5:** Pulsante destro (`btnR`) premuto.

* **Input:** SpaceMouse invia segnale che `btnR` è attivo.
* **Output atteso:** Modalità operativa aggiornata al prossimo indice (`currentModeIndex`).

**Caso 6:** Errore durante la lettura dei dati dal dispositivo.

* **Input:** SpaceMouse invia dati corrotti o in formato non valido.
* **Output atteso:** Notifica di errore "Wrong data from \[deviceName]".

**Caso 7:** Nessuna inversione configurata.

* **Input:** SpaceMouse invia dati, ma non sono presenti impostazioni di inversione.
* **Output atteso:** I dati mappati riflettono i valori del dispositivo senza inversione.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>21/03/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>21/03/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>21/03/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>23/03/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>23/03/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>23/03/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC7</td><td>23/03/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests SpaceMouse

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Dispositivo non collegato</td><td>Verificare che il sistema gestisca correttamente l'assenza di un dispositivo collegato.</td></tr><tr><td align="center">RT2</td><td>Dispositivo non supportato</td><td>Assicurarsi che il componente segnali correttamente l'uso di un dispositivo non supportato.</td></tr><tr><td align="center">RT3</td><td><code>lastScenario.json</code> mancante o corrotto</td><td>Garantire che il caricamento dello scenario precedente gestisca file mancanti o non validi.</td></tr><tr><td align="center">RT4</td><td>Valori negativi dal dispositivo</td><td>Testare che i valori negativi vengono mappati e trasformati correttamente, considerando l'inversione.</td></tr><tr><td align="center">RT5</td><td>Pulsante destro (btnR) premuto</td><td>Verificare la corretta gestione della transizione tra modalita operative configurate.</td></tr><tr><td align="center">RT6</td><td>Errore durante la lettura dei dati</td><td>Assicurarsi che errori nei dati ricevuti dal dispositivo vengano gestiti con notifiche appropriate.</td></tr><tr><td align="center">RT7</td><td>Nessun inversione configurata</td><td>Controllare che i dati vengano processati senza inversione se non configurata.</td></tr></tbody></table>
