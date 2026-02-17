# Scenarios

{% hint style="info" %}
Questa pagina contiene i requisiti e le prove per tutti i "routes" nella sezione SCENARIOS.
{% endhint %}

## Route scAddMode

### **Requirements**

**Salvataggio dei Dati del Modalità Scenario:**

1. Gestire le richieste HTTP POST al percorso `/save_scenario_mode`.
2. Estrarre i dati dello scenario e il nome dello scenario dal corpo della richiesta.
3. Leggere il contenuto esistente dal file JSON `./json/scenarios.json`:
   * Se il file non esiste, inizializzare un array vuoto.
   * Se il file esiste, analizzare il contenuto JSON.
4. Rimuovere eventuali dati di scenario esistenti con lo stesso nome dal file.
5. Aggiungere i nuovi dati dello scenario al file JSON.
6. Scrivere i dati aggiornati nel file `scenarios.json`, mantenendo una formattazione leggibile con due spazi di indentazione.
7. Restituire una risposta con stato HTTP 200 e un messaggio di conferma in caso di successo.
8. Gestire eventuali errori, come:
   * Impossibilità di leggere o scrivere il file.
   * Formato JSON non valido.
   * Restituire una risposta HTTP 500 con un messaggio di errore appropriato.
9. Registrare nella console:
   * Il nome dello scenario salvato.
   * Eventuali errori durante il processo di salvataggio.

### Test

**Test Case 1: Salvataggio di nuovi dati di scenario**

* **Descrizione:** Verificare che un nuovo scenario venga salvato correttamente nel file JSON.
* **Passi:**
  1.  Inviare una richiesta POST con i dati dello scenario:

      ```json
      jsonCopia codice[
        { "scenario": "NuovoScenario", "data": "EsempioDati" }
      ]
      ```
  2. Controllare il contenuto del file `scenarios.json`.
* **Risultato atteso:** Il file contiene i dati dello scenario salvato.

**Test Case 2: Sovrascrittura di uno scenario esistente**

* **Descrizione:** Verificare che i dati di uno scenario con lo stesso nome vengano sovrascritti.
* **Passi:**
  1.  Salvare uno scenario esistente:

      ```json
      jsonCopia codice[
        { "scenario": "ScenarioEsistente", "data": "VecchiDati" }
      ]
      ```
  2.  Inviare una richiesta POST con nuovi dati per lo stesso scenario:

      ```json
      jsonCopia codice[
        { "scenario": "ScenarioEsistente", "data": "NuoviDati" }
      ]
      ```
  3. Controllare il contenuto del file `scenarios.json`.
* **Risultato atteso:** Il file contiene solo i nuovi dati dello scenario.

**Test Case 3: File JSON inesistente**

* **Descrizione:** Verificare il comportamento quando il file `scenarios.json` non esiste.
* **Passi:**
  1. Rinominare o eliminare temporaneamente il file `scenarios.json`.
  2. Inviare una richiesta POST con dati validi.
* **Risultato atteso:** Il file viene creato e contiene i dati forniti.

**Test Case 4: Formato JSON non valido nel file**

* **Descrizione:** Simulare la presenza di dati JSON malformati nel file.
* **Passi:**
  1.  Modificare il file `scenarios.json` con dati non validi:

      ```json
      jsonCopia codice{ "scenario": "MancanzaVirgola" "data": "EsempioDati" }
      ```
  2. Inviare una richiesta POST con dati validi.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 con un messaggio di errore.

**Test Case 5: Salvataggio di più scenari**

* **Descrizione:** Verificare che più scenari vengano salvati correttamente.
* **Passi:**
  1. Inviare richieste POST consecutive con dati per scenari diversi.
  2. Controllare il contenuto del file `scenarios.json`.
* **Risultato atteso:** Il file contiene tutti gli scenari salvati.

**Test Case 6: Gestione di input non validi**

