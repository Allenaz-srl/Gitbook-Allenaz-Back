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

# Setting

{% hint style="info" %}
Questa pagina contiene i requisiti e le prove per tutti i "routes" nella sezione SETTING.
{% endhint %}

## Route stRestPos

### Requirements

Recupero e calcolo della posizione di riposo:

1. Gestire le richieste HTTP GET al percorso `/last_saved_rest_pos`.
2. Leggere il file JSON `./json/restPos.json` per ottenere i dati della posizione di riposo:
   * Se il file non esiste o è vuoto, restituire un errore.
3.  Convertire i valori grezzi della posizione di riposo in gradi utilizzando la funzione&#x20;

    `updateActualPositionDeg`:

    * Calcolare i valori basandosi su parametri predefiniti:
      * Risoluzioni dei sensori di posizione (`position_sensor_resolution`).
      * Lunghezze delle corse dei sensori (`position_sensor_stroke`).
      * Offset cinematici (`position_sensor_offset_kinematics`).
      * Direzione invertita (`position_sensor_dir_inverted_kinematics`).
    * Applicare la formula per la conversione a ciascun asse.
4. Restituire un array con i valori convertiti in formato JSON nella risposta.
5. Gestire eventuali errori, come:
   * Impossibilità di leggere o analizzare il file.
   * Dati malformati o mancanti.
   * Restituire una risposta HTTP 500 con un messaggio di errore.
6. Registrare nella console:
   * Gli errori occorsi durante l'operazione.
   * I valori grezzi letti e i valori convertiti.

### Test

**Test Case 1: Recupero della posizione di riposo salvata**

* **Descrizione**: Verificare che i dati di posizione di riposo vengono recuperati e convertiti correttamente.
* **Passi:**
  1.  Salvare i seguenti dati nel file `restPos.json`:

      ```json
      jsonCopia codice{
        "axis1": 120,
        "axis2": 110,
        "axis3": 100,
        "axis4": 90,
        "axis5": 80
      }
      ```
  2. Configurare i parametri cinematici, ad esempio:
     * `position_sensor_resolution = [1024, 1024, 1024, 1024, 1024]`
     * `position_sensor_stroke = [360, 360, 360, 360, 360]`
     * `position_sensor_offset_kinematics = [10, 10, 10, 10, 10]`
     * `position_sensor_dir_inverted_kinematics = [1, 1, 1, 1, 1]`
  3. Inviare una richiesta GET a `/last_saved_rest_pos`.
* **Risultato atteso:**
  *   Risposta HTTP 200 con i valori convertiti in gradi:

      ```json
      jsonCopia codice[38.9, 38.9, 38.9, 38.9, 38.9]
      ```

**Test Case 2: `restPos.json` inesistente**

* **Descrizione**: Verificare il comportamento quando il file `restPos.json` non esiste.
* **Passi**:
  1. Rinominare o eliminare temporaneamente il file `restPos.json` .
  2. Inviare una richiesta GET a `/last_saved_rest_pos` .
* **Risultato atteso**:
  * Risposta HTTP 500 con il messaggio `"Data retrieval failed"` .

**Test Case 3: File restPos.json vuoto**

* **Descrizione:** Verificare il comportamento quando il file `restPos.json` è vuoto.
* **Passi:**
  1. Salvare un file vuoto o con contenuto non valido in `restPos.json` .
  2. Inviare una richiesta GET a `/last_saved_rest_pos` .
* **Risultato atteso:**
  * Risposta HTTP 500 con il messaggio `"Data retrieval failed"` .

**Test Case 4: Dati malformati in `restPos.json`**

* **Descrizione:** Simulare un file JSON malformato.
* **Passi:**
  1.  Inserire dati non validi in `restPos.json`:

      ```json
      jsonCopia codice{ "axis1": 120 "axis2": 110 }
      ```
  2. Inviare una richiesta GET a `/last_saved_rest_pos`.
* **Risultato atteso:**
  * Risposta HTTP 500 con il messaggio `"Data retrieval failed"` .

**Test Case 5: Calcolo della posizione con parametri cinematici diversi**

