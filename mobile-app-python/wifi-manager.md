---
description: wifi_manager.py
---

# WiFi Manager

### 📌 Descrizione generale

Il modulo `WIFIManager` gestisce tutte le operazioni di rete Wi-Fi e hotspot su Raspberry Pi. Include funzionalità per:

* Connettersi a reti Wi-Fi tramite `netplan`
* Ottenere l'IP e SSID attuale
* Avviare/interrompere un hotspot `AllyHotspot`
* Scansionare reti dispoibili
* Verificare lo stato della connessione
* Interagire con `BLEManager` tramite status codificati (es. `#\01:<IP>*`)

### 📡 Connessione a Wi-Fi

🔧 `connect_to_wifi(ssid: str, password: str, use_saved: int) -> bool`&#x20;

Crea un file `netplan`(`_generate_netplan_yaml`), lo applica e avvia un thread per controllare se la connessione ha successo.

* **ssid**: nome della rete
* **password**: chiave d'accesso (vuota per reti aperte o password salvata)
* **use\_saved**: **1** se usare password salvata, **0** se inviata

Codici di stato BLE:

| Stato          | Codifica      |
| -------------- | ------------- |
| In connessione | #\x00\*       |
| Connesso       | #\x01;\<IP>\* |
| Non connesso   | #\x02\*       |
| Errore         | #\x03\*       |

Esempio:

```python
WiFiManager.connect_to_wifi("HomeWiFi", "mypassword", 0)
# Avvia connessione + aggiorna BLE status
```

### ✅ Verifica connessione

🔍 `is_connected() -> bytes`&#x20;

Usa `nmcli` per verificare se il Raspberry e connesso. Restituisce lo stato BLE raw.

Esempio di output:

```python
b"#\x01;\xC0\xA8\x01\x64*"  # Connesso a 192.168.1.100
b"#\x02*"                   # Non connesso
```

### 🌐 Ottieni IP corrente

📥 `get_ip_address() -> str`&#x20;

Restituisce l'indirizzo IP della rete Wi-Fi.

Esempio:

```python
WiFiManager.get_ip_address()
# Output: '192.168.1.100' o None
```

### 📡 Reti disponibili

📶 `scan_networks() -> list[tuple]`&#x20;

Esegue una scansione Wi-Fi con `nmcli`. Restituisce lista di tuple: (`SSID, flas_salvata`)

* **`flag = 1`** se esistono credenziali salvate
* **`flag = 0`** non ci sono i credenziali salvate

Esempio:

```python
WiFiManager.scan_networks()
# Output: [('HomeWiFi', 1), ('Guest', 0)]
```

### 📛 Nome rete connessa

📟 `get_current_ssid() -> str`&#x20;

Restituisce il nome della rete Wi-Fi attualmente connessa, o None.

Esempio:

```python
WiFiManager.get_current_ssid()
# Output: 'HomeWiFi'
```

### 🔥 Gestione Hotspot

#### 🚀 Avvio hotspot

🟢 `start_hotspot() -> bool`&#x20;

Esegue lo script `/usr/local/bin/start-hotspot.sh` per creare la rete `AllyHotspot`.

* Crea file flag `hotspot_active`
* Restituisce **True** se avvio riuscito

#### 🛑 Arresto hotspot

🔴 `stop_hotspot() -> bool`&#x20;

Chiama `/usr/local/bin/stop-hotspot.sh` e rimuove il flag `hotspot_active`.

* Restituisce **True** se arresto riuscito

#### ❓ Verifica hotspot attivo

🕵️ `is_hotspot_active() -> bool`&#x20;

Controlla se esiste il file hotspot\_active.

```python
WiFiManager.is_hotspot_active()
# Output: True / False
```

### 🧾 Generazione netplan

🧷 `_generate_netplan_yaml(ssid, password, use_saved) -> str`&#x20;

Restituisce una stringa YAML per `netplan` con SSID e password.

Esempio:

```yaml
network:
  version: 2
  renderer: NetworkManager
  wifis:
    wlan0:
      access-points:
        "HomeWiFi":
          password: "mypassword"
      dhcp4: true

```

### 🔁 Interazioni

| Metodo chiamato        | Azione                                                    |
| ---------------------- | --------------------------------------------------------- |
| `connect_to_wifi()`    | Connettersi a Wi-Fi da credenziali BLE, aggiorna lo stato |
| `scan_network()`       | Fornisce lista reti BLE                                   |
| `start/stop_hotspot()` | Attivare/disattivare AllyHotspot                          |
| `get_current_ssid()`   | Visualizzare rete Wi-Fi connessa attuale                  |
| `get_ip_address()`     | Mostrare indirizzo IP al telefono                         |
