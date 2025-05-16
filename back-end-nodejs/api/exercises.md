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

# Exercises

{% hint style="info" %}
Questa pagina contiene i requisiti e le prove per tutti i "routes" nella sezione EXERCISES.
{% endhint %}

## **Route exAddName**

### **Requirements**

**Gestione dell'aggiunta di nomi degli esercizi:**

1. Accettare dati in formato JSON tramite una richiesta HTTP POST al percorso `/add_exercise_name`.
   * Campi richiesti:
     * `name` (stringa): Nome dell'esercizio da aggiungere.
     * `rep` (numero intero): Numero delle ripetizioni (non utilizzato direttamente nella query ma incluso per possibili estensioni future).
2. Eseguire la procedura `exo_exercises.load_exercise` utilizzando i parametri `name` e un valore costante `1` come flag.
3. Stabilire una connessione con il database MariaDB tramite il pool definito nel modulo `paramConnDB`.
4. Rilasciare correttamente la connessione al database dopo l'esecuzione della query.
5. Rispondere con un messaggio di successo se l'operazione è completata correttamente.
6. Gestire eventuali errori, registrandoli nella console e rispondendo con uno stato HTTP 500 e un messaggio di errore.

### **Test**

**Test Case 1: Aggiunta corretta del nome dell'esercizio**

* **Descrizione:** Verificare che la procedura `exo_exercises.load_exercise` aggiunga correttamente il nome e il flag al database.
* **Passi:**
  1. Inviare una richiesta POST a `/add_exercise_name` con i parametri validi `name` e `rep`.
  2. Controllare che il nome dell'esercizio sia presente nel database.
* **Risultato atteso:** Il nome viene aggiunto con successo e viene restituito un messaggio `{ "message": "Exercise name and rep added successfully" }`.

**Test Case 2: Gestione degli errori del database**

* **Descrizione:** Simulare un errore durante la connessione o l'esecuzione della query.
* **Passi:**
  1. Configurare il pool del database con credenziali errate o disabilitare il database temporaneamente.
  2. Inviare una richiesta POST a `/add_exercise_name`.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 e un messaggio `{ "message": "Error adding exercise name and rep" }`.

**Test Case 3: Validazione dei dati in ingresso**

* **Descrizione:** Verificare il comportamento quando i dati richiesti non vengono forniti o sono errati.
* **Passi:**
  1. Inviare una richiesta POST a `/add_exercise_name` con parametri mancanti o vuoti (`name` e/o `rep`).
  2. Controllare la risposta e il comportamento dell'API.
* **Risultato atteso:** Il server restituisce un messaggio d'errore appropriato o gestisce correttamente l'assenza di dati.

### Result **exAddName**

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests **exAddName**

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Aggiunta corretta del nome dell'esercizio</td><td>Verificare che la procedura <code>load_exercise</code> aggiunga correttamente il nome e il flag al database.</td></tr><tr><td align="center">RT2</td><td>Gestione errori del database</td><td>Assicurarsi che l'API gestisca correttamente errori di connessione o esecuzione della query nel DB.</td></tr><tr><td align="center">RT3</td><td>Validazione dei dati in ingresso</td><td>Confermare che l'API risponda in modo appropriato quando mancano parametri obbligatori (<code>name</code>, <code>rep</code>).</td></tr></tbody></table>

***

## **Route exDataPos**

### **Requirements**

**Gestione del recupero dati esercizi:**

1. Accettare richieste HTTP POST al percorso `/exercises_data`.
   * Campo richiesto in ingresso:
     * `name` (stringa): Nome dell'esercizio per filtrare i dati.
2. Eseguire una query SQL sulla vista `exo_exercises.view_positions` per recuperare tutte le posizioni correlate al nome fornito.
   * Applicare un filtro `LIKE` basato sul parametro `name`.
   * Ordinare i risultati in base alla colonna `order_pos`.
3. Restituire i dati ottenuti dal database come risposta JSON.
4. Stampare i risultati della query sulla console per verifiche durante l'esecuzione.
5. Gestire eventuali errori:
   * Registrare l'errore nella console.
   * Rispondere con uno stato HTTP 500 e un messaggio di errore appropriato.

### **Test**

**Test Case 1: Recupero corretto dei dati**

* **Descrizione:** Verificare che la query restituisca correttamente le posizioni associate al nome dell'esercizio.
* **Passi:**
  1. Popolare la vista `exo_exercises.view_positions` con dati di esempio contenenti diversi esercizi.
  2. Inviare una richiesta POST a `/exercises_data` con un valore valido per `name`.
  3. Controllare che i risultati siano corretti e ordinati per `order_pos`.