* **Descrizione:** Verificare che il calcolo della posizione di riposo cambi correttamente al variare dei parametri cinematici.
* **Passi:**
  1.  Salvare i seguenti dati in `restPos.json`:

      ```json
      jsonCopia codice{
        "axis1": 150,
        "axis2": 140,
        "axis3": 130,
        "axis4": 120,
        "axis5": 110
      }
      ```
  2. Configurare i parametri cinematici con valori non uniformi, ad esempio:
     * `position_sensor_resolution = [1024, 2048, 512, 1024, 512]`
     * `position_sensor_stroke = [360, 180, 360, 360, 180]`
     * `position_sensor_offset_kinematics = [20, 30, 15, 25, 10]`
     * `position_sensor_dir_inverted_kinematics = [1, -1, 1, -1, 1]`
  3. Inviare una richiesta GET a `/last_saved_rest_pos`.
* **Risultato atteso:**
  * Risposta HHTP 200 con valori calcolati corretti in base ai nuovi parametri.

**Test Case 6: Verifica dei log del server**

* **Descrizione:** Controllare che i dati letti e convertiti siano registrati correttamente nella console.
* **Passi:**
  * Inviare una richiesta valida a `/last_saved_rest_pos` .
  * Controllare i log della console del server.
* **Risultato atteso:**
  * La console registra i dati grezzi letti e i valori convertiti.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests scGetMode

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Recupero della posizione di riposo</td><td>Assicurarsi che i dati salvati vengano recuperati e convertiti correttamente in gradi.</td></tr><tr><td align="center">RT2</td><td>File <code>restPos.json</code> inesistente</td><td>Verificare che il server gestisca correttamente l'assenza del file richiesto.</td></tr><tr><td align="center">RT3</td><td>File <code>restPos.json</code> vuoto</td><td>Garantire che il server reagisca appropriatamente a un file JSON vuoto o carrotto.</td></tr><tr><td align="center">RT4</td><td>Dati malformati in <code>restPos.json</code></td><td>Controllare la gestione degli errori in caso di dati non validi nel file JSON.</td></tr><tr><td align="center">RT5</td><td>Calcolo con parametri cinematici differenti</td><td>Verificare che il calcolo cambi correttamente in base ai parametri configurati.</td></tr><tr><td align="center">RT6</td><td>Verificare dei log del server</td><td>Assicurarsi che i dati "grezzi" e convertiti siano registrati correttamente per il debug.</td></tr></tbody></table>

***

## Route stWearingPos

### Requirements

**Recupero e calcolo della posizione indossata:**

1. Gestire le richieste HTTP GET al percorso `/last_saved_wearing_pos`.
2. Leggere il file JSON `./json/wearingPos.json` per ottenere i dati della posizione indossata:
   * Se il file non esiste o è vuoto, restituire un errore.
3. Convertire i valori grezzi della posizione indossata in gradi utilizzando la funzione `updateActualPositionDeg`:
   * Calcolare i valori basandosi su parametri predefiniti:
     * Risoluzioni dei sensori di posizione (`position_sensor_resolution`).
     * Lunghezze delle corse dei sensori (`position_sensor_stroke`).
     * Offset cinematici (`position_sensor_offset_kinematics`).
     * Direzione invertita (`position_sensor_dir_inverted_kinematics`).
   * Applicare la formula per la conversione a ciascun asse.
4. Restituire un array con i valori convertiti in formato JSON nella risposta.
5. Gestire eventuali errori, come:
   * Impossibilità di leggere o analizzare il file.
   * Dati malformati o mancanti.
   * Restituire una risposta HTTP 500 con un messaggio di errore.
6. Registrare nella console:
   * Gli errori occorsi durante l’operazione.
   * I valori grezzi letti e i valori convertiti.

### Test

**Test Case 1: Recupero della posizione indossata salvata**

* **Descrizione:** Verificare che i dati della posizione indossata vengano recuperati e convertiti correttamente.
* **Passi:**
  1.  Salvare i seguenti dati nel file `wearingPos.json`:

      ```json
      jsonCopia codice{
        "axis1": 120,
        "axis2": 110,
        "axis3": 100,
        "axis4": 90,
        "axis5": 80
      }
      ```
  2. Configurare i parametri cinematici, ad esempio:
     * `position_sensor_resolution = [1024, 1024, 1024, 1024, 1024]`
     * `position_sensor_stroke = [360, 360, 360, 360, 360]`
     * `position_sensor_offset_kinematics = [10, 10, 10, 10, 10]`
     * `position_sensor_dir_inverted_kinematics = [1, 1, 1, 1, 1]`
  3. Inviare una richiesta GET a `/last_saved_wearing_pos`.
