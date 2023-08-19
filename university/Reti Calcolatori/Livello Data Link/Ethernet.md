# LAN
## Indirizzi MAC
I nodi in una LAN possiedono un indirizzo data link.
L'indirizzo data link viene riferito come NIC ed è conosciuto come indirizzo **MAC** (Medium Access Control).

Generalmente gli indirizzi MAC sono costituiti da 6 byte (2<sup>48</sup> possibili indirizzi).
L'indirizzo MAC di ogni adattatore internet è definito a livello hardware nella ROM, non può essere cambiato.

Non esistono 2 indirizzi MAC identici.

L'indirizzo MAC viene integrato nei pacchetti da inviare all'interno di una LAN, per sapere dove devono arrivare.

E possibile inviare un pacchetto a tutti gli adattatori nella rete tramite indirizzo MAC:
*FF-FF-FF-FF-FF-FF*

L'indirizzo MAC è composto da 2 parti:

I primi 3 bytes rappresentano l'identificatore unico dell'organizzazione (OUI, organizational unique identifier)
Gli ultimi 3 bytes rappresentano l'interfaccia del controller della rete (NIC, network interface card)

Il bit meno significativo del primo byte dell'indirizzo MAC indica se l'indirizzo si riferisce ad un indirizzo singolo o ad un gruppo di indirizzi.

Il secondo bit meno significativo del primo byte dell'indirizzo MAC indica se l'indirizzo MAC è indipendente o se l'amministratore della LAN può cambiare gli indirizzi MAC all'interno della rete.

## Ethernet
Nasce negli anni 70.
Ad oggi rappresenta l'equivalente dell'internet nelle reti globali, per le reti LAN.

### Categorie di Ethernet
Principalmente si divide per velocità:
- **Standard Ethernet**: 10 Mbps
- **Fast Ethernet**: 100 Mbps
- **Gigabit Ethernet**: 1 Gbps
- **Ten-Gigabit Ethernet**: 10 Gbps

Un altro modo di determinare il tipo di Ethernet è quello di usare la nomenclatura:
\[data rate in Mbps]\[Baseband / Broadband]\[cable type / length limit]

Esempio:
10Base5

#### Standard Ethernet

![[Standard Ethernet Cables.png]]

#### Fast Ethernet
Tipi di Fast Ethernet:
- 100Base-TX (2 cavi categoria 5 (cat5))
- 100Base-FX (2 cavi in fibra)
- 100Base-T4 (4 cavi categoria 3 (cat3))

Tradizionalmente, Ethernet è metà duplex (o si trasmette o si riceve, ma non contemporaneamente)

#### Full Duplex
Usando full-duplex, le stazioni possono trasmettere e ricevere dati contemporaneamente.

Questa tecnica raddoppia il throughput:
- Ethernet 10-Mbps in full-duplex raggiunge una velocità di trasferimento di 20 Mbps
- Ethernet 100-Mbps in full-duplex raggiunge una velocità di trasferimento di 200 Mbps

Per poter utilizzare il full-duplex, ogni stazione collegata alla rete full-duplex deve avere:
- delle schede di rete (NIC) adatte per il full-duplex
- deve usare 2 coppie di cavi:
	- una coppia per trasmettere dal host allo switch (inbound)
	- una coppia per trasmettere dallo switch al host (outbound)
- deve usare uno switch come dispositivo centrale, invece di un hub
- i dispositivi devono essere collegati point-to-point allo switch
- ogni stazione costituisce domini separati delle collisioni (non ci sono più collisioni)
	- CSMA/CD non più necessario
	- nessun limite di lunghezza dei segmenti

![[Full-Duplex Ethernet.png]]

#### Gigabit Ethernet
Velocità di 1 Gbps
Lunghezza minima di frame = 512 bytes

![[Gigabit Ethernet.png]]

- 1000Base-SX: due cavi fibra ad onda corta
- 1000Base-LX: due cavi fibra ad onda lunga
- 1000Base-CX: due cavi di rame (STP, shielded twisted pair)
- 1000Base-T: quattro cavi di rame (UTP, unshielded twisted pair)

Per Gigabit full-duplex, non ci sono collisioni.
La lunghezza massima del cavo è determinata dall'attenuazione del segnale nel cavo.

Gigabit Ethernet può essere condivisa (hub) o con switch:
- **hub**
	- half-duplex, utilizza CSMA/CD con cambiamenti MAC:
		- Estensione carrier
		- Frame bursting
