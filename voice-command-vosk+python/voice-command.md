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

\
