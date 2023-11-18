Proprietà degli algoritmi di routing:
- Tipo di algoritmo
	- Statico (non adattivo)
	- Adattivo (prende in considerazione lo stato della rete)
- Principio di ottimalità
- Routing per strada minima
- Inondazione
- Routing con vettore distanza
- Routing con stato di collegamento
- Routing gerarchico
- Routing broadcast
- Routing multicast

## Principio di ottimalità
**Se J si trova sulla strada ottimale da I a K (r<sub>1</sub> = IJ, r<sub>2</sub> = JK), allora la strada ottimale da J a K è r<sub>2</sub>** 

La strada ottimale da ogni possibile sorgente da una destinazione singola forma una struttura ad albero

![[Routing Tree.png]]

## Routing a strada minima
Si calcolano i pesi degli archi di un grafo (rete) tramite alcune metriche:
- distanza, larghezza della banda, traffico mediano, costo di comunicazione, lunghezza di coda, ritardo etc...

Una volta ottenuto un grafo pesato, si usa l'algoritmo di **Dijkstra** per trovare la strada minima da un nodo ad ogni altro nodo del grafo.

## Algoritmo di Inondazione
L'inondazione è una possibile strategia per fare **routing statico**, in quanto ogni pacchetto viene inviato.

Questo metodo genera un enorme numero di pacchetti duplicati.
Per ovviare a questo problema, vengono aggiunti dei contatori nell'header di ogni pacchetto (ridotti di 1 ad ogni hop), oppure possono essere tracciati dal router per evitare ri-trasmissioni.

Una possibile variabile è **l'inondazione selettiva (geografica)** (ovvero viene inondata solo una parte della rete).

Nonostante sia un algoritmo applicabile solo in casi particolari, anche lui trova il suo utilizzo:
- applicazioni militari con alta necessità di ridondanza
- database distribuiti
- esempio di comparazione per altri algoritmi (in quanto minimizza il delay)

## Routing con vettore distanza
In questo algoritmo, ogni router della sottorete tiene traccia tramite una tavola di tutte le tavole degli altri router.

Ogni tavola contiene informazioni su:
- Linea preferita per una certa destinazione
- Tempo o distanza stimate associate alla destinazione
- La distanza, che viene calcolata in hops o in tempo di risposta, tramite pacchetti **ECHO** speciali
- Ogni T secondi, ogni router comunica la sua lista di tempi di ritardo ai propri vicini

Possono avvenire molti scambi nel caso di ritardi alti.

La strada ottimale viene individuata tramite la comunicazione tra i router nelle sottoreti delle proprie informazioni riguardo alla propria distanza da ogni nodo.

Eventualmente, la rete **convergerà** ad uno stato in cui ogni router contiene informazioni ottimali sulle strade per ogni nodo della rete.

### Protocolli di routing con vettore distanza
Esempi di alcuni protocolli:
- Routing Information Protocol (**RIP**)
- Interior Gateway Routing Protocol (**IGRP**)
- Enhanced Interior Gateway Routing Protocol (**EIGRP**)

Ogni router che sfrutta un protocollo di routing con vettore distanza deve conoscere due cose:
- Distanza dalla destinazione finale
- Vettore, o direzione, in cui deve essere diretto il traffico

#### Caratteristiche dei protocolli di routing con vettore distanza
Ogni protocollo di routing con vettore distanza sono caratterizzati da:
- Aggiornamenti periodici
- Vicini (router vicini)
- Aggiornamenti broadcast
- La tavola di routing viene inclusa interamente con ogni aggiornamento

I parametri di paragone tra protocolli di routing generalmente sono:
- Tempo di convergenza (lenta)
- Scalabilità (limitata)
- Uso di risorse (basso)
- Implementazione e manutenzione (semplice)

#### Componenti di routing con vettore distanza
##### Scoprimento della rete (Inizio)
Inizialmente vengono inserite reti dirette nelle tavole di routing dei router.

Se il protocollo è configurato, i router iniziano a scambiare le proprie informazioni con i router vicini.
Se i router riceventi ottengono nuove informazioni dai propri vicini, aggiornano la loro tabella con i nuovi dati.

La convergenza completa viene raggiunta quando ogni router nella rete contiene le stesse informazioni nella propria tavola (detettata nel momento in cui ogni router non aggiorna le proprie informazioni in seguito ad uno scambio)

Prima che una rete venga definita operativa, bisogna raggiungere la convergenza.
Il tempo di raggiunta della convergenza dipende da due categorie interdipendenti:
- Velocità di broadcast delle informazioni di routing
- Velocità di calcolo delle strade

##### Manutenzione delle tavole di routing
Avvengono **aggiornamenti periodici: RIPv1 e RIPv2**: consistono in intervalli di tempo in cui i router condividono la propria tavola di routing intera.

Nel caso di protocollo RIP, vengono utilizzati 4 timer:
- Update
- Invalid
- Holddown
- Flush

Nel caso di protocollo EIGRP, gli aggiornamenti sono:
- Parziali
- Attivati da cambiamenti topologici
- Vincolati
- Non periodici