* **Descrizione:** Verificare il comportamento con dati mancanti o non validi.
* **Passi:**
  1. Inviare una richiesta POST senza il campo `scenario`.
  2. Inviare una richiesta POST con un array vuoto.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 con un messaggio di errore appropriato.

**Test Case 7: Verifica dei log del server**

* **Descrizione:** Controllare che le informazioni corrette siano registrate nella console.
* **Passi:**
  1. Inviare una richiesta valida.
  2. Controllare la console per i log relativi al nome dello scenario e al salvataggio.
* **Risultato atteso:** I log mostrano correttamente il nome dello scenario e lo stato del processo di salvataggio.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC7</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests scAddMode

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Salvataggio di nuovi dati di scenario</td><td>Verificare che un nuovo scenario venga salvato correttamente nel file JSON senza errori.</td></tr><tr><td align="center">RT2</td><td>Sovrascrittura di uno scenario esistente</td><td>Garantire che uno scenario con lo stesso nome venga sovrascritto correttamente.</td></tr><tr><td align="center">RT3</td><td>Formato JSON non valido nel file</td><td>Verificare che il sistema gestisca errori di parsing JSON e restituisca uno stato HTTP 500.</td></tr><tr><td align="center">RT4</td><td>Salvataggio di più scenari</td><td>Assicurarsi che il file JSON supporti la memorizzazione di più scenari consecutivi.</td></tr><tr><td align="center">RT5</td><td>Gestione di input non validi</td><td>Garantire che il server gestisca correttamente richieste con dati mancanti o non validi.</td></tr><tr><td align="center">RT6</td><td>Verifica dei log del server</td><td>Controllare che il nome dello scenario e lo stato del salvataggio siano registrati correttamente nella console.</td></tr></tbody></table>

***

## Route scGetMode

### **Requirements**

**Recupero del Modalità Scenario:**

1. Accettare richieste HTTP POST al percorso `/get_scenario_mode`.
2. Utilizzare il middleware `body-parser` per elaborare i dati della richiesta come testo di tipo `text/plain`.
3. Estrarre dalla richiesta il nome dello scenario tramite il campo `scenarioNameURL` nel corpo della richiesta.
4. Leggere i dati dal file JSON `./json/scenarios.json`.
5. Analizzare il contenuto del file JSON per individuare lo scenario richiesto:
   * Lo scenario viene identificato confrontando il valore `scenario` del primo elemento (`scenario[0].scenario`) con il nome specificato.
6. Restituire una risposta JSON contenente lo scenario selezionato.
7. Gestire eventuali errori, come:
   * Mancata lettura del file.
   * Dati JSON non validi.
   * Scenario non trovato.
   * Restituire uno stato HTTP 500 con un messaggio di errore appropriato.
8. Registrare nella console:
   * Il nome dello scenario richiesto.
   * La risposta inviata al client.

### Test

**Test Case 1: Recupero di uno scenario esistente**

* **Descrizione:** Verificare che uno scenario esistente venga recuperato correttamente dal file JSON.
* **Passi:**
  1.  Aggiungere uno scenario nel file `scenarios.json`, ad esempio:

      ```json
      jsonCopia codice[[{ "scenario": "ScenarioTest", "data": "ExampleData" }]]
      ```
  2.  Inviare una richiesta POST a `/get_scenario_mode` con un corpo:

      ```json
      jsonCopia codice{ "scenarioNameURL": "ScenarioTest" }
      ```
  3. Osservare la risposta del server.
* **Risultato atteso:** La risposta contiene i dettagli dello scenario corrispondente.

**Test Case 2: Gestione di uno scenario inesistente**

* **Descrizione:** Verificare il comportamento quando lo scenario richiesto non è presente.
* **Passi:**
  1.  Inviare una richiesta POST con un nome di scenario non esistente:

      ```json
      jsonCopia codice{ "scenarioNameURL": "NonExistentScenario" }
      ```
  2. Osservare la risposta del server.
* **Risultato atteso:** La risposta è `null` o un messaggio che indica che lo scenario non è stato trovato.

**Test Case 3: Gestione di dati JSON non validi**

