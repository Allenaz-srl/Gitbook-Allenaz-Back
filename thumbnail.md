---
description: '📂 API: /api/thumbnail/:filename'
---

# Thumbnail

## 🎯 Descrizione

Questo endpoint serve come proxy interno per fornire immagini _**thumbnail**_ di disegni tecnici salvati nel percorso NAS <kbd>/Drawings/Thumbnail</kbd> . E progettato per essere utilizzato da **altre applicazioni nella stessa rete**, che **non hanno accesso diretto alla cartella condivisa**, ma possono interrogare <kbd>Log\_Viewer</kbd> tramite HTTP.

## 🛣️ Rotta

```bash
GET /api/thumbnail/:filename
```

| Parametro   | Tipo     | Descrizione                                            |
| ----------- | -------- | ------------------------------------------------------ |
| `:filename` | `string` | Nome file completo (con estensione, es: `ABC-123.jpg`) |

## 🧠 Funzionamento interno

```javascript
const filename = decodeURIComponent(req.params.filename);
const filePath = path.join("/Drawings/Thumbnail", filename);
```

* Il nome del file viene decodificato in caso di caratteri speciali (%20, ecc.).
* Si costituisce il **percorso assoluto** al file nella cartella <kbd>Thumbnail</kbd>.

### ✅ Se il file esiste:

```javascript
res.sendFile(filePath)
```

→ L'immagine viene servita come file statico.

### ❌ Se il file non esiste:

```javascript
res.status(404).send("Not found")
```

### ❌ Se c’è un errore nel caricamento:

```javascript
res.status(500).send("Error to send file")
```

## 📂 Esempio di struttura (NAS)

```markdown
/Drawings
  └── Thumbnail
        ├── ABC-001.jpg
        ├── XYZ-101.png
        └── Schema_23.svg
```

## 🧪 Esempi di richieste

### ✅ Richiesta corretta:

```perl
GET http://<nas-ip>:<port>/api/thumbnail/ABC-001.jpg
```

📥 **Risposta:**

* Header: Content-type: image/jpeg
* Body: contenuto binario del file

### ❌ File non trovato:

```plsql
GET /api/thumbnail/Inesistente.png
→ 404 Not found
```

## 🛠️ Test suggeriti

| Caso                                    | Metodo                                        | Risultato previsto       |
| --------------------------------------- | --------------------------------------------- | ------------------------ |
| File esistente                          | `curl http://localhost/api/thumbnail/XYZ.png` | Immagine restituita      |
| File con spazi o caratteri speciali     | `ABC%2012.jpg`                                | Immagine restituita      |
| File inesistente                        | Nome fittizio                                 | `404 Not found`          |
| Permessi mancanti (es. dir non montata) | Simulare accesso negato                       | `500 Error to send file` |

## 📦 Integrazione nel progetto

Nel file principale **app.js**:

```javascript
const thumbnailRouter = require("./thumbnailRouter");
app.use("/api/thumbnail", thumbnailRouter);
```