Gli eventi che possono attivare un aggiornamento delle tavole di routing possono essere:
- Cambio di stato dell'interfaccia
- Una strada diventa irraggiungibile
- Una strada viene aggiunta ad una tabella di routing

Gli aggiornamenti delle tavole possono avvenire contemporaneamente e con informazioni non concordanti. La soluzione per questo problema consiste nell'usare una variable random chiamata **RIP_JITTER**, così da sfasare gli aggiornamenti per evitare collisioni.
##### Cicli di routing
Condizione in cui un pacchetto viene **continuamente ritrasmesso** tra alcuni router in una rete senza **mai raggiungere la destinazione**.

Possono essere causati da:
- Strade statiche mal configurate
- Ridistribuzioni di strade mal configurate
- Convergenza lenta
- Strade di scarto mal configurate

E possono creare i seguenti problemi:
- Uso eccessivo di larghezza di banda
- Abuso di risorse CPU
- La convergenza della rete viene deteriorata
- Gli aggiornamenti di routing possono essere persi o non processati in tempo

###### Count to infinity
I protocolli di routing vettore 

distanza usano un valore massimo di hop che può compiere un pacchetto prima di definire la sua strada come irraggiungibile.

Quando un router **conta a infinito**, dove infinito è la variabile di hop massimi, viene scartata la strada di routing che causa questo problema.

###### Holddown timers
E possibile ovviare a questo problema anche tramite l'uso di **Holddown timers**:
rappresentano intervalli di tempo in cui un router non accetta informazioni di aggiornamento.

Questo meccanismo garantisce che i router si aggiornino solo con le informazioni più recenti e aggiornate.

###### Regola Split Horizon
Questa regola viene usata per evitare la presenza di cicli di routing.
La regola è:
Un router non può condividere informazioni sull'interfaccia da cui è arrivato l'aggiornamento (per evitare cicli diretti).

La regola inversa (**Split Horizon con veleno inverso**) ci consente di eliminare strade che portano a cicli:
Quando un router riconosce una strada irraggiungibile attraverso un interfaccia, comunica la strada come irraggiungibile attraverso la stessa interfaccia.

###### TTL (Time To Live)
TTL è un header IP, indica un numero intero che viene decrementato dai router ad ogni hop. Quando il valore raggiunge 0, il pacchetto viene scartato.

#### Differenze tra protocolli di routing con vettore distanza
**RIP:**
- Supporta Split Horizon e Split Horizon con veleno inverso
- Distribuisce il carico tra i router
- Facile da configurare
- Funziona con router provenienti da diversi proprietari

**EIGRP**
- Aggiornamenti attivati (non automatici e continui)
- Utilizza il protocollo EIGRP Hello per stabilire la vicinanza tra router
- Usa la tavola topologica per mantenere tutte le strade
- Protocollo proprietario Cisco


