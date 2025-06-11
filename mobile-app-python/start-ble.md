# Start BLE

## 🚀 Avvio automatico del servizio BLE (Bluetooth Low Energy)

Per garantire l'operativita immediata del modulo BLE all'avvio del sistema, viene utilizzato uno script di avvio automatico gestito tramite **PM2**.

### 🧩 Script `start_ble.sh`&#x20;

All'interno della directory del server <kbd>**Allenaz\_HMI**</kbd>, e presente lo script `start_ble.sh` con il seguente contenuto:

```bash
#!/bin/bash
cd /home/raspberry/Allenaz_HMI/ble_py && python3 -u ble_server.py
```

### ⚙️ Funzionamento:

* **Navigazione nella cartella** <kbd>ble\_py</kbd>, che contiene il server BLE.
* **Esecuzione dello script Python** <kbd>ble\_server.py</kbd> in modalità live (<kbd>-u</kbd> per output non bufferizzato)

### 🔁 Avvio automatico con PM2

Il processo viene gestito da PM2, che garantisce il riavvio automatico del servizio BLE al boot del Raspberry Pi:

```bash
pm2 start /home/raspberry/Allenaz_HMI/start_ble.sh --name ble_server
pm2 save
pm2 startup
```

In questo modo, il servizio BLE sara sempre attivo dopo ogni riavvio, senza bisogno di avvio manuale.
