# Function in app.js

## 📘 Funzione: `sendShortTermLog(serialNumber, logs)`&#x20;

### 🎯 Scopo

Questa funzione ha lo scopo di **ricevere**, **decodificare** e **salvare** nel database una lista di pacchetti di log(dati della diagnostica) a breve termine provenienti da un esoscheletro. I pacchetti contengono **byte numerati**, e alcuni di essi rappresentano valori in virgola mobile (**float**) come **frequenze** e **coordinate**. I dati vengono inviati a una stored procedure MariaDB (`exo_log.load_short_log_line`) con una sintassi dinamica, generata in base al contenuto ricevuto.

### 🧾 Parametri

| Parametro      | Tipo            | Descrizione                                  |
| -------------- | --------------- | -------------------------------------------- |
| `serialNumber` | `string`        | Numero di serie del dispositivo esoscheletro |
| `logs`         | `Array<object>` | Array di oggetti log ricevuti da un client   |

#### Esempio di singolo log:

```json
[INFO]:1728656160955,255,255,38,49,57,39,16,39,149,148,38,196,0,0,3,0,161,49,185,52,188,52,0,0,0,0,0,0,0,0,37,49,245,42,248,42,0,0,0,0,0,0,144,62,161,49,51,33,52,33,0,0,0,0,0,0,0,0,34,34,0,0,0,0
```

### 🗂️ Struttura del log

Ogni log ricevuto dall'esoscheletro e un array ordinato di byte (`byte1`, `byte2`, ..., `byteN`) che contiene una sequenza di dati binari, dove ogni blocco ha un significato fisico. Ogni messaggio contiene piu sezioni di 16 byte, ognuna riferita a uno dei seguenti assi:

* **Master – byte 3 → 18**
* **Asse 1 – byte 19 → 34**
* **Asse 2 – byte 35 → 50**
* **Asse 3 – byte 51 → 66**
* **Asse 4 – byte 67 → 82**
* **Asse 5 – byte 83 → 98**

### 🧠 Logica di parsing nella funzione

#### **🎯 Frequenze (valori float)**

| Asse   | Byte da leggere (4) | Valore                      |
| ------ | ------------------- | --------------------------- |
| Asse 1 | `byte25` - `byte28` | `frequency_1`               |
| Asse 2 | `byte41` - `byte44` | `frequency_2`               |
| Asse 3 | `byte57` - `byte60` | `frequency_3`               |
| Asse 4 | `byte73` - `byte76` | `frequency_4`               |
| Asse 5 | `byte89` - `byte92` | `frequency_5` (se presente) |

#### 🎯 Coordinate (Cartesiane)

Ultimi 16 byte sono usati per coordinate:

```javascript
x = Buffer.from([byteN-15, ..., byteN-12]).readFloatLE(0)
y = Buffer.from([byteN-11, ..., byteN-8]).readFloatLE(0)
z = Buffer.from([byteN-7, ..., byteN-4]).readFloatLE(0)
g = Buffer.from([byteN-3, ..., byteN 0]).readFloatLE(0)
```

{% hint style="info" %}
Dove `byteN` è l’ultimo byte disponibile nel log.
{% endhint %}

#### 📋 Campi presenti in ogni sezione (16 byte)

| Offset | Campo                | Descrizione                  |
| ------ | -------------------- | ---------------------------- |
| 1      | Parity + debug bits  | 1 byte: bit di controllo     |
| 2      | Driver/master + mode | 4 bit stato + 4 bit modalità |
| 3–4    | actual\_position     | Posizione attuale            |
| 5–6    | actual\_torque       | Coppia attuale               |
| 7–10   | actual\_frequency    | Frequenza (float)            |
| 11–12  | error\_code          | Codice di errore             |
| 13–14  | warning\_code        | Codice di avviso             |
| 15–16  | target\_position     | Posizione target             |

{% hint style="info" %}
🟡 Nota: La sezione <kbd>master</kbd> contiene valori <kbd>spare</kbd> , ma mantiene lo stesso layout per coerenza.
{% endhint %}

### 🧠 Struttura della funzione

1. Connessione al database

```javascript
conn = await pool.getConnection();
```

> Si apre una connessione alla pool MySQL (pool preconfigurato).

***

2. Ciclo su ciascun log

```javascript
for (const log of logs) {
  const { level, time, ...bytes } = log;
```

* Estrae level (es. <kbd>"info"</kbd>), <kbd>time</kbd> (timestamp) e il resto dei byte.
* Conta quanti byte sono presenti:

```javascript
const numBytes = Object.keys(bytes).length;
```

***

3. Costruzione query MySQL dinamica

Si inizia con:

```javascript
let query = `CALL exo_log.load_short_log_line ('${numBytes}', '${serialNumber}', '${level}', ${time}`;
```

Poi si aggiungono tutti i byte come parametri:

```javascript
for (let i = 1; i <= numBytes; i++) {
  query += `, ${bytes[`byte${i}`]}`;
}
```

***

4. Riempimento campi mancanti

Per uniformare i record a 127 colonne:

```javascript
let maxValueBD = 127 - (numBytes + 13);
for (i = 0; i < maxValueBD; i++) {
  query += `, 0`;
}
```

5. Append dei dati <kbd>float</kbd> alla fine della query

```javascript
query += ` , ${frequency_1}, ${frequency_2}, ..., ${g}`
```

### ❗ Controllo e gestione errori

* Ogni valore <kbd>**NaN**</kbd> nei <kbd>float</kbd> viene sostituito con <kbd>0</kbd>.
* Se `conn.query()` fallisce → l'errore viene propagato:

```javascript
throw err;
```

* La connessione viene chiusa:

```javascript
finally { if (conn) conn.end(); }
```