* **Risultato atteso:** I dati corretti vengono restituiti come array JSON.

**Test Case 2: Assenza di corrispondenze**

* **Descrizione:** Verificare il comportamento quando non ci sono posizioni associate al nome fornito.
* **Passi:**
  1. Inviare una richiesta POST a `/exercises_data` con un valore di `name` che non esiste nella vista.
  2. Controllare la risposta del server.
* **Risultato atteso:** La risposta è un array vuoto `[]`.

**Test Case 3: Gestione di errori SQL**

* **Descrizione:** Simulare un errore di query per verificare la gestione degli errori.
* **Passi:**
  1. Modificare temporaneamente la vista o il database per causare un errore nella query (ad esempio, rimuovere la vista).
  2. Inviare una richiesta POST a `/exercises_data`.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 e il messaggio `{ "message": "Error fetching exercise names" }`.

**Test Case 4: Validazione dei dati in ingresso**

* **Descrizione:** Verificare il comportamento quando il campo `name` non viene fornito o è vuoto.
* **Passi:**
  1. Inviare una richiesta POST a `/exercises_data` senza il campo `name` o con un valore vuoto.
  2. Controllare la risposta del server e il comportamento.
* **Risultato atteso:** Il server restituisce un messaggio d'errore appropriato o un array vuoto.

### Result **exDataPos**

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests **exDataPos**

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Recupero corretto dei dati</td><td>Verificare che i dati filtrati per il parametro <code>name</code> vengano restituiti e ordinati correttamente.</td></tr><tr><td align="center">RT2</td><td>Assenza di corrispondenze</td><td>Assicurarsi che l'API restituisca un array vuoto quando non ci sono dati associati al <code>name</code>.</td></tr><tr><td align="center">RT3</td><td>Gestione di errori SQL</td><td>Confermare che errori di query o problemi con il database siano gestiti con un messaggio HTTP 500.</td></tr><tr><td align="center">RT4</td><td>Validazione dei dati in ingresso</td><td>Verificare la risposta quando il campo <code>name</code> è mancante o vuoto, evitando crash o comportamenti errati.</td></tr></tbody></table>

***

## **Route exDelete**

### **Requirements**

**Gestione della cancellazione di esercizi:**

1. Accettare richieste HTTP POST al percorso `/delete_exercise`.
   * Campo richiesto in ingresso:
     * `exerciseName` (stringa): Nome dell'esercizio da eliminare.
2. Utilizzare una procedura memorizzata (`exo_exercises.delete_exercise`) per eliminare l'esercizio specificato dal database.
   * Passare il valore di `exerciseName` come parametro alla procedura memorizzata.
3. Registrare nella console il comando SQL generato per scopi di debug.
4. Restituire una risposta JSON con un messaggio di conferma (`"Exercise was deleted successfully"`) in caso di successo.
5. Gestire eventuali errori:
   * Registrare l'errore nella console.
   * Rispondere con uno stato HTTP 500 e un messaggio di errore appropriato.

### **Test**

**Test Case 1: Cancellazione corretta di un esercizio**

* **Descrizione:** Verificare che l'esercizio specificato venga eliminato correttamente.
* **Passi:**
  1. Aggiungere un esercizio di esempio al database con il nome `exampleExercise`.
  2. Inviare una richiesta POST a `/delete_exercise` con `exerciseName: "exampleExercise"`.
  3. Controllare che l'esercizio non sia più presente nel database.
* **Risultato atteso:** L'esercizio viene eliminato correttamente e la risposta è `{ "message": "Exercise was deleted successfully" }`.

**Test Case 2: Esercizio inesistente**

* **Descrizione:** Verificare il comportamento quando si tenta di eliminare un esercizio non presente nel database.
* **Passi:**
  1. Inviare una richiesta POST a `/delete_exercise` con un valore di `exerciseName` non esistente.
  2. Controllare la risposta del server.
* **Risultato atteso:** La risposta è `{ "message": "Exercise was deleted successfully" }`, ma non ci sono modifiche al database.

**Test Case 3: Gestione di errori SQL**

* **Descrizione:** Simulare un errore SQL per verificare la gestione degli errori.
* **Passi:**
  1. Rimuovere temporaneamente la procedura memorizzata `exo_exercises.delete_exercise` dal database.
  2. Inviare una richiesta POST a `/delete_exercise` con un valore valido per `exerciseName`.
  3. Controllare la risposta e il comportamento del server.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 e il messaggio `{ "message": "Error fetching exercise names" }`.

**Test Case 4: Validazione dei dati in ingresso**

