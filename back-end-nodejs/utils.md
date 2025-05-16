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

# Utils

## **Requirements**

1. **Funzione `shutdown()`**
   * **Descrizione:** La funzione deve spegnere correttamente il Raspberry Pi eseguendo un comando di spegnimento.
   * **Comportamento:** Quando viene chiamata, esegue il comando `sudo shutdown now`. In caso di errore, deve inviare una notifica di errore con il codice 70, descrivendo l'errore (messaggio).
   * **Comportamento Atteso:** Il Raspberry Pi deve essere spento senza errori. Se si verifica un errore, deve essere notificato con il messaggio di errore.
2. **Funzione `restartServer()`**
   * **Descrizione:** La funzione deve riavviare il server creando un file denominato `restart.js` nella stessa directory del modulo.
   * **Comportamento:** La funzione scrive il valore `1` nel file `restart.js`. In caso di errore durante il processo, deve inviare una notifica di errore con il codice 71, descrivendo l'errore.
   * **Comportamento Atteso:** Il file `restart.js` viene creato correttamente con il valore `1`. In caso di errore, viene inviata una notifica.
3. **Funzione `readFile(filePath)`**
   * **Descrizione:** La funzione deve leggere un file JSON dalla posizione specificata.
   * **Comportamento:** Se il file esiste, il contenuto viene letto e analizzato in un oggetto JSON. Se il file è vuoto, ritorna un array vuoto. Se il file non esiste o si verifica un errore, deve inviare una notifica di errore con il codice 72.
   * **Comportamento Atteso:** I dati letti devono essere restituiti come un oggetto JSON. In caso di errore o se il file non esiste, viene inviata una notifica.
4. **Funzione `formatExData(data)`**
   * **Descrizione:** La funzione deve formattare i dati passati in input, creando un oggetto con chiavi numeriche e valori corrispondenti.
   * **Comportamento:** Se i dati non sono un array, la funzione deve trasformarli in un array contenente un singolo elemento. Durante il processo di formattazione, se l'indirizzo è uguale a `4`, deve essere sostituito con `10`, e il valore deve essere impostato a `4`. In caso di errore, deve inviare una notifica di errore con il codice 73.
   * **Comportamento Atteso:** I dati devono essere restituiti come un oggetto con indirizzi numerici e valori associati. Se si verifica un errore, viene inviata una notifica.

***

## **Test**

**Test Case**

1. **TC1 - Spegnimento del Raspberry Pi**
   * **Descrizione:** Verificare che la funzione `shutdown()` spegne correttamente il Raspberry Pi.
   * **Passi:**
     1. Chiamare la funzione `shutdown()`.
     2. Verificare i log per il messaggio "Raspberry Pi shut down...".
     3. Verificare che non ci siano errori durante l'esecuzione.
   * **Risultato atteso:** Il Raspberry Pi si spegne correttamente e non si verificano errori.
2. **TC2 - Riavvio del server**
   * **Descrizione:** Verificare che la funzione `restartServer()` crei correttamente il file `restart.js`.
   * **Passi:**
     1. Chiamare la funzione `restartServer()`.
     2. Verificare che il file `restart.js` contenga il valore `1`.
     3. Verificare che non ci siano errori durante la creazione del file.
   * **Risultato atteso:** Il file `restart.js` viene creato correttamente con il valore `1`, senza errori.
3. **TC3 - Lettura di un file JSON**
   * **Descrizione:** Verificare che la funzione `readFile()` legga correttamente un file JSON.
   * **Passi:**
     1. Creare un file JSON di test con contenuti validi.
     2. Chiamare la funzione `readFile(filePath)` con il percorso del file.
     3. Verificare che i dati letti corrispondano a quelli nel file.
     4. Testare anche il caso in cui il file sia vuoto e verificare che venga restituito un array vuoto.
   * **Risultato atteso:** Il file viene letto correttamente come JSON e il contenuto restituito è valido. In caso di errore, viene notificato.
4. **TC4 - Formattazione dei dati**
   * **Descrizione:** Verificare che la funzione `formatExData()` formatti correttamente i dati.
   * **Passi:**
     1. Passare un array di oggetti con vari indirizzi e valori alla funzione `formatExData()`.
     2. Verificare che l'indirizzo `4` venga sostituito con `10` e che il valore venga modificato correttamente.
     3. Verificare che i dati vengano restituiti come un oggetto con chiavi numeriche.
     4. Testare anche il caso in cui si verifichi un errore (ad esempio passando dati non validi) e verificare che venga inviata una notifica di errore.
   * **Risultato atteso:** I dati vengono formattati correttamente e gli errori vengono gestiti correttamente.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>02/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>20/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>04/12/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>13/12/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests Utils

