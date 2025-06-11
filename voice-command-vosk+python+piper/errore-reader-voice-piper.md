# Errore Reader Voice (PIPER)

## 🗣️ Sintesi Vocale degli Errori con Piper

### 🎯 Obiettivo

In questo modulo, il sistema è in grado di generare messagi vocali in italiano offline per comunicare errori relivati nel funzionamento dell'esoscheletro.\
La generazione vocale è gestita tramite **PIPER**, un motore TTS (Text-to-Speech) leggero e performante progettato per dispositivi embedded come il Raspberry Pi 4B+.

📦 Che cos’è Piper?

Piper èun sintetizzatore vocale open-source sviluppato dal progetto Rhasspy (github), ottimizzato per CPU ARM e dispositivi a bassa potenza. Supporta modelli preaddestrati in piu lingue e funziona **completamente offline**.

* Formato modelli: .onnx (inference ONNX)
* Supporta piu lingue e voci (es. italiano - voce femminile "Paola" e maschile "Riccardo"
* Compatibile con ARM64 - ideale per Raspberry Pi

### ⚙️ Installazione di Piper su Raspberry Pi 4B+

#### Requisiti

* Raspberry Pi 4B+ con **Ubuntu Server 64-bit**
* Connessione internet per scaricare file iniziali
* Permessi <kbd>**sudo**</kbd>&#x20;



1. **Scaricare e decomprimere Piper**

```bash
wget https://github.com/rhasspy/piper/releases/download/2023.11.14-2/piper_linux_aarch64.tar.gz
tar -xzf piper_linux_aarch64.tar.gz
cd piper
```

Questo comando scarica la versione binaria compilata per sistemi ARM64



2. Scaricare la voce italiana "Paola"

```bash
wget https://huggingface.co/rhasspy/piper-voices/resolve/main/it/it_IT/paola/medium/it_IT-paola-medium.onnx
wget https://huggingface.co/rhasspy/piper-voices/resolve/main/it/it_IT/paola/medium/it_IT-paola-medium.onnx.json
```

Questa voce ("Paola") e media qualità, ottimizzata per il compromesso tra performance e qualità audio.



3. Impostare i permessi eseguibili

```bash
chmod +x piper
```

Questo rende l'eseguibile <kbd>piper</kbd> avviabile dal terminale

### 🛠️ File di configurazione <kbd>**.onnx.json**</kbd>

Ogni modello ha un file **.onnx.json** associato, che definisce i parametri della voce.\
Per "Paola", il contenuto e solitamente il seguente:

```json
    "dataset": "paola",
    "audio": {
        "sample_rate": 22050,
        "quality": "medium"
    },
    "espeak": {
        "voice": "it"
    },
    "inference": {
        "noise_scale": 0.9,
        "length_scale": 1.25,
        "noise_w": 0.9
    },
```

#### 📚 Spiegazione dei parametri

🗂️ <kbd>**"dataset": "paola"**</kbd>

* Descrizione: Nome del dataset o modello utilizzato per l'addestramento della voce.
* In questo caso, il modello e stato addestrato sulla voce "Paola" (italiano femminile)

🔊 <kbd>**"audio": {...}**</kbd>

Contiene informazioni legate alle qualita del suono

* **"sample\_rate":** Frequenza di campionamento dell'audio di output. 22050 Hz e un buon compromesso tra qualita e leggerezza del file
* **"quality":** Qualita del modello ("low", "medium", "high"). **"medium"** e un modello bilanciato, adatto alla maggior parte dei dispositivi.

🗣️ <kbd>**"espeak": {...}**</kbd>

Sezione per l'integrazione con il sistema di trascrizione fonemica e pronuncia

* **"voice"**: Lingua e voce per espeak-ng, il sistema che converte il testo in fonemi. `"it"` indica italiano

🔹 Questo garantisce che la pronuncia sia coerente con le regole fonetiche italiane.

🧠 <kbd>**"inference": {...}**</kbd>

Controlla il comportamento del modello durante la sintesi vocale, ovvero come viene "parlato" il testo.

* **"noise\_scale"**: Controlla la variazione **dell'intonazione** (prosodia). Più e basso, più la voce sarà monotona; più e alta, più sarà espressiva. Default: <kbd>0.9</kbd>
* **"length\_scale"**: Modifica la velocita di parlato. Valore > 1 = più veloce. Default 1.25 (più chiaro e rallentato)
* **"noise\_w"**: Aggiunge variazione casuali alla voce per sembrare più naturale. Simile a noise\_scale, ma più locale. Default: <kbd>0.9</kbd>&#x20;

### 🎧 Suggerimenti

* Per ottenere una voce più veloce: abbassare **`length_scale`** (es. 1.0)
* Per ottenere una voce più neutra/robotica: ridurre **`noise_scale`** a 0.3-0.5
* Per ottenere una voce più "umana": lasciare i valori alti o aumentare leggermente
