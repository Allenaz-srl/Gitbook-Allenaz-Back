# Logger

## **Requirements**

1. **Registrazione dei Log:**
   * Creare e gestire file di log in base a specifiche configurazioni di intervalli e categorie.
   * Supportare vari tipi di log, inclusi:
     * **Log generali:** Archiviazione in `logs/` con dimensione massima di 10 MB per file.
     * **Log a lungo termine:** Archiviazione in `longTermLog/` con dimensione massima di 100 MB per file.
     * **Log di debug per AMA1 e AMA2:** Archiviazione in `DebugLogs/AMA1` e `DebugLogs/AMA2`, rispettivamente.
     * **Log di I2C per AMA1 e AMA2:** Archiviazione in `I2CLogs/AMA1` e `I2CLogs/AMA2`, rispettivamente.
   * Assicurarsi che ogni file di log sia identificato in modo univoco dal numero seriale e dalla data/ora locale.
2. **Gestione degli errori:**
   * Notificare eventuali errori di sistema tramite la funzione `notifyError` con ID specifici per ogni tipo di errore:
     * Lettura/scrittura file.
     * Copia di file in caso di arresto o riavvio.
   * Salvare e aggiornare un elenco di file di log associati agli errori in `json/lastErrorLog.json`.
3. **Emettitore di errori:**
   * Attivare l'evento `errorEmitter` per segnare file problematici e copiarli in una directory di backup (`errorLogs/`).
4. **Rotazione dei Log:**
   * Aggiornare periodicamente i file di log in base al tempo corrente, con intervalli configurabili tramite `UARTSetup.json`.
5. **Funzioni di supporto:**
   * Gestire l'eliminazione di file di log obsoleti dall'elenco degli errori.
   * Controllare e copiare i file di log mancanti durante il riavvio.

***

## **Test**

1. **Test di registrazione:**
   * **Scenario:** Creazione di file di log in base al tipo (generale, a lungo termine, AMA1/AMA2 debug, AMA1/AMA2 I2C).
   * **Pass/Fail:** I file devono essere creati con nome e percorso corretti.
2. **Test sulla dimensione dei file:**
   * **Scenario:** Simulare il superamento della dimensione massima (10 MB o 100 MB).
   * **Pass/Fail:** Deve avvenire la rotazione del file.
3. **Test sull'integrità dei dati:**
   * **Scenario:** Registrare dati consecutivi in log differenti.
   * **Pass/Fail:** Ogni file deve contenere solo i dati associati all'intervallo temporale previsto.
4. **Test di gestione degli errori:**
   * **Scenario:** Provocare errori (ad esempio, mancanza di file sorgente).
   * **Pass/Fail:** Gli errori devono essere notificati tramite `notifyError`.
5. **Test di copia file in errore:**
   * **Scenario:** Simulare la necessità di copiare file non salvati in `errorLogs/`.
   * **Pass/Fail:** I file devono essere copiati correttamente e rimossi dall'elenco degli errori.
6. **Test di configurazione intervalli:**
   * **Scenario:** Configurare intervalli di registrazione personalizzati tramite `UARTSetup.json`.
   * **Pass/Fail:** I log devono rispettare l'intervallo configurato.
7. **Test di persistenza:**
   * **Scenario:** Riavviare l'applicazione dopo un crash o spegnimento.
   * **Pass/Fail:** Tutti i file mancanti devono essere copiati e l'elenco degli errori aggiornato.

### Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>12/06/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>12/06/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>12/06/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>12/06/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC5</td><td>12/06/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC6</td><td>12/06/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC7</td><td>12/06/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>
