La congestione avviene quando il numero di pacchetti scambiati all'interno di una rete si avvicina al numero massimo di pacchetti gestibili dalla rete.

Le tecniche di controllo di congestione tentano di ridurre il numero di pacchetti per rimanere sotto la soglia critica (generalmente **80%** di utilizzo), per evitare una riduzione drammatica di performance.

All'interno della rete sono presenti delle code dove vanno i pacchetti che non possono essere processati immediatamente. Le code sono di dimensione finita, quindi se le code sono piene, alcune informazioni in arrivo vengono perse.

I pacchetti in arrivo vengono messi nelle code di input. Una volta eseguite le necessarie decisioni di routing, i pacchetti di input vengono messi nelle code di output.
Dentro le code di output, i pacchetti vengono processati il più velocemente possibile.
Se qualsiasi di questi procedimenti avviene più lentamente dell'arrivo di pacchetti, le code si riempiono e bisogna o scartare dei pacchetti, o usare tecniche di controllo di flusso.

Queste code sono presenti in ogni nodo della rete.

## Meccanismi di controllo di congestione
### Segnalazione di Congestione Implicita
Consiste in un meccanismo di analisi propria: la rete analizza le proprie metriche di performance per determinare se è in corso una congestione. Questo metodo evita la presenza di messaggi specifici per indicare una congestione, che potrebbero favorire la congestione stessa.

Alcune metriche controllate sono:
- Perdita di pacchetti
- Round-Trip Time (RTT): tempo di trascorrimento della rete intera
- Lunghezza delle code
- Riduzione di throughput
- Ritardo dei pacchetti
- Analisi dei pattern di traffico

### Segnalazione di Congestione Esplicita
La congestione viene riconosciuta e viene espressamente comunicata al sistema finale (che accetta o manda pacchetti) per ridurre il carico.

Il messaggio può essere spedito **all'indietro**: si tenta di ridurre il carico proveniente dalla sorgente del pacchetto. Oppure **all'avanti**: si tenta di ridurre il carico in direzione del pacchetto.

La segnalazione esplicita è divisa in base al tipo di segnalazione:
- Binaria
	- un bit nel pacchetto indica la presenza o assenza di congestione
- Basata su credito:
	- indica quanti pacchetti possono essere mandati da una sorgente
- Basata sul rate:
	- indica un limite di rate di dati

### Gestione del traffico
Il meccanismo di gestione del traffico deve essere basato su principi di:
- Giustizia
	- ogni pacchetto va trattato ugualmente
- Quality Of Service (QoS)
	- il servizio della rete deve essere mantenuto
- Riserva
	- alcune strade devono rimanere libere per alcune comunicazioni prioritarie

### Backpressure
Se un nodo diventa congestionato, può rallentare o addirittura bloccare i pacchetti provenienti da altri nodi.
Se questo avviene, i nodi vicini devono applicare meccanismi di controllo di rate per i pacchetti entranti (per dare tempo al nodo congestionato di gestire la situazione).

Questo meccanismo può propagarsi all'indietro fino alla **sorgente**.

### Pacchetto Choke
Pacchetto di controllo generato in un nodo congestionato che viene inviato al nodo sorgente.
Un esempio è il source quench per ICMP.

Viene inviato questo messaggio per ogni pacchetto scartato o anticipato, può peggiorare la situazione di congestione.

### Pacchetto di controllo
In quanto il pacchetto choke ha un tempo di reazione molto lento, è preferibile usare un pacchetto di controllo:
- Viene generato nel nodo congestionato
- Viene inviato al nodo precedente
- Fa effetto ad ogni Hop

### Perdita di pacchetti
Nel caso sia necessario scartare dei pacchetti, ci sono alcuni meccanismi da scegliere:
- Riduzione del carico:
	- meccanismo **wine**: scarta i pacchetti nuovi e mantieni i pacchetti vecchi
		- utile per il trasferimento di files
	- meccanismo **milk**: scarta i pacchetti vecchi e mantieni i pacchetti nuovi
		- utile per informazioni multimediali
