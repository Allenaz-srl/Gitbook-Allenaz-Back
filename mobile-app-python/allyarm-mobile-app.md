# AllyArm Mobile App

### 📘 Introduzione

Questo progetto ha lo scopo di semplificare la configurazione della rete WI-FI su un Raspberry pi 4B+ integrato in un esoscheletro.

Tramite un'interfaccia BLE (Bluetooth Low Energy), l'utente puo:

* Inviare le credenziali WI-FI da uno smartphone.
* Ricevere la lista delle reti WI-FI disponibili.
* Ottenere lo stato della connessione e l'indirizzo IP attuale che server per "Ally App".
* Attivare un hotspot se necessario.

### 🧩 **Componenti principali e ruoli**

| Componente                                                             | Ruolo                                                                  |
| ---------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| [`ble_server.py`](../ble-server.md)                                    | Entry point. Avvia il ciclo BLE.                                       |
| [`ble_manager.py`](../ble-manager.md)                                  | Gestisce i servizi BLE e le caratteristiche (UUID BLE, callback).      |
| [`wifi_manager.py`](../wifi-manager.md)                                | Connessione/disconnessione Wi-Fi, scansione, gestione hotspot.         |
| [`message_parser.py`](../message-parser.md)                            | Codifica e decodifica dei messaggi BLE (dati Wi-Fi, SSID, stato, ecc). |
| [`/usr/local/bin/start-hotspot.sh - stop-hotspot.sh`](../allyhotspot/) | Script per disattivare WIFI e attivare l'hotspot                       |

