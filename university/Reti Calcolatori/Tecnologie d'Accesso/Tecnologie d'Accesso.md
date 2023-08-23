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
Il sfrutta sia connessioni Digitali che Analogiche, la conversione da un tipo all'altro viene fatta dai Modem e Codecs.

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

## ADSL
Utilizza uno spettro di circa 1.1 MHz, diviso in 256 canali, ognuno di circa 4312.5 Hz

Canale 0: POTS (Plain Old Telephone Service), canale dedicato alla telecomunicazione
Canali 1-5: Banda di guardia tra voce e dati:
- 2 canali di controllo, uno per download e uno per upload

Il resto dei canali sono divisi tra upstream e downstream: a seconda del service provider, generalmente i canali sono asimmetrici, 80-90% utilizzati per il downstream e il resto per l'upstream.