* **Descrizione:** Verificare il comportamento quando il campo `exerciseName` non viene fornito o è vuoto.
* **Passi:**
  1. Inviare una richiesta POST a `/delete_exercise` senza il campo `exerciseName` o con un valore vuoto.
  2. Controllare la risposta del server e il comportamento.
* **Risultato atteso:** Il server restituisce un messaggio d'errore appropriato o si genera un errore SQL gestito.

### Result **exDelete**

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests **exDelete**

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Cancellazione corretta di un esercizio</td><td>Verificare che l'API elimini correttamente un esercizio esistente e fornisca la conferma JSON.</td></tr><tr><td align="center">RT2</td><td>Esercizio inesistente</td><td>Assicurarsi che l'API non generi errori quando l'esercizio da eliminare non esiste nel database.</td></tr><tr><td align="center">RT3</td><td>Gestione di errori SQL</td><td>Verificare che l'API risponda con HTTP 500 se la procedura memorizzata è mancante o fallisce.</td></tr><tr><td align="center">RT4</td><td>Validazione dei dati in ingresso</td><td>Confermare che il sistema gestisca input mancanti o vuoti per il parametro <code>exerciseName</code>.</td></tr></tbody></table>

***

## Route exEdit

### **Requirements**

**Gestione della modifica degli esercizi:**

1. Accettare richieste HTTP POST al percorso `/edit_exercise`.
   * Campo richiesto in ingresso:
     * `exerciseName` (stringa): Nome dell'esercizio da modificare.
2. Eseguire una query sul database per recuperare il numero di ripetizioni (`rep`) dell'esercizio specificato.
   * Tabella di riferimento: `exo_exercises.exercises`.
   * Condizione: il valore di `name` deve corrispondere a `exerciseName`.
3. Restituire una risposta JSON contenente il risultato della query in formato `{ result }`.
4. Gestire eventuali errori:
   * Registrare l'errore nella console.
   * Rispondere con uno stato HTTP 500 e un messaggio di errore appropriato.

**Requirements (Requisiti del componente exEdit)**

**Gestione della modifica degli esercizi:**

1. Accettare richieste HTTP POST al percorso `/edit_exercise`.
   * Campo richiesto in ingresso:
     * `exerciseName` (stringa): Nome dell'esercizio da modificare.
2. Eseguire una query sul database per recuperare il numero di ripetizioni (`rep`) dell'esercizio specificato.
   * Tabella di riferimento: `exo_exercises.exercises`.
   * Condizione: il valore di `name` deve corrispondere a `exerciseName`.
3. Restituire una risposta JSON contenente il risultato della query in formato `{ result }`.
4. Gestire eventuali errori:
   * Registrare l'errore nella console.
   * Rispondere con uno stato HTTP 500 e un messaggio di errore appropriato.

### **Test**

**Test Case 1: Recupero corretto delle informazioni dell'esercizio**

* **Descrizione:** Verificare che il numero di ripetizioni (`rep`) venga recuperato correttamente per un esercizio esistente.
* **Passi:**
  1. Inserire nel database un esercizio di esempio con `name: "exampleExercise"` e `rep: 10`.
  2. Inviare una richiesta POST a `/edit_exercise` con `exerciseName: "exampleExercise"`.
  3. Verificare che la risposta contenga il valore corretto di `rep`.
* **Risultato atteso:** La risposta è `{ result: [{ rep: 10 }] }`.

**Test Case 2: Esercizio inesistente**

* **Descrizione:** Verificare il comportamento quando si richiedono informazioni su un esercizio non presente nel database.
* **Passi:**
  1. Inviare una richiesta POST a `/edit_exercise` con un valore di `exerciseName` non esistente.
  2. Controllare la risposta del server.
* **Risultato atteso:** La risposta è `{ result: [] }` e nessun errore viene generato.

**Test Case 3: Gestione di errori SQL**

* **Descrizione:** Simulare un errore SQL per verificare la gestione degli errori.
* **Passi:**
  1. Rimuovere temporaneamente la tabella `exo_exercises.exercises` o il database di riferimento.
  2. Inviare una richiesta POST a `/edit_exercise` con un valore valido per `exerciseName`.
  3. Controllare la risposta e il comportamento del server.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 e il messaggio `{ "message": "Error fetching exercise names" }`.

**Test Case 4: Validazione dei dati in ingresso**

* **Descrizione:** Verificare il comportamento quando il campo `exerciseName` non viene fornito o è vuoto.
* **Passi:**
  1. Inviare una richiesta POST a `/edit_exercise` senza il campo `exerciseName` o con un valore vuoto.
  2. Controllare la risposta del server e il comportamento.