* **Descrizione:** Simulare la presenza di dati JSON malformati nel file `scenarios.json`.
* **Passi:**
  1.  Modificare il file `scenarios.json` per includere dati non validi, ad esempio:

      ```json
      jsonCopia codice[[{ "scenario": "MissingComma" "data": "ExampleData" }]]
      ```
  2. Inviare una richiesta valida al server.
* **Risultato atteso:** Il server restituisce un errore con stato HTTP 500 e registra l'errore nella console.

**Test Case 4: Mancata disponibilità del file JSON**

* **Descrizione:** Verificare il comportamento quando il file `scenarios.json` è assente.
* **Passi:**
  1. Rinominare o spostare temporaneamente il file `scenarios.json`.
  2. Inviare una richiesta POST valida al server.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 con un messaggio di errore appropriato.

**Test Case 5: Gestione di input non validi**

* **Descrizione:** Verificare il comportamento del server con dati non validi o mancanti.
* **Passi:**
  1.  Inviare una richiesta POST senza il campo `scenarioNameURL`:

      ```json
      jsonCopia codice{}
      ```
  2. Osservare la risposta del server.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 o una risposta JSON con un messaggio di errore.

**Test Case 6: Verifica dei log del server**

* **Descrizione:** Controllare che le informazioni corrette siano registrate nella console.
* **Passi:**
  1. Inviare una richiesta valida.
  2. Controllare la console per i log relativi al nome dello scenario richiesto e alla risposta inviata.
* **Risultato atteso:** I log mostrano correttamente il nome dello scenario e la risposta.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests scGetMode

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Recupero di uno scenario esistente</td><td>Verificare che uno scenario presente nel file JSON venga restituito correttamente.</td></tr><tr><td align="center">RT2</td><td>Gestione di uno scenario inesistente</td><td>Garantire che il server gestisca correttamente la richiesta per uno scenario non esistente.</td></tr><tr><td align="center">RT3</td><td>Gestione di dati JSON non validi</td><td>Assicurarsi che il server rilevi e gestisca dati JSON malformati nel file scenarios.json.</td></tr><tr><td align="center">RT4</td><td>Gestione di input non validi</td><td>Garantire che il server restituisca un errore adeguato per richieste senza scenarioNameURL o con dati non validi.</td></tr><tr><td align="center">RT5</td><td>Verifica dei log del server</td><td>Controllare che il server registri correttamente il nome dello scenario richiesto e la risposta inviata.</td></tr></tbody></table>

***

## Route scDelete

### Requirements

**Eliminazione di uno Scenario Specifico:**

1. Gestire le richieste HTTP POST al percorso `/delete_scenario`.
2. Estrarre il nome dello scenario da eliminare dal corpo della richiesta (`req.body.scenario`).
3. Leggere il contenuto del file JSON `./json/scenarios.json`:
   * Se il file non esiste o è vuoto, restituire un errore.
4. Trovare lo scenario con il nome corrispondente:
   * Utilizzare il metodo `findIndex` per individuare l'indice dell'elemento corrispondente.
   * Confrontare il campo `scenario` del primo oggetto di ogni elemento.
5. Eliminare lo scenario trovato dall'elenco utilizzando `splice`.
6. Salvare l'elenco aggiornato nel file `scenarios.json` mantenendo una formattazione leggibile (2 spazi di indentazione).
7. Restituire una risposta HTTP 200 con un messaggio di conferma se lo scenario è stato eliminato.
8. Se lo scenario non viene trovato, restituire una risposta HTTP 404 con un messaggio appropriato.
9. Gestire eventuali errori, come:
   * Impossibilità di leggere o scrivere il file.
   * JSON malformato o dati mancanti.
   * Restituire una risposta HTTP 500 con un messaggio di errore.
10. Registrare nella console:
    * Lo scenario eliminato.
    * Eventuali errori durante l'operazione.

### Test

**Test Case 1: Eliminazione di uno scenario esistente**

