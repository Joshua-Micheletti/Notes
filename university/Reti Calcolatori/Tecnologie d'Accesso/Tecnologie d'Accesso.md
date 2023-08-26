La tecnologia di reti telefoniche Public Switched Telephone Network (PSTN) rappresenta la tecnologia più simile alle tecnologie usate per la comunicazione informatica.

# Struttura di una rete telefonica
Le reti telefoniche possono essere:
- Totalmente connesse
	- Ogni nodo è collegato ad ogni altro nodo della rete (totalmente inusabile)
- Switch centralizzato
	- Ogni nodo della rete è collegato ad un nodo centrale che smista le informazioni tra i nodi.
	- Introduce un primo livello di gerarchia
- Gerarchia a multiplo livello
	- Si introducono più livelli di gerarchia

## Componenti principali
- Cicli locali
	- Cavi a coppie incrociate **analogici**
- Trunks
	- Fibre ottiche **digitali** che collegano gli uffici di switching
- Uffici di switching
	- Dove si muovono le chiamate da un trunk all'altro

La comunicazione digitale rimane più semplice, economica ed affidabile rispetto alla comunicazione analogica.

## Architetture di reti
- Reti a spina dorsale
	- Reti nello stesso edificio, edifici diversi, campus o vaste aree.
	- Scambio di informazioni tra LAN differenti
- Metro Area Networks (MAN)
	- Rete che collega utenti in aree maggiori rispetto a LAN, ma più piccole di reti WAN
- Access Network
	- Architetture EPON distribuite
	- Architetture ad anelli WDM-PON distribuite
	- Access Network Convergenti Ottiche / Wireless

### Access Network
Reti in cui l'**ultimo miglio** rimane un bottleneck, a causa delle tecnologie disponibili a livello commerciale (l'ultimo miglio rappresenta il cavo di connessione domestico dell'utente (DSL, modem)).

Tra non molto, le Access Network basate su rame non saranno più sufficienti per star dietro alla necessità di banda.

Reti FTTC (Fiber To The Curb) o FTTH (Fiber To The Home) basate su connessioni PON (Passive Optical Network) sono considerate i successori delle Access Network basate su rame.

Le due architetture più usate sono:
- Single Channgel Time-Division Multiplexed PON (TDM-PON)
- Multi-channel Wavelength-Division Multiplexed PON (WDM-PON)

## Ciclo Locale
Rappresenta la connessione da un'abitazione ad un nodo della rete dell'ISP (ultimo miglio).
Il ciclo locale sfrutta sia connessioni Digitali che Analogiche, la conversione da un tipo all'altro viene fatta dai Modem e Codecs.

Problemi di comunicazione:
- Attenuazione
- Distorsione
- Rumore

### Modem
I modem si occupano di modulare il segnale per cambiare il segnale da analogico a digitale o vice versa.

Un segnale sinusoidale (carrier) (f = 1000-2000 MHz) viene modulato, ovvero una delle sue caratteristiche (ampiezza, frequenza, fase) vengono modificate a seconda del segnale di informazione (segnale modulante) per produrre il segnale modulato.

I processi di modulazione delle varie caratteristiche del segnale sono:
- ASK (Amplitude Shift Keying)
- FSK (Frequency Shift Keying)
- PSK (Phase Shift Keying)

![[Modulation.png]]

#### Definizioni di modulazione
- Pass Bandwidth:
	- Intervallo di frequenza che passa attraverso il mezzo di trasmissione con attenuazione minima
- Baud:
	- Numero di sample trasmessi al secondo
	- 1 baud = 1 simbolo
- Bit Rate:
	- Quantità di informazioni trasmesse attraverso il canale in ogni unità di tempo (simbol rate * bit per simbolo)

#### Diagramma di costellazione
I diagrammi di costellazione indicano le possibili combinazioni di ampiezza e fase possibili per la modulazione del segnale. Ogni modem possiede un suo CD (Constellation Diagram).

- QPSK: 4 possibili combinazioni, 2 bits/simbolo
- QAM-16: 16 possibili combinazioni, 4 bits/simbolo (2400 baud -> 9600 bps).
- QAM-64: 64 possibili combinazioni, 6 bits/simbolo

![[Constellation Diagram.png]]

Per la gestione di errori, vengono aggiunti bit aggiuntivi per ogni sample (TCM, Trellis Code Modulation)

