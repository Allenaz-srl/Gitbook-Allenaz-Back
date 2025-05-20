---
description: GitBook → GitHub → Nas → Gitea
---

# Sincronizzazione documentazione

## 🧾 Descrizione generale del sistema di sincronizzazione documentazione

### 🎯 Obiettivo del sistema

Allo scopo di garantire la **conservazione sicura, accessibile e autonoma** della documentazione tecnica e progettuale dell’azienda, è stato implementato un sistema automatico che sincronizza il contenuto dei repository GitBook aziendali (scritti in Markdown) verso:

1. Un mirror di backup pubblico su **GitHub**, mantenuto per continuità operativa.
2. Un repository centralizzato su **Gitea** installato all’interno dell’infrastruttura NAS aziendale(+ coppia locale NAS "Web/WebDocumentation").

Questo sistema è pensato per essere **resiliente**, funzionare **senza intervento umano**, e rappresentare una **fonte di verità locale** nel caso in cui:

* GitBook diventi inaccessibile,
* GitHub venga dismesso o non disponibile,
* sia necessaria una visualizzazione offline o autonoma della documentazione.

### 📌 Scenari di utilizzo

* **Recupero d'emergenza**: in caso di perdita o indisponibilità del contenuto GitBook.
* **Audit o certificazioni**: accesso interno completo e verificabile alla documentazione aziendale.
* **Visualizzazione su NAS**: utilizzo in combinazione con strumento come Obsidian per rendere i documenti navigabili anche in assenza di connessione Internet.

## 🧭 Mappa dell’implementazione e flusso di sincronizzazione

1. **GitBook**: contenuto documentale originario.
2. **GitHub**: repository pubblici che fungono da punto intermedio tra GitBook e i sistemi interni. Sync GitBook → Github tramite webhooks, ogni volta che la documentazione viene modificata.
3. **Script Node.js (`syncGitbookRepo.js`)**:
   * Clona automaticamente tutti i repository GitHub contenenti la documentazione.
   * Rimuove cartelle `.git` interne.
   * Aggiorna un repository Gitea centrale (`Allenaz-Doc`) con tutti i contenuti sincronizzati.
4. **Pianificazione (`node-cron`)**:
   * Eseguita dallo script `app.js` (parte del sistema di backend Log\_Viewer).
   * Programmata per attivarsi **3 volte al giorno** (6:00, 12:00, 18:00, ora locale).
   * Assicura che la sincronizzazione avvenga regolarmente e in modo automatico.
5. **Gitea (su NAS/QNAP)**:
   * Conserva una copia completa e autonoma dell'intera documentazione.
   * Funziona come backup principale e punto di accesso interno.
