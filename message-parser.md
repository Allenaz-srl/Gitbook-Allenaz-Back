---
description: message_parser.py
---

# Message parser

**Ruolo:** Questo modulo contiene la logica per la codifica e la decodifica dei messaggi BLE utilizzati per la configurazione Wi-Fi, la comunicazione dello stato e la lista delle reti disponibili.

### 🧩 Classe `MessageParser`

Classe statica per la gestione dei messaggi BLE secondo un protocollo personalizzato delimitato da `#` (inizio) e `*` (fine).

{% tabs %}
{% tab title="Python" %}
```python
    START_CHAR = ord('#')
    END_CHAR = ord('*')
    SEP_FIELD = ord(':')
    SEP_STRING = ord(';')
```
{% endtab %}
{% endtabs %}

### 🔤 Def `encode_wifi_data(ssid: str, password: str, use_saved: int) -> bytes`

**Descrizione:**\
Codifica i dati di connessione Wi-Fi (SSID, password, flag per password salvata) in un messaggio BLE.

**Formato del messaggio:**

```textile
#<L1>:<SSID_ASCII>;<L2>:<PASSWORD_ASCII>;<USE_SAVED_FLAG>*
```

**Esempio:**

```python
MessageParser.encode_wifi_data("MyWiFi", "password123", 0)
# Output: b'#6:MyWiFi;11:password123;0*'
```

### 🔍 **Def** `decode_wifi_data(message: bytes) -> dict`

**Descrizione:**\
Decodifica un messaggio BLE ricevuto con i dati Wi-Fi.

**Restituisce:**

```python
{
    'ssid': 'MyWiFi',
    'password': 'password123',
    'use_saved': 0
}
```

**Esempio:**

```python
raw = b'#6:MyWiFi;11:password123;0*'
MessageParser.decode_wifi_data(raw)
# Output: {'ssid': 'MyWiFi', 'password': 'password123', 'use_saved': 0}
```

### 📡 Def `encode_ssid_list(networks: list) -> bytes`

**Descrizione:**\
Codifica una lista di reti Wi-Fi (SSID e flag di rete conosciuta/salvata).

**Formato:**

```textile
#<L1>:<SSID1><FLAG1>;<L2>:<SSID2><FLAG2>;...*
```

**Esempio:**

```python
MessageParser.encode_ssid_list([("HomeWiFi", 1), ("Guest", 0)])
# Output: b'#8:HomeWiFi1;5:Guest0*'
```

### ✅ Def `validate_message(message: bytes) -> bool`

**Descrizione:**\
Verifica la validità strutturale di un messaggio BLE:

* Deve iniziare con `#`, finire con `*`
* Deve contenere `:` e `;`

**Esempio:**

```python
MessageParser.validate_message(b'#6:MyWiFi;0*')
# Output: True
```

### 🌐 Def `encode_connection_status(status: int, ip: str = None) -> bytes`

**Descrizione:**\
Codifica lo stato della connessione di rete.

**Formato:**

* Solo stato: `#<STATUS>*`
* Stato + IP: `#<STATUS>;<IP_BYTE1><IP_BYTE2><IP_BYTE3><IP_BYTE4>*` (se `status == 1`)

**Esempio:**

```python
MessageParser.encode_connection_status(1, "192.168.0.10")
# Output: b'#1;\xc0\xa8\x00\n*'
```

### 📶Def `encode_current_wifi_name(ssid: str) -> bytes`

**Descrizione:**\
Codifica il nome della rete Wi-Fi attualmente connessa.

**Formato:**

```textile
#<L>:<SSID_ASCII>*
```

**Esempio:**

```python
MessageParser.encode_current_wifi_name("MyWiFi")
# Output: b'#6:MyWiFi*'
```
