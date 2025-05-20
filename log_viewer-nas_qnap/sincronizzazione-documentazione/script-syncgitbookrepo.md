# Script syncGitbookRepo

## 🎯 Finalità

Questo modulo Node.js automatizza il processo di sincronizzazione della documentazione da piu repository GitHub (contenenti mirror GitBook) verso un repository Gitea centralizzato installato su server NAS interno. L'obiettivo e consolidare tutta la documentazione tecnica in un'unica fonte locale, in modo resiliente e accessibile anche offline.

## 🧱 Struttura del modulo

### 🔧 1. Importazione dei moduli

```javascript
const { execSync } = require("child_process");
const fs = require("fs");
const path = require("path");
```

Questi moduli core di Node.js permettono rispettivamente di:

* Eseguire comandi shell (`child_process`),
* Leggere, scrivere ed eliminare file o cartelle (`fs`),
* Gestire path cross-platform (`path`).

### 📁 2. Configurazioni iniziali

```javascript
const TEMP_DIR_BASE = "/Web/WebDocumentation";
const GITEA_REPO_NAME = "Allenaz-Doc";
const GITEA_USERNAME = "rkrasnozhon";
const GITEA_TOKEN = "TOKEN_PRIVATO";
const GITEA_REPO = `http://${GITEA_USERNAME}:${GITEA_TOKEN}@gitea:3000/${GITEA_USERNAME}/${GITEA_REPO_NAME}.git`;
```

* <kbd>TEMP\_DIR\_BASE</kbd>: directory locale in cui verranno clonati i repository.
* <kbd>GITEA\_REPO</kbd>: URL completo per il push verso il repository Gitea (incluso token personale per autenticazione HTTP).

{% hint style="info" %}
🔐**NOTA:** il token va protetto e mai esposto pubblicamente.
{% endhint %}

### 📚 3. Elenco dei repository di origine (GitHub)

```javascript
const REPOSITORIES = [
  { name: "...", github: "https://github.com/..." },
  ...
];
```

### ⚙️ 4. Funzioni principali

#### 🛠️ `runCommand(cmd, cwd)`

Esegue un comando shell in una directory specificata. Mostra anche a termine il comando lanciato per tracciabilita.

#### 🧹 `cleanDirectory(dir)`

Cancella in modo ricorsivo tutti i file e le sottocartelle all'interno della directory dir, se esiste.

#### 🧼 `removeInnerGitFolders(dir)`

Rimuove tutte le sottocartelle `.git` annidate nei singoli repository clonati, per evitare conflitti Git durante la fusione nel repository unico.

#### ✅ `ensureGitSafeDirectory(dir)`

Assicura che Git consideri la directory come sicura (necessario in alcuni ambienti, come Docker o ambienti root).

## 🔁 Funzione principale: `syncAll()`

La funzione `syncAll()` e il cuore del processo di sincronizzazione automatica della documentazione. Il suo scopo e raccogliere il contenuto da diversi repository GitHub, pulirlo e unirlo in un unico repository centralizzato ospitato su Gitea.

**Passaggi dettagliati:**

### 1. Pulizia cartella

```javascript
cleanDirectory(TEMP_DIR_BASE);
```

Cancella tutti i contenuti precedenti per evitare duplicazione o residui.

### 2. Clonazione di ogni repository Github:

```javascript
REPOSITORIES.forEach(({ name, github }) => {
  ...
  runCommand(`git clone ${github} "${localPath}"`, "/");
});
```

Ogni repository viene clonato in una sottocartella del path













