---
description: Descrizione generale del sistema
---

# Voice Command

## 🔧 Scopo

Il componente Voice Command utilizza la libreria <kbd>**Vosk**</kbd> per il riconoscimento vocale offline e consente di controllare un esoscheletro tramite comandi vocali predefiniti.\
Il sistema è progettato per funzionare su **Raspberry Pi 4b+**, dove uno script **Python** rimane sempre in ascolto.\
I comandi riconosciuti vengono trasmessi tramite **WebSocket** a un **server Node.js**, che a sua volta li inoltra al Master dell'esoscheletro.\
Al momento dell'avvio del Raspberry, è il server Node.js che si occupa di eseguire automaticamente lo script vocale.\
Sebbene la libreria Vosk ascolti costantemente, è attiva una **modalita "wake word"**, nella quale vengono riconosciuti ed elaborati solo comandi esplicitamente autorizzati. Questa modalita si attiva dopo il relivamento della frase chiave (es. "ciao alli") e dura per un intervallo di tempo definito tramite timer.\
Oltre ai comandi standard, esiste un insieme di **comandi di emergenza** (es. "stop", "basta", "alt", "fermati" ecc.) che vengono accettati **in qualsiasi momento**, anche al di fuori della modalità attiva.\
Questi comandi rappresentano un livello aggiuntivo di sicurezza, permettendo all'utente di bloccare immediatamente l'esoscheletro in caso di necessita.

## 🧭 Architettura generale

```
Utente → Microfono → Raspberry Pi (Vosk) → WebSocket → Node.js → Esoscheletro
```

| Modulo                    | Tecnologia         | Funzione principale                              |
| ------------------------- | ------------------ | ------------------------------------------------ |
| `VCVosk/recognizer.py`    | Python + Vosk      | Ascolta continuamente e riconosce comandi vocali |
| `vosk-small-model-it`     | Modello Vosk       | Modello vocale offline per lingua italiana       |
| WebSocket Client (Python) | `websocket-client` | Invia comandi riconosciuti al server Node.js     |
| Server HMI                | Node.js            | Smista i comandi verso l’esoscheletro            |

## 🧠 Funzionalità chiave

### ✅ Modalità Wake Word

Il sistema è sempre in ascolto, ma **ignora tutto finche non sente la frase d'attivazione**:

```
Wake Word → "ciao alli"
```

Una volta attivata, entra in **modalità comandi attiva** per 10 sec, durante i quali accetta:

```
"alli su", "alli giù", "alli sali", "alli scendi" ecc.
```

Ogni comando vocale ricevuto **resetta il timer** di 10 secondi.

### 🛑 Comandi di emergenza (sicurezza)

Alcuni comandi possono essere pronunciati in **qualsiasi momento**, anche a sistema "addormentato":

```
"stop", "fermati", "fermo", "alt", "basta"
```

Questi comandi vengono **sempre riconosciuti e inviati**, anche senza attivazione vocale.

### 🔁 Anti-duplicazione

Per evitare ripetizioni involontarie (eco microfono ecc), il  sistema:

* Confronta ogni nuovo comando col precedente
* Ignora comandi identici ricevuti entro 0.5 - 1.5 secondi (da settare)

### 📤 Invio comandi al server

Quando un comando è valido (di emergenza o in modalita attiva), il sistema:

* Lo invia via WebSocket a un server node.js nella stessa rete (stesso raspberry)
* Il server passa il comando all'esoscheletro

### 🔐 Sicurezza integrata

* I comandi sono predefiniti: il sistema riconosce **solo quelli elencati** (nessun rischio di comandi causali)
* La gestione di emergenza è **sempre attiva**
* Il sistema è **offline** (nessun connessione internet necessaria)
