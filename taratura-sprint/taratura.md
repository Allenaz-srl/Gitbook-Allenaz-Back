---
description: taratura.js
---

# Taratura

### 🔍 Descrizione generale

Il modulo taratura.js e responsabile dell'intero processo automatico di calibrazione (taratura) degli assi di un esoscheletro. L'interfaccia principale gira su Node.js su Raspberry Pi 4B+ e comunica con il microcontrollore master (Arduino) tramite:

* Seriale per l'invio delle posizioni target agli attuatori tramite sendViaSerialComm()
* SPI/CAN (via spiComm.js) per leggere lo stato di movimento e la coppia effetiva (actualTorque)

### 📂 File e risorse utilizzati

<table><thead><tr><th width="205.11114501953125">Componente</th><th>Scopo</th></tr></thead><tbody><tr><td>taratura.js</td><td>Script principale di gestione ciclo di calibrazione</td></tr><tr><td>serialComm.js</td><td>Comunicazione seriale con Arduino (trasmissione comandi)</td></tr><tr><td>spiComm.js</td><td>Lettura di flag (motionOnGoing) e sensori di coppia (actualTorue)</td></tr><tr><td>json/taraturaLimits.json</td><td>File di configurazione: limit/start/stop/step per ogni asse</td></tr><tr><td>json/ROM.json</td><td>Range hardware (ROM) ammissibilli per ciascun asse</td></tr><tr><td>json/output.csv</td><td>File finale con i dati raccolti: posizione e coppia per asse</td></tr></tbody></table>

### ⚙️ Funzionalità e flusso logico

#### 🔁 Ciclo principale: `performTaratura()`&#x20;

1. Caricamento dei limiti:

```javascript
const limits = loadLimits();
```

↳ Legge e valida i limiti da `taraturaLimits.json`, eseguendo controlli su `step` e range.

2. Controllo ROM:

```javascript
if (!areLimitsWithinROM(limits, romData)) return;
```

↳ Ogni asse deve avere valori `start/stop` **dentro il range definito** in `ROM.json`.

3. Per ogni combinazione di posizione:

* Invia messaggio di inizialiazzazione:

```javascript
await sendViaSerialComm(10, 4);
```

* Invia la posizione di ciascun asse tramite:

```javascript
const targetPositionBitsArray = updateActualPositionEx(tempPosition);
await sendViaSerialComm(address, targetPositionBits);
```

* Attende il completamento del movimento:

```javascript
await waitUntilMotionComplete();
```

* Legge i valori della coppia dal master:

```java
actualTorque = "100,200,-150,80" → [100,200,-150,80]
```

* Salva il tutto in `output.csv`

### 🔎 Analisi dettagliata delle funzioni

#### `loadLimits()`

📄 **Input:** file `json/taraturaLimits.json`

```json
{
  "axis": 4,
  "start": 8,
  "stop": 110,
  "step": 2
}
```

📤 **Output:** array di oggetti con logica di inversione automatica per step negativi:

```json
[
  { axis: 4, start: 8, stop: 110, step: 2 },
  { axis: 2, start: 10, stop: -100, step: -2 },
  ...
]
```

📍 Logica notevole(controllo dei limiti iniziali):

```javascript
if (limits[i].start > limits[i].stop) {
    limits[i].step = -Math.abs(limits[i].step);
}
```

#### `areLimitsWithinROM(limits, romData)`

📋 **Controlla che i limiti di ogni asse siano dentro l’intervallo fisico valido.**

🔧 Estratti dal file `ROM.json`:

```json
{
  "101": 0,
  "102": 120,
  "103": -120,
  "104": 100,
  ...
}
```

📍 Logica chiave:

```javascript
const minROMKey = (101 + (limits[i].axis-1) * 2).toString();
const maxROMKey = (102 + (limits[i].axis-1) * 2).toString();
```

#### `sendInitMessage()`&#x20;

Inizializza ogni ciclo di taratura, inviando:

```javascript
sendViaSerialComm(10, 4);
```

{% hint style="info" %}
Significa “inizio movimento controllato” lato Arduino.
{% endhint %}

#### `updateActualPositionEx(tempPosition)`&#x20;

Converte gradi in formato binario comprensibile da Arduino, es.:

```javascript
tempPosition = [0, 0, 0, 12]
→ [0, 0, 0, 512] // esempio output
```

#### `waitUntilMotionComplete()`&#x20;

Loop asincrono che attende il termine del movimento:

```javascript
if (motionOnGoing[0] === 0)
```

Subito dopo, legge:

```javascript
actualTorque = [-17194, -16767, 16206, -11357]
```

#### `writeToCSV(positionValues, torqueValues)`&#x20;

📋 Scrive una riga nel file `json/output.csv` con:

```csv
Pos asse1, Pos asse2, Pos asse3, Pos asse4, Coppia asse1, Coppia asse2, Coppia asse3, Coppia asse4
90, 35, 0, 12, -16557, -14451, 16379, -7396
```

### 📊 Calcolo delle iterazioni

Numero totale di combinazioni generate da:

```javascript
stepsPerAxis = Math.floor((stop - start) / step) + 1
totalIterations = product of all axes
```

Esempio:

```json
{
  "axis": 4, "start": 8, "stop": 12, "step": 2 → 3 posizioni
}
→ totale iterazioni = 3 * ... (assi restanti)
```

### ❌ Gestione degli errori

* Se il formato di `actualTorque` e errato:

```javascript
throw new Error("Unexpected format of actualTorque.");
```

* Se `step <= 0` o `start/stop` fuori range → errore bloccante con messaggio dettagliato.
* Il file `output.csv` e sempre aggiornato solo **se il movimento e stato completato**.

### ✅ Sintesi: cosa garantisce `taratura.js`

| Funzionalita                     | Realizzazione                                                    |
| -------------------------------- | ---------------------------------------------------------------- |
| Calibrazione multi-asse          | Iterazioni posizionali complete con controllo singolo asse       |
| Verifica limiti hardware         | Confronto diretto con dati `ROM.json`                            |
| Integrazione con Arduino(Master) | Serial tramite `sendViaSerialComm` e `motionOnGoing` via SPI/CAN |
| Salvataggio dati                 | CSV generato in tempo reale                                      |
| Sicurezza e ripresa              | Arresto immediato tramite flag `isTaraturaStopped`               |