* **Descrizione:** Verificare che uno scenario esistente venga eliminato correttamente.
* **Passi:**
  1.  Salvare uno scenario di esempio nel file `scenarios.json`:

      ```json
      jsonCopia codice[
        [{ "scenario": "ScenarioDaEliminare", "data": "DatiEsempio" }]
      ]
      ```
  2.  Inviare una richiesta POST con il corpo:

      ```json
      jsonCopia codice{ "scenario": "ScenarioDaEliminare" }
      ```
  3. Controllare il file `scenarios.json`.
* **Risultato atteso:** Il file non contiene più lo scenario eliminato.

**Test Case 2: Eliminazione di uno scenario inesistente**

* **Descrizione:** Verificare il comportamento quando lo scenario non è presente nel file.
* **Passi:**
  1. Assicurarsi che `scenarios.json` non contenga lo scenario specificato.
  2.  Inviare una richiesta POST con il corpo:

      ```json
      jsonCopia codice{ "scenario": "ScenarioNonEsistente" }
      ```
  3. Controllare la risposta del server.
* **Risultato atteso:** Il server restituisce uno stato HTTP 404 con un messaggio "Scenario not found".

**Test Case 3: File JSON inesistente o vuoto**

* **Descrizione:** Verificare il comportamento quando il file `scenarios.json` non esiste o è vuoto.
* **Passi:**
  1. Rinominare o eliminare temporaneamente `scenarios.json`.
  2. Inviare una richiesta POST con un nome di scenario qualsiasi.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 con un messaggio "Error deleting scenario".

**Test Case 4: JSON malformato nel file**

* **Descrizione:** Simulare un file JSON malformato.
* **Passi:**
  1.  Modificare il file `scenarios.json` con dati non validi:

      ```json
      jsonCopia codice[{ "scenario": "MancaVirgola" "data": "Esempio" }]
      ```
  2. Inviare una richiesta POST valida.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 con un messaggio "Error deleting scenario".

**Test Case 5: Eliminazione di uno scenario tra più presenti**

* **Descrizione:** Verificare che l'eliminazione di uno scenario non influenzi gli altri.
* **Passi:**
  1.  Salvare più scenari nel file `scenarios.json`:

      ```json
      jsonCopia codice[
        [{ "scenario": "Scenario1", "data": "Dati1" }],
        [{ "scenario": "Scenario2", "data": "Dati2" }]
      ]
      ```
  2.  Inviare una richiesta POST con il corpo:

      ```json
      jsonCopia codice{ "scenario": "Scenario1" }
      ```
  3. Controllare il file `scenarios.json`.
* **Risultato atteso:** Solo `Scenario1` è stato rimosso; `Scenario2` rimane invariato.

**Test Case 6: Verifica dei log del server**

* **Descrizione:** Controllare che il nome dello scenario eliminato e gli eventuali errori siano registrati correttamente nella console.
* **Passi:**
  1. Eseguire una richiesta valida per eliminare uno scenario.
  2. Controllare la console del server.
* **Risultato atteso:** La console registra correttamente il nome dello scenario eliminato.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests scGetMode

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Eliminazione di uno scenario esistente</td><td>Garantire che uno scenario esistente venga eliminato correttamente dal file JSON.</td></tr><tr><td align="center">RT2</td><td>Eliminazione di uno scenario inesistente</td><td>Verificare che il server restituisca uno stato HTTP 404 se lo scenario non esiste.</td></tr><tr><td align="center">RT3</td><td>File JSON inesistente o vuoto</td><td>Assicurarsi che il server gestisca correttamente l'assenza o il contenuto vuoto del file.</td></tr><tr><td align="center">RT4</td><td>JSON malformato nel file</td><td>Verificare che il server gestisca correttamente un file JSON con dati non validi.</td></tr><tr><td align="center">RT5</td><td>Eliminazione di uno scenario tra più presenti</td><td>Garantire che l'eliminazione di uno scenario non influenzi gli altri presenti nel file.</td></tr><tr><td align="center">RT6</td><td>Verifica dei log del server</td><td>Controllare che i log contengano correttamente il nome dello scenario eliminato e gli errori.</td></tr></tbody></table>

