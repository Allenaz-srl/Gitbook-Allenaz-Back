# Voice Command (Code)

## 🔁 Ciclo principale e riconoscimento vocale

### 🧠 Obiettivo

Il ciclo principale dello script ha il compito di:

1. Ascoltare costantemente il microfono in tempo reale
2. Utilizzare il motore Vosk per riconoscere comandi vocali (da grammatica predefinita)
3. Attivare e gestire la modalita **"wake word"**
4. Riconoscere e inoltrare comandi vocali validi
5. Riconoscere immediatamente i **comandi di emergenza** in qualsiasi momento

### 🎧 Inizializzazione dell'ascolto

```python
with sd.RawInputStream(samplerate=16000, blocksize=8000, dtype='int16',
                       channels=1, callback=callback):
```

Questa linea apre uno stream audio continuo dal microfono, usando la libreria <kbd>**sounddevice**</kbd>**&#x20;.**

* samplerate = 16000 Hz: richiesto dal modello Vosk
* callback = calback: ogni blocco audio viene messo in una **`queue`** per elaborazione

### 📤 Struttura della callback audio

```python
def callback(indata, frames, time_info, status):
    if status:
        print("⚠️", status)
    q.put(bytes(indata))
```

* Lo stream è assincrono: ogni pezzo di audio viene convertito in bytes e inserito della coda.

### 🗣️ Ciclo di ascolto vocale

```python
while True:
    data = q.get()
```

* Ogni iterazione del ciclo elabora uno spezzone audio (chunk).

### ⏰ Timeout della modalità comando

```python
if wake_active and time.monotonic() > wake_timeout:
    print("😴 Wake mode timed out.")
    wake_active = False
```

* Quando il sistema entra in modalita **"wake word"**, ha 10 secondi (**`WAKE_DURATION`**) per accettare comandi vocali.
* Dopo il timeout, si disattiva e attende un nuovo trigger ("ciao alli").

### 🤖 Riconoscimento vocale con Vosk

```python
if recognizer.AcceptWaveform(data):
    result = json.loads(recognizer.Result())
    text = result.get("text", "").strip()
else:
    partial = json.loads(recognizer.PartialResult())
    text = partial.get("partial", "").strip()
```

* Se Vosk ha riconosciuto una frase completa, la estrae da **`Result()`**
* Altrimenti, estrae parole parziali da **`PartialResult()`**
* Tutti i risultati vengono convertiti in text (minuscolo, senza spazi)

### 🚨 Comandi di emergenza (sempre attivi)

```python
for cmd in ALWAYS_ON_COMMANDS:
    if text.startswith(cmd) and should_process(cmd):
        print("🛑 Emergency command detected:", cmd)
        # Handle emergency command here
```

* stop, fermati, alt, ecc. vengono gestiti immediatamente
* Funzionano anche a sistema inattiva
* Vengono filtrati per evitare ripetizioni ravvicinate tramite:

```python
def should_process(cmd):
    if cmd == last_command and (now - last_command_time) < DUPLICATE_DELAY:
        return False
```

### 🟢 Wake Word

```python
if not wake_active:
    for wake in WAKE_WORDS:
        if text.startswith(wake):
            wake_active = True
            wake_timeout = time.monotonic() + WAKE_DURATION
            print("🟢 Wake word detected:", wake)
```

* Dopo il triger "ciao alli", il sistema accetta comandi vocali per 10 secondi
* Il timer si resetta **ogni volta che un comando valido viene eseguito**

### ✅ Comandi vocali (solo in modalità attiva)

```python
if wake_active:
    for cmd in COMMANDS:
        if text.startswith(cmd) and should_process(cmd):
            print("✅ Command detected:", cmd)
            wake_timeout = time.monotonic() + WAKE_DURATION
            # Handle command here
```

* <kbd>"alli su"</kbd>, <kbd>"alli giu"</kbd>, ecc. vengono elaborati solo se wake word è attivo
* Il timer viene esteso dopo ogni comando riconosciuto

## 🧪 Voice Test

| Scenario                                  | Atteso                                    |
| ----------------------------------------- | ----------------------------------------- |
| `"ciao alli"` + `"alli su"`               | ✅ Comando riconosciuto                    |
| `"stop"` pronunciato in qualsiasi momento | 🛑 Comando di emergenza accettato         |
| Nessun suono dopo "ciao alli" per 10 sec  | 😴 Timeout modalità comando               |
| Dico due volte “stop” in 0.3s             | Secondo comando ignorato                  |
| Rumore o comandi non definiti             | Nessuna azione viene eseguita (sicurezza) |