* **Risultato atteso:** Il server restituisce un messaggio d'errore appropriato o si genera un errore SQL gestito.

### Result exEdit

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>13/02/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests exEdit

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Recupero corretto delle informazioni dell'esercizio</td><td>Verificare che l'API restituisca correttamente il numero di ripetizioni (<code>rep</code>) per un esercizio esistente.</td></tr><tr><td align="center">RT2</td><td>Esercizio inesistente</td><td>Assicurarsi che l'API risponda correttamente con un array vuoto se l'esercizio non è presente nel database.</td></tr><tr><td align="center">RT3</td><td>Gestione di errori SQL</td><td>Testare la risposta dell'API quando il database o la tabella <code>exo_exercises.exercises</code> non sono disponibili.</td></tr><tr><td align="center">RT4</td><td>Validazione dei dati in ingresso</td><td>Verificare che il parametro <code>exerciseName</code> sia obbligatorio e che l'API gestisca input mancanti o vuoti.</td></tr></tbody></table>

***

## Route exGetName

### **Requirements**

**Gestione dei nomi degli esercizi:**

1. Accettare richieste HTTP GET al percorso `/exercises_names`.
2. Eseguire una query al database per ottenere un elenco di esercizi con i campi:
   * `name`: Nome dell'esercizio.
   * `rep`: Numero di ripetizioni.
3. Mappare i risultati della query in un array di oggetti con la struttura:
   * `{ name: <nome_esercizio>, rep: <numero_ripetizioni> }`.
4. Restituire la lista degli esercizi come risposta JSON.
5. Gestire eventuali errori:
   * Registrare l'errore nella console.
   * Rispondere con uno stato HTTP 500 e un messaggio di errore appropriato.

### **Test**

**Test Case 1: Recupero corretto dei nomi e delle ripetizioni degli esercizi**

* **Descrizione:** Verificare che tutti gli esercizi presenti nel database vengano restituiti correttamente.
* **Passi:**
  1. Inserire nel database alcuni esercizi di esempio:
     * `name: "Pushups", rep: 20`.
     * `name: "Squats", rep: 15`.
  2. Inviare una richiesta GET a `/exercises_names`.
  3. Controllare la risposta.
*   **Risultato atteso:** La risposta è:

    ```json
    jsonCopia codice[
      { "name": "Pushups", "rep": 20 },
      { "name": "Squats", "rep": 15 }
    ]
    ```

**Test Case 2: Nessun esercizio nel database**

* **Descrizione:** Verificare il comportamento quando non ci sono esercizi nel database.
* **Passi:**
  1. Assicurarsi che la tabella `exo_exercises.exercises` sia vuota.
  2. Inviare una richiesta GET a `/exercises_names`.
  3. Controllare la risposta.
* **Risultato atteso:** La risposta è un array vuoto `[]`.

**Test Case 3: Gestione di errori SQL**

* **Descrizione:** Simulare un errore SQL per verificare la gestione degli errori.
* **Passi:**
  1. Rimuovere temporaneamente la tabella `exo_exercises.exercises` o il database di riferimento.
  2. Inviare una richiesta GET a `/exercises_names`.
  3. Controllare la risposta e il comportamento del server.
*   **Risultato atteso:** Il server restituisce uno stato HTTP 500 e il messaggio:

    {% code fullWidth="false" %}
    ```json
    jsonCopia codice{ "message": "Error fetching exercise names" }
    ```
    {% endcode %}

**Test Case 4: Performance con dati di grandi dimensioni**

* **Descrizione:** Valutare le prestazioni con un grande numero di esercizi nel database.
* **Passi:**
  1. Popolare la tabella `exo_exercises.exercises` con almeno 10.000 esercizi di esempio.
  2. Inviare una richiesta GET a `/exercises_names`.
  3. Misurare il tempo di risposta del server.
* **Risultato atteso:** La risposta viene restituita entro un tempo accettabile (ad esempio, <1 secondo).

### Result exGetName

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests exGetName

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Recupero corretto dei dati</td><td>Verificare che tutti gli esercizi nel database vengano restituiti correttamente con i campi <code>name</code> e <code>rep</code>.</td></tr><tr><td align="center">RT2</td><td>Nessun esercizio nel database</td><td>Garantire che venga restituito un array vuoto quando non ci sono esercizi nella tabella.</td></tr><tr><td align="center">RT3</td><td>Gestione di errori SQL</td><td>Assicurarsi che errori del database siano gestiti correttamente con uno stato HTTP 500 e un messaggio d'errore.</td></tr></tbody></table>

***

## Route exSave

### **Requirements**

**Salvataggio dei dati degli esercizi:**

