2 o più dispositivi di elaborazione collegati da una singola "tecnologia" formano una rete.

CN: computer network
DS: software costruito su reti di calcolatori (distributed system)

il world wide web è un software DS costruito sull'internet (CN)


una rete di calcolatori è un sistema di comunicazione

la comunicazione dipende da 4 caratteristiche:

- Consegna
- Precisione
- Prontezza
- Jitter

Componenti di un sistema di comunicazione:

- Messaggio
- Mittente
- Destinatario
- Mezzo di trasmissione
- Protocollo

I dati possono essere trasmessi in modo:

- Simplex
- Duplex medio (Half-Duplex)
- Duplex intero (Full-Duplex)


Applicazioni di reti:

Client-server, richieste e risposte, peer-to-peer (client e server non fissi, ogni macchina può agire come entrambi)


Componenti di reti:

- Griglie: forniscono potere computazionale combinando tutte le macchine della rete per eseguire un compito
- Cloud: computazione remota come servizio piuttosto che come prodotto
- Virtualizzazione: tecnologia alla base di molti cloud

#### Griglie:
Infrastrutture per condivisione di risorse e problem solving, non soggette ad un controllo centralizzato. Utilizzano standard e protocolli open e general purpose per garantire un servizio.

#### Cloud:
Ci sono 3 definizioni per il cloud:
- Gartner: Il cloud computing è uno stile di computazione dove capacità informatiche enormemente scalabili sono fornite come servizio attraverso l'internet a clienti esterni multipli
- Forrester: Una pozzo di infrastrutture astratte e altamente scalabili capaci di hostare applicazioni dei clienti e pagate a consumo
- IBM: Un paradigma di computazione emergente dove dati e servizi risiedono in data center enormemente scalabili e accessibili ad ogni dispositivo attraverso internet

Parola chiave: architettura scalabile

Funzionalità chiave del cloud
- self service su richiesta
- accesso massivo alla rete
- elasticità
- misurazione del servizio

Funzionalità comuni del cloud
- scalabilità
- resilienza
- omogeneità
- distrubuzione geografica
- virtualizzazione
- orientata ai servizi
- software economico
- sicurezza


#### Virtualizzazione

Separa il software dall'hardware che lo esegue, incapsula il sistema operativo e le applicazioni in macchine virtuali.

Una macchina virtuale è un istanza di una macchina fisica che da agli utenti l'illusione di star accedendo il computer fisico direttamente.

La virtualizzazione è una parte fondante del cloud computing, in quanto è possibile installare multipli sistemi operativi sulla stessa macchina fisica


### Classificazione delle reti

#### Per dimensioni fisiche della rete:
- Personal Area Network (PAN): 1m
- Local Area Network (LAN): 10m - 1Km
- Metropolitan Area Network (MAN): 10Km
- Wide Area Network (WAN): 100Km - 1000Km
- Wireless Network
- Home Network
- Internetwork: 10000Km

#### Per tipo di trasmissione:
- Link Broadcast
- Link point-to-point


#### Topologie di reti:
- Bus
- Anello
- Stella
- Gerarchica
- Mesh

![[Network Topology.png]]


##### Personal Area Network (PAN)
Le PAN sono reti usate per la comunicazione tra dispositivi personali.
Possono essere reti wireless (WiFi, Bluetooth) o cablate (USB, FireWire).

![[PAN.png]]

##### Local Area Network (LAN)
Reti locali, possono essere di tipo:
- Bus: solo 1 host può trasmettere informazioni in un certo momento (master), bisogna regolare i conflitti (esempio: IEEE 802.3 Ethernet, 10Mbps-10Gbps)
- Anello: i bit si propagano in modo autonomo senza aspettare gli altri bit nei pacchetti, richiede un meccanismo in caso di conflitti (esempio: IEEE 802.5 IBM token ring, 4-16Mbps)

![[LAN.png]]


##### Metropolitan Area Network (MAN)
Esempio: TV cablata
Esempio: IEEE 802.16 (wireless)

![[MAN.png]]


##### Wide Area Network (WAN)
Rappresenta la relazione tra host all'interno di LANs e la sotto rete.
Le applicazioni risiedono nell'host e la sotto rete è incaricata di trasportare i pacchetti.

I pacchetti sono salvati e poi inviati dai router sulle linee di output.
Questo tipo di rete è chiamato rete packet-switched o rete store-and-forward.

La comunicazione consiste in una serie di pacchetti dal mittente al ricevente.
Alcune reti impongono che tutti i pacchetti seguano la stessa strada.
Normalmente, ogni pacchetto può seguire una strada diversa.
Le strade sono scelte attraverso algoritmi di routing.

![[WAN.png]]


##### Reti Wireless e Mobile
Classificate in:
- Sistema interconnesso (Bluetooth)
- LAN wireless (IEEE 802.11 standard for wireless LANs)
- WAN wireless (reti mobili cellulari (3G, 4G))

![[Wireless Network.png]]


##### Home Network
Categorie:
- Computer (desktop PC, PDA)
- Intrattenimento (TV, DVD, Stereo)
- Telecomunicazioni (cellulare, fax, telefono)
- Elettrodomestici (microonde, frigorifico)
- Telemetria (sensori, camera)


#### Criteri di valutazione delle reti:
- Performance: ritardo, throughput
- Affidabilità: abilità di consegnare i messaggi senza errori
- Sicurezza: protezione dei dati

#### Protocolli e Standard:
- Protocolli
	- Sintassi
	- Semantica
	- Sincronizzazione
- Standard
	- De facto (di fatto)
	- De jure (di diritto)


#### Organizzazioni Standard
- ISO
- ANSI
- IEEE
- EIA/TIA
- CENELEC
- ETSI


#### Organizzazione a livelli del software
Il software delle reti deve essere strutturato a livelli per gestire complessità.
Ogni livello nasconde ai livelli limitrofi i suoi dettagli interni (simile OOP).
I livelli possono essere visti come macchine virtuali per i livelli sovrastanti.
I livelli uguali comunicano attraverso protocolli.
I livelli contigui sono connessi da interfacce che definiscono il servizio del livello inferiore al livello superiore.
Livelli + Protocolli formano l'architettura della rete.

![[Network Architecture.png]]


Le relazioni orizzontali sono virtuali, le relazioni verticali sono reali


##### Compiti dei layer
- Addressing: identifica una sorgente e una destinazione
- Controllo di errori: gestisci errori causati da fenomeni fisici
- Controllo di flusso: per evitare il sovraccarico di un ricevente lento da una sorgente veloce
- Multiplexing: consente comunicazioni multiple e indipendenti nella stessa connessione
- Routing: decidi la strada dalla fonte alla destinazione