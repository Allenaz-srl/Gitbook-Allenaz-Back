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

# IpAddress

## **Requirements**

1. **Funzione `getLocalIpAddress()`**
   * **Descrizione:** La funzione `getLocalIpAddress` è progettata per determinare l'indirizzo IP locale (IPv4) della macchina in cui è in esecuzione. Utilizza il modulo `os` di Node.js per ottenere le informazioni sulle interfacce di rete e cerca l'indirizzo IP esterno (non interno) di tipo IPv4.
   * **Comportamento:**
     * La funzione deve accedere alle interfacce di rete locali utilizzando `os.networkInterfaces()`.
     * Per ciascuna interfaccia, deve verificare se è di tipo `IPv4` e se non è un'interfaccia interna (ad esempio, un'interfaccia di loopback).
     * Una volta trovato l'indirizzo IP valido, deve restituirlo come risultato.
     * Se non viene trovato un indirizzo IP valido, deve chiamare la funzione `notifyError` con un codice di errore 52 per notificare che l'indirizzo IP locale non può essere determinato.
     * In caso di errore durante l'esecuzione della funzione (ad esempio, se si verifica un'eccezione), deve chiamare `notifyError` con un codice di errore 53, riportando il messaggio di errore generato dall'eccezione.
   * **Comportamento Atteso:** La funzione deve restituire correttamente l'indirizzo IP locale in formato IPv4, o null in caso di errore. Se si verificano errori, devono essere notificati tramite il sistema di gestione degli errori.

***

## **Test**

**Test Case**

1. **TC1 - Recupero dell'indirizzo IP locale valido**
   * **Descrizione:** Verificare che la funzione `getLocalIpAddress()` restituisca correttamente un indirizzo IP locale valido di tipo IPv4.
   * **Passi:**
     1. Assicurarsi che la macchina su cui viene eseguito il test abbia un'interfaccia di rete attiva e configurata con un indirizzo IPv4.
     2. Chiamare `getLocalIpAddress()`.
     3. Verificare che la funzione restituisca un indirizzo IP non nullo.
   * **Risultato atteso:** La funzione restituisce l'indirizzo IP locale valido (esempio: `192.168.x.x`).
2. **TC2 - Nessun indirizzo IP trovato**
   * **Descrizione:** Verificare che la funzione chiami `notifyError(52, "Unable to determine local IP address")` quando non viene trovato un indirizzo IP locale.
   * **Passi:**
     1. Assicurarsi che la macchina non abbia interfacce di rete attive o configurate correttamente.
     2. Chiamare `getLocalIpAddress()`.
     3. Verificare che venga emesso un errore con il codice 52 e il messaggio "Unable to determine local IP address".
   * **Risultato atteso:** La funzione restituisce `null` e viene chiamata la funzione `notifyError` con i parametri corretti.
3. **TC3 - Errore durante il recupero dell'indirizzo IP**
   * **Descrizione:** Verificare che la funzione chiami `notifyError(53, "Error getting local IP address", <messaggio errore>)` se si verifica un errore imprevisto durante l'esecuzione.
   * **Passi:**
     1. Simulare un errore (per esempio, simulando un'eccezione durante l'esecuzione di `os.networkInterfaces()`).
     2. Chiamare `getLocalIpAddress()`.
     3. Verificare che venga emesso un errore con il codice 53, includendo il messaggio di errore appropriato.
   * **Risultato atteso:** La funzione restituisce `null` e viene chiamata la funzione `notifyError` con il codice 53 e il messaggio dell'errore.
4. **TC4 - Verifica che la funzione non includa indirizzi IP interni**
   * **Descrizione:** Verificare che la funzione non restituisca indirizzi IP di interfacce di rete interne (come `127.0.0.1`).
   * **Passi:**
     1. Chiamare `getLocalIpAddress()` su una macchina con un'interfaccia di loopback configurata.
     2. Verificare che l'indirizzo IP restituito non sia di tipo `127.0.0.1`.
   * **Risultato atteso:** La funzione restituisce un indirizzo IP esterno (non `127.0.0.1`).

*   ### Result

    <table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>28/05/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>
