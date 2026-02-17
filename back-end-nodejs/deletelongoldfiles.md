# DeleteLongOldFiles

## **Requirements**

1. **Funzione `deleteLongOldFiles`**
   * **Descrizione:** Questa funzione si occupa di eliminare file obsoleti o con nomi non validi dalla directory `./longTermLog/`.
   * **Comportamento:**
     * Deve leggere il contenuto della directory `./longTermLog/`.
     * Deve eliminare file con i seguenti criteri:
       * File creati più di un mese fa rispetto alla data attuale.
       * File il cui nome contiene la stringa `"undefined"`.
     * Deve gestire eventuali errori durante:
       * La lettura della directory.
       * L'eliminazione dei file.
     * Deve utilizzare la funzione `notifyError` per notificare errori con codici specifici:
       * **47:** Errore durante la lettura della directory.
       * **48:** Errore durante l'eliminazione di un file vecchio.
       * **49:** Errore durante l'eliminazione di un file con il nome `"undefined"`.

***

## **Test**

**Test Case**

1. **TC1 - Lettura corretta della directory**
   * **Descrizione:** Verificare che la funzione legga correttamente i file presenti nella directory `./longTermLog/`.
   * **Passi:**
     1. Popolare la directory `./longTermLog/` con alcuni file di esempio.
     2. Chiamare la funzione `deleteLongOldFiles`.
     3. Verificare che i file siano letti senza errori.
   * **Risultato atteso:** La directory viene letta correttamente e non viene invocata `notifyError` con codice 47.
2. **TC2 - Eliminazione di file vecchi**
   * **Descrizione:** Verificare che la funzione elimini file creati più di un mese fa.
   * **Passi:**
     1. Creare file nella directory `./longTermLog/` con timestamp di creazione superiori e inferiori a un mese fa.
     2. Chiamare la funzione `deleteLongOldFiles`.
     3. Verificare che i file più vecchi di un mese siano stati eliminati.
   * **Risultato atteso:** Solo i file più vecchi di un mese vengono eliminati.
3. **TC3 - Eliminazione di file con nome `"undefined"`**
   * **Descrizione:** Verificare che la funzione elimini file il cui nome contiene la stringa `"undefined"`.
   * **Passi:**
     1. Creare file con nomi validi e file con la stringa `"undefined"` nella directory `./longTermLog/`.
     2. Chiamare la funzione `deleteLongOldFiles`.
     3. Verificare che i file con la stringa `"undefined"` siano stati eliminati.
   * **Risultato atteso:** Solo i file contenenti `"undefined"` nel nome vengono eliminati.
4. **TC4 - Gestione degli errori durante la lettura della directory**
   * **Descrizione:** Verificare che la funzione notifichi un errore se la directory `./longTermLog/` non può essere letta.
   * **Passi:**
     1. Rimuovere i permessi di lettura dalla directory `./longTermLog/`.
     2. Chiamare la funzione `deleteLongOldFiles`.
     3. Verificare che venga invocata `notifyError` con il codice 47.
   * **Risultato atteso:** L'errore viene notificato correttamente.
5. **TC5 - Gestione degli errori durante l'eliminazione dei file**
   * **Descrizione:** Verificare che la funzione notifichi un errore se un file non può essere eliminato.
   * **Passi:**
     1. Creare un file nella directory `./longTermLog/` con permessi di sola lettura.
     2. Chiamare la funzione `deleteLongOldFiles`.
     3. Verificare che venga invocata `notifyError` con il codice 48 o 49, a seconda del tipo di file.
   * **Risultato atteso:** L'errore viene notificato correttamente.
6. **TC6 - Gestione dei file non esistenti**
   * **Descrizione:** Verificare che la funzione gestisca correttamente i file che non esistono più al momento dell'eliminazione.
   * **Passi:**
     1. Creare un file nella directory `./longTermLog/`.
     2. Eliminare manualmente il file prima che la funzione `fs.unlink` venga eseguita.
     3. Chiamare la funzione `deleteLongOldFiles`.
     4. Verificare che l'errore con codice `ENOENT` venga gestito senza invocare `notifyError`.
   * **Risultato atteso:** L'assenza del file non genera un errore.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>21/07/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>21/07/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>21/07/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>21/07/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>21/07/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>21/07/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>
