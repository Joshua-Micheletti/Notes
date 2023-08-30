Ogni livello scambia cose diverse:
- [[Livello Rete]]: pacchetti (routers)
- [[Livello Data Link]]: frames (ponti e switch)
- [[Livello Fisico]]: segnali elettrici (ripetitori e hubs)

Ad ogni pacchetto vengono aggiunti gli header provenienti dai dati in ogni livello

# Livello Fisico
## Ripetitori
Limite di distanza riferito a Local-Area Network (LAN) (i segnali elettrici si indeboliscono a distanze lunghe, ciò indica il limite di distanza delle LAN).

I ripetitori uniscono LAN tra di loro:
- I ripetitori sono dispositivi elettrici analogici
- Monitorano continuamente i segnali elettrici provenienti da ogni LAN connessa
- Trasmettono una copia amplificata del segnale

## Hubs
Uniscono multiple linee di input elettricamente, sono sviluppati per gestire più schede di rete, ma non amplificano necessariamente il segnale.

Sono molto simili ai ripetitori, e come i ripetitori, operano al livello fisico.

## Limiti di Ripetitori e Hubs
- Costruiscono semplicemente una grossa rete locale, quindi ogni bit viene inviato ad ogni stazione della rete, ed il throughput deve essere condiviso da tutte le stazioni della rete (banda limitata)
- Non supportano tecnologie diverse di reti LAN, in quanto non bufferano o interpretano i frame con diverse codifiche (di conseguenza non si possono nemmeno collegare reti LAN a velocità diverse)
- Limitano le distanze e il numero di nodi di una rete, in quanto i segnali elettrici si disperdono sulle lunghe distanze.

# Livello Data Link
## Ponti
Connettono due o più LAN al livello Data Link:
- Estraggono l'indirizzo di destinazione dal frame in entrata
- Cercano la destinazione in una tavola interna
- Mandano il frame al destinatario corretto

## Switch
Generalmente collegano più computer tra loro, funziona come un bridge ma invece di collegare LAN tra loro, collegano Hosts.

Esattamente come i ponti, consentono la comunicazione concorrente.

### Accesso dedicato e Full Duplex
Gli host hanno un accesso diretto allo switch, piuttosto che un accesso condiviso come in una LAN.

Ogni connessione può essere inviata in entrambe le direzioni contemporaneamente. Gli host inviano allo switch e ricevono dallo switch.

Completo supporto di trasmissioni concorrenti, ogni connessione è un collegamento bidirezionale point-to-point

### Isolamento Traffico
Gli switch e i ponti possono dividere le sottoreti in segmenti di LAN, così da poter filtrare i pacchetti/frame direttamente alla sottorete corretta, riducendo il carico sulle altre sottoreti.

## Vantaggi rispetto ad Hubs e Ripetitori
- Vengono inviati solo i frame necessari
	- I frame non necessari vengono filtrati
	- I frame vengono inviati solo ai segmenti di LAN di destinazione
- Estendono l'espansione geografica della rete
	- Segmenti separati consentono distanze maggiori
- Migliorano la privatezza limitando l'area d'azione dei frame
- Possono unire segmenti della LAN che usano tecnologie diverse

## Svantaggi rispetto ad Hubs e Ripetitori
- Viene introdotto un ritardo nel trasferimento di frame
	- ponti e switch devono intercettare il frame
	- leggere il frame per decidere la destinazione
	- salvare e re-inviare il pacchetto introduce un delay
	- (soluzione: cut-through switching)
- Devono imparare la destinazione del frame
	- Devono costruire una tavola con gli indirizzi di destinazione
	- Idealmente senza intervento da parte degli amministratori
	- (soluzione: self-learning)
- Costo maggiore
	- switch e ponti sono dispositivi più complessi e costano di più
## Cut-Through Switching
Processo per ridurre il delay introdotto da switch e ponti:
- Inizia a trasmettere il prima possibile
	- Controlla l'header del frame e controlla la tabella di indirizzi
	- Se il collegamento in uscita è in idle, manda il frame immediatamente
- Trasmissioni sovrapposte
	- Utilizza i collegamenti in entrata e uscita separatamente, mentre viene inviato un frame, ricevi un altro frame in entrata