## Routing con stati di collegamento (LSR)
Chiamato anche **Shortest Path First (SPF) forwarding** (chiamato in onore dell'algoritmo di Dijkstra).

In questo protocollo, ogni router contiene una tavola della topologia intera della rete, sottoforma di liste di router e informazioni sui vicini di ogni router e la loro connessione.

Ogni router genera un **Link State Packet (LSP)** che contiene nomi (indirizzi) e costo da ognuno dei suoi vicini.

Questo pacchetto viene trasmesso ad ogni altro router della rete, che aggiorna i propri dati in base alle informazioni fornite.

Una volta ricevuti gli LSP da ogni altro router della rete, un router può fare decisioni di routing basate sulla topologia intera della rete.

### Link State Packet (LSP)
Vengono generati e distribuiti quando:
- E passato un lungo periodo di tempo
- Nuovi vicini vengono connessi alla rete
- Il costo di un collegamento ad un vicino è cambiato
- Un collegamento ad un vicino è fallito
- Un vicino è fallito

Gli LSP in pratica sono una lista di coppie formate da:
- nome del vicino ad un router (può essere un altro router o una rete)
- costo del collegamento a quel vicino

La distribuzione dei pacchetti LSP può essere difficile, in quanto vanno comunicati a tutti gli altri router della rete, che però non è ancora configurata.

Un metodo per assicurarsi che ogni router riceva i pacchetti LSP consiste nell'**inondamento**:
Ogni LSP ricevuto da ogni router viene ritrasmesso ai sui vicini immediati (eccetto il vicino da cui proviene l'LSP).

Questa soluzione garantisce la comunicazione corretta degli LSP ma genera anche un numero esponenziale di pacchetti che circolano nella rete in fase di inizializzazione.

Per ottimizzare un po questo meccanismo, si può **comparare l'LSP ricevuto con le proprie informazioni interne**, se le informazioni **coincidono**, l'LSP ricevuto viene **scartato** (e non ritrasmesso), se invece l'informazione è **nuova**, l'LSP ricevuto rimpiazza le informazioni interne del router e viene ritrasmesso ai vicini.

Lo scarto degli LPS doppi non è un problema in quanto se un router ha già ricevuto un LSP uguale, significa che è anche già stato ritrasmesso, quindi non viene persa nessuna informazione.

### Algoritmo
L'algoritmo per Routing con stati di collegamento si basa principalmente sull'algoritmo di **Dijkstra**. Viene eseguito su ogni router che calcola ogni possibile strada verso la destinazione, sommando i costi. In fine viene usata la strada con **costo minore**.

#### Strutture dati
L'algoritmo richiede la presenza delle seguenti informazioni:
- Database di stato di collegamenti (lista di tutti gli ultimi LSP da ogni router)
- Path
	- struttura ad albero riempita con le strade migliori
	- simil cache
	- ogni nodo consiste in una tupla: **(ID, costo, porta)**
- Tent
	- struttura ad albero contenente le strade che stanno venendo testate e comparate (approccio a tentativi)
	- simil workspace
	- ogni nodo consiste in una tupla: **(ID, costo, porta)**
- Database di forwarding
	- tavola contenente ogni ID che può essere raggiunto e la porta a cui mandare i messaggi (porta indica il router vicino da cui passare per arrivare all nodo)
	- versione ridotta del Path ma non pesata (senza costo)
	- ogni riga consiste in una tupla: **(ID, porta)**

#### Procedimento algoritmo
Inizialmente Path è composto solo dalla radice formata da (ID router, 0, 0).

- Per ogni nodo **N** inserito nel Path:
	- Per ogni vicino **M** del nodo N:
		- Se M non è in Tent, aggiungi un nodo a Tent per M (usa l'LSP per N per determinare il costo del collegamento)
		- Se M è già in Tent e il suo costo è inferiore ad un valore precedente per M, rimpiazza quel valore con l'informazione dal LSP di N
		- Se M è già in Tent ma ha un costo superiore, ignora il collegamento di N ad M
		- Calcola la strada minima nel Tent
			- Se la strada minima ha un costo inferiore rispetto alla strada nel Path, rimpiazza la strada nel Path con la strada nel Tent

#### Cambiamento Topologico
Quando avviene un cambiamento topologico nella rete, le tavole di forwarding vanno aggiornate.

Il cambiamento topologico può avvenire in seguito all'inserimento / rimozione di un nodo, ma anche in seguito al cambiamento di un costo di connessione.

Quando avviene un cambiamento topologico quindi, le tavole di forwarding e la struttura Path vengono **invalidate** e vanno ricalcolate.

Tramite l'uso della memoria sarebbe possibile ricordarsi dello stato del Path prima dell'ottimizzazione, così da poter individuare strade alternative in seguito ad un cambiamento topologico, ma purtroppo l'intero albero va ricalcolato.

### Open Shortest Path First (OSPF)
Versione aperta del protocollo SPF, ovvero ognuno può implementare il protocollo senza nessun costo.

Essendo un protocollo LSR, può implementare un bilanciamento di carico.

#### Sistemi Autonomi (AS)
Il protocollo OSPF prevede l'utilizzo di **Sistemi Autonomi (AS)** per lo sviluppo di una strategia di routing su più livelli.
Il sistema intero viene partizionato in Sistemi Autonomi:
- All'interno di ogni AS, viene applicato routing OSPF (questa soluzione risolve il problema di LSR su reti con molti router)
- I AS possono comunicare tra loro tramite **Asynchronous Transfer Mode (ATM)**

#### Messaggi OSPF
Esistono 5 messaggi specifici al protocollo OSPF:
- Messaggi **Hello**
	- usati dai router per verificare se un nodo è raggiungibile
- Link State Advertisement (**LSA**)
	- informazione topologica da un router, per esempio LSP
- Link Status Request (**LSR**)
	- richieste inviate ad un altro router per determinare lo stato di uno o più collegamenti
- Link Status Update (**LSU**)
	- risposte a messaggi LSR
- Link Status Acknowledgement
	- usato per indicare che è stato ricevuto un LSU

##### Pacchetti Hello
Quando un router vuole verificare la raggiungibilità di un altro router, manda un messaggio Hello. Se il nodo è raggiungibile, risponderà con lo stesso messaggio Hello.

I pacchetti Hello contengono una lista di router raggiungibili.

##### Pacchetti LSA
Quando richiesti, questi messaggi contengono una lista di collegamenti ad una destinazione (strada completa).
Questi messaggi possono essere usati nel calcolo della strada minima (LSP).

##### Pacchetti LSR
Pacchetto che rappresenta una richiesta di informazioni su uno o più collegamenti.
Un router può fornire questa richiesta se un altro router ha informazioni datate.

##### Pacchetti LSU
Risposta ad un messaggio LSR. Contiene informazioni su ogni collegamento richiesto dal LSR.

#### Routing Multicast
Il protocollo OSPF implementa il multicasting. In caso di multicasting, viene comunque calcolato l'albero Path di Dijkstra ma con alcune differenze:
- La radice rappresenta il mittente, invece che il router corrente
- Il router manda il messaggio ad ogni nodo figlio diretto del router stesso nell'albero Path
