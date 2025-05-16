---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Login

## **Requirements**

1. **Autenticazione degli utenti**\
   L'API deve permettere agli utenti di autenticarsi utilizzando un livello di accesso (`accessLevel`) e una password.
2. **Mappatura del livello di accesso**\
   Deve esistere una mappatura tra il livello di accesso fornito dall'utente e i livelli interni predefiniti:
   * Base → 5
   * Advance → 15
   * Admin → 20
3. **Verifica delle credenziali**\
   Deve essere verificato se:
   * Esiste un utente con il livello di accesso specificato.
   * La password fornita corrisponde a quella crittografata salvata nel file `users.json`.
4. **Emissione di un token JWT**
   * Dopo una verifica valida, deve essere generato un token JWT con una scadenza di 1 giorno.
   * Il token deve essere firmato utilizzando una chiave segreta generata dinamicamente.
5. **Gestione degli errori**
   * Deve essere restituito un errore HTTP 401 se il livello di accesso o la password sono invalidi.
   * Deve essere restituito un errore HTTP 500 in caso di errore generico durante l'elaborazione.

***

## **Test**

**Test Case**

**TC1 - Verifica livello di accesso valido**\
**Descrizione:** Assicurarsi che l'API verifichi correttamente se un livello di accesso esiste nel file `users.json`.\
**Passi:**

1. Aggiungere un livello di accesso valido nel file `users.json`.
2. Eseguire una richiesta POST all'endpoint `/login` con un livello di accesso e una password validi.
3. Verificare che l'API restituisca il token JWT.\
   **Risultato atteso:** Viene restituito un token JWT con codice di stato HTTP 200.

***

**TC2 - Verifica password corretta**\
**Descrizione:** Assicurarsi che l'API accetti solo password valide per un livello di accesso esistente.\
**Passi:**

1. Inserire un utente valido nel file `users.json` con un livello di accesso e una password crittografata.
2. Eseguire una richiesta POST all'endpoint `/login` con il livello di accesso corretto e una password sbagliata.\
   **Risultato atteso:** L'API restituisce un errore con codice di stato HTTP 401.

***

**TC3 - Generazione token JWT**\
**Descrizione:** Verificare che il token JWT generato sia valido e contenga i dati dell'utente autenticato.\
**Passi:**

1. Eseguire una richiesta POST all'endpoint `/login` con credenziali valide.
2. Decodificare il token restituito.
3. Verificare che i dati dell'utente siano correttamente inclusi nel token.\
   **Risultato atteso:** Il token JWT è valido, contiene i dati dell'utente e scade in 24 ore.

***

**TC4 - Gestione degli errori**\
**Descrizione:** Verificare che l'API gestisca correttamente gli errori.\
**Passi:**

1. Rimuovere il file `users.json` e inviare una richiesta POST all'endpoint `/login`.
2. Verificare che l'API restituisca un errore con codice di stato HTTP 500.\
   **Risultato atteso:** L'API restituisce un messaggio di errore chiaro con codice HTTP 500.

**TC5 - Verifica password errata**\
**Descrizione:** Verificare che l'API gestisca correttamente gli errori in caso di un errore errato.\
**Passi:**

1. Accedi con la password sbagliata.\
   **Risultato atteso:** L'API restituisce un errore con codice di stato HTTP 401.

## Result

<table><thead><tr><th width="104">Test ID</th><th width="128">Data</th><th width="91">Person</th><th width="85" data-type="checkbox">Check</th><th>Commenti</th></tr></thead><tbody><tr><td>TC1</td><td>28/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC2</td><td>28/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC3</td><td>28/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>28/09/2023</td><td>Roman</td><td>true</td><td></td></tr><tr><td>TC4</td><td>28/09/2023</td><td>Roman</td><td>true</td><td></td></tr></tbody></table>

## Regression tests

<table><thead><tr><th width="80" align="center">ID</th><th>Test</th><th>Motivo</th></tr></thead><tbody><tr><td align="center">RT1</td><td>Verifica livello di accesso valido</td><td>Garantire che l'API accetti solo livelli di accesso esistenti nel file <code>users.json</code>.</td></tr><tr><td align="center">RT2</td><td>Verifica password corretta</td><td>Assicurarsi che la password fornita corrisponda a quella crittografata salvata nel sistema.</td></tr><tr><td align="center">RT3</td><td>Generazione token JWT</td><td>Verificare che il token JWT sia generato correttamente, valido e contenga i dati utente appropriati.</td></tr><tr><td align="center">RT4</td><td>Gestione credenziali non valide</td><td>Verificare che venga restituito un errore HTTP 401 per credenziali non valide o password errata.</td></tr></tbody></table>

