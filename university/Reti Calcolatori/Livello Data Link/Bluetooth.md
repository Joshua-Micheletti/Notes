Utilizzato per reti PAN wireless (WPAN).
Rispetto alle WLAN, coprono un area inferiore, sono strutturate solo in topologie ad hoc (connessioni dirette tra dispositivi), hanno un'architettura plug and play, supportano voce e consumano poca energia.

# Standard 802.15 WPAN
Questo standard definisce protocolli per la gestione di reti portatili per la connessione di dispositivi.
È divisa in 6 categorie con successive sottocategorie:
- Bluetooth
- WPAN over WLAN
- WPAN over HR (high rate)
- Ultralow power consumption (ZigBee)
- Mesh Networks over WLAN
- BAN (Body Area Network) a frequenze basse (> 1 MHz)
- PAN ottica

# Bluetooth
Bluetooth è la prima tecnologia popolare per reti ad hoc su distanze corte, sviluppato per integrare applicazioni di voce e dati.

Il Bluetooth è una WPAN ad hoc che opera intorno ai 2.4 GHz.

Gli scenari generici per Bluetooth sono:
- rimpiazzo di cavi (wireless)
- reti personali ad hoc
- access point integrato

## Architettura (piconet)
La topologia delle reti Bluetooth è una rete ad hoc sparsa:
più reti piccole, con terminali, coesistono e operano tra di loro.
Una cella (rete) all'interno di una rete Bluetooth è definita come **piconet** ed identifica 4 stati per un terminale:
- Master (M)
- Slave (S)
- Standby (SB)
- Parked / Hold (P)

Un piconet consente la presenza di al massimo 8 dispositivi (1 master e 7 slave), con una distanza massima di 10m.

Il **Piconet Master** è il dispositivo che gestisce i canali fisici basati sul suo indirizzo e clock.
Ogni altro dispositivo in una piconet si chiama **Piconet Slave**.

Le informazioni vengono trasmesse dal Master verso un singolo Slave alla volta. Per non lasciare indietro alcuni Slave, il Master passa da uno slave all'altro con metodo round robin.

In ogni momento, il ruolo di Master e Slave può essere cambiato.

### Frequency Hopping Spread Spectrum
Il Bluetooth opera a 2.4 GHz usando uno spettro esteso, frequency hopping e full-duplex. Il rate di frequency hopping è di 1600 hops/s.

La banda del Bluetooth è di 83.5 MHz, quindi va da 2.4 GHz a 2.4835 GHz, saltando da un canale all'altro (79 canali) di 1 MHz di banda, in modo pseudocasuale.

Dal Bluetooth 1.2 viene introdotto l'**Adaptive Frequency Hopping (AFH)**, una tecnologia di frequency hopping che cerca di evitare frequenze comuni in altri dispositivi (Wi-Fi, microonde...) per ridurre le frequenze.

Il Master della piconet decide la sequenza di frequenze per il frequency hopping, tutti gli Slave devono sincronizzarsi alla sequenza decisa dal Master. La trasmissione avviene attraverso TDD-TDMA:
- TDD: Time Division Duplex, ovvero la stessa frequenza viene usata sia per mandare che per ricevere informazioni, ma non contemporaneamente.
- TDMA: Time Division Multiple Access, ovvero viene assegnato uno slot di tempo per ogni dispositivo che vuole comunicare, così che possono comunicare sulla stessa frequenza senza che ci siano collisioni (in quanto trasmettono sempre in tempi diversi).

I pacchetti in Bluetooth possono occupare 1, 3 o 5 slot di tempo, quindi la loro trasmissione può occupare più di uno slot singolo.

## Classi di energia
Per Bluetooth ci sono diverse classi di energia che influenzano la distanza massima di connessione.

| Classe | Potenza | Range |
| --- | --- | --- |
| Classe 1 | 100 mW | 100 m |
| Classe 2 | 2.5 mW | 10 m |
| Classe 3 | 1 mW | 10 cm |

(Classe 2 è quella utilizzata in uso commerciale, Classe 1 e 3 sono utilizzate solo a livello industriale)