- V.32: 32 punti in CD, 4 + 1 (parità) bits/simbolo, 2400 bauds -> 9600 bps + correzione d'errori
- V.32 bis: 32768 punti in CD, 14 + 1 bits/simbolo, 2400 bauds -> 33600 bps
- modem standard: 33.6 Kbps
	- Banda telefonica: 4 KHz -> 8000 sample/s, 7 + 1 bits/sample -> 56 Kbps -> V.90 standard

## Asymmetric Digital Subscriber Line (ADSL)
Utilizza uno spettro di circa 1.1 MHz, diviso in 256 canali, ognuno di circa 4312.5 Hz

Canale 0: POTS (Plain Old Telephone Service), canale dedicato alla telecomunicazione
Canali 1-5: Banda di guardia tra voce e dati:
- 2 canali di controllo, uno per download e uno per upload

Il resto dei canali sono divisi tra upstream e downstream: a seconda del service provider, generalmente i canali sono asimmetrici, 80-90% utilizzati per il downstream e il resto per l'upstream.

All'interno di ogni canale, lo schema di modulazione è simile a V.34:
- QAM con 15 bit per baud
- 4000 baud invece di 2400
- Con 224 canali per downstream, teoricamente è possibile raggiungere velocità di 13.44 Mbps
- In pratica, la qualità del segnale non consente certe velocità, generalmente in condizioni buone, si possono raggiungere gli 8 Mbps.

![[ADSL.png]]

## Very-high-bit-rate Digital Subscriber Line (VDSL)
Tecnologia DSL (Digital Subscriber Line) che forniscono trasmissioni di dati più rapide di ADSL (asincrono).

Raggiungono fino a 52 Mbps downstream e 16 Mbps upstream, utilizzando cavi a coppie incrociate di rame, attraverso una frequenza che va dai 25 KHz a 12 MHz.

## Caratteristiche del segnale
### Attenuazione
Il segnale viene trasmesso ad una certa potenza, ma viaggiando nel mezzo, il segnale perde potenza (attenuazione), e a seconda della frequenza, ci può essere più o meno rumore.

Il rapporto tra la potenza attenuata del segnale e il livello di rumore indica la performance della connessione (**SNR, Signal to Noise Ratio**).

### Bits per Tone
Maggiore è la distanza di trasmissione dell'informazione, più risulta difficile distinguere il messaggio dal rumore

## Standard ADSL VDSL
![[Standard ADSL VDSL.png]]

## Reti Ottiche
Una rete ottica è una rete di telecomunicazione con collegamenti attraverso fibre ottiche e con un architettura sviluppata per sfruttare le caratteristiche della fibra ottica.

Alcune delle reti ottiche sono ad alte performance, e richiedono l'utilizzo sia di dispositivi ottici che elettronici.

La "colla" che tiene unite le reti ottiche consiste in:
- Optical Network Nodes (ONN), che collegano le fibre all'interno della rete
- Network Access Stations (NAS), che interfacciano i terminali degli utenti ed altre componenti non ottiche alla rete.

L'uso di reti ottiche è risultato necessario a causa dell'incremento della necessità di banda per trasferire dati sempre più grandi, ingrandimento del World Wide Web (molti più utenti e contenuti), businesses che dipendono da Internet, necessità di Quality Of Service (QoS) per servizi Web.

### Generazioni di reti ottiche
- Prima generazione:
	- Fibre ottiche usate per trasmissione e fornire capacità
	- Le operazioni di switching ed ogni altra operazione intelligente vengono gestite da hardware elettronico (**SONET Synchronous Optical Network, SDH Synchronous Digital Hierarchy**)
- Seconda generazione:
	- Le operazioni di routing, switching ed ogni operazione intelligente avviene a livello ottico
	- Si utilizzano tecniche di multiplexing

### Livello Ottico
Nelle reti ottiche, i livelli sono divisi in:
- [[Livello fisico]]
	- Contiene le componenti che eseguono operazioni lineari (trasparenti) su segnali ottici
	- Fornisce i servizi base di comunicazione ad un numero di reti logiche (LNs) indipendenti
- Le reti logiche (LNs) stanno nel [[Livello Data Link]]
	- Contiene le componenti elettroniche necessarie per eseguire operazioni non lineari su segnali elettrici

Funzionalità del livello ottico:
- Multiplex di multiple strade di luce in una singola fibra
- Consente di estrarre i raggi singoli in modo efficiente dal segnale multiplex composto nei nodi della rete
- Incorpora sistemi sofisticati di ristorazione del segnale
- Incorpora tecniche gestionali
- Fornisce raggi, usati da SONET ed elementi della rete IP