1. Accettare richieste HTTP POST al percorso `/save_exercises`.
2. Ricevere un array di oggetti JSON contenenti i dettagli di un esercizio. Ogni oggetto deve includere i seguenti campi:
   * `name`: Nome dell'esercizio.
   * `rep`: Numero di ripetizioni.
   * `order_pos`: Ordine della posizione.
   * `axis_1, axis_2, axis_3, axis_4, axis_5`: Coordinate degli assi.
   * `pause`: Tempo di pausa.
   * `speed`: Velocità di esecuzione.
3. Per ogni esercizio:
   * Inserire il nome e il numero di ripetizioni nel database usando la procedura `exo_exercises.load_exercise`.
   * Cancellare le posizioni esistenti associate all'esercizio usando la procedura `exo_exercises.clear_positions_for_exercise`.
   * Salvare le nuove posizioni usando la procedura `exo_exercises.load_positions_exercise`.
4. Gestire la connessione al database:
   * Aprire e chiudere la connessione in modo appropriato.
5. Restituire una risposta JSON con un messaggio di conferma in caso di successo.
6. Gestire eventuali errori:
   * Registrare l'errore nella console.
   * Rispondere con uno stato HTTP 500 e un messaggio di errore appropriato.

### **Test**

**Test Case 1: Salvataggio corretto dei dati degli esercizi**

* **Descrizione:** Verificare che i dati dell'esercizio vengano salvati correttamente nel database.
* **Passi:**
  1.  Preparare un array di oggetti JSON con i dettagli di un esercizio, ad esempio:

      ```json
      jsonCopia codice[
        { "name": "Pushups", "rep": 20, "order_pos": 1, "axis_1": 0, "axis_2": 0, "axis_3": 0, "axis_4": 0, "axis_5": 0, "pause": 2, "speed": 5 }
      ]
      ```
  2. Inviare una richiesta POST a `/save_exercises` con il corpo della richiesta contenente questi dati.
  3. Controllare che i dati siano stati inseriti correttamente nelle tabelle del database.
* **Risultato atteso:** I dati sono salvati correttamente senza errori.

**Test Case 2: Gestione di dati incompleti**

* **Descrizione:** Verificare il comportamento con dati incompleti o mancanti.
* **Passi:**
  1.  Preparare un array con oggetti che mancano di alcuni campi obbligatori, ad esempio:

      ```json
      jsonCopia codice[{ "name": "Pushups", "rep": 20 }]
      ```
  2. Inviare una richiesta POST a `/save_exercises`.
  3. Controllare il comportamento e il messaggio di errore restituito.
* **Risultato atteso:** La richiesta fallisce con uno stato HTTP 500 e un messaggio di errore.

**Test Case 3: Gestione di errori del database**

* **Descrizione:** Simulare un errore del database durante l'operazione di salvataggio.
* **Passi:**
  1. Interrompere temporaneamente la connessione al database o bloccare le procedure memorizzate.
  2. Inviare una richiesta POST a `/save_exercises`.
  3. Controllare il comportamento del server e il messaggio di errore restituito.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 e registra l'errore nella console.

**Test Case 4: Salvataggio di più esercizi**

* **Descrizione:** Verificare il comportamento con più esercizi nell'array di input.
* **Passi:**
  1.  Preparare un array con più oggetti di esercizio, ad esempio:

      ```json
      jsonCopia codice[
        { "name": "Pushups", "rep": 20, "order_pos": 1, "axis_1": 0, "axis_2": 0, "axis_3": 0, "axis_4": 0, "axis_5": 0, "pause": 2, "speed": 5 },
        { "name": "Squats", "rep": 15, "order_pos": 2, "axis_1": 0, "axis_2": 0, "axis_3": 0, "axis_4": 0, "axis_5": 0, "pause": 3, "speed": 4 }
      ]
      ```
  2. Inviare una richiesta POST a `/save_exercises`.
  3. Controllare che tutti gli esercizi siano salvati correttamente.
* **Risultato atteso:** I dati di tutti gli esercizi vengono salvati senza errori.

### Result exSave

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests exSave

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Salvataggio corretto dei dati</td><td>Verificare che i dettagli degli esercizi vengano salvati correttamente nel database senza errori.</td></tr><tr><td align="center">RT2</td><td>Gestione di dati incompleti</td><td>Garantire che richieste con dati mancanti o incompleti siano gestite con un messaggio di errore appropriato.</td></tr><tr><td align="center">RT3</td><td>Gestione degli errori del database</td><td>Assicurarsi che errori durante l’operazione di salvataggio siano gestiti correttamente con stato HTTP 500.</td></tr><tr><td align="center">RT4</td><td>Validazione delle procedure memorizzate</td><td>Confermare che le procedure <code>load_exercise</code>, <code>clear_positions_for_exercise</code> e <code>load_positions_exercise</code> siano eseguite correttamente e nell’ordine previsto.</td></tr></tbody></table>

