# Voice Command (Code)

## 🔁 Ciclo principale e riconoscimento vocale

### 🧠 Obiettivo

Il ciclo principale dello script Python è progettato per:

* Ascoltare costantemente il microfono in tempo reale
* Riconoscere comandi vocali tramite **Vosk**, con vocabolario limitato(predefinito)
* Gestire **due modalità di attivazione vocale ("wake word")** con timer distinti
* Eseguire comandi in base alla modalità attiva
* Riconoscere e reagire **immediatamente** ai comandi di emergenza, indipendentemente dallo stato

### 🎧 Inizializzazione dell'ascolto

```python
with sd.RawInputStream(samplerate=16000, blocksize=8000, dtype='int16',
                       channels=1, callback=callback):
```

Questa linea apre uno stream audio continuo dal microfono, usando la libreria <kbd>**sounddevice**</kbd>**&#x20;.**

* Lo stream passa i dati audio grezzi a una **coda (`queue`)**
* La **frequenza di campionamento (16kHz)** è compatibile con i modelli Vosk

### 📤 Struttura della callback audio

```python
def callback(indata, frames, time_info, status):
    if status:
        print("⚠️", status)
    q.put(bytes(indata))
```

* Ogni blocco di dati audio viene **trasformato in bytes** e inserito nella coda `q`, per l’elaborazione da parte del motore vocale.

### 🗣️ Ciclo di ascolto vocale

```python
while True:
    data = q.get()
```

* Ogni iterazione estrae un blocco di dati audio e lo sottopone al riconoscimento vocale.

### ⏰ Timeout della modalità attiva (Wake Word)

```python
if wake_mode and time.monotonic() > wake_timeout:
    print("😴 Wake mode timed out.")
    wake_mode = None
```

* Se è attiva una modalità (`"main"` o `"movement"`), ma **non viene ricevuto alcun comando entro il tempo previsto**, il sistema torna inattivo
* Il timeout è configurabile:
  * `10s` per modalità "main"
  * `30s` per modalità "movement"

### 🤖 Riconoscimento vocale con Vosk

```python
if recognizer.AcceptWaveform(data):
    result = json.loads(recognizer.Result())
else:
    result = json.loads(recognizer.PartialResult())

text = result.get("text", "").strip()
```

* Il motore Vosk restituisce una stringa testuale del comando riconosciuto
* Il testo viene **pulito e normalizzato** per l'elaborazione

### 🚨 Comandi di emergenza (sempre attivi)

```python
for cmd in COMMANDS_EMERGENCY:
    if text.startswith(cmd) and should_process(cmd):
        print("🛑 Emergency command detected:", cmd)
        break
```

* Questi comandi sono **sempre attivi**, indipendentemente dalla modalità
* Esempi: `"stop"`, `"fermati"`, `"aiuto"`...
* Usano il controllo anti-duplicazione:

```python
def should_process(cmd):
    if cmd == last_command and (now - last_command_time) < DUPLICATE_DELAY:
        return False
```

### 🟢 Attivazione vocale (Wake Words)

```python
if not wake_mode:
    if WAKE_WORD_MAIN in text:
        wake_mode = "main"
        reset_wake_timer(WAKE_DURATION_MAIN)
        print("🟢 Wake word ciao alli detected.")
    elif WAKE_WORD_MOVEMENT in text:
        wake_mode = "movement"
        reset_wake_timer(WAKE_DURATION_MOVEMENT)
        print("🟡 Wake word alli muovi detected.")
```

<table><thead><tr><th width="156">Wake Word</th><th width="155">Modalità attiva</th><th width="100.3333740234375">Durata</th><th>Comandi disponibili</th></tr></thead><tbody><tr><td><kbd>"ciao alli"</kbd></td><td><kbd>main</kbd></td><td>10 sec</td><td><code>inizia movimento</code>, <code>reset errore</code>, <code>posizione ...</code></td></tr><tr><td><kbd>"alli muovi"</kbd></td><td><kbd>movement</kbd></td><td>30 sec</td><td><code>alli su</code>, <code>alli giu</code>, <code>ok</code>, ecc.</td></tr></tbody></table>

### 🧭 Comandi nella modalità `"main"`

```python
pythonCopiaModificaif wake_mode == "main":
```

### **🟢 Comando Start**

```python
pythonCopiaModificaif text.startswith("inizia movimento"):
    print("🚀 Start command")
```

### **♻️ Comando Reset**

```python
pythonCopiaModificaif text.startswith("reset errore"):
    print("♻️ Reset command")
```

### **📌 Posizione salvata**

```python
pythonCopiaModificaif text.startswith("posizione"):
    print("📌 Saved position command:", text)
```

***

### 🤖 Comandi nella modalità `"movement"`

```python
pythonCopiaModificaif wake_mode == "movement":
```

I comandi validi sono:

* `"alli su"` – movimento breve verso l’alto
* `"alli giu"` – movimento breve verso il basso
* `"alli sali"` – movimento continuo verso l’alto
* `"alli scendi"` – movimento continuo verso il basso
* `"ok"` – arresta movimenti continui

Ogni comando **resetta il timer** a 30 secondi.

## 🧪 Voice Test

<table><thead><tr><th width="224.888916015625">Scenario</th><th width="260.333251953125">Risultato</th><th width="83.5555419921875">Person</th><th width="102.4443359375">Data</th><th width="84.0001220703125" data-type="checkbox">Checked</th></tr></thead><tbody><tr><td>Nessun wake word → <code>"stop"</code></td><td>Comando d’emergenza accettato</td><td>Roman</td><td>30/05/25</td><td>true</td></tr><tr><td><code>"ciao alli"</code> → <code>"inizia movimento"</code></td><td>Modalità attiva + comando start</td><td>Roman</td><td>30/05/25</td><td>true</td></tr><tr><td><code>"ciao alli"</code> → silenzio per 10s</td><td>Timeout modalità</td><td>Roman</td><td>30/05/25</td><td>true</td></tr><tr><td><code>"alli muovi"</code> → <code>"alli su"</code> → <code>"ok"</code></td><td>Movimento + arresto</td><td>Roman</td><td>30/05/25</td><td>true</td></tr><tr><td><code>"alli muovi"</code> → silenzio per 30s</td><td>Timeout movimento</td><td>Roman</td><td>30/05/25</td><td>true</td></tr><tr><td><code>"ciao alli"</code> → <code>"reset errore"</code></td><td>Riavvio software</td><td>Roman</td><td>30/05/25</td><td>true</td></tr><tr><td><code>"alli muovi"</code> → <code>"posizione tavolo"</code></td><td>Ignorato (non supportato in questa modalità)</td><td>Roman</td><td>30/05/25</td><td>true</td></tr></tbody></table>
