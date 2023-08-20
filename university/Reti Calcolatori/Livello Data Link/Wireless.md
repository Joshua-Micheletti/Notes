# PNRF
Piano Nazionale di Ripartizione delle Frequenze (PNRF) costituisce il piano regolatore dell'utilizzo dello spettro radioelettrico in Italia.

Si occupa di attribuire ai diversi servizi le bande di frequenza adeguate, di indicare a ciascun servizio all'interno di una singola banda l'autorità governativa rispetto alle frequenze e di verificare l'efficiente utilizzo dello spettro tra gli 0 e i 3000 Ghz al fine di liberare risorse per il settore televisivo e gestire eventuali conflitti con paesi limitrofi.

Le decisioni del PNRF seguono le direttive indicate dalla conferenza mondiale delle radiocomunicazioni (WRC), dall'unione europea e dai provvedimenti della conferenza europea delle poste e telecomunicazioni (CEPT).

# Tipi di reti wireless
- WPAN (Wireless Personal Area Network):
	- Bluetooth
- WLAN (Wireless Local Area Network):
	- WiFi
	- HiperLAN
- WMAN (Wireless Metropolitan Area Network):
	- WIMax
- WWAN (Wireless Wide Area Network):
	- GSM
	- GPRS
	- UMTS (3G)

# Standard IEEE 802.11 (Wi-Fi)
Lo standard per il Wi-Fi originale venne rilasciato nel 1997.
Definisce la trasmissione dati nella banda di frequenza di 2.4GHz attraverso onde radio, garantendo una velocità di trasmissione tra l'1 e 2 Mbps.
Lo standard è stato abbandonato per creare 2 gruppi indipendenti:

## IEEE 802.11b
Rilasciato 2 anni dopo il IEEE 802.11 legacy, aumenta la velocità teorica a 11 Mbps, portandolo più vicino allo standard [[Ethernet]].
L'accesso al canale viene fatto tramite algoritmo **CSMA/CA** (Carrier Sense Multiple Access con Collision Avoidance)

Questo algoritmo consente di minimizzare le interferenze nel canale di comunicazione e di massimizzare la probabilità che i pacchetti arrivino al destinatario.

Le tecniche di Collision Avoidance però saturano la banda, quindi la velocità effettiva raggiungibile è di circa 6-7 Mbps.

## IEEE 802.11a
Rilasciato nello stesso anno del IEEE 802.11b, trasmette ad una frequenza di 5 GHz (invece che 2.4 GHz) e divide la banda in canali non sovrapposti, con una velocità massima teorica di 54 Mbps.

Meccanismi di controllo e correzione di errori riducono la velocità effettiva a 20 Mbps.

Lo sviluppo di hardware per 5 Ghz risulta più costoso rispetto a 2.4 GHz, e penetra meno nei muri, riducendo la comodità.

## IEEE 802.11g
Uscito nel 2002/2003 e cerca di unire i lati positivi di entrambi gli standard a e b: supporta una larghezza di banda fino a 54 Mbps ed è retrocompatibile con 802.11b.

## IEEE 802.11n
Migliora lo standard 802.11g con l'uso di più antenne per aumentare la potenza del segnale (tecnologia MIMO). Teoricamente può raggiungere una larghezza di banda fino a 300 Mbps.


## Standard meno usati Wi-Fi
### WiGig
60 Ghz, usato per applicazioni ad altissima precisione (come realtà virtuale) (IEEE 802.11ay / ad)

### Wi-Fi 6E
6 Ghz

### Wi-Fi 6
Standard molto comune, lavora a 2.4 e 5 Ghz (IEEE 802.11ax)

### Wi-Fi 5
5 Ghz only, supporta utenti multipli (IEEE 802.11ac)

### Wi-Fi 4
Standard più vecchio, usato per applicazioni leggere, lavora a 2.4 e 5 Ghz (IEEE 802.11n)

### Wi-Fi HaLow
Sotto il GHz, utilizzato per IoT, connessioni a lungo raggio e bassa energia, penetra molto nelle pareti (IEEE 802.11ah)

## Cambio di nomenclatura
Ad un certo punto venne abbandonata la nomenclatura IEEE e si passò ad un nome più semplice:
- Wi-Fi 6 = IEEE 802.11ax (wifi più recente)
- Wi-Fi 5 = IEEE 802.11ac
- Wi-Fi 4 = IEEE 802.11n

