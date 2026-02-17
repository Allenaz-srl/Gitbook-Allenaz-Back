# DeleteOldFiles

## **Requirements**

1. **Funzione `deleteOldFiles`**
   * **Descrizione:** Questa funzione si occupa di eliminare file vecchi presenti nella directory `./logs/` che sono stati creati più di un'ora fa.
   * **Comportamento:**
     * Deve leggere il contenuto della directory `./logs/`.
     * Deve identificare i file creati più di un'ora prima del momento corrente.
     * Deve eliminare i file che soddisfano il criterio sopra descritto.
     * Deve gestire eventuali errori durante:
       * La lettura della directory.
       * L'eliminazione dei file.
     * Deve utilizzare la funzione `notifyError` per notificare errori con codici specifici:
       * **50:** Errore durante la lettura della directory.
       * **51:** Errore durante l'eliminazione di un file (esclusi i casi in cui il file non esiste più).

***

## **Test**

**Test Case**

1. **TC1 - Lettura corretta della directory**
   * **Descrizione:** Verificare che la funzione legga correttamente i file presenti nella directory `./logs/`.
   * **Passi:**
     1. Popolare la directory `./logs/` con alcuni file di esempio.
     2. Chiamare la funzione `deleteOldFiles`.
     3. Verificare che i file siano letti senza errori.
   * **Risultato atteso:** La directory viene letta correttamente e non viene invocata `notifyError` con codice 50.
2. **TC2 - Eliminazione di file vecchi**
   * **Descrizione:** Verificare che la funzione elimini file creati più di un'ora prima.
   * **Passi:**
     1. Creare file nella directory `./logs/` con timestamp di creazione maggiori e minori di un'ora fa.
     2. Chiamare la funzione `deleteOldFiles`.
     3. Verificare che i file più vecchi di un'ora siano stati eliminati.
   * **Risultato atteso:** Solo i file più vecchi di un'ora vengono eliminati.
3. **TC3 - Gestione degli errori durante la lettura della directory**
   * **Descrizione:** Verificare che la funzione notifichi un errore se la directory `./logs/` non può essere letta.
   * **Passi:**
     1. Rimuovere i permessi di lettura dalla directory `./logs/`.
     2. Chiamare la funzione `deleteOldFiles`.
     3. Verificare che venga invocata `notifyError` con il codice 50.
   * **Risultato atteso:** L'errore viene notificato correttamente.
4. **TC4 - Gestione degli errori durante l'eliminazione dei file**
   * **Descrizione:** Verificare che la funzione notifichi un errore se un file non può essere eliminato.
   * **Passi:**
     1. Creare un file nella directory `./logs/` con permessi di sola lettura.
     2. Chiamare la funzione `deleteOldFiles`.
     3. Verificare che venga invocata `notifyError` con il codice 51.
   * **Risultato atteso:** L'errore viene notificato correttamente.
5. **TC5 - Gestione dei file non esistenti**
   * **Descrizione:** Verificare che la funzione gestisca correttamente i file che non esistono più al momento dell'eliminazione.
   * **Passi:**
     1. Creare un file nella directory `./logs/`.
     2. Eliminare manualmente il file prima che la funzione `fs.unlink` venga eseguita.
     3. Chiamare la funzione `deleteOldFiles`.
     4. Verificare che l'errore con codice `ENOENT` venga gestito senza invocare `notifyError`.
   * **Risultato atteso:** L'assenza del file non genera un errore.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>19/07/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>19/07/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>19/07/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>19/07/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>19/07/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>