- **Random Early Detection (RED)**:
	- quando la lunghezza media delle code supera un tetto massimo, i pacchetti in coda vengono scelti in modo casuale e scartati.
	- avviene ancora prima che le code siano del tutto piene, così da mantenere le performance della rete ottimali.
	- i pacchetti in entrata vengono già scelti come candidati per essere scartati, attraverso una curva di probabilità che aumenta all'avvicinarsi al lunghezza della coda.
	- la funzione di probabilità è configurabile con un **limite minimo** e un **limite massimo**:
		- limite minimo: limite di minima probabilità di scartare un pacchetto dopo aver raggiunto la soglia della coda
		- limite massimo: limite di massima probabilità di scartare un pacchetto prima di scartare ogni pacchetto
		- ogni lunghezza di coda prima del limite minimo **non comporta perdita di pacchetti**, ogni lunghezza di coda dopo il limite massimo **comporta la perdita sicura di pacchetti**

### Quality of Service (QoS)
Per assicurarsi di mantenere un buon QoS è necessario pianificare quali servizi hanno bisogno di quali caratteristiche per decidere come strutturare gli algoritmi di controllo della congestione.

Generalmente le metriche prese in considerazione per ogni tipo di servizio fornito sono:
- Affidabilità
- Ritardo
- Jitter
- Larghezza di banda

### Algoritmi di controllo di traffico
#### Secchio che perde
Questo algoritmo consiste nell'utilizzo di un buffer di input in cui finiscono i pacchetti con **flusso non controllato**.

Il buffer processa i pacchetti in modo **controllato** per fornire alla rete il numero esatto di pacchetti che può gestire.

Simile ad un secchio d'acqua che perde da un piccolo buco: indipendentemente da quanta acqua viene inserita nel secchio, la portata d'acqua che esce dal buco sarà sempre la stessa.

#### Secchio con token
Il buffer in questo caso contiene dei token, che fungono da permessi per i pacchetti per passare ed entrare nella rete.

Questo algoritmo consente di gestire momenti di burst (molti pacchetti alla volta), ma rispettando le capacità della rete (in quanto una volta finiti i token, i pacchetti non possono passare).

I token vengono rigenerati ogni dT secondi, a seconda delle capacità della rete.

### Riserva di risorse
Il controllo del traffico risulta più efficiente se i pacchetti seguono la stessa strada.
Per sfruttare questo principio, possiamo riservare delle strade e delle risorse per certe strade controllate.

Le risorse principali che vengono riservate sono:
- Bitrate
- Spazio nel buffer
- Cicli di CPU

I router devono tenere traccia di questi parametri per poter decidere come assegnarli in modo dinamico per ottimizzare il traffico.

### Scheduling di pacchetti
Il criterio di gestione dei pacchetti deve essere **giusto**, per questo spesso le code vengono organizzate in **pesi basati sul diritto** di un pacchetto ad essere processato.

Un semplice algoritmo FIFO causerebbe la starvation di alcuni pacchetti in fila dietro a pacchetti troppo lenti.

Per esempio le reti ATM (async transfer mode) classificano le categorie di priorità di flusso in base alle loro specifiche per il mantenimento del QoS:
- Bit rate costante (es. telefonia)
- Bit rate variabile in tempo reale (es. video conferenze)
- Bit rate variabile non in tempo reale (es. video streaming)
- Bit rate disponibile (es. trasferimento files)

### Servizi Integrati (IntServ)
Consiste in un approccio **basato sul flusso** per il mantenimento di QoS tramite riserva di risorse.

Rappresenta un insieme di protocolli con l'obbiettivo di streaming di multimedia (IETF) che prevede trasmissioni unicast e multicast.

Il protocollo usato per la riserva di risorse è il **Resource reSerVation Protocol (RSVP)**:
- Riserva di risorse in router intermedi tra mittente e destinatari
- Consente a più mittenti di trasmettere a multipli gruppi di destinatari
- Consente agli utenti individuali di cambiare canali
- Ottimizza l'utilizzo di larghezza di banda
- Elimina la congestione

Viene implementato tramite la ricerca e riserva di strade nello spanning tree della rete. 

IntServ è molto potente ma ha degli svantaggi:
- C'è una fase di setup prima di poter sfruttare al pieno le risorse disponibili
- Ogni router devono mantenere uno stato che dipende dal flusso. Questa caratteristica rende il sistema poco scalabile.
- Lo scambio di informazioni tra router per il controllo del flusso è molto complesso

### Servizi Differenziati (DiffServ)
Simile a IntServ ma **basato su classi** invece che basato su flusso.

Introduce **classi di servizi** con **regole di forwarding** associate.

Ogni pacchetto contiene un indicatore di **tipo di servizio** e a seconda del tipo di servizio, verranno riservate certe risorse per il trattamento corretto del pacchetto (dettate dalle regole di forwarding del servizio).