## Wi-Fi 6
Wi-Fi 6 amplia ancora la larghezza di banda disponibile, arrivando ad un limite teorico di 9.6 Gbps.

Applica anche tecniche per la riduzione della congestione della rete, come **MU-MIMO** (Multiple User Multiple-Input Multiple-Output) e **beamforming**.

Utilizza ancora 2.4 e 5 GHz, però la versione più nuova del 2021 chiamata **Wi-Fi 6E** supporta la banda 6 GHz.

## Architettura Wireless 802.11
L'architettura logica di 802.11 è composta da diverse strutture:
- una stazione (utente, macchina)
- un access point wireless
- basic service set (simil LAN)
- extended service set (simil WAN)

Un BSS (basic service set) rappresenta una rete wireless con un singolo access point (AP) che supporta una o più stazioni wireless (clients).

Ogni stazione all'interno di un BSS comunica attraverso l'access point (AP).

L'access point fornisce connettività attraverso la LAN cablata, attua da ponte tra la rete e la stazione wireless.

Un ESS (extended service set) indica l'insieme di due o più access point wireless connessi alla stessa rete cablata (subnet).

Una rete wireless tra due stazioni, senza access point o distribution system (accesso a internet), si chiama **IBSS** oppure **rete ad hoc** (hotspot per esempio).

# Tecniche di trasmissione
Nelle trasmissioni radio si usa la tecnica di trasmissione a banda stretta, che consente di trasmettere diverse comunicazioni su canali differenti.

A causa dei limiti di trasmissione delle onde radio, questo tipo di trasmissione risulta difficile in quanto:
- la condivisione della larghezza di banda tra stazioni vicine crea interferenze
- le onde radio possono propagarsi per più percorsi, essere riflessa e rifratta

## Tecniche a Spettro Espanso
Spread-Spectrum è una tecnica per cui si usa un'ampiezza di banda più larga di quella necessaria.

Ci sono 2 tipi di tecniche a spettro espanso:
- FHSS (Frequency Hopping Spread Spectrum)
- DSSS (Direct Sequence Spread Spectrum)

### Frequency Hopping Spread Spectrum (FHSS)
Questa tecnica si basa sullo scomporre la banda in almeno 75 canali (hops larghi 1 MHz), e trasmettere usando una combinazione di canali conosciuti da tutte le stazioni nella cella.

La trasmissione avviene saltando da un canale all'altro in modo automatico per ridurre le interferenze.

### Direct Sequence Spread Spectrum (DSSS)
La tecnica consiste nel rimpiazzare ogni bit di informazione con una sequenza di bit, se 1 viene rappresentato da una certa sequenza, 0 viene rappresentato dalla frequenza complementare.

I riceventi, conoscendo la codifica, ricostruiscono il messaggio interpretando le sequenze come 0 e 1. Questo metodo aggiunge molta ridondanza al messaggio, che però consentono di eseguire controlli di errori e di risolverli.

![[DSSS.png]]

# Pacchetti IEEE 802.11 (Wi-Fi)
I pacchetti nel Wi-Fi contengono un **Payload** (messaggio) che può arrivare a 2312 byte.
Sono presenti anche 4 campi **indirizzo** (piuttosto che 2 come in Ethernet):
- indirizzo 1: indirizzo destinazione
- indirizzo 2: indirizzo sorgente
- indirizzo 3: indirizzo MAC dell'interfaccia router della sottorete (utile per connessione a Ethernet)
- indirizzo 4: usato in reti ad hoc (IBSS)

![[Wi-FI Frame.png]]

Se una stazione collegata in IEEE 802.11 attraverso un access point vuole comunicare con Ethernet (router), utilizza l'indirizzo 3, ovvero l'indirizzo MAC del router. I pacchetti devono essere convertiti tra IEEE 802.11 (Wi-Fi) e IEEE 802.3 (Ethernet) e vice versa.

Altre componenti del packet IEEE 802.11:
- Sequence Control: numero di sequenze per la gestione di ACK(nowledgment)
- Durata: tempo di riserva del canale per la trasmissione del messaggio
- Frame Control: insieme di informazioni per il controllo del frame:
	- Tipo e sottotipo: usati per distinguere associazioni, RTS, CTS, ACK e pacchetti di dati
	- To e From: definiscono altre funzioni per indirizzi che cambiano (infrastutture, ad hoc)
	- WEP: tipo di encryption dei dati

