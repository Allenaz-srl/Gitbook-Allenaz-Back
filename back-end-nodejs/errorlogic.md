# ErrorLogic

## **Requirements**

1. **Funzione `processErrorData()`**
   * **Descrizione:** La funzione deve elaborare i dati relativi agli errori, gestendo la creazione e la rimozione di voci di errore, nonché il conteggio delle iterazioni per ciascun errore.
   * **Comportamento:**
     * La funzione deve mantenere il conteggio delle iterazioni di ciascun errore e rimuovere gli errori che hanno superato una soglia di 50 iterazioni.
     * Deve rilevare gli errori attivi, basandosi sui codici di errore e sugli assi.
     * Gli errori devono essere aggiunti a `errorData` se non esistono già, altrimenti il conteggio delle iterazioni deve essere resettato.
     * Se un errore supera le 50 iterazioni, deve essere rimosso.
     * Se non ci sono errori, la funzione deve svuotare l'elenco `errorData`.
   * **Comportamento Atteso:** Gli errori devono essere correttamente aggiunti, gestiti e rimossi, mantenendo la coerenza del conteggio delle iterazioni.
2. **Funzione `writeErrorHistoryToFile()`**
   * **Descrizione:** La funzione deve scrivere gli errori storici in un file JSON e mantenere solo gli ultimi 10 errori.
   * **Comportamento:**
     * I nuovi errori devono essere aggiunti all'inizio della cronologia degli errori.
     * La cronologia deve essere limitata a un massimo di 10 voci.
     * In caso di errore durante la lettura o scrittura del file, devono essere inviate notifiche.
   * **Comportamento Atteso:** La cronologia degli errori deve essere correttamente aggiornata e archiviata, mantenendo al massimo 10 voci.
3. **Funzione `getErrorEntry(errorId, axis)`**
   * **Descrizione:** La funzione deve restituire una voce di errore formattata con i dettagli dell'errore, tra cui descrizione, causa, soluzione, e messaggio, se non esiste già una voce corrispondente.
   * **Comportamento:**
     * Verifica se esiste già una voce di errore corrispondente nel `errorData`. Se esiste, non aggiungerla.
     * Recupera i dettagli dell'errore dal database `errorDB.json` e li restituisce.
     * Se l'errore non è presente nel database, restituisce una voce di errore generica con un messaggio "Not found in DB".
   * **Comportamento Atteso:** La funzione deve restituire correttamente una voce di errore formattata con i dettagli, gestendo correttamente gli errori che non sono nel database.
4. **Funzione `getFormattedTime(timestamp)`**
   * **Descrizione:** La funzione deve formattare un timestamp in una stringa di tempo leggibile nel formato `hh:mm:ss:SSS`.
   * **Comportamento:** Verifica se il timestamp è valido e lo formatta correttamente. In caso contrario, restituisce "Invalid timestamp".
   * **Comportamento Atteso:** La funzione restituisce una stringa con l'orario formattato correttamente, oppure un messaggio di errore se il timestamp non è valido.
5. **Funzione `getFormattedDate(timestamp)`**
   * **Descrizione:** La funzione deve formattare un timestamp in una stringa di data nel formato `yyyy-mm-dd`.
   * **Comportamento:** Verifica se il timestamp è valido e lo formatta correttamente. In caso contrario, restituisce "Invalid timestamp".
   * **Comportamento Atteso:** La funzione restituisce una stringa con la data formattata correttamente, oppure un messaggio di errore se il timestamp non è valido.

***

## **Test**

**Test Case**

1. **TC1 - Elaborazione dei dati di errore**
   * **Descrizione:** Verificare che la funzione `processErrorData()` gestisca correttamente l'aggiunta, la rimozione e la gestione del conteggio delle iterazioni degli errori.
   * **Passi:**
     1. Impostare `error` con il valore `1` e `errorCode` con vari codici di errore.
     2. Chiamare la funzione `processErrorData()`.
     3. Verificare che gli errori vengano aggiunti a `errorData` e che il conteggio delle iterazioni venga correttamente aggiornato.
     4. Testare il comportamento con errori che superano 50 iterazioni e verificare che vengano rimossi.
   * **Risultato atteso:** Gli errori devono essere correttamente aggiunti, gestiti e rimossi, con il conteggio delle iterazioni correttamente aggiornato.
2. **TC2 - Scrittura della cronologia degli errori**
   * **Descrizione:** Verificare che la funzione `writeErrorHistoryToFile()` scriva correttamente gli errori storici in un file e limiti la cronologia a 10 voci.
   * **Passi:**
     1. Aggiungere nuovi errori e chiamare `writeErrorHistoryToFile()`.
     2. Verificare che la cronologia degli errori contenga al massimo 10 voci.
     3. Verificare che vengano registrati correttamente gli errori nel file `errorHistory.json`.
   * **Risultato atteso:** La cronologia degli errori deve essere correttamente aggiornata e limitata a un massimo di 10 voci.
