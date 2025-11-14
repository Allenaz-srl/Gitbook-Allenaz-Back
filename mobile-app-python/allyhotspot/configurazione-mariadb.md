---
description: Configurazione MariaDB per IP Dinamico su Raspberry Pi
---

# Configurazione MariaDB

### 📦 Configurazione  per MariaDB quando il Raspberry utilizza un IP dinamico

Quando il Raspberry Pi passa da un **IP statico** ad un **IP dinamico** (necessario per permettere l'attivazione dell’hotspot “AllyHotspot” e la gestione corretta di NetworkManager), anche MariaDB deve essere configurato per funzionare correttamente indipendentemente dall’indirizzo IP assegnato.

Questa sezione descrive i passi necessari per garantire che MariaDB rimanga accessibile dall’applicazione backend anche dopo il cambio IP.

#### 🧩 **1. Rimuovere riferimenti a IP statici nel file `/etc/hosts`**

Se il sistema era configurato precedentemente con un IP statico, è possibile che in `/etc/hosts` siano presenti righe obsolete del tipo:

```
192.168.0.200   raspberry
192.168.0.200   localhost
```

{% hint style="info" %}
**È necessario commentare o rimuovere queste righe**, altrimenti MariaDB o altri servizi potrebbero cercare ancora il vecchio IP.
{% endhint %}

Modifica:

```bash
sudo nano /etc/hosts
```

E commenta eventuali righe contenenti il vecchio IP statico.

#### 🧩 **2. Aggiornare la configurazione globale di MariaDB (`/etc/mysql/mariadb.cnf`)**

Apri il file:

```bash
sudo nano /etc/mysql/mariadb.cnf
```

Trova la direttiva:

```
bind-address = 192.168.0.200
```

Imposta così:

```
bind-address = 127.0.0.1
```

Questo assicura che MariaDB accetti sempre connessioni locali dal Raspberry, indipendentemente dal suo IP di rete.

#### 🧩 **3. Modificare il file principale del server MariaDB (`50-server.cnf`)**

Apri:

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Trova la sezione `[mysqld]` e imposta:

```
bind-address = 0.0.0.0
```

📌 _Perché “0.0.0.0”?_\
Questo permette a MariaDB di ascoltare su tutte le interfacce.\
(Inclusa quella locale 127.0.0.1, che usiamo dal backend.)

Riavvia il servizio:

```bash
sudo systemctl restart mariadb
```

#### 🧩 **4. Configurazione del backend Node.js**

Nel backend, nel file:

```
/home/raspberry/Allenaz_HMI/routes/api/paramConnDB.js
```

È importante NON usare l’IP di rete del Raspberry (che è dinamico), ma sempre l’indirizzo locale:

```js
const pool = mariadb.createPool({
  host: "127.0.0.1",
  port: "3306",
  user: "user_hmi",
  password: "Allenaz.2023",
  connectionLimit: 100,
});
```

Questo garantisce una connessione stabile al database, anche quando il Raspberry cambia IP per via del Wi-Fi, dell’hotspot o di NetworkManager.

#### 🟢 **Verifica finale**

#### Controllo servizio:

```bash
sudo systemctl status mariadb
```

#### Controllo porta:

```bash
sudo ss -tulpen | grep 3306
```

Deve risultare:

```
LISTEN ... 0.0.0.0:3306
```

{% hint style="info" %}
**Passando da un IP statico a uno dinamico, è normale che MariaDB richieda questa modifica di configurazione.**\
**Il sistema funzionerà in modo più stabile e compatibile con la gestione automatica della rete e dell’hotspot.**
{% endhint %}