***

## Route exUpdate

### **Requirements**

**Aggiornamento dei dati di un esercizio:**

1. Accettare richieste HTTP POST al percorso `/update_exercise`.
2. Ricevere un oggetto JSON con i seguenti campi:
   * `oldName`: Nome attuale dell'esercizio.
   * `newName`: Nuovo nome dell'esercizio.
   * `rep`: Nuovo numero di ripetizioni.
3. Aggiornare il nome e il numero di ripetizioni dell'esercizio nel database utilizzando la procedura `exo_exercises.update_exercise`.
4. Gestire la connessione al database:
   * Aprire e chiudere correttamente la connessione.
5. Restituire una risposta JSON con un messaggio di conferma in caso di successo.
6. Gestire eventuali errori:
   * Registrare l'errore nella console.
   * Restituire uno stato HTTP 500 con un messaggio di errore appropriato.

### **Test**

**Test Case 1: Aggiornamento corretto di un esercizio**

* **Descrizione:** Verificare che i dati di un esercizio vengano aggiornati correttamente nel database.
* **Passi:**
  1. Creare un esercizio nel database con i dati:
     * `oldName`: `"Pushups"`.
     * `rep`: `20`.
  2.  Inviare una richiesta POST a `/update_exercise` con il corpo:

      ```json
      jsonCopia codice{ "oldName": "Pushups", "newName": "Pullups", "rep": 15 }
      ```
  3. Controllare che il nome e le ripetizioni siano stati aggiornati nel database.
* **Risultato atteso:** I dati vengono aggiornati correttamente senza errori.

**Test Case 2: Gestione di un esercizio inesistente**

* **Descrizione:** Verificare il comportamento quando l'esercizio specificato non esiste.
* **Passi:**
  1. Preparare una richiesta POST con `oldName` che non corrisponde ad alcun esercizio nel database.
  2. Inviare la richiesta a `/update_exercise`.
* **Risultato atteso:** Nessun dato viene aggiornato e il server restituisce un messaggio appropriato (ad esempio, "Nessun esercizio trovato").

**Test Case 3: Gestione di input non validi**

* **Descrizione:** Verificare il comportamento con dati mancanti o non validi.
* **Passi:**
  1.  Inviare una richiesta POST con uno dei campi mancanti o con valori non validi, ad esempio:

      ```json
      jsonCopia codice{ "oldName": "Pushups", "rep": "invalid" }
      ```
  2. Controllare il comportamento del server e il messaggio restituito.
* **Risultato atteso:** La richiesta fallisce con uno stato HTTP 500 e un messaggio di errore appropriato.

**Test Case 4: Gestione degli errori del database**

* **Descrizione:** Simulare un errore del database durante l'aggiornamento.
* **Passi:**
  1. Interrompere temporaneamente la connessione al database o bloccare la procedura `update_exercise`.
  2. Inviare una richiesta POST a `/update_exercise`.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 e registra l'errore nella console.

**Test Case 5: Aggiornamento parziale (solo ripetizioni)**

* **Descrizione:** Verificare che sia possibile aggiornare solo il numero di ripetizioni senza modificare il nome.
* **Passi:**
  1. Creare un esercizio con nome `Pushups` e `rep` pari a `20`.
  2.  Inviare una richiesta POST con il corpo:

      ```json
      jsonCopia codice{ "oldName": "Pushups", "newName": "Pushups", "rep": 25 }
      ```
  3. Controllare che solo il campo `rep` sia stato aggiornato.
* **Risultato atteso:** Il numero di ripetizioni viene aggiornato correttamente senza modificare il nome.

### Result exUpdate

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>15/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>14/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>15/02/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests exUpdate

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Aggiornamento corretto di un esercizio</td><td>Verificare che il nome e le ripetizioni dell'esercizio siano aggiornati correttamente nel database senza errori.</td></tr><tr><td align="center">RT2</td><td>Gestione di input non validi</td><td>Garantire che richieste con dati mancanti o non validi siano bloccate con uno stato HTTP 500 e un messaggio di errore.</td></tr><tr><td align="center">RT3</td><td>Gestione degli errori del database</td><td>Validare che errori legati al database durante l'aggiornamento siano gestiti correttamente con stato HTTP 500.</td></tr><tr><td align="center">RT4</td><td>Aggiornamento parziale</td><td>Confermare che sia possibile aggiornare solo il numero di ripetizioni senza modificare il nome dell'esercizio.</td></tr></tbody></table>