# Meccanismi livello MAC
Funzioni del livello MAC per comunicazioni wireless:
- gestisce le richieste di trasmissione di stazioni wireless
- multiplex di richieste di trasmissione di stazioni wireless
- supporto di roaming
- autentica le stazioni wireless
- riduce l'energia consumata

Oltretutto supporta i seguenti servizi:
- servizi di dati **asincroni** sono necessari
- servizi in **tempo reale** sono opzionali

## Metodi di accesso
Il metodo principale di accesso ai dati in IEEE 802.11 è la **Distributed Coordination Function (DCF)** basata su CSMA/CA e usa meccanismi **RTS** (Request To Send) e **CTS** (Clear To Send).

Un metodo aggiuntivo è la **Point Coordination Function (PCF)** implementata supra la DCF per garantire servizi in tempo reale. 

### Spazio tra frame
**InterFrame Spacing (IFS)** rappresenta l'intervallo di tempo tra la fine di una trasmissione e l'inizio di una trasmissione seguente.

Tipi di IFS:
- Short IFS (SIFS):
	- IFS più corto possibile, massima priorità
	- usato per frame RTS/CTS e ACK
- PCF IFS (PIFS): rappresenta il tempo di attesa tra SIFS e DIFS (tempo reale)
	- usato da PCF in operazioni libere
	- nel caso di contesa tra stazioni, la trasmissione viene prevenuta
- DCF IFS (DIFS): usata in stazioni DFS (asincrono)
	- tempo di idle minimo per stazioni contese
	- le stazioni possono trasmettere dopo DIFS se sono state in idle per un periodo più lungo di un DIFS
- Extended IFS (EIFS): tempo più lungo (minima priorità)
	- usato quando c'è un errore nella trasmissione del frame

### Meccanismi di accesso al mezzo per accessi contesi
Ci sono 2 scelte per la gestione di accessi multipli:
- CSMA/CD
	- + utilizzato in reti Ethernet
	- - le collisioni in reti wireless sono difficili da identificare
	- - collisioni causano un grande uso di larghezza di banda
- CSMA/CA
	- + si utilizzò questo, in quanto evitare le collisioni costa meno che gestirle

Per implementare il Carrier Sensing in reti wireless necessario per CSMA/CA, ci sono due metodi:
- Sensing fisico
	- Sensing diretto del livello fisico
	- costoso e molto complesso, dipende dal livello fisico
- Sensing virtuale
	- fornito dal **network allocation vector (NAV)**
	- il NAV indica per quanto tempo il canale è riservato
	- il NAV viene impostato a seconda di campi presenti nei frame (durata)

## Problema del terminale nascosto
In una rete wireless, le informazioni trasmesse da altre stazioni sono nascoste. Una stazione non può sapere se le altre stazioni stanno trasmettendo informazioni, e ciò causa collisioni.

### Multiple Access Collision Avoidance (MACA)
Si usano pacchetti segnalatori aggiuntivi:
- Il mandante chiede al ricevente se è disponibile a ricevere una trasmissione (**Request To Send (RTS)**)
- Il ricevente concorda, e invia una trasmissione (**Clear To Send (CTS)**)
- Il mandante invia la trasmissione, e il ricevente manda un **ACKnowledgment**

#### Problema del terminale esposto
Con questo metodo, può succedere che il RTS di un sender arrivi ad un altro sender, che pensa che il canale si occupato, nonostante ci siano ulteriori canali liberi. Ancora peggio se il secondo sender ascolta il RTS del primo sender, ma non sente il CTS del ricevitore.

Nel metodo MACA, il tempo è sincronizzato tra tutte le stazioni presenti, e lo schedule (tempo di occupazione di ogni stazione) è noto a tutte le stazioni.

In questo modo, quando un sender nota la presenza di un terminale esposto (RTS ma no CTS), il sender può comunicare con un altro receiver, invece che aspettare la risposta CTS mai arrivata (dopo che è passato un certo tempo dalla mancata risposta).

# HiperLAN
HiperLAN è una tecnologia per la connessione a reti LAN ampie. La rete comunica tramite un antenna ad una stazione collegata ad Internet.

