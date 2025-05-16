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

# WarningLogic

## **Requirements**

1. **Funzione `processWarningData()`**
   * **Descrizione:** La funzione deve elaborare i dati di avviso in base ai codici di avviso ricevuti e agli assi associati.
   * **Comportamento:**
     * Se viene rilevato l'avviso con valore `1` e uno dei codici di avviso è inferiore a 16000, la funzione deve aggiungere voci di avviso nel database locale `warningData`.
     * Se il codice di avviso è maggiore di 16000, la funzione deve rimuovere le voci di avviso corrispondenti dal `warningData`.
     * Se il codice di avviso è uguale a `0`, deve rimuovere tutte le voci di avviso associate a un asse specifico.
   * **Comportamento Atteso:**
     * I dati di avviso devono essere gestiti correttamente in base ai codici di avviso. Vengono aggiunti o rimossi da `warningData` in modo appropriato.
2. **Funzione `getWarningEntry(warningId, axis)`**
   * **Descrizione:** La funzione deve creare una voce di avviso se non esiste già una corrispondente nel `warningData`.
   * **Comportamento:**
     * Verifica se la voce di avviso esiste già in `warningData`. Se esiste, non la aggiunge.
     * Recupera i dettagli dell'avviso dal file `warningDB.json`, che include descrizione, soluzione, causa e messaggio.
     * Se l'avviso è valido, restituisce una voce di avviso formattata, comprensiva di timestamp.
   * **Comportamento Atteso:**
     * La funzione restituisce una voce di avviso formattata correttamente. In caso di errore, restituisce `null` e invia una notifica.
3. **Funzione `getFirstWarningTime(warningId, axis)`**
   * **Descrizione:** La funzione deve restituire il timestamp della prima volta che un avviso per un determinato asse è stato registrato.
   * **Comportamento:** Se la voce di avviso esiste già in `warningData`, restituisce il suo timestamp. Se non esiste, restituisce il timestamp corrente.
   * **Comportamento Atteso:** La funzione restituisce il timestamp del primo avviso. In caso di errore, restituisce il timestamp corrente.
4. **Funzione `getFormattedTime(timestamp)`**
   * **Descrizione:** La funzione deve formattare un timestamp in una stringa leggibile, nel formato `hh:mm:ss:SSS`.
   * **Comportamento:** Verifica se il timestamp è valido e lo formatta correttamente. In caso contrario, restituisce "Invalid timestamp".
   * **Comportamento Atteso:** La funzione restituisce una stringa contenente l'ora, il minuto, il secondo e i millisecondi. Se il timestamp non è valido, restituisce una stringa di errore.

***

## **Test**

**Test Case**

1. **TC1 - Elaborazione dei dati di avviso**
   * **Descrizione:** Verificare che la funzione `processWarningData()` gestisca correttamente l'aggiunta, la rimozione e la gestione degli avvisi.
   * **Passi:**
     1. Impostare `warning` con il valore `1` e `warningCode` con valori inferiori a 16000.
     2. Chiamare la funzione `processWarningData()`.
     3. Verificare che i dati di avviso vengano correttamente aggiunti a `warningData`.
     4. Impostare `warningCode` con valori superiori a 16000 e chiamare nuovamente la funzione.
     5. Verificare che le voci di avviso corrispondenti vengano rimosse.
     6. Impostare `warningCode` con il valore `0` e verificare che tutte le voci relative agli assi vengano rimosse.
   * **Risultato atteso:** Gli avvisi devono essere correttamente aggiunti, rimossi o ignorati in base ai codici di avviso.
2. **TC2 - Creazione di una voce di avviso**
   * **Descrizione:** Verificare che la funzione `getWarningEntry()` crei correttamente una voce di avviso se non esiste già.
   * **Passi:**
     1. Chiamare la funzione `getWarningEntry()` con un `warningId` e un `axis` specifici.
     2. Verificare che una nuova voce di avviso venga aggiunta a `warningData` con i dati corretti, inclusi timestamp, descrizione, causa, soluzione e messaggio.
     3. Verificare che la funzione restituisca `null` se l'avviso esiste già.
   * **Risultato atteso:** Una voce di avviso viene creata e aggiunta a `warningData` se non esiste già, con tutti i dettagli correttamente formattati.
3. **TC3 - Recupero del timestamp del primo avviso**
   * **Descrizione:** Verificare che la funzione `getFirstWarningTime()` restituisca correttamente il timestamp del primo avviso.
   * **Passi:**
     1. Chiamare la funzione `getFirstWarningTime()` con un `warningId` e un `axis` specifici.
     2. Verificare che venga restituito il timestamp della prima occorrenza dell'avviso.
     3. Testare anche il caso in cui l'avviso non esista ancora e verificare che venga restituito il timestamp corrente.
   * **Risultato atteso:** La funzione restituisce il timestamp corretto della prima occorrenza dell'avviso.
4. **TC4 - Formattazione del timestamp**
   * **Descrizione:** Verificare che la funzione `getFormattedTime()` formatti correttamente il timestamp.
   * **Passi:**
     1. Passare un timestamp valido alla funzione `getFormattedTime()`.
     2. Verificare che venga restituito il timestamp formattato nel formato `hh:mm:ss:SSS`.
     3. Passare un timestamp non valido e verificare che venga restituito "Invalid timestamp".
   * **Risultato atteso:** La funzione formatta correttamente i timestamp validi e restituisce un errore per i timestamp non validi.

*   ### Result

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>
