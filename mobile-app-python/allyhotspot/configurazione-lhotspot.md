---
description: 📡 Come configurare l’Hotspot AllyHotspot su Raspberry Pi 4B+
---

# Configurazione l'Hotspot

### 🧾 Obiettivo

Permettere al Raspberry Pi di **attivare un hotspot** Wi-Fi (senza adattatori esterni) quando non è connesso a nessun rete, in modo da poter comunicare via BLE con un smartphone per la configurazione iniziale del Wi-Fi o per l'utilizzo diretto della _**Ally App**_ (per esempio fuori di casa< dove non c'è nessun rete Wi-Fi)

### 📦 Requisiti

* Raspberry Pi 4B+ con Ubuntu Server 22.04
* Modulo Wi-Fi integrato attivo
* Permessi `sudo`

### 📁 Utilità minime richieste per il funzionamento dell’Hotspot Wi-Fi

Per garantire il corretto funzionamento dell'hotspot (AllyHotspot) sul Raspberry Pi, è necessario assicurarsi che le seguenti utilità siano installate e funzionanti.

<table><thead><tr><th width="94.77783203125">Utilità</th><th width="275.7779541015625">Descrizione</th><th>Comando per verificare</th><th>Installazione (se assent</th></tr></thead><tbody><tr><td><code>hostapd</code></td><td>Gestisce la modalità Access Point (SSID, password, canale, ecc.)</td><td><code>hostapd -v</code></td><td><code>sudo apt install hostapd</code></td></tr><tr><td><code>dnsmasq</code></td><td>Server DHCP/DNS: assegna IP ai dispositivi connessi</td><td><code>dnsmasq --versione</code></td><td>sudo apt install dnsmasq</td></tr><tr><td><code>netplan</code></td><td>Gestione delle interfacce di rete (standard su Ubuntu Server)</td><td><code>netplan --version</code></td><td>(già incluso in Ubuntu Server)</td></tr><tr><td><code>iw</code></td><td>Verifica modalità supportate dal WI-Fi (es. managed, AP)</td><td><code>iw list | grep -A 10 "Supported interface modes</code>"</td><td><code>sudo apt install iw</code></td></tr><tr><td><code>rfkill</code></td><td>Controllo del blocco hardware/software del Wi-Fi</td><td><code>rfkill list</code></td><td><code>sudo apt install rfkill</code></td></tr></tbody></table>

{% hint style="info" %}
Suggerimento: E buona pratica eseguire questi comandi prima di tentare l'attivazione dell'hotspot, specialmente su un sistema configurato da zero.
{% endhint %}

### 🛠️ Passaggi di configurazione

1. 🧩Installare i pacchetti necessari

```bash
sudo apt update
sudo apt install hostapd dnsmasq network-manager -y
```

2. 🧩Disabilitare l'avvio automatico di `hostapd` e `dnsmasq`

```bash
sudo systemctl disable hostapd
sudo systemctl disable dnsmasq
```

3. 🧩Creare o modificare il file di configurazione `dnsmasq`

```bash
sudo nano /etc/dnsmasq.conf
```

* Inserisci:

```ini
interface=wlan0
dhcp-range=192.168.50.10,192.168.50.100,12h
```

4. 🧩Creare o modificare il file di configurazione `hostapd`

```bash
sudo nano /etc/hostapd/hostapd.conf
```

* Inserisci:

```ini
interface=wlan0
driver=nl80211
ssid=AllyHotspot
hw_mode=g
channel=6
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=12345678
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
```

* Creare o modificare file di configurazione del servizio

```bash
sudo nano /etc/default/hostapd
```

* Inserisci

```ini
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

5. 🧩 Configurazione di **Netplan** (aggiunta importante ✅)

Per permettere la gestione dinamica dell’IP e il corretto ripristino dopo la disattivazione dell’hotspot:

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Inserisci:

```yaml
network:
  version: 2
  renderer: NetworkManager
  ethernets:
    eth0:
      dhcp4: true
      optional: true
  wifis:
    wlan0:
      access-points:
        "Allenaz_Office":
          password: "Allenaz2023"
      dhcp4: true
      optional: true
```

Applica la configurazione:

```bash
sudo netplan apply
```

{% hint style="info" %}
⚠️ **Attenzione:** Se prima il Raspberry Pi utilizzava un indirizzo IP statico, dopo questa modifica l’indirizzo diventerà **dinamico (assegnato via DHCP)**. Assicurati di aggiornare eventuali configurazioni o servizi che dipendono da un IP fisso.
{% endhint %}

6. 🧩**Creare lo script `start-hotspot.sh`**

```bash
sudo nano /usr/local/bin/start-hotspot.sh
```

* Inserisci:

```bash
#!/bin/bash

echo "[+] Turn off WIFI (if connected)"
nmcli radio wifi off
sleep 1
nmcli radio wifi on
sleep 1
nmcli device set wlan0 managed no
sleep 1

echo "[+] Set IP-address"
sudo ip addr flush dev wlan0
sudo ip addr add 192.168.50.1/24 dev wlan0
sudo ip link set wlan0 up

echo "[+] Start dnsmasq"
sudo systemctl restart dnsmasq

echo "[+] Start hostapd"
sudo systemctl restart hostapd
```

* Facciamo eseguibile:

```bash
sudo chmod +x /usr/local/bin/start-hotspot.sh
```

7. 🧩Creare lo script `stop-hotspot.sh`

```bash
sudo nano /usr/local/bin/stop-hotspot.sh
```

* Inserisci:

```bash
#!/bin/bash

echo "[+] Stop hostapd and dnsmasq"
sudo systemctl stop hostapd
sudo systemctl stop dnsmasq

echo "[+] Clear IP-address and turn on controll of wlan0 by NetworkManager"
sudo ip addr flush dev wlan0
nmcli device set wlan0 managed yes

echo "[+] Turn On Wi-Fi again"
nmcli radio wifi on
sudo netplan apply

echo "[+] READY - Raspberry work again in client WIFI"
```

* Facciamo esegiubule:

```bash
sudo chmod +x /usr/local/bin/stop-hotspot.sh
```

### 💡 Integrazione nel codice Python

Nel modulo WIFIManager:

* Lo script `start-hotspot.sh` viene eseguito da:

```python
subprocess.run(["sudo", "/usr/local/bin/start-hotspot.sh"], check=True)
```

* Lo script `stop-hotspot.sh` viene eseguito da:

```python
subprocess.run(["sudo", "/usr/local/bin/stop-hotspot.sh"], check=True)
```

### 🔁 Logica di commutazione (BLEManager)

| Condizione                    | Azione                            |
| ----------------------------- | --------------------------------- |
| SSID ricevuto = `AllyHotspot` | Esegui start-hotspot.sh           |
| Qualsiasi altro SSID          | Esegui stop-hotspot.sh(se attivo) |

### ✅ Verifica finale

* Verificare attivazione dell'hotspot:

```bash
sudo "/usr/local/bin/start-hotspot.sh"
```

* Verificare disattivazione dell'hotspot:

```bash
sudo "/usr/local/bin/stop-hotspot.sh"
```
