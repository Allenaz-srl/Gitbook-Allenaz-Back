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

# ErrorNotify

## **Requirements**

1. **Funzione `notifyError(id, errorMessage, sysErrorMsg)`**
   * **Descrizione:** La funzione `notifyError` è responsabile per la gestione degli errori nel sistema. Quando viene chiamata, la funzione stampa un messaggio di errore nella console, memorizza l'errore in un buffer e emette un evento con i dettagli dell'errore.
   * **Comportamento:**
     * Quando viene chiamata, la funzione deve ricevere un ID dell'errore, un messaggio di errore e (opzionalmente) un messaggio di errore del sistema (`sysErrorMsg`).
     * Il messaggio di errore deve essere stampato nella console. Se è presente un messaggio di errore di sistema, deve essere incluso nel log.
     * L'errore deve essere memorizzato nel buffer `errorServerMessageBuffer` come un oggetto contenente l'ID, il messaggio e l'eventuale messaggio di errore del sistema.
     * Deve emettere un evento chiamato `"errorServerMessage"` con il buffer dell'errore come argomento, consentendo ad altri componenti di ascoltare e gestire l'errore.
     * Il messaggio di errore deve essere stampato anche nel log della console, e il buffer dell'errore deve essere visibile tramite un log con il formato `"Buffered error message:"`.
   * **Comportamento Atteso:** La funzione deve correttamente stampare i messaggi di errore, memorizzare le informazioni nel buffer e emettere l'evento appropriato per notificare il sistema di un errore.

***

## **Test**

**Test Case**

1. **TC1 - Notifica di errore con messaggio di sistema**
   * **Descrizione:** Verificare che la funzione `notifyError()` notifichi correttamente un errore, includendo il messaggio di errore di sistema.
   * **Passi:**
     1. Chiamare `notifyError()` con un ID di errore, un messaggio di errore e un messaggio di errore del sistema.
     2. Verificare che il messaggio di errore venga stampato correttamente nella console, includendo anche il messaggio di errore del sistema.
     3. Verificare che l'errore venga memorizzato nel buffer e che l'evento `"errorServerMessage"` venga emesso.
   * **Risultato atteso:** Il messaggio di errore deve essere visibile nella console, il buffer deve contenere l'errore e l'evento `"errorServerMessage"` deve essere emesso.
2. **TC2 - Notifica di errore senza messaggio di sistema**
   * **Descrizione:** Verificare che la funzione `notifyError()` notifichi correttamente un errore, ma senza includere il messaggio di errore del sistema quando non è presente.
   * **Passi:**
     1. Chiamare `notifyError()` con un ID di errore e un messaggio di errore, senza fornire un messaggio di errore del sistema.
     2. Verificare che il messaggio di errore venga stampato nella console, senza il messaggio di errore del sistema.
     3. Verificare che l'errore venga memorizzato nel buffer e che l'evento `"errorServerMessage"` venga emesso.
   * **Risultato atteso:** Il messaggio di errore deve essere visibile nella console e il buffer deve contenere l'errore. L'evento `"errorServerMessage"` deve essere emesso senza il messaggio di errore del sistema.
3. **TC3 - Verifica dell'evento emesso**
   * **Descrizione:** Verificare che l'evento `"errorServerMessage"` venga correttamente emesso con il buffer dell'errore.
   * **Passi:**
     1. Creare un listener per l'evento `"errorServerMessage"` tramite `eventEmitterError.on()`.
     2. Chiamare `notifyError()` con un ID di errore, un messaggio di errore e (facoltativamente) un messaggio di errore del sistema.
     3. Verificare che il listener riceva il buffer dell'errore emesso come argomento.
   * **Risultato atteso:** Il listener deve ricevere correttamente l'errore emesso e il buffer deve essere visibile nel listener.
4. **TC4 - Verifica del contenuto del buffer**
   * **Descrizione:** Verificare che il buffer dell'errore contenga correttamente l'ID, il messaggio di errore e il messaggio di errore del sistema (se fornito).
   * **Passi:**
     1. Chiamare `notifyError()` con un errore e un messaggio di errore del sistema.
     2. Verificare che il buffer contenga le informazioni corrette (ID, messaggio di errore e messaggio di errore del sistema).
     3. Chiamare `notifyError()` senza il messaggio di errore del sistema e verificare che il buffer contenga solo l'ID e il messaggio di errore.
   * **Risultato atteso:** Il buffer deve contenere correttamente le informazioni relative all'errore, inclusi l'ID, il messaggio di errore e, se presente, il messaggio di errore del sistema.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests ErrorNotify

