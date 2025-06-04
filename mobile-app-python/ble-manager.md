---
description: ble_manager.py
---

# BLE Manager

### 📌 Descrizione generale

Il modulo **BLEManager** e il cuore del server Bluetooth Low Energy (BLE) che consente la comunicazione tra uno smartphone e il Raspberry Pi (montato su esoscheletro). Gestisce l'interfaccia GATT, le caratteristiche BLE, e interagisce con:

* `WiFiManager` per connessione Wi-Fi e hotspot
* `MessageParser` per codificare/decodificare messaggi BLE

### 🔄 Interazione con altri moduli

<table><thead><tr><th width="163.4444580078125">Modulo</th><th>Metodo chiamato</th><th>Azione</th></tr></thead><tbody><tr><td><code>WiFiManager</code></td><td><code>connect_to_wifi()</code></td><td>Connessione a rete Wi-Fi con credenziali</td></tr><tr><td><code>WiFiManager</code></td><td><code>start/stop_hotspot()</code></td><td>Attivazione/disattivazione dell'hotspot AllyHotspot</td></tr><tr><td><code>WiFiManager</code></td><td><code>scan_networks()</code></td><td>Scansione delle reti Wi-Fi disponibili</td></tr><tr><td><code>WiFiManager</code></td><td><code>get_ip_address()</code>, <code>get_current_ssid()</code></td><td>Lettura IP o SSID corrente</td></tr><tr><td><code>MessageParser</code></td><td><code>decode_wifi_data()</code>, <code>encode_*()</code></td><td>Conversione tra messaggi binari e strutture leggibili</td></tr></tbody></table>

***

### 📡 Servizio BLE e Caratteristiche

<table><thead><tr><th width="177.66668701171875">Caratteristica</th><th>UUID</th><th width="84.7777099609375">Direzione</th><th>Descrizione</th></tr></thead><tbody><tr><td>Wi-Fi Data</td><td>12345678-1234-5678-1234-56789abc0002</td><td><code>write</code></td><td>Riceve credenziali Wi-Fi (SSID, password, flag)</td></tr><tr><td>SSID List</td><td>12345678-1234-5678-1234-56789abc0003</td><td><code>read</code></td><td>Lista delle reti Wi-Fi disponibili</td></tr><tr><td>Wi-Fi Status</td><td>12345678-1234-5678-1234-56789abc0004</td><td><code>read</code></td><td>Stato attuale della connessione Wi-Fi</td></tr><tr><td>Nome rete connessa</td><td>12345678-1234-5678-1234-56789abc0005</td><td><code>read</code></td><td>Nome della rete attualmente connessa (o AllyHotspot)</td></tr></tbody></table>

***

### 🧠 Metodi principali

#### 🟢 `start()`&#x20;

Avvia il server BLE e pubblica il servizio:

```python
ble_manager = BLEManager()
ble_manager.start()
```

#### 📥 `_handle_new_wifi_credentials(value, options)`&#x20;

Ricever e interpreta dati BLE dal telefono:

```
#9:HomeSSID;0;10:password123*
```

* Se  `SSID == AllyHotspot` : attiva hotspot
* Altrimenti: avvia connessione Wi-Fi
* Se `use_saved == 1`, cerca password salvata in **wifi\_credentials.json**
* `b"#3*"` in caso di errore

#### 📤 `_read_ssid_list(options)`&#x20;

* Ferma  l'hotspot se attivo
* Aspetta che il Wi-Fi sia pronto
* Richiama wifi\_manager.scan\_network()
* Aggiunge manualmente "AllyHotspot" alla lista

Output:

```python
b'#7:HomeWiFi1;6:Guest0;12:AllyHotspot0*'
```

#### 🔄 `_read_status(options)`&#x20;

Restituisce lo stato attuale della connessione:

<table><thead><tr><th>Stato</th><th width="185.66668701171875">Codifica</th><th>Descrizione</th></tr></thead><tbody><tr><td>In connessione</td><td><code>#\x00*</code></td><td>Netplan applicato, attesa IP</td></tr><tr><td>Connesso</td><td><code>#\x01;&#x3C;IP_BYTES>*</code></td><td>Connesso + IP</td></tr><tr><td>Non connesso</td><td><code>#\x02*</code></td><td>Nessuna rete</td></tr><tr><td>Error</td><td><code>#\x03*</code></td><td>Errore di sistema</td></tr></tbody></table>

#### 📶 `_read_current_ssid(options)`&#x20;

Se l'hotspot e attivo, ritorna `"AllyHotspot"`. Altrimenti ritorna il SSID reale tramite:

```python
ssid = wifi_manager.get_current_ssid()
```

Output BLE:

```python
b'#9:HomeSSID*' oppure b'#12:AllyHotspot*'
```

### 🧪 Esempi BLE (tramite nRF Connect o app custom)

#### 📤 Scrittura credenziali Wi-Fi

UUID: `12345678-1234-5678-1234-56789abc0002`&#x20;

Payload (esadecimale):

```
23 09 3A 48 6F 6D 65 57 69 46 69 3B 00 3B 0A 3A 70 61 73 73 77 6F 72 64 31 32 33 2A
```

**ASCII:** `#9:HomeWiFi;0;10:password123*`&#x20;

#### 📥 Lettura stato:

**UUID:** `12345678-1234-5678-1234-56789abc0004`&#x20;

**Risposta esempio:**

```plaintext
plaintextCopiaModifica#1;192.168.4.1*  → connesso
#2*              → non connesso
#3*              → errore
```

### 🔁 Ciclo completo tipico

1. 📲 Smartphone si connette via BLE a `AllyArm-25003#1`
2. Invia credenziali tramite `WIFI_NEW_CONN_CHAR_UUID`
3. `BLEManager` → decodifica → chiama `WiFiManager.connect_to_wifi()`
4. Raspberry si connette e restituisce `#1;<IP>*`
5. L’app può ora aprire l’interfaccia web via IP
