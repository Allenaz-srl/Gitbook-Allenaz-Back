# Comandi Vocali

## 🗣️ Comandi Vocali – Struttura e Funzionamento

Il sistema di riconoscimento vocale è progettata per garantire un'interazione **sicura, naturale e senza mani** con l'esoscheletro. I comandi sono organizzati in **gruppi funzionali**, ciascuno con modalità di attivazione e comportamenti specifici.

### 🔓 1. Wake Word (Frasi di attivazione)

Le frasi di attivazione **attivano temporaneamente la modalità di ascolto** per accettare solo determinati gruppi di comandi.

<table><thead><tr><th width="135.88885498046875">Wake Word</th><th width="252.888916015625">Gruppi abilitati</th><th width="132.3333740234375">Durata attivazione</th><th>Rinnovo timer</th></tr></thead><tbody><tr><td><code>"ciao alli"</code></td><td><code>Start</code>, <code>Posizione salvata</code>, <code>Reset</code></td><td>⏱️ 10 secondi</td><td>✅ Dopo ogni comando valido</td></tr><tr><td><code>"alli muovi"</code></td><td><code>Movimenti</code></td><td>⏱️ 30 secondi</td><td>✅ Dopo ogni comando valido</td></tr></tbody></table>

{% hint style="info" %}
📌 Durante il periodo di attivazione, il sistema riconosce solo i comandi appartenenti ai gruppi autorizzati da quelle wake word, ma riconosce comunque anceh le altre wake word o il gruppo emergenza.
{% endhint %}

### 🟢 2. Gruppo Start

**Obbiettivo:** attivare il controllo manuale tramite joistick.

<table><thead><tr><th width="297.3333740234375">Comando</th><th>Descrizione</th></tr></thead><tbody><tr><td><kbd>"inizia movimento"</kbd></td><td>Abilita il controllo tramite joystick per il paziente</td></tr></tbody></table>

✅ Funziona **solo in modalità attiva dopo&#x20;**_**"ciao alli"**_

### 🔁 3. Gruppo Movimenti

**Obbiettivo:** permettere movimenti guidati o precisi dell'esoscheletro.

<table><thead><tr><th width="290.666748046875">Comando</th><th>Descrizione</th></tr></thead><tbody><tr><td><kbd>"alli sali"</kbd></td><td>Muove il braccio verso l'alto <strong>finche non riceve "ok"</strong></td></tr><tr><td><kbd>"alli scendi"</kbd></td><td>Muove il braccio verso il basso <strong>finche non riceve "ok"</strong></td></tr><tr><td><kbd>"alli su"</kbd></td><td>Movimento leggero verso l'alto, poi si ferma automaticamente</td></tr><tr><td><kbd>"alli giù"</kbd></td><td>Movimento leggero verso il basso, poi si ferma automaticamente</td></tr><tr><td><kbd>"basta"</kbd></td><td>Interrompe immediatamente i movimenti "sali" o "scendi", senza disattivare la possibilità di movimento</td></tr></tbody></table>

✅ Funzionano **solo in modalità attiva dopo&#x20;**_**"alli muovi"**_

### 💾 4. Gruppo Posizioni Salvate

**Obbiettivo:** richiamare posizione salvate (es. tavolo, bocca, ecc.) impostate tramite l'applicazione.

<table><thead><tr><th width="289.5555419921875">Comando</th><th>Descrizione</th></tr></thead><tbody><tr><td><kbd>"posizione &#x3C;nome>"</kbd></td><td>Esegue il movimento verso la posizione salvata con quel <strong>nome</strong></td></tr></tbody></table>

📝 Esempi:

* "posizione tavolo" → esegue il movimento per avvicinarsi al tavolo
* "posizione bocca" → esegue il movimento verso la bocca per bere

✅ Funziona **solo in modalità attiva dopo&#x20;**_**"ciao alli"**_

### 🔄 5. Gruppo Reset

**Obbiettivo:** permettere il riavvio del software dell'esoscheletro senza usare l'app.

<table><thead><tr><th width="289.5555419921875">Comando</th><th>Descrizione</th></tr></thead><tbody><tr><td><kbd>"reset errore"</kbd></td><td>Riavvia il software in caso di errore</td></tr></tbody></table>

✅ Funziona **solo in modalita attiva dopo&#x20;**_**"ciao alli"**_

### 🚨 6. Gruppo Emergenza

**Obbiettivo:** garantire la **massima sicurezza**.\
I comandi sono **sempre attivi**, anche senza wake word, e permettono l'interruzione immediata di ogni movimento.

<table><thead><tr><th width="289.5555419921875">Comando</th><th>Descrizione</th></tr></thead><tbody><tr><td><kbd>"aiuto"</kbd></td><td>Arresta immediatamente ogni movimento dell'esoscheletro, ed esce da tutte le modalità attive</td></tr><tr><td><kbd>"stop"</kbd></td><td>Idem</td></tr><tr><td><kbd>"fermo"</kbd></td><td>Idem</td></tr><tr><td><kbd>"ferma"</kbd></td><td>Idem</td></tr><tr><td><kbd>"fermati"</kbd></td><td>Idem</td></tr><tr><td><kbd>"alt"</kbd></td><td>Idem</td></tr></tbody></table>

⚠️ Questi comandi **non disattivano il sistema**, ma fermano qualsiasi azione in corso.\
Sono pensati per situazioni in cui l'utente ha bisogno di **interrompere subito** il movimento.

## 🧪 Test consigliati

<table><thead><tr><th width="337.88873291015625">Scenario</th><th width="112.888916015625" align="center">Comando</th><th>Risultato atteso</th></tr></thead><tbody><tr><td>Modalità inattiva + <code>"stop"</code></td><td align="center">✅</td><td>Arresto immediato del movimento</td></tr><tr><td><code>"ciao alli"</code> → <code>"inizia movimento"</code></td><td align="center">✅</td><td>Attiva controllo joystick</td></tr><tr><td><code>"alli muovi"</code> → <code>"alli su"</code></td><td align="center">✅</td><td>Movimento breve verso l’alto</td></tr><tr><td><code>"alli muovi"</code> → <code>"alli sali"</code> → <code>"basta"</code></td><td align="center">✅</td><td>Movimento continuo verso l’alto poi arresto</td></tr><tr><td><code>"ciao alli"</code> → <code>"posizione tavolo"</code></td><td align="center">✅</td><td>Movimento verso posizione salvata "tavolo"</td></tr><tr><td><code>"ciao alli"</code> → <code>"reset errore"</code></td><td align="center">✅</td><td>Riavvio software esoscheletro</td></tr><tr><td><code>"alli muovi"</code> → <code>"reset errore"</code></td><td align="center">❌</td><td>Comando ignorato (non ha permesso in questa modalità)</td></tr></tbody></table>
