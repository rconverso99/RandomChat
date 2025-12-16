# RandomChats
💬**RandomChats** è un'applicazione di messaggistica istantanea basata su architettura Client-Server, sviluppata per dispositivi Android. Il progetto gestisce l'autenticazione, la creazione di stanze e lo scambio di messaggi in tempo reale.

## 👥 Autore
* **Riccardo Converso** 

*Corso:* LSO_2122_14 



---

## 🏗️ Architettura del Sistema
Il sistema si basa su una struttura cloud distribuita che separa la logica dell'applicazione (Client Android) dalla gestione dei dati e del backend.

### Backend & Database (AWS)
L'infrastruttura di backend è ospitata su **Amazon Web Services (AWS)** e utilizza i seguenti componenti:

* **OpsWorks Stack:** Per la gestione della configurazione e del deploy.

* **Server Apache:** Gestisce le richieste HTTP ed esegue gli script PHP.


* **Amazon RDS (PostgreSQL):** Database relazionale per la persistenza dei dati utente e chat.



<img width="689" height="632" alt="image" src="https://github.com/user-attachments/assets/7ede4953-89cd-4810-835c-04b1501e8619" />


---

## 🌐 Comunicazione Client-Server
### API REST (Database)
La comunicazione tra l'app Android e il database AWS avviene tramite richieste HTTP standard, utilizzando il formato **JSON** per lo scambio dati.


**Libreria Client:** Retrofit.


 
* **Metodi HTTP:** GET, POST, PUT, DELETE.


* **Flusso:** Il client invia una request, il server PHP elabora e risponde.

<img width="957" height="399" alt="image" src="https://github.com/user-attachments/assets/148f29a6-c884-4811-bb8f-271c079575d8" />


---

### Socket TCP (Chat Real-time)
Per la funzionalità di chat in tempo reale, l'app utilizza una connessione socket diretta (riferimento a server Azure/TCP) gestita dalla classe `TCPClient`.

#### Gestione della Connessione
La classe `TCPClient` gestisce il ciclo di vita della connessione socket:

1. Creazione della Socket e connessione all'indirizzo del server.
2. Inizializzazione dei buffer di input (ricezione) e output (invio).


3. Loop di ascolto (`run()`) per i messaggi in arrivo.



**Codice: Connessione e Loop di Ascolto**


<img width="1253" height="989" alt="image" src="https://github.com/user-attachments/assets/98bf49a1-77ea-45cf-89ef-3d558075e419" />


#### Invio dei Messaggi
L'invio dei messaggi avviene tramite il metodo `sendMessage`, che scrive sul buffer di output. Per non bloccare il thread principale (UI Thread) di Android, l'invio è gestito da un task asincrono: `SendMessageTask`.

**Codice: Metodo SendMessage e AsyncTask**


<img width="774" height="286" alt="image" src="https://github.com/user-attachments/assets/1439975a-7c51-40d9-bab9-10380aecaa79" />

<img width="805" height="696" alt="image" src="https://github.com/user-attachments/assets/86dfc351-88cb-4820-b7ed-3382da21674c" />


#### Ricezione e Aggiornamento UI
Quando il client riceve un messaggio dal server, questo viene elaborato nel metodo `messageReceived`. Tramite `publishProgress` e `onProgressUpdate`, l'interfaccia utente (ChatActivity) viene aggiornata in tempo reale.

**Codice: Ricezione Messaggio**

<img width="1003" height="365" alt="image" src="https://github.com/user-attachments/assets/5af7bec2-e1d9-4272-8924-f286b6615892" />

<img width="1096" height="355" alt="image" src="https://github.com/user-attachments/assets/34d43da8-0eed-4c76-9269-89ad2c21905f" />


---

## ✨ Funzionalità Principali
L'applicazione offre le seguenti feature lato client:

* 🔐 **Login:** Autenticazione utente.
* ➕ **Creazione Stanza:** Possibilità di creare nuove room di chat.
* door **Accesso Stanza:** Entrare in stanze esistenti.
* 💬 **Chat:** Scambio di messaggi testuali in tempo reale.

---