## Velocità di trasferimento
Bluetooth 1.2 offre un bit rate di 1 Mbps.
Bluetooth 2.0 offre un bit rate di 3 Mbps.

Le velocità effettivamente disponibili agli utenti sono molto inferiori in quanto ci sono molti overhead per i protocolli necessari, e le interferenze riducono la velocità.

## Servizi di collegamenti
Ci sono due tipi di collegamenti che possono essere stabiliti tra piconet Master e uno o più Slave:
- **Synchronous connection-oriented (SCO)**:
	- Il collegamento alloca una larghezza di banda fissa per una connessione point-to-point tra Master e Slave.
	- Sono possibili fino a 3 SCO in una piconet.
- **Asynchronous connectionless (ACL)**:
	- Collegamento tra il Master e tutti gli Slave della piconet point-to-point.
	- Solo un ACL possibile in una piconet.

### Synchronous Connection-Oriented (SCO)
Usati principalmente per trasportare dati in tempo reale (voce, audio), riduce di molto il delay (nessuna ritrasmissione) ma è possibile che ci sia perdita di dati.

In collegamenti SCO, vengono riservati periodicamente degli slot di tempo per trasmettere agli Slave in tempo reale.
Prima di stabilire una connessione SCO, è necessario che lo Slave sia collegato in ACL.

### Asynchronous ConnectionLess (ACL)
Un collegamento ACL offre una trasmissione di dati a pacchetti.
La consegna dei pacchetti è garantita tramite gestione di errori e ritrasmissione, ma non vengono riservati slot di tempo (priorità inferiore).

Gli Slave possono inviare pacchetti al Master solo se precedentemente sono stati parte di una comunicazione da parte del Master, altrimenti non possono occupare nessuno slot di tempo.

I pacchetti possono occupare 1, 3 o 5 slot di tempo, e possono essere inviati in maniera non protetta (ma comunque salvaguardati da ARQ) oppure protetti da un codice Forward Error Correction (FEC).

I pacchetti non protetti sono più veloci ma più esposti ad errori e possibili ritrasmissioni.

## Protocolli
Bluetooth è definito con un architettura di protocolli a livelli, alcuni di questi sono:
Protocolli Core, Protocolli di Rimpiazzo Cavi, Protocolli di Telefonia, Protocolli Adottati.

I protocolli necessari per Bluetooth sono:
- **Link Management Protocol (LMP)**:
	- Usato per stabilire e controllare il collegamento radio tra due dispositivi
	- Implementato nel controllore (hardware)
- **Logical Link Control and Adaptation Protocol (L2CAP):
	- Usato per il multiplex di multiple connessioni logiche tra due dispositivi che usano protocolli a più alto livello
- **Service Discovery Protocol (SDP)**:
	- Consente ai dispositivi di scoprire servizi forniti da altri dispositivi, ed i loro parametri associati.

## Livelli

![[Bluetooth Levels.png]]

### Livello radio ([[Livello Fisico]])
Questo livello specifica i dettagli dell'interfaccia aerea (wireless), includendo l'uso di sequenze di frequency hopping, schema di modulazione e potenza di trasmissione.

### Livello Baseband
Il livello Baseband specifica le operazioni a basso livello dei bit dei pacchetti: forward error correction (FEC), operazione di codifica, cyclic redundancy check (CRC), calcoli, gestione di ritrasmissioni e uso del protocollo ARQ (Automatic Repeat reQuest).

### Livello di Gestione dei Collegamenti
Questo livello specifica il collegamento ed il rilascio di connessioni SCO e ACL, autenticazione, scheduling del traffico, supervisione dei collegamenti e operazioni di gestione della potenza.

Implementa il protocollo LMP (Link Management Protocol).

Tutte queste operazioni sono automatiche e necessarie, e non sono controllabili dall'utente, ne hanno a che fare con i dati dell'utente.

### Interfaccia Controller del Host
Rappresenta l'interfaccia tra il controllore Bluetooth (scheda Bluetooth) ed il dispositivo che usa la connessione Bluetooth (per esempio un PC).

In dispositivi integrati, questa interfaccia risulta inutile.