## Self-Learning
L'obiettivo è quello di costruire una tabella di switch in modo automatico, mappando gli indirizzi MAC di destinazione alle interfacce in uscita.

Procedimento:
- Quando arriva un frame:
	- controlla l'indirizzo MAC sorgente
	- associa l'indirizzo con l'interfaccia in entrata corrispondente
	- salva la mappatura nella tavola di switch
	- se non arriva nessun frame da quell'interfaccia per molto tempo, dimenticala
- Quando un frame arriva con una destinazione sconosciuta:
	- invia il frame a tutte le interfacce (eccetto l'interfaccia sorgente) (**Flooding**)
	- When in doubt, Shout!


### Flooding
Gli switch usano il meccanismo di Flooding (inviare un frame ad ogni interfaccia meno la sorgente) per inviare frame in broadcast.

Purtroppo però il meccanismo di Flooding può portare a loop nella rete, in caso ci siano più switch.

Per evitare la presenza di loop (che bloccherebbero la rete), si usano due metodi:
- semplicemente ci si assicura che non ci siano loop nella topologia della rete evitando alcuni collegamenti che formano il loop
- **Spanning Tree**:
	- si rappresenta la rete come un sotto-grafo della topologia originale ma che non contiene cicli
	- ogni collegamento non presente nello spanning tree non manda frame

### Costruzione di uno Spanning Tree
Richiede un algoritmo distribuito tra tutti gli switch per la costruzione dello Spanning Tree.
Gli switch selezionano un nodo come radice.
Ogni switch identifica se la sua interfaccia è nella strada più corta dalla radice.
Invia un messaggio (Y, d, X):
- X: nodo sorgente
- Y: radice
- d: distanza

Passaggi:
- Inizialmente ogni switch pensa di essere la radice
	- Ogni switch manda un messaggio ad ogni interfaccia del tipo (X, 0, X), ovvero ogni switch indica distanza dalla radice uguale a 0 e indicano se stessi come radice
- Aggiorna la vista dell'albero:
	- Quando gli switch ricevono i messaggi dagli altri switch, controllano l'id degli switch, e impostano lo switch con ID più piccolo come radice
- Calcola la distanza dalla radice:
	- Aggiungi 1 alla distanza ricevuta dallo switch vicino
	- Identifica interfacce che non appartengono alla strada più corta alla radice ed escludile dallo Spanning Tree

### Algoritmo di Spanning Tree Robusto
L'algoritmo deve essere resistente ad errori:
- errore del nodo radice (bisogna eleggere un nuovo nodo radice)
- errore di altri nodi (bisogna ricalcolare lo Spanning Tree)

Ogni tot tempo, il nodo radice manda un messaggio (1, 0, 1), ricordando agli altri switch che lui è la radice.

Se uno switch non riesce a comunicare per molto tempo con gli altri switch della rete, si auto-proclama radice.


# Livello Rete
## Routers
I router sono un altro tipo di dispositivo di internetworking che collega due o più reti packet switched o sotto-reti.

Questi dispositivi passano pacchetti tra reti attraverso il livello di rete usando protocolli di rete.

I router sono in grado di scegliere in modo intelligente quale strada è migliore per trasferire i dati sulla rete.

Un router re-dirige i pacchetti al loro IP di destinazione.
I router usano una tavola di routing interna, ovvero una lista di varie destinazioni di rete.

Il router legge l'indirizzo di destinazione scritto nell'header del pacchetto, e lo invia per la strada più efficiente verso la destinazione.

### Funzionalità del Router
- Piano di controllo:
	- Il piano di controllo è responsabile di popolare la tavola di routing, di disegnare la topologia della rete, di popolare la tavola di forwarding e quindi di consentire al piano di dati di funzionare.
- Piano di dati:
	- La tavola di routing, la tavola di forwarding e la logica di routing costituiscono le funzionalità del piano di dati.

### Tipi di Router
- **Router Nucleo**: usati in grosse corporazioni e businesses che trasmettono una grande quantità di pacchetti di dati all'interno della loro rete
- **Router Limite**: situati al bordo di una rete con router nucleo, usano il protocollo BGP (Border Gateway Protocol) per comunicare tra LANs e WANs.
- **Router Virtuale**: applicazione software che svolge le stesse funzionalità di un router hardware standard