Vantaggio della struttura a livelli:
- È possibile controllare e gestire ogni rete logica indipendentemente (semplificazione)
- Condivisione delle risorse totali del livello fisico ad ogni rete logica (ottimizzazione)
- Personalizzare ogni rete logica per fornire servizi utenti specializzati (migliora QoS)
- Riconfigura dinamicamente ogni rete logica (cambio di rotta in caso di avaria)
- Utilizza funzionalità sia ottiche che elettroniche

### Passive Optical Network (PON)
Livello intermedio tra reti interamente ottiche e DSL.
Coprono distanze superiori rispetto a DSL, hanno una banda maggiore e gli elementi della rete sono passivi.

Nella distribuzione di PON, si parte dall'ufficio centrale (**CO, Central Office**) dell'ISP, in cui è locato il terminale di linea ottica (**OLT, Optical Line Terminal**).
Dal OLT, si distribuisce la connessione tramite fibre ottiche all'interno della rete di distribuzione ottica (**ODN, Optical Distribution Network**), fino ad arrivare alle unità di rete ottiche (**ONU, Optical Network Units**), che distribuiscono la fibra in modo **FTTC (Fiber to the curb), FTTN (Fiber to the neighborhood) o FTTP/FTTH (Fiber to the premises / home**, che richiede la presenza di un **ONT, Optical Network Terminal**)

### Tecniche di multiplexing 
- **Time Division Multiplexing (TDM)**:
	- Tipo di multiplexing che combina gli stream di dati assegnando ad ognuno di essi uno slot di tempo diverso. TDM trasmette ripetutamente una sequenza fissa di slot di tempo attraverso un singolo canale.
- Wavelength Division Multiplexing
	- Tecnica per inviare segnali di frequenze diverse di luce su una fibra singola contemporaneamente. Utilizza colori di luce laser diversi per separare i segnali. Simile al Frequency Division Multiplexing (FDM).

### Optical Network Terminal (ONT)
Convertitore di media installato all'interno o esterno delle abitazioni destinatarie.
L'ONT converte segnali di luce provenienti dalle fibre ottiche in segnali elettrici su rame.

Vengono utilizzate 3 lunghezze d'onda tra l'ONT e l'Optical Line Terminal (OLT):
- 1310 nm per voce e trasmissione dati
- 1490 nm per voce e ricezione dati
- 1550 nm per ricezione video

Ogni ONT è capace di fornire multipli POTS (Plain Old Telephone Service), dati internet e video.

### Optical Network Unit (ONU)
L'Optical Network Unit converte i segnali ottici in segnali elettrici.
Applicazione simile ad ONT, ma privata all'edificio proprietario della connessione.

I segnali vengono inviati individualmente agli iscritti al servizio internet, sono comunemente usati in applicazioni fiber-to-the-home (FTTH) e fiber-to-the-curb (FTTC).

### Optical Line Terminal (OLT)
Le OLT sono presenti nell'ufficio dell'ISP.
Funzionano come punto di origine di linee FTTP (fiber to the premises).
Ogni OLT contiene molte schede PON (per stabilire più connessioni attraverso il singolo OLT).
Contengono anche schede GWR (Gateway Router) e VGW (Voice Gateway).

Ogni scheda PON trasmette un laser a 1490 nm attraverso l'ONT e riceve una trasmissione dall'ONT con un laser a 1310 nm.

Il laser a 1550 nm per il video viene inserito dal CO (Company Office) verso l'ONT

### Splitter e Combiner ottici
Lo splitter di fibre ottiche divide il raggio di luce in due strade, ognuna delle quali riceve una percentuale della luce originaria. (prende 1 input e fornisce 2 output).

Il combiner di fibre ottiche unisce i raggi di luce provenienti da 2 fibre sommandole in una singola fibra.
(prende 2 input e fornisce 1 output).

## Fixed Wireless Access
Esempi di FWA (Fixed Wireless Access):
- Broadband, Broaderband, Narrowband, Voice Data, INTERNET, Video, Telemedicine, Tele-Education, Connectivity.

I dati attraverso FWA vengono trasmessi a velocità di Megabit o Gigabits / secondo.

FWA funziona su bande multiple, non esiste uno standard in quanto gli utenti FWA non si muovono (?).

