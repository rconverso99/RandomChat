
#RandomChats 💬**RandomChats** è un'applicazione di messaggistica istantanea basata su architettura Client-Server, sviluppata per dispositivi Android. Il progetto gestisce l'autenticazione, la creazione di stanze e lo scambio di messaggi in tempo reale.

##👥 Autori* 
**Riccardo Converso** 





* 
*Corso:* LSO_2122_14 



---

##🏗️ Architettura del SistemaIl sistema si basa su una struttura cloud distribuita che separa la logica dell'applicazione (Client Android) dalla gestione dei dati e del backend.

###Backend & Database (AWS)L'infrastruttura di backend è ospitata su **Amazon Web Services (AWS)** e utilizza i seguenti componenti:

* **OpsWorks Stack:** Per la gestione della configurazione e del deploy.
* 
**Server Apache:** Gestisce le richieste HTTP ed esegue gli script PHP.


* 
**Amazon RDS (PostgreSQL):** Database relazionale per la persistenza dei dati utente e chat.



(Riferimento immagine dalla slide: Diagramma architetturale OpsWorks Stack e PHP App Server Layer )

---

##🌐 Comunicazione Client-Server###API REST (Database)La comunicazione tra l'app Android e il database AWS avviene tramite richieste HTTP standard, utilizzando il formato **JSON** per lo scambio dati.

* 
**Libreria Client:** Retrofit.


* 
**Metodi HTTP:** GET, POST, PUT, DELETE.


* **Flusso:** Il client invia una request, il server PHP elabora e risponde.

(Riferimento immagine dalla slide: Schema Client Request / Server Response con metodi HTTP )

---

###Socket TCP (Chat Real-time)Per la funzionalità di chat in tempo reale, l'app utilizza una connessione socket diretta (riferimento a server Azure/TCP) gestita dalla classe `TCPClient`.

####Gestione della ConnessioneLa classe `TCPClient` gestisce il ciclo di vita della connessione socket:

1. Creazione della Socket e connessione all'indirizzo del server.
2. Inizializzazione dei buffer di input (ricezione) e output (invio).


3. Loop di ascolto (`run()`) per i messaggi in arrivo.



**Codice: Connessione e Loop di Ascolto**


(Snippet del metodo `run()` che mostra la creazione della socket e il loop di lettura `while(run)` )

####Invio dei MessaggiL'invio dei messaggi avviene tramite il metodo `sendMessage`, che scrive sul buffer di output. Per non bloccare il thread principale (UI Thread) di Android, l'invio è gestito da un task asincrono: `SendMessageTask`.

**Codice: Metodo SendMessage e AsyncTask**


(Snippet che mostra la classe `SendMessageTask` che estende `AsyncTask` e invoca `mTcp.sendMessage` )

####Ricezione e Aggiornamento UIQuando il client riceve un messaggio dal server, questo viene elaborato nel metodo `messageReceived`. Tramite `publishProgress` e `onProgressUpdate`, l'interfaccia utente (ChatActivity) viene aggiornata in tempo reale.

**Codice: Ricezione Messaggio**


(Snippet che mostra la gestione del messaggio in entrata e l'aggiornamento dell'UI con `publishProgress` )

---

##✨ Funzionalità PrincipaliL'applicazione offre le seguenti feature lato client:

* 🔐 **Login:** Autenticazione utente.
* ➕ **Creazione Stanza:** Possibilità di creare nuove room di chat.
* door **Accesso Stanza:** Entrare in stanze esistenti.
* 💬 **Chat:** Scambio di messaggi testuali in tempo reale.

---