***

## Route scName

### Requirements

**Recupero dei nomi degli scenari e dell'ultimo scenario salvato:**

1. Gestire le richieste HTTP GET al percorso `/scenario_names`.
2. Leggere il contenuto del file JSON `./json/scenarios.json`:
   * Se il file non esiste o è vuoto, restituire un errore.
   * Estrarre i nomi di tutti gli scenari presenti nel file.
3. Leggere il contenuto del file JSON `./json/lastScenario.json`:
   * Se il file non esiste o è vuoto, restituire un errore.
   * Estrarre il nome dell'ultimo scenario salvato.
4. Creare un oggetto combinato contenente:
   * La lista dei nomi degli scenari (`scenarioNames`).
   * Il nome dell'ultimo scenario (`lastScenario`).
5. Restituire una risposta HTTP 200 con i dati combinati in formato JSON.
6. Gestire eventuali errori, come:
   * Impossibilità di leggere o scrivere i file.
   * JSON malformato o dati mancanti.
   * Restituire una risposta HTTP 500 con un messaggio di errore.
7. Registrare nella console eventuali errori durante l'operazione.

### Test

**Test Case 1: Recupero corretto dei nomi degli scenari e dell'ultimo scenario**

* **Descrizione:** Verificare che il componente restituisca correttamente i dati combinati.
* **Passi:**
  1. Salvare i seguenti dati nei file:
     *   `scenarios.json`:

         ```json
         jsonCopia codice[
           [{ "scenario": "Scenario1", "data": "Esempio1" }],
           [{ "scenario": "Scenario2", "data": "Esempio2" }]
         ]
         ```
     *   `lastScenario.json`:

         ```json
         jsonCopia codice[
           { "scenario": "Scenario2" }
         ]
         ```
  2. Inviare una richiesta GET a `/scenario_names`.
  3. Controllare la risposta del server.
*   **Risultato atteso:**

    ```json
    jsonCopia codice{
      "scenarioNames": ["Scenario1", "Scenario2"],
      "lastScenario": "Scenario2"
    }
    ```

**Test Case 2: File `scenarios.json` vuoto o inesistente**

* **Descrizione:** Verificare il comportamento quando il file `scenarios.json` è vuoto o mancante.
* **Passi:**
  1. Eliminare o svuotare il file `scenarios.json`.
  2. Assicurarsi che `lastScenario.json` contenga dati validi.
  3. Inviare una richiesta GET a `/scenario_names`.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 con un messaggio "Data retrieval failed".

**Test Case 3: File `lastScenario.json` vuoto o inesistente**

* **Descrizione:** Verificare il comportamento quando il file `lastScenario.json` è vuoto o mancante.
* **Passi:**
  1. Assicurarsi che `scenarios.json` contenga dati validi.
  2. Eliminare o svuotare il file `lastScenario.json`.
  3. Inviare una richiesta GET a `/scenario_names`.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 con un messaggio "Data retrieval failed".

**Test Case 4: File JSON malformati**

* **Descrizione:** Simulare file JSON malformati per entrambi i file.
* **Passi:**
  1.  Inserire contenuti non validi in `scenarios.json`:

      ```json
      jsonCopia codice[{ "scenario": "Scenario1", "data": "MancaVirgola" "data": "Errore" }]
      ```
  2.  Inserire contenuti non validi in `lastScenario.json`:

      ```json
      jsonCopia codice[{ "scenario": "MancaChiusura" ]  
      ```
  3. Inviare una richiesta GET a `/scenario_names`.
* **Risultato atteso:** Il server restituisce uno stato HTTP 500 con un messaggio "Data retrieval failed".

**Test Case 5: File `scenarios.json` con scenari multipli**