***

## Route exUpdatePos

### **Requirements**

**Aggiornamento delle posizioni di un esercizio:**

1. Accettare richieste HTTP POST al percorso `/update_pos`.
2. Ricevere un array JSON contenente oggetti con i seguenti campi:
   * `name`: Nome dell'esercizio.
   * `order_pos`: Ordine della posizione.
   * `axis_1, axis_2, axis_3, axis_4, axis_5`: Coordinate degli assi.
   * `pause`: Durata della pausa tra le posizioni.
   * `speed`: Velocità di esecuzione.
3. Per ogni richiesta:
   * Pulire le posizioni esistenti per l'esercizio specificato utilizzando la procedura `exo_exercises.clear_positions_for_exercise`.
   * Inserire le nuove posizioni usando la procedura `exo_exercises.load_positions_exercise`.
4. Gestire correttamente la connessione al database:
   * Aprire e chiudere correttamente la connessione.
5. Restituire una risposta JSON con un messaggio di conferma in caso di successo.
6. Gestire eventuali errori:
   * Registrare l'errore nella console.
   * Restituire uno stato HTTP 500 con un messaggio di errore appropriato.

### **Test**

**Test Case 1: Aggiornamento corretto delle posizioni di un esercizio**

* **Descrizione:** Verificare che le posizioni di un esercizio vengano aggiornate correttamente.
* **Passi:**
  1. Creare un esercizio con posizioni predefinite.
  2.  Inviare una richiesta POST a `/update_pos` con un array JSON contenente nuove posizioni:

      ```json
      jsonCopia codice[
        { "name": "Pushups", "order_pos": 1, "axis_1": 10, "axis_2": 15, "axis_3": 20, "axis_4": 25, "axis_5": 30, "pause": 5, "speed": 2 },
        { "name": "Pushups", "order_pos": 2, "axis_1": 12, "axis_2": 18, "axis_3": 24, "axis_4": 30, "axis_5": 36, "pause": 3, "speed": 1 }
      ]
      ```
  3. Controllare che le vecchie posizioni siano state rimosse e sostituite con quelle nuove.
* **Risultato atteso:** Le nuove posizioni vengono salvate correttamente senza errori.

**Test Case 2: Gestione di un esercizio inesistente**

* **Descrizione:** Verificare il comportamento quando l'esercizio specificato non esiste nel database.
* **Passi:**
  1. Preparare una richiesta POST con un nome di esercizio non esistente.
  2. Inviare la richiesta a `/update_pos`.
* **Risultato atteso:** Nessuna posizione viene aggiornata e il server restituisce un messaggio appropriato.

**Test Case 3: Gestione di input non validi**

* **Descrizione:** Verificare il comportamento del sistema con dati non validi o mancanti.
* **Passi:**
  1.  Inviare una richiesta POST con un array JSON contenente campi mancanti o valori non validi, ad esempio:

      ```json
      jsonCopia codice[
        { "name": "Pushups", "order_pos": 1, "axis_1": null, "pause": 5, "speed": "invalid" }
      ]
      ```
  2. Osservare la risposta del server.
* **Risultato atteso:** La richiesta fallisce con uno stato HTTP 500 e un messaggio di errore appropriato.

**Test Case 4: Aggiornamento parziale delle posizioni**

* **Descrizione:** Verificare che sia possibile aggiornare solo una parte delle posizioni mantenendo le altre.
* **Passi:**
  1. Creare un esercizio con più posizioni.
  2. Inviare una richiesta POST con un array JSON che modifica solo alcune delle posizioni.
  3. Controllare che le posizioni non specificate nella richiesta rimangano inalterate.
* **Risultato atteso:** Solo le posizioni specificate vengono aggiornate, senza influire sulle altre.

**Test Case 5: Gestione di errori del database**

* **Descrizione:** Simulare un errore del database durante l'eliminazione o l'inserimento delle posizioni.
* **Passi:**
  1. Interrompere temporaneamente la connessione al database o bloccare le procedure `clear_positions_for_exercise` o `load_positions_exercise`.
  2. Inviare una richiesta POST a `/update_pos`.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 e registra l'errore nella console.

