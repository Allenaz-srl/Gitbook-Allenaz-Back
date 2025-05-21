# Integrazione nel Sistema (in app.js)

## 🧩 Integrazione della Sincronizzazione in `app.js`

### 🧱 Descrizione

Nel file `app.js`, la funzione di sincronizzazione `syncAll()` viene integrata all’interno di un meccanismo **programmato e automatico** grazie al pacchetto `node-cron`. L'obiettivo è garantire che la documentazione GitBook venga sincronizzata con il repository Gitea in **orari prestabiliti**, senza intervento manuale.

### 📆 Pianificazione della Sincronizzazione (cron)

```javascript
const cron = require("node-cron");
```

Il modulo <kbd>node-cron</kbd> consente di pianificare compiti in modo simile al **cron** di Unix. Viene Definita una constante `SCHEDULES` che contiene tre orari in cui eseguire la sincronizzazione ogni giorno:

```javascript
const SCHEDULES = ["0 4 * * *", "0 10 * * *", "0 16 * * *"];
```

Questi orari corrispondono a:

* 06:00 (UTC +2)
* 12:00 (UTC +2)
* 18:00 (UTC +2)

Quindi, la sincronizzazione viene eseguita tre volte al giorno, coprendo le principali fasce orarie lavorative.

### ⚙️ Esecuzione del processo

Per ciascun orario definito, viene creato un processo schedulato che:

1. Construisce il percorso assoluto del file `syncGitbookRepo.js` (dove risiede la logica di sincronizzazione).
2. Lancia uno script Node.js in un sottoprocesso (`child_process.exec`) per garantire l'isolamento dell'esecuzione.
3. Gestisce l'output:
   * Eventuali errori sono loggati.
   * L'output <kbd>stdout</kbd> e <kbd>stderr</kbd> sono direttamente reindirizzati al terminale del processo principale.

Esempio:

```javascript
const child = exec(`node "${scriptPath}"`, (err, stdout, stderr) => {
  ...
});
```

Questo approccio ha diversi vantaggi:

* Il processo di sincronizzazione è **separato** dal server principale, evitando blocchi o crash in caso di errori.
* È facile da **monitorare** e da **estendere** (ad esempio, aggiungendo notifiche via email o messanger come telegram).

### 🟢 Messaggio di conferma iniziale

Alla fine dello script è presente un semplice log per confermare che la pianificazione è attiva:

```javascript
console.log("📅 Planning sync is started at: 6:00, 12:00, 18:00 every day.");
```

### ✅ Benefici

* Automazione completa del ciclo GitBook → GitHub → Nas → Gitea.
* Manutenzione ridotta: basta aggiornare la documentazione su Gitbook.
* Alta affidabilita grazie a processi isolati e loggati.
* Facile da personalizzare: aggiunta di orari o gestione avanzata degli errori.