<table><thead><tr><th width="79" align="center">ID</th><th width="186">Test</th><th width="300">Passi</th><th>Risultato Atteso</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Verificare che la funzione <strong><code>notifyError()</code></strong> stampi correttamente il messaggio di errore con il messaggio di sistema</td><td><ul><li>Chiamare <strong><code>notifiError()</code></strong> con un ID, un messaggio di errore e un messaggio di errore di sistema</li><li>Verificare che entrambi i messaggi siano stampati in console</li></ul></td><td>Il messaggio di errore e il messaggio di sistema vengono stampati correttamente nella console</td></tr><tr><td align="center">RT2</td><td>Verificare che la funzione <strong><code>notifyError()</code></strong> gestisca correttamente un errore senza messaggio di sistema</td><td><ul><li>Chiamare <strong><code>notifyError()</code></strong> con un ID e un messaggio di errore, senza il messaggio di sistema</li><li>Controllare che in console venga stampato solo il messaggio di errore principale</li></ul></td><td>Il messaggio di errore viene stampato senza il messaggio di errore di sistema</td></tr><tr><td align="center">RT3</td><td>Verificare che l'errore venga correttamente memorizzato nel buffer <strong><code>errorServerMessageBuffer</code></strong></td><td><ul><li>Chiamare <strong><code>notifiError()</code></strong> con un ID, un messaggio di errore e (facoltativamente) un messaggio di errore di sitema</li><li>Verificare che il buffer contenga i dati corretti</li></ul></td><td>Il buffer deve contenere ID, messaggio di errore e, se presente, il messaggio di sistema</td></tr><tr><td align="center">RT4</td><td>Verificare che il buffer dell'errore non venga sovrascritto se vengono notificati piu errori consecutivi</td><td><ul><li>Chiamare <strong><code>notifiError()</code></strong> piu volte con ID e messaggio di errore devirsi</li><li>Verificare che il buffer mantenga tutte le informazioni senza sovrascriverle</li></ul></td><td>Il buffer mantiene correttamente tutte le notifiche di errore senza perdere dati</td></tr><tr><td align="center">RT5</td><td>Verificare che l'evento <strong><code>errorServerMessaggio</code></strong> venga emesso correttamente quando si verifica un errore</td><td><ul><li>Creare un listener per l'evento errorServerMessage</li><li>Chiamare <strong><code>notifyError()</code></strong> con ID e messaggio di errore</li><li>Verificare che l'evento venga ricevuto dal listener</li></ul></td><td>Il listener riceve correttamente l'evento con i dettagli dell'errore</td></tr><tr><td align="center">RT6</td><td>Verificare ch l'evento <strong><code>errorServerMessaggio</code></strong> contenga il buffer corretto</td><td><ul><li>Creare un listener per l'evento <strong><code>errorServerMessage</code></strong></li><li>Chiamare <strong><code>notifyError()</code></strong> e verificare il contenuto ricevuto dall'evento</li></ul></td><td>Il listener riceve esattamente i dati del buffer degli errori</td></tr><tr><td align="center">RT7</td><td>Testare il comportamento di <strong><code>notifyError()</code></strong> con input non validi</td><td><ul><li>Chiamare <strong><code>notifyError()</code></strong> senza fornire ID o messaggio di errore</li><li>Controllare che venga gestito correttamente senza causare crash</li></ul></td><td>La funzione gestisce l'input non valido senza errori critici</td></tr><tr><td align="center">RT8</td><td>Verificare la corretta stampa del log "Buffered error message:"</td><td><ul><li>Chiamare <strong><code>notifyError()</code></strong> con un errore</li><li>Controllare la console per il log "Buffered error message:" e il contenuto del buffer</li></ul></td><td>Il messaggio "Buffered error message:" appare correttamente in console</td></tr><tr><td align="center">RT9</td><td>Testare la gestione di errori in rapida successione</td><td><ul><li>Chiamare <strong><code>notifyError()</code></strong> ripetutamente in un breve intervallo di tempo</li><li>Controllare che il buffer e i log siano aggiornati correttamente</li></ul></td><td>Il sistema registra correttamente errori consecutivi senza perdita di dati o rallentamenti</td></tr></tbody></table>