* **Risultato atteso:**
  *   Risposta HTTP 200 con i valori convertiti in gradi:

      ```json
      jsonCopia codice[38.9, 38.9, 38.9, 38.9, 38.9]
      ```

**Test Case 2: File `wearingPos.json` inesistente**

* **Descrizione:** Verificare il comportamento quando il file `wearingPos.json` non esiste.
* **Passi:**
  1. Rinominare o eliminare temporaneamente il file `wearingPos.json`.
  2. Inviare una richiesta GET a `/last_saved_wearing_pos`.
* **Risultato atteso:**
  * Risposta HTTP 500 con il messaggio `"Data retrieval failed"`.

**Test Case 3: File `wearingPos.json` vuoto**

* **Descrizione:** Verificare il comportamento quando il file `wearingPos.json` è vuoto.
* **Passi:**
  1. Salvare un file vuoto o con contenuto non valido in `wearingPos.json`.
  2. Inviare una richiesta GET a `/last_saved_wearing_pos`.
* **Risultato atteso:**
  * Risposta HTTP 500 con il messaggio `"Data retrieval failed"`.

**Test Case 4: Dati malformati in `wearingPos.json`**

* **Descrizione:** Simulare un file JSON malformato.
* **Passi:**
  1.  Inserire dati non validi in `wearingPos.json`:

      ```json
      jsonCopia codice{ "axis1": 120 "axis2": 110 }
      ```
  2. Inviare una richiesta GET a `/last_saved_wearing_pos`.
* **Risultato atteso:**
  * Risposta HTTP 500 con il messaggio `"Data retrieval failed"`.

**Test Case 5: Calcolo della posizione con parametri cinematici diversi**

* **Descrizione:** Verificare che il calcolo della posizione indossata cambi correttamente al variare dei parametri cinematici.
* **Passi:**
  1.  Salvare i seguenti dati in `wearingPos.json`:

      ```json
      jsonCopia codice{
        "axis1": 150,
        "axis2": 140,
        "axis3": 130,
        "axis4": 120,
        "axis5": 110
      }
      ```
  2. Configurare i parametri cinematici con valori non uniformi, ad esempio:
     * `position_sensor_resolution = [1024, 2048, 512, 1024, 512]`
     * `position_sensor_stroke = [360, 180, 360, 360, 180]`
     * `position_sensor_offset_kinematics = [20, 30, 15, 25, 10]`
     * `position_sensor_dir_inverted_kinematics = [1, -1, 1, -1, 1]`
  3. Inviare una richiesta GET a `/last_saved_wearing_pos`.
* **Risultato atteso:**
  * Risposta HTTP 200 con valori calcolati corretti in base ai nuovi parametri.

**Test Case 6: Verifica dei log del server**

* **Descrizione:** Controllare che i dati letti e convertiti siano registrati correttamente nella console.
* **Passi:**
  1. Inviare una richiesta valida a `/last_saved_wearing_pos`.
  2. Controllare i log della console del server.
* **Risultato atteso:**
  * La console registra i dati grezzi letti e i valori convertiti

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>08/10/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

### Regression tests stWearingPos

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Recupero della posizione indossata salvata</td><td>Assicurarsi che i dati salvati vengano recuperati e convertiti correttamente in gradi.</td></tr><tr><td align="center">RT2</td><td>File <code>wearingPos.json</code> inesistente</td><td>Verificare che il server gestisca corretamente l'assenza del file richiesto</td></tr><tr><td align="center">RT3</td><td>File <code>earingPos.json</code> vuoto</td><td>Garantire che il server reagisca appropriatamente a un file JSON vuoto o corrotto.</td></tr><tr><td align="center">RT4</td><td>Dati malformati in <code>wearingPos.json</code></td><td>Controllare la gestione degli errori in caso di dati non validi nel file JSON.</td></tr><tr><td align="center">RT5</td><td>Calcola con parametri cinematici differenti</td><td>Verificare che il calcolo cambi correttamente in base ai parametri configurati.</td></tr><tr><td align="center">RT6</td><td>Verificare dei log del server</td><td>Assicurarsi che i dati "grezzi" e convertiti siano registrati correttamente per il debug.</td></tr></tbody></table>