- Switch
	- full-duplex, distributore buffered

##### Estensione Carrier
Consiste nel trasmettere caratteri di controllo per riempire gli intervalli di collisione (per esempio riempire il canale di *R* fino a quando non viene risolta la collisione).

Questa tecnica permette la gestione di frame minimi di 64 byte.

I caratteri di controllo vengono rimossi alla destinazione.

##### Frame Bursting
Il trasmettitore trasmette frame uno dopo l'altro, senza aspettare il controllo da parte del ricevente.
La dimensione massima del frame burst è 8192 bytes.
Questa tecnica consente il triplo del throughput per frame piccoli.
Può però essere pericoloso in caso di connessione instabile.

##### Distributore Buffered
Un distributore buffered è un nuovo tipo di hub 802.3 dove i frame entranti vengono salvati in una lista FIFO.

Utilizza CSMA/CD per gestire le collisioni nel trasferimento dei frame dalla lista FIFO di entrata alla lista FIFO di uscita.

Si utilizza un controllo di flusso basato sui frame per gestire la collisione (definito in 802.3).
Tutti i collegamenti sono full-duplex.

#### 10Gbps Ethernet
Distanza massima di un collegamento copre dai 300m a 40Km.
Solo modalità full-duplex.
Nessun algoritmo di CSMA/CD.
Usa solo fibra ottica, niente rame.


## Struttura pacchetti Ethernet
Composti da:
- **Data**: (46-1500 byte), se i dati superano il MTU (Maximum Transfer Unit = 1500 byte), il livello network frammenta il pacchetto o lo riempie di bytes dummy.
- **Indirizzo di destinazione**: (6 byte), i pacchetti con altri indirizzi vengono scartati.
- **Indirizzo di sorgente**: (6 byte), indirizzo MAC della sorgente.
- **Tipo**: (2 byte), protocollo di (de)multiplexing.
- **CRC**: (4 byte), funzione ciclica per il controllo di errori
- **Preambolo**: (8 byte), i primi 7 byte sono *10101010* e l'ultimo byte è *10101011*, viene sempre posto all'inizio del pacchetto e serve a sincronizzare il clock con il trasmittente.

| Preambolo | Indirizzo di dest. | Indirizzo di sorg. | Tipo | Data | CRC |
| --- | --- | --- | --- | --- | --- |

## Rappresentazione di bit in Ethernet
Si usa la codifica Manchester:
- Si divide il tempo in slot
- A seconda del tipo di informazione che si vuole rappresentare, si genera una transizione di voltaggio da basso ad alto (o vice versa) in mezzo allo slot di tempo:
	- 0: transizione da alto a basso
	- 1: transizione da basso ad alto

(Con questo metodo, ogni bit deve essere rappresentato da una transizione, quindi per esempio se avessimo una fila di 0 (0000), non basterebbe lasciare il segnale a basso, ma bisogna riportarlo ad alto per poi eseguire la transizione a basso, che viene identificata come uno 0)

Questa codifica facilita la sincronizzazione tra i sistemi collegati tramite Ethernet


## CSMA/CD Ethernet
Quando i nodi di una LAN sono collegati ad un hub piuttosto che ad uno switch, quando un nodo manda un pacchetto, tutti i nodi lo ricevono.
E necessario quindi l'uso di un protocollo di accesso multiplo.

Ethernet usa CSMA/CD:
- Senza divisione in slot
- Carrier sense (i nodi ascoltano il canale per sapere quando è libero)
- Identifica le collisioni
- Ritrasmissione delle informazioni molto veloce

Questo algoritmo consente un efficienza vicina al 100%.

La network interface card (NIC) esegue i passaggi necessari per implementare CSMA/CD (ovvero ascolta il canale, indietreggia nel caso ci sia una collisione e applica l'algoritmo di attesa per il reinvio dei pacchetti).

L'efficienza di CSMA/CD in Ethernet può essere stimata come:
- **Eff = 1 / (1 + 5 * T<sub>prop</sub> / T<sub>trasm</sub>)**

Dove:
- T<sub>prop</sub> = massimo tempo di propagazione tra 2 nodi
- T<sub>trasm</sub> = tempo per inviare un MTU (pacchetto di grandezza massima) (intorno a 1.2 ms per Ethernet 1 Mbps)