Lavora su frequenze simili a 802.11a (5 GHz).

Il limite di trasmettitori HiperLAN esterni è di 1 watt di potenza, quindi si dividono in canali di 20 MHz.

Sono possibili lunghezze d'onda fino ai 40 MHz, ma sempre limitate in potenza.

HiperLAN possiede alcuni protocolli:
- TCP: Transmitter Power Control
	- Il trasmettitore può cambiare istantaneamente la potenza per non superare il Watt
- DFS: Dynamic Frequency Selection
	- Il trasmettitore può cambiare istantaneamente la frequenza per evitare interferenze


# Interferenze Wi-Fi
Le interferenze Wi-Fi possono essere di 3 tipi:
- Interferenze tra stazioni dello stesso access point (stesso canale)
- Interferenze tra più access point adiacenti (canale adiacente) (peggiore)
- Interferenze provenienti da fonti diverse dal Wi-Fi (altri segnali e onde)

Visto che i canali sono di 20 Mhz, con uno spazio tra di loro di 5 MHz e in un intervallo di 100 MHz totali, per farci stare 13 canali, è necessario sovrapporre i canali tra di loro.

Il tipo peggiore di congestione Wi-Fi consiste nell'interferenza da uso di canale adiacente.

Nel caso di interferenze sullo stesso canale, spesso ci cerca di tenere 20 dB tra i livelli di potenza delle trasmissioni sullo stesso canale, per ridurre la confusione e la congestione.

## Received Signal Strength Indicator (RSSI)
L'RSSI indica la qualità del segnale ricevuta da un dispositivo proveniente da un AP o router.
Utile per quantificare la qualità della connessione wireless.
Dipende dalla scheda wireless del dispositivo oltre che al segnale stesso.

L'attenuazione del segnale segue la legge del quadrato inverso, ovvero al raddoppiare della distanza, il segnale riceve un attenuazione di -6 dB di potenza. (curva simile logaritmo)

Il RSSI è un valore relativo, mentre dBm rappresenta una misura assoluta di potenza del segnale in mW.

Il RSSI è un valore che tipicamente va da 0 a 255, ma che ogni produttore adatta ad una scala propria (0 a 100, 0 a 60 etc...), ma indipendentemente dalla scala, il valore sempre indica la qualità della connessione (numero più alto significa connessione migliore).

Nel caso di dBm, questo indica la quantità di energia persa, quindi più ci avviciniamo a 0 dBm, migliore sarà il segnale.

![[dBm Signal Quality.png]]


# Caratteristiche Wi-Fi
## MIMO
Con MIMO si usano più antenne per poter aumentare la banda, o per gestire più utenti contemporaneamente:
- SU-MIMO: Single User MIMO (Multiple Input Multiple Output)
	- Avendo più antenne e porte disponibili per un solo utente, è possibile aumentare la velocità di banda e ridurre enormemente le interferenze
- MU-MIMO: Multiple User MIMO
	- Con questo metodo è possibile gestire trasmissioni multiple senza perdita di banda e riducendo le interferenze e collisioni.

## Channel Bonding
Con il channel bonding, è possibile unire due canali da 20 MHz in un singolo canale a 40 MHz, così da raddoppiare la banda.
Per poter sfruttare questa tecnologia, è necessario che sia router che schede riceventi applichino questa modalità.

Se nella rete con channel bonding sono presenti dispositivi con standard che non sono del tutto compatibili, la qualità della rete ne potrebbe risentire.
Impostando i dispositivi correttamente è possibile ovviare a questo problema.
In ogni caso, la velocità massima raggiungibile con router wireless 802.11n con channel bonding è intorno ai 200 Mbps.

## Punti morti e zone lente
A causa della natura ondulatoria del Wi-Fi, non tutte le zone sono uguali, in quanto in alcune zone, la rete deve penetrare più o meno ostacoli, e la velocità e affidabilità della connessione ne risentono di conseguenza.

In spazi aperti, il Wi-Fi ha potenza e penetrazione massima, e a seconda del tipo di muri, la potenza del segnale viene dissipata più o meno.

### Influenza dei materiali per onde Wi-Fi
- Muri a secco: 3 dB
- Porte in legno: 4 dB
- Muri in mattone: 6 dB
- Cemento: 8 dB
- Frigorifero: 19 dB