### Livello L2CAP (Logical Link Control and Application Protocol)
Si tratta del livello che gestisce il multiplexing di protocolli di livelli superiori e di segmentare e riassemblare pacchetti grandi (SAR).

Il livello L2CAP fornisce servizi sia connectionless (ACL) che connection-oriented (SCO).

### Livelli di Protocolli Superiori
I protocolli superiori non sono definiti in IEEE 802.15.1 (Bluetooth), quindi l'uso di questi protocolli dipende dal profilo Bluetooth che si vuole implementare.

#### Radio Frequency Communication Protocol (RFCOMM)
Consente il rimpiazzamento di molte porte seriali (funzionalità di rimpiazzamento cavi con connessioni wireless).
Questo protocollo emula i segnali di controllo RS-232 (standard di comunicazione seriale) come TxD, RxD, CTS, RTS...

### TCP/IP/PPP
Livello che consente di trasformare informazioni trasmesse tramite Wireless Application Protocol (WAP) che usano TCP/IP ad informazioni per Bluetooth tramite Point-to-Point Protocol (PPP) sopra il livello RFCOMM.

### Object Exchange Protocol (OBEX)
Si tratta di un protocollo a livello di sessione usato per scambiare oggetti.
Questo protocollo può essere usato per libri telefonici, calendari, sincronizzazione di messaggi o per trasferimento file tra dispositivi collegati.

### Telephony Control Specification - Binary (TCS BIN)
Protocollo che definisce i segnali di controllo di chiamata per stabilire connessioni di chiamate vocali e chiamate di dati tra dispositivi Bluetooth.
Contiene anche procedure per la gestione di gruppi di dispositivi Bluetooth.

### Service Discovery Protocol (SDP)
Protocollo usato per accedere ad un dispositivo specifico ed ottenere le sue informazioni.
Oppure per usare un'applicazione specifica e trovare dispositivi che supportano quella applicazione.

## Applicazioni
Alcune applicazioni della connessione Bluetooth sono definiti nello standard:
- Trasferimento File
- Accesso a LAN
- Cuffie wireless
- Telefono Cordless

### Trasferimento File
Un dispositivo Bluetooth può accedere al file system di un altro dispositivo Bluetooth, modificare i suoi file, manipolare oggetti e trasferire file da un file system all'altro.

Questa applicazione interessa i livelli:
- OBEX (per scambiare oggetti e file)
- RFCOMM (per simulare un collegamento fisico)
- L2CAP (per stabilire una connessione ACL immagino)
- SDP (per trovare il dispositivo nella rete Bluetooth)

### Accesso a LAN
I dispositivi Bluetooth possono accedere a servizi LAN che usano il protocollo TCP/IP passando al protocollo Point-to-Point (PPP).
Una volta connessi, i dispositivi funzionano come se fossero normali dispositivi all'interno di una LAN cablata.

I livelli interessati sono:
- TCP/IP (per la connessione LAN)
- PPP (per la connessione tra LAN e Bluetooth)
- RFCOMM (per simulare la connessione fisica)
- L2CAP (per stabilire una connessione ACL immagino)
- SDP (per trovare i dispositivi nella rete Bluetooth)

### Cuffie Wireless
Attraverso questa applicazione, è possibile collegare cuffie Bluetooth ad un dispositivo Bluetooth (telefono, PC) in modo Wireless.
Il dispositivo che manda l'audio deve fornire un audio in input e output in modo full-duplex.

Livelli interessati:
- RFCOMM (per simulare la connessione cablata)
- L2CAP (per stabilire la connessione SCO credo)
- SPD (per trovare i dispositivi nella rete Bluetooth)

### Telefono Cordless
Un dispositivo Bluetooth con questa applicazione può essere usato come telefono senza fili per chiamare utenti o ricevere telefonate.
I dispositivi che implementano questa funzionalità possono anche comunicare tra loro.

Livelli interessati:
- TCS BIN (per la gestione di telecomunicazioni)
- L2CAP (per la connessione SCO credo)
- SDP (per trovare i dispositivi nella rete Bluetooth)