* **Descrizione:** Verificare che il componente gestisca più scenari correttamente.
* **Passi:**
  1.  Salvare nel file `scenarios.json`:

      ```json
      jsonCopia codice[
        [{ "scenario": "Scenario1", "data": "Dati1" }],
        [{ "scenario": "Scenario2", "data": "Dati2" }],
        [{ "scenario": "Scenario3", "data": "Dati3" }]
      ]
      ```
  2. Assicurarsi che `lastScenario.json` contenga un riferimento valido.
  3. Inviare una richiesta GET a `/scenario_names`.
* **Risultato atteso:** Il server restituisce la lista di tutti i nomi degli scenari e l'ultimo scenario salvato.

**Test Case 6: Verifica dei log del server**

* **Descrizione:** Controllare che eventuali errori siano registrati correttamente nella console.
* **Passi:**
  1. Simulare un errore (es. file inesistente o malformato).
  2. Controllare la console del server.
* **Risultato atteso:** La console registra l'errore con dettagli utili per il debug.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests scName

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Recupero corretto dei nomi degli scenari e dell'ultimo scenario</td><td>Garantire che il componente restituisca i dati combinati correttamente.</td></tr><tr><td align="center">RT2</td><td>File <code>scenario.json</code> vuoto o inesistente</td><td>Assicurarsi che il server gestisce correttamente l'assenza o il contenuto vuoto del file.</td></tr><tr><td align="center">RT3</td><td>File JSON inesistente o vuoto</td><td>Assicurarsi che il server gestisca correttamente l'assenza o il contenuto vuoto del file.</td></tr><tr><td align="center">RT4</td><td>JSON malformato nel file</td><td>Verificare che il server gestisca correttamente un file JSON con dati non validi.</td></tr><tr><td align="center">RT5</td><td>Eliminazione di uno scenario tra più presenti</td><td>Garantire che l'eliminazione di uno scenario non influenzi gli altri presenti nel file.</td></tr><tr><td align="center">RT6</td><td>Verifica dei log del server</td><td>Controllare che i log contengano correttamente il nome dello scenario eliminato e gli errori.</td></tr></tbody></table>

***

## Route scPlayMode

### Requirements

**Avvio della modalità scenario selezionata e salvataggio come ultimo scenario usato:**

1. Gestire le richieste HTTP POST al percorso `/play_scenario_mode`.
2. Estrarre il nome dello scenario selezionato dal corpo della richiesta (`req.body`).
3. Leggere il contenuto del file JSON `./json/scenarios.json`:
   * Se il file non esiste o è vuoto, restituire un errore.
   * Cercare lo scenario con il nome corrispondente utilizzando il metodo `find`.
4. Salvare lo scenario selezionato in una variabile globale `modes` per utilizzo successivo.
5. Inviare un evento personalizzato `"modeReceived"` tramite un `EventEmitter`, passando i dati dello scenario selezionato.
6. Scrivere lo scenario selezionato nel file `./json/lastScenario.json` in formato JSON leggibile (2 spazi di indentazione).
7. Restituire una risposta HTTP 200 con un messaggio di conferma se l'operazione è riuscita.
8. Gestire eventuali errori, come:
   * Impossibilità di leggere o scrivere i file.
   * JSON malformato o dati mancanti.
   * Restituire una risposta HTTP 500 con un messaggio di errore.
9. Registrare nella console:
   * Il nome dello scenario ricevuto.
   * Eventuali errori durante l'operazione.

### Test

**Test Case 1: Avvio di uno scenario esistente**

* **Descrizione:** Verificare che uno scenario valido venga avviato correttamente.
* **Passi:**
  1.  Salvare i seguenti dati nel file `scenarios.json`:

      ```json
      jsonCopia codice[
        [{ "scenario": "Scenario1", "data": "Esempio1" }],
        [{ "scenario": "Scenario2", "data": "Esempio2" }]
      ]
      ```
  2.  Inviare una richiesta POST con il corpo:

      ```json
      jsonCopia codice"Scenario1"
      ```
  3. Controllare il file `lastScenario.json` e verificare la risposta del server.
