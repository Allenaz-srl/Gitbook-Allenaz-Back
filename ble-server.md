---
description: ble_server.py
---

# BLE Server

**Ruolo:** Punto di ingresso principale per l’esecuzione del server BLE. Inizializza il gestore BLE (`BLEManager`), avvia il server e mantiene il loop principale attivo fino all'interruzione del processo.

### 📋 Descrizione generale

Il file rappresenta il **main script** dell'applicazione BLE lato Raspberry Pi. Utilizza GLib per mantenere attivo il ciclo di eventi e gestisce i segnali di arresto (es. `Ctrl+C`) per una chiusura pulita.

### ⚙️ Funzionamento

1. **Inizializza il BLEManager** (definito in `ble/ble_manager.py`).
2. **Avvia il server BLE** tramite `ble_manager.start()`.
3. **Registra gli handler dei segnali** `SIGINT` e `SIGTERM` per permettere l’uscita controllata.
4. **Avvia un ciclo principale GLib**, che gestisce eventi BLE e sistema.

### 🧪 Esempio di esecuzione

```textile
$ python3 ble_server.py
[BLE] BLE Manager initialized
[BLE] Starting BLE Server...
```

### 🧼 Gestione dei segnali

Il codice definisce la funzione `exit_gracefully()` che intercetta:

* `SIGINT` (Ctrl+C)
* `SIGTERM` (arresto da sistema)

e chiude il server BLE senza errori pendenti, permettendo una **terminazione sicura del servizio**.