3. **TC3 - Recupero di una voce di errore**
   * **Descrizione:** Verificare che la funzione `getErrorEntry()` restituisca correttamente i dettagli di un errore.
   * **Passi:**
     1. Chiamare la funzione `getErrorEntry()` con un `errorId` e un `axis` specifici.
     2. Verificare che vengano restituiti i dettagli corretti dell'errore, inclusi descrizione, causa, soluzione e messaggio.
     3. Verificare che la funzione restituisca una voce generica con "Not found in DB" se l'errore non esiste nel database.
   * **Risultato atteso:** La funzione deve restituire correttamente i dettagli dell'errore, o un errore generico se non presente nel database.
4. **TC4 - Formattazione del timestamp**
   * **Descrizione:** Verificare che la funzione `getFormattedTime()` formatti correttamente il timestamp.
   * **Passi:**
     1. Passare un timestamp valido alla funzione `getFormattedTime()`.
     2. Verificare che venga restituito il timestamp formattato nel formato `hh:mm:ss:SSS`.
     3. Passare un timestamp non valido e verificare che venga restituito "Invalid timestamp".
   * **Risultato atteso:** La funzione formatta correttamente i timestamp validi e restituisce un errore per i timestamp non validi.
5. **TC5 - Formattazione della data**
   * **Descrizione:** Verificare che la funzione `getFormattedDate()` formatti correttamente la data.
   * **Passi:**
     1. Passare un timestamp valido alla funzione `getFormattedDate()`.
     2. Verificare che venga restituita la data nel formato `yyyy-mm-dd`.
     3. Passare un timestamp non valido e verificare che venga restituito "Invalid timestamp".
   * **Risultato atteso:** La funzione restituisce una data formattata correttamente, oppure un messaggio di errore per i timestamp non validi.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>30/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>30/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>30/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>30/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>30/05/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests ErrorNotify

<table><thead><tr><th width="79" align="center">ID</th><th width="186">Test</th><th width="300">Passi</th><th>Risultato Atteso</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Verificare che la funzione <strong><code>processErrorData()</code></strong> aggiunga correttamente gli errori a <strong><code>errorData</code></strong></td><td><ul><li>Chiamare <strong><code>processErrorData()</code></strong> con un nuovo errore e codice errore</li><li>Verificare che l'errore sia presente in <strong><code>errorData</code></strong></li><li>Verificare che il conteggio delle iterazioni sia inizializzato a 1</li></ul></td><td>Il nuovo errore viene aggiunto a <strong><code>errorData</code></strong> con il conteggio delle iterazioni impostato a 1</td></tr><tr><td align="center">RT2</td><td>Verificare che la funzione <strong><code>processErrorData()</code></strong> aggiorni il conteggio delle iterazioni per errori esistenti</td><td><ul><li>Aggiungere un errore a <strong><code>errorData</code></strong></li><li>Chiamare <strong><code>processErrorData()</code></strong> con li stesso errore</li><li>Verificare che il conteggio delle iterazioni sia incrementato</li></ul></td><td>Il conteggio delle iterazioni dell'errore esistente viene aggiornato correttamente</td></tr><tr><td align="center">RT3</td><td>Verificare che <strong><code>processErrorData()</code></strong> svuoti <strong><code>errorData</code></strong> se non ci sono errori attivi</td><td><ul><li>Aggiungere errori a <strong><code>errorData</code></strong></li><li>Chiamare <strong><code>processErrorData()</code></strong> con nessun errore attivo</li><li>Verificare che <strong><code>errorData</code></strong> sia vuoto</li></ul></td><td><strong><code>errorData</code></strong> viene svuotato correttamente in assenza di errori attivi</td></tr><tr><td align="center">RT4</td><td>Verificare che <strong><code>writeErrorHistoryToFile()</code></strong> aggiunga nuovi errori in cima alla cronologia</td><td><ul><li>Aggiungere un nuovo errore</li><li>Chiamare <strong><code>writeErrorHistoryToFile()</code></strong></li><li>Verificare che l'errore piu recente sia il primo nel file <strong><code>errorHistory.json</code></strong></li></ul></td><td>Il nuovo errore viene scritto correttamente in cima alla cronologia</td></tr><tr><td align="center">RT5</td><td></td><td><ul><li></li></ul></td><td></td></tr><tr><td align="center">RT6</td><td></td><td><ul><li></li></ul></td><td></td></tr><tr><td align="center">RT7</td><td></td><td><ul><li></li></ul></td><td></td></tr><tr><td align="center">RT8</td><td></td><td><ul><li></li></ul></td><td></td></tr><tr><td align="center">RT9</td><td></td><td><ul><li></li></ul></td><td></td></tr></tbody></table>
