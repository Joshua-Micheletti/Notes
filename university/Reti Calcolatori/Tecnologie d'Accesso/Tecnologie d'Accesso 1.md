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

###