* **Risultato atteso:**
  * Risposta HTTP 200 con il messaggio `"Data saved successfully"`.
  *   Il file `lastScenario.json` contiene lo scenario:

      ```json
      jsonCopia codice[
        { "scenario": "Scenario1", "data": "Esempio1" }
      ]
      ```

**Test Case 2: Avvio di uno scenario inesistente**

* **Descrizione:** Verificare il comportamento quando lo scenario non è presente nel file `scenarios.json`.
* **Passi:**
  1. Assicurarsi che `scenarios.json` non contenga lo scenario specificato.
  2.  Inviare una richiesta POST con il corpo:

      ```json
      jsonCopia codice"ScenarioNonEsistente"
      ```
* **Risultato atteso:**
  * Risposta HTTP 500 con il messaggio `"Data retrieval failed"`.
  * Nessun cambiamento al file `lastScenario.json`.

**Test Case 3: File `scenarios.json` vuoto o inesistente**

* **Descrizione:** Verificare il comportamento quando il file `scenarios.json` è vuoto o mancante.
* **Passi:**
  1. Eliminare o svuotare il file `scenarios.json`.
  2.  Inviare una richiesta POST con il corpo:

      ```json
      jsonCopia codice"ScenarioQualsiasi"
      ```
* **Risultato atteso:**
  * Risposta HTTP 500 con il messaggio `"Data retrieval failed"`.

**Test Case 4: File JSON malformato in `scenarios.json`**

* **Descrizione:** Simulare un file JSON malformato per `scenarios.json`.
* **Passi:**
  1.  Inserire dati non validi in `scenarios.json`:

      ```json
      jsonCopia codice[{ "scenario": "MancaVirgola" "data": "Errore" }]
      ```
  2. Inviare una richiesta POST valida.
* **Risultato atteso:**
  * Risposta HTTP 500 con il messaggio `"Data retrieval failed"`.

**Test Case 5: Emissione evento `modeReceived`**

* **Descrizione:** Verificare che l'evento `modeReceived` venga emesso correttamente con i dati dello scenario selezionato.
* **Passi:**
  1. Configurare un listener per l'evento `modeReceived`.
  2. Inviare una richiesta POST valida per avviare uno scenario.
  3. Verificare i dati ricevuti dal listener.
* **Risultato atteso:**
  * L'evento `modeReceived` viene emesso con i dati dello scenario corretto.

**Test Case 6: Verifica dei log del server**

* **Descrizione:** Controllare che il nome dello scenario ricevuto e gli eventuali errori siano registrati correttamente nella console.
* **Passi:**
  1. Inviare una richiesta valida per avviare uno scenario.
  2. Controllare la console del server.
* **Risultato atteso:**
  * La console registra il nome dello scenario ricevuto.
  * In caso di errore, la console registra il messaggio e i dettagli dell'errore.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>29/01/2024</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests scPlayMode

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Avvio di uno scenario esistente</td><td>Garantire che uno scenario valido venga avviato correttamente e salvato come ultimo.</td></tr><tr><td align="center">RT2</td><td>Avvio di uno scenario inesistente</td><td>Verificare che il server gestisca correttamente l'assenza dello scenario richiesto</td></tr><tr><td align="center">RT3</td><td>File <code>scenarios.json</code> vuoto o inesistente</td><td>Assicurarsi che il server gestisca correttamente file vuoti o mancanti.</td></tr><tr><td align="center">RT4</td><td>File JSON malformato in <code>scenarios.json</code></td><td>Verificare che il server gestisca errori di formattazione nel file JSON.</td></tr><tr><td align="center">RT5</td><td>Emissione evento <code>modeReceived</code></td><td>Controllare che l'evento personalizzato venga emesso correttamente con i dati corretti.</td></tr><tr><td align="center">RT6</td><td>Verifica dei log del server</td><td>Garantire che i dettagli dello scenario e gli eventuali errori vengano registrati.</td></tr></tbody></table>
