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

# ParamConnDB

## **Requirements**

1. **Connessione al database MariaDB**\
   Il componente deve gestire una connessione a un database MariaDB utilizzando un pool di connessioni.
2. **Individuazione dinamica dell'indirizzo IP locale**\
   L'indirizzo host per la connessione al database deve essere determinato dinamicamente tramite la funzione `getLocalIpAddress`.
3. **Parametri di connessione predefiniti**\
   La connessione al database deve utilizzare i seguenti parametri:
   * **Porta:** 3306
   * **Utente:** `user_hmi`
   * **Password:** `Allenaz.2023`
   * **Limite connessioni:** 100
4. **Gestione delle risorse**\
   Il componente deve utilizzare un pool di connessioni per ottimizzare le risorse e gestire un elevato numero di richieste simultanee.
5. **Modularità**\
   Il pool di connessioni deve essere esportato come modulo per l'uso in altre parti dell'applicazione.

***

## **Test**

**Test Case**

**TC1 - Creazione del pool di connessioni**\
**Descrizione:** Verificare che il pool di connessioni venga creato correttamente con i parametri forniti.\
**Passi:**

1. Inizializzare il componente.
2. Verificare che il pool di connessioni venga creato con:
   * Indirizzo host corrispondente a quello restituito da `getLocalIpAddress`.
   * Parametri di connessione predefiniti (porta, utente, password, limite connessioni).\
     **Risultato atteso:** Il pool viene creato senza errori e utilizza i parametri specificati.

***

**TC2 - Validazione dell'indirizzo IP**\
**Descrizione:** Verificare che l'indirizzo IP ottenuto da `getLocalIpAddress` venga utilizzato correttamente.\
**Passi:**

1. Simulare un indirizzo IP locale diverso da quello predefinito.
2. Inizializzare il componente.
3. Verificare che l'indirizzo IP utilizzato nel pool di connessioni corrisponda a quello restituito da `getLocalIpAddress`.\
   **Risultato atteso:** L'indirizzo IP nel pool coincide con quello fornito dalla funzione.

***

**TC3 - Connessione al database**\
**Descrizione:** Verificare che il pool consenta di stabilire connessioni valide al database.\
**Passi:**

1. Inizializzare il componente.
2. Richiedere una connessione dal pool.
3. Verificare che la connessione venga stabilita senza errori.\
   **Risultato atteso:** La connessione viene stabilita correttamente e non si verificano errori.

***

**TC4 - Gestione del limite di connessioni**\
**Descrizione:** Verificare che il componente gestisca correttamente il limite massimo di connessioni.\
**Passi:**

1. Inizializzare il componente.
2. Effettuare richieste simultanee superiori al limite di connessioni configurato (100).
3. Verificare che venga restituito un errore per le connessioni che superano il limite.\
   **Risultato atteso:** Il pool limita il numero di connessioni simultanee a 100.

***

**TC5 - Modularità**\
**Descrizione:** Verificare che il modulo esporti correttamente il pool di connessioni per l'uso in altre parti dell'applicazione.\
**Passi:**

1. Importare il componente in un'altra parte dell'applicazione.
2. Utilizzare il pool per effettuare una connessione al database.\
   **Risultato atteso:** Il modulo viene importato correttamente e il pool di connessioni funziona come previsto.

## Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>19/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>19/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>19/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>19/09/2023</td><td>Roman</td><td>true</td><td>Se avremo bisogno, possiamo alzare il numero di connessioni</td></tr><tr><td>TC5</td><td>21/09/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

## Regression tests

| **ID** | **Test**                             | **Motivo**                                                                                          |
| ------ | ------------------------------------ | --------------------------------------------------------------------------------------------------- |
| RT1    | Creazione del pool di connessioni    | Verificare che il pool venga creato correttamente con i parametri predefiniti e senza errori.       |
| RT2    | Validazione dell'indirizzo IP        | Garantire che l'indirizzo IP utilizzato sia determinato dinamicamente tramite `getLocalIpAddress`.  |
| RT3    | Connessione valida al database       | Assicurarsi che il pool consenta connessioni valide al database MariaDB.                            |
| RT4    | Modularità del componente            | Confermare che il pool sia esportato correttamente e utilizzabile in altre parti dell’applicazione. |
| RT5    | Gestione degli errori di connessione | Validare la gestione di errori, come credenziali errate o database non disponibile.                 |
