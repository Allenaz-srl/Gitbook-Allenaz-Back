# Start VCV

## 🚀 Avvio automatico del modulo Voice Command (Vosk)

Il modulo di riconoscimento vocale, basato su **Vosk**, viene avviato automaticamente subito dopo l'avvio del servizio BLE, per garantire il controllo vocale dell'esoscheletro senza interventi manuali.

### 🧩 Script `start_voice.sh`

Lo script start\_voice.sh si trova nella cartella principale del server Allenaz\_HMI e ha il seguente contenuto:

```bash
#!/bin/bash
sleep 5
cd /home/raspberry/Allenaz_HMI/VCVosk && python3 -u recognizer.py
```

### ⚙️ Funzionamento:

* **Attesa di 5 secondo**: permette al server BLE di inizializzarsi completamente prima dell'avvio del modulo vocale
* **Navigazione nella directory** **VCVosk** centenente il file <kbd>recognizer.py</kbd>
* **Esecuzione dello script Python** in modalità interattiva (<kbd>-u</kbd> per log immediati)

### 🔁 Gestione tramite PM2

Come per il BLE, anche il modulo vocale viene gestito da PM2 per l'avvio automatico e il monitoraggio:

```bash
pm2 start /home/raspberry/Allenaz_HMI/start_voice.sh --name voice_command
pm2 save
pm2 startup
```

In questo modo, il sistema vocale sarà sempre attivo dopo ogni riavvio del Raspberry Pi, pronto a ricevere comandi vocali
