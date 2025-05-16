# AllyHotspot

### 🎯 Obiettivo

Il progetto è in grado di **attivare un hotspot Wi-Fi chiamato** `AllyHotspot` direttamente tramite BLE, **senza utilizzare un adattatore esterno**, sfruttando l'interfaccia Wi-Fi integrata del Raspberry Pi 4B+.

### 🔧 Modalità di funzionamento

1. Quando lo smartphone invia `SSID = "AllyHotspot"` tramite BLE,
2. Il **BLEManager** intercetta questa richiesta nella funzione `_handle`_`_`_`new_wifi_credentials()`,
3. Invece di tentare una connessione Wi-Fi, viene richiamato:

```python
self.wifi_manager.start_hotspot
```

4. Questo esegue uno script Bash:

```bash
/usr/local/bin/start-hotspot.sh
```

che:

* Ferma **NetworkManager** o qualsiasi connessione attiva.
* Configura **hostapd** e **dnsmasq** per trasmettere la rete **AllyHotspot**.
* Imposta l'interfaccia **wlan0** in modalita access point.

### 📂 Tracciamento dello stato

Per sapere se l'hotspot è attivo, il sistema utilizza un **flag file:**

```python
HOTSPOT_FLAG_FILE = os.path.join(os.path.dirname(__file__), "hotspot_active")
```

* Creato quando l'hotspot è attivo (start\_hotspot()).
* Eliminato quando viene fermato (stop\_hotspot()).

Questa Logica viene gestita da:

```python
WiFiManager.is_hotspot_active()
```

### 🔄 Commutazione Wi-Fi ↔ Hotspot

| Situazione                               | Azione                                                    |
| ---------------------------------------- | --------------------------------------------------------- |
| L'utente seleziona AllyHotspot           | Viene avviato l'hotspot tramite script Bash               |
| L'utente richiede lista SSID (read)      | Se hotspot è attivo → viene fermato prima della scansione |
| L'utente seleziona una rete WI-FI reale  | Il sistema ferma l'hotspot prima di connettersi           |

### 📜 Script utilizzati

* `/usr/local/bin/start-hotspot.sh`
  * Imposta **hostapd**, **dnsmasq**, IP statico, abilita **wlan0** in modalita **AP**
* `/usr/local/bin/stop-hotspot.sh`
  * Arresta hotspot, riavvia **NetworkManager**, rimuove flag
