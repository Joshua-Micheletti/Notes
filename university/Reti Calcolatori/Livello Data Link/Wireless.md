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
Rilasciato 2 anni dopo il IEEE 802.11 legacy, aumenta la velocità teorica a 11 Mbps, portandolo più vicino allo standard Ethernet.
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