<table><thead><tr><th width="86" align="center">ID</th><th width="162">Test</th><th width="333">Passi</th><th>Risultato Atteso</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Verificare il corretto spegnimento del Raspberry PI.</td><td><ul><li>Chiamare la funzione <strong><code>shutdown()</code></strong></li><li>Verificare che il comando <strong><code>sudo shutdown now</code></strong> venga eseguito</li><li>Controllare i log di sistema</li></ul></td><td>Il Raspberry Pi si spegne correttamente e viene registrato un log.</td></tr><tr><td align="center">RT2</td><td>Gestione degli errori durante lo spegnimento del Raspberry Pi</td><td><ul><li>Simulare un errore nell'esecuzione del comando <strong><code>shutdown</code></strong></li><li>Verificare che venga inviata una notifica con codice errore 70</li></ul></td><td>L'errore viene notificato correttamente con codice 70</td></tr><tr><td align="center">RT3</td><td>Verificare il corretto riavvio del server tramite <strong><code>restartServer()</code></strong></td><td><ul><li>Chiamare <strong><code>restartServer()</code></strong></li><li>Verificare che il file <strong><code>restart.js</code></strong> venga creato e contenga il valore <strong><code>1</code></strong></li></ul></td><td>Il file viene modificato correttamente senza errori</td></tr><tr><td align="center">RT4</td><td>Gestione degli errori durante la creazione del file <strong><code>restart.js</code></strong></td><td><ul><li>Simulare un errore di scrittura su server</li><li>Chiamare <strong><code>restartServer()</code></strong> </li><li>Verificare che venga inviata una notificare con codice 71</li></ul></td><td>L'errore viene notificato correttamento con codice 71</td></tr><tr><td align="center">RT5</td><td>Verificare la corretta lettura di un file JSON valido</td><td><ul><li>Creare un file JSON con dati validi</li><li>Chiamare <strong><code>readFile(filePath)</code></strong></li><li>Confrontare i dati letti con il contenuto del file</li></ul></td><td>Il file viene letto correttamente e il contenuto restituito e valido</td></tr><tr><td align="center">RT6</td><td>Verificare il comportamento di <strong><code>readFile()</code></strong> con un file JSON vuoto</td><td><ul><li>Creare un file JSON vuoto</li><li>Chiamare <strong><code>readFile(filePath)</code></strong></li><li>Verificare che venga rstituito un array vuoto</li></ul></td><td>Viene restituito un array vuoto, senza errori</td></tr><tr><td align="center">RT7</td><td>Gestione degli errori durante la lettura di un file JSON inesistente</td><td><ul><li>Chiamare readFile() con un percorso inesistente</li><li>Verificare che venga inviata una notifica con codice errore 72</li></ul></td><td>L'errore viene notificato correttamente con codice 72</td></tr><tr><td align="center">RT8</td><td>Verificare la corretta formattazione dei dati con <strong><code>formatExData()</code></strong></td><td><ul><li>Passare un array di oggetti alla funzione</li><li>Controllare che l'indirizzo <strong><code>4</code></strong> venga trasformato in <strong><code>10</code></strong> e il valore sia impostato a <strong><code>4</code></strong></li></ul></td><td>I dati vengono formattati correttamente secondo le regole previste</td></tr><tr><td align="center">RT9</td><td>Gestione degli errori con dati non validi in <strong><code>formatExData()</code></strong></td><td><ul><li>Passare un valore non valido (es. una stringa)</li><li>Verificare che venga inviata una notifica con codice errore 73</li></ul></td><td>L'errore viene notificato correttamente con codice 73</td></tr><tr><td align="center">RT10</td><td>Validare il comportamento della funzione <strong><code>formatExData()</code></strong> con input che non sono array</td><td><ul><li>Passare un oggetto singolo invece di un array</li><li>Verificare che l'input venga trasformato in un array con un singolo elemento</li></ul></td><td>L'input viene convertito correttamente in un array prima della formattazione</td></tr></tbody></table>