### Result exUpdatePos

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>16/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>16/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>16/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>16/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>16/02/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests exUpdatePos

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Aggiornamento corretto delle posizioni</td><td>Verificare che le posizioni esistenti vengano eliminate e sostituite correttamente con le nuove posizioni fornite.</td></tr><tr><td align="center">RT2</td><td>Gestione di input non validi</td><td>Garantire che richieste con campi mancanti o valori non validi siano bloccate con uno stato HTTP 500.</td></tr><tr><td align="center">RT3</td><td>Aggiornamento parziale delle posizioni</td><td>Confermare che sia possibile aggiornare solo una parte delle posizioni mantenendo inalterate quelle non specificate nella richiesta.</td></tr><tr><td align="center">RT4</td><td>Gestione di errori del database</td><td>Validare che errori legati al database siano gestiti correttamente, con stato HTTP 500 e registrazione dell'errore nella console.</td></tr></tbody></table>

***

## Route exViewPos

### Requirements

**Visualizzazione delle posizioni di un esercizio:**

1. Accettare richieste HTTP POST al percorso `/view_positions_exercise`.
2. Ricevere un oggetto JSON con il campo:
   * `name`: Nome dell'esercizio di cui visualizzare le posizioni.
3.  Estrarre dal database le posizioni associate al nome dell'esercizio specificato utilizzando la query:

    {% code overflow="wrap" %}
    ```sql
    sqlCopia codiceSELECT * FROM exo_exercises.view_positions WHERE name LIKE "${name}"
    ```
    {% endcode %}
4. Gestire correttamente la connessione al database:
   * Aprire e chiudere correttamente la connessione.
5. Restituire una risposta JSON contenente il risultato della query.
6. Gestire eventuali errori:
   * Registrare l'errore nella console.
   * Restituire uno stato HTTP 500 con un messaggio di errore appropriato.

### Test

**Test Case 1: Visualizzazione delle posizioni per un esercizio esistente**

* **Descrizione:** Verificare che le posizioni di un esercizio esistente siano recuperate correttamente.
* **Passi:**
  1. Aggiungere un esercizio nel database con posizioni definite.
  2.  Inviare una richiesta POST a `/view_positions_exercise` con un oggetto JSON:

      ```json
      jsonCopia codice{ "name": "Pushups" }
      ```
  3. Osservare la risposta del server.
* **Risultato atteso:** La risposta contiene un array con tutte le posizioni dell'esercizio specificato.

**Test Case 2: Gestione di un esercizio inesistente**

* **Descrizione:** Verificare il comportamento quando l'esercizio specificato non esiste.
* **Passi:**
  1.  Inviare una richiesta POST a `/view_positions_exercise` con un nome inesistente:

      ```json
      jsonCopia codice{ "name": "NonExistentExercise" }
      ```
  2. Osservare la risposta del server.
* **Risultato atteso:** La risposta è un array vuoto senza errori.

**Test Case 3: Gestione di input non validi**

* **Descrizione:** Verificare il comportamento del sistema con dati non validi o mancanti.
* **Passi:**
  1.  Inviare una richiesta POST con un oggetto JSON privo del campo `name` o con un valore non valido:

      ```json
      jsonCopia codice{}
      ```
  2. Osservare la risposta del server.
* **Risultato atteso:** Il server restituisce un messaggio di errore con stato HTTP 500.

**Test Case 4: Verifica della query SQL generata**

* **Descrizione:** Controllare che la query SQL generata sia corretta e non vulnerabile a SQL injection.
* **Passi:**
  1.  Inviare un nome con caratteri speciali o stringhe sospette, ad esempio:

      ```json
      jsonCopia codice{ "name": "Test'; DROP TABLE users; --" }
      ```
  2. Osservare la query generata e il comportamento del database.
* **Risultato atteso:** La query deve essere sicura e non eseguire operazioni non previste.

**Test Case 5: Gestione di errori del database**

* **Descrizione:** Simulare un errore del database durante l'esecuzione della query.
* **Passi:**
  1. Disattivare temporaneamente il database o simulare un errore.
  2. Inviare una richiesta POST a `/view_positions_exercise`.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 e registra l'errore nella console.

### Result exViewPos

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>15/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>15/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>15/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>15/02/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>15/02/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests exViewPos

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Visualizzazione delle posizioni per un esercizio esistente</td><td>Verificare che le posizioni associate a un esercizio esistente siano recuperate correttamente dal database.</td></tr><tr><td align="center">RT2</td><td>Gestione di input non validi</td><td>Garantire che richieste prive del campo <code>name</code> o con valori non validi siano bloccate con uno stato HTTP 500.</td></tr><tr><td align="center">RT3</td><td>Gestione di errori del database</td><td>Confermare che errori del database siano gestiti correttamente con uno stato HTTP 500 e registrazione dell'errore nella console.</td></tr><tr><td align="center">RT4</td><td>Gestione della connessione al database</td><td>Verificare che la connessione al database venga aperta e chiusa correttamente per ogni richiesta POST.</td></tr></tbody></table>

