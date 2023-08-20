Il [[Livello Data Link]] contiene due sottolivelli:
- Logical Link Control (LLC)
	- Si interfaccia al livello superiore Network
	- Indipendente dalla tecnologia LAN utilizzata nel livello superiore
- Media Access Control (MAC)
	- Si interfaccia al [[Livello Fisico]]
	- Specifico per la tecnologia sottostante

# Problema di allocazione dei canali
Una trasmissione può contenere più trasmittenti e più riceventi che condividono lo stesso canale.
E necessario gestire accessi multipli.

## Protocollo di accesso multiplo
Idealmente il protocollo per un canale di _R_ bit/sec dovrebbe essere:
- Quando un solo nodo sta inviando dati, deve avere R bps di velocità
- Quando M nodi inviano dati contemporaneamente, devono avere una velocità media di _R/M_ bps
- Deve essere decentralizzata (evita problemi in caso di errori da parte del nodo Master)
- Deve essere il più semplice possibile da implementare

# Tassonomia di protocolli MAC
Tipi di protocolli ad accesso multiplo:
- Protocolli ad accesso casuale
	- ALOHA
	- CSMA
	- CSMA/CD
	- CSMA/CA
- Protocolli con accesso controllato
	- Reservation
	- Polling
	- Token parsing
- Protocolli di canalizzazione
	- FDMA
	- TDMA
	- CDMA

## Protocolli ad accesso casuale
Caratteristiche generali dei protocolli di questo tipo:
- Nessuna stazione è superiore ad ogni altra stazione, e a nessuna stazione viene assegnato controllo sopra ogni altra stazione
- Una stazione con un frame da trasmettere può usare il collegamento direttamente basato su una procedura definita dal protocollo, per decidere se mandare il frame o no

#### Ritardo massimo di propagazione
Indica il tempo per la trasmissione di un bit tra i le due stazioni più lontane nella rete

### ALOHA
In questo protocollo primitivo, ogni stazione della rete invia messaggi quando vuole, senza ascoltare il canale per sapere quando è libero.
### CSMA (Carrier Sense Multiple Access)
Ogni protocollo che ascolta il canale per sapere quando può trasmettere un informazione si dice che è un **Carrier Sense Protocol**.

Il concetto principale di CSMA è:
**Se qualcuno sta trasmettendo, aspetta fino a che non ha finito**

#### CSMA non persistente
Una stazione con un frame da mandare ascolta il canale:
- Se il canale è in idle, **trasmetti** il frame.
- Se il canale è occupato, (**indietreggia**) aspetta una quantità casuale di tempo e ripeti il primo passaggio

Stazioni non persistenti sono deferenziali (rispettano gli altri).

Il fatto che il tempo sia casuale riduce la probabilità di collisioni, in quanto le due stazioni che aspettano aspetteranno tempi diversi.

La banda viene sprecata se i tempi di attesa sono lunghi.

#### CSMA 1-persistente
Per evitare tempi morti nel canale, si usano protocolli 1-persistenti:
Una stazione con un frame da trasmettere ascolta il canale:
- Se il canale è in idle, **trasmetti** immediatamente
- Se il canale è occupato, **ascolta continuamente** fino a quando il canale non diventa idle, poi trasmetti immediatamente con probabilità 1

Le stazioni nel protocollo CSMA 1-persistente è egoista (cerca di trasmettere il prima possibile, senza considerare gli altri).

Se due o più stazioni stanno ascoltando il canale che diventa idle, è garantita la presenza di una collisione

#### CSMA P-persistente
Il tempo è diviso in slot dove ogni unità di tempo (slot) generalmente è equivalente al ritardo massimo di propagazione.

Una stazione con un frame da trasmettere ascolta il canale:
- Se il canale è in idle:
	- trasmetti con probabilità **p**, oppure
	- aspetta un unità di tempo (slot) con probabilità **1-p**, successivamente ripeti il primo step
- Se il canale è occupato, **ascolta continuamente** fino a quando il canale non diventa idle

Questo metodo riduce la probabilità di collisioni tanto quanto il protocollo non persistente.
Riduce il tempo idle del canale tanto quanto il protocollo 1-persistente.

#### Differenze tra i 3 protocolli CSMA
![[CSMA Persistent.png]]

### CSMA/CD (Collision Detection)
Questo protocollo si comporta come CSMA ma sa come comportarsi nel caso in cui ci sia una collisione tra stazioni che trasmettono contemporaneamente:
**Se qualcuno inizia a trasmettere con te, ferma la tua trasmissione e riprova più tardi**

Questo accorgimento consente di risparmiare tempo e banda in caso di collisioni.

Protocollo molto usato in [[Ethernet]].

Il tempo di vulnerabilità di CSMA corrisponde al ritardo massimo di propagazione, quindi più è grande il ritardo di propagazione, peggiori saranno le performance di questo protocollo.

Le collisioni vengono identificate da parte da un nodo considerando la potenza del segnale:
Se la potenza osservata è maggiore della somma tra la potenza trasmessa e gli effetti di attenuazione, allora c'è stata una collisione.

Quando viene identificata una collisione, la stazione che identifica la collisione deve:
- fermare la trasmissione
- trasmettere un segnale di **ingorgo (48 bit)** per notificare alle altre stazioni della collisione, così che ogni stazione cancelli il frame trasmesso, e così da lasciare il segnale di ingorgo nel canale fino al ricevimento dalla stazione più lontana
- dopo aver mandato il segnale di ingorgo, indietreggia (aspetta una quantità casuale di tempo)
- alla fine, invia il frame di nuovo

Nel caso peggiore, il tempo di identificazione di una collisione corrisponde al doppio del ritardo massimo di propagazione nel mezzo.

#### Restrizioni:
Il tempo di trasmissione deve essere almeno lungo quanto il tempo necessario ad identificare una collisione (2 * ritardo massimo di propagazione + tempo di trasmissione della sequenza di ingorgo)

Altrimenti il CSMA/CD non ha alcun vantaggio sul CSMA

#### Algoritmo di indietreggiamento esponenziale
L'Ethernet usa l'algoritmo di indietreggiamento esponenziale per determinare la migliore durata del tempo casuale di attesa dopo una collisione.

- Imposta lo **tempo di slot** uguale a 2 * ritardo massimo di propagazione + tempo di trasmissione della sequenza di ingorgo (= 51.2 usec per Ethernet 10-Mbps LAN)
- Dopo la K<sup>th</sup> collisione, selezione un numero random (*R*) compreso tra 0 e 2<sup>k</sup> - 1 e aspetta per un tempo uguale a R * tempo di slot e ritrasmetti quando il mezzo è in idle
	- per esempio per la prima collisione (K = 1), scegli un numero R compreso tra 0 e 2<sup>1</sup> - 1 = {0, 1} e aspetta un tempo uguale a R * tempo di slot (aspetta un periodo tra 0 e 51.2 usec) e ritrasmetti quando il canale è in idle.
- Non aumentare l'intervallo di numeri se K = 10
	- Intervallo massimo: {0, 1023}
- Arrenditi dopo 16 tentativi senza successo e riferisci il fallimento ai livelli superiori

Questo algoritmo riduce la probabilità che due stazioni in attesa usino lo stesso tempo di attesa casuale.
Quando il traffico della rete è leggero, risulta in un tempo di attesa minimo prima di una ritrasmissione.
Con l'aumentare della congestione della rete, le collisioni aumentano, e le stazioni indietreggiano per tempi maggiori per ridurre le probabilità di collisioni.
Questo algoritmo da un effetto last-in, first-out:
- Stazioni con nessun o poche collisioni avranno la possibilità di trasmettere prima di stazioni che aspettano più tempo a causa delle trasmissioni fallite in precedenza

Il throughput in CSMA-CD dipende dal metodo di persistenza utilizzato: (G è il numero medio di frame generati per slot di tempo)
- 1-persistente ottiene un throughput massimo per G = 1 (circa 50%)
- non persistente ottiene un throughput massimo per 3 < G < 8 (circa 90%)

### Performance di protocolli ad accesso casuale
- semplici e facili da implementare
- decentralizzati
- in situazioni di poco traffico, il trasferimento di pacchetti ha un ritardo basso
- il throughput è limitato con traffico alto, e il ritardo di trasmissione di pacchetti non ha un limite
- in alcuni casi, una stazione potrebbe non avere mai una opportunità di trasferire un pacchetto (protocollo ingiusto)
- un nodo con frame da trasmettere può occupare il rate intero del canale se è l'unico nodo nella rete con frame da inviare
- se M nodi vogliono trasmettere, possono esserci molte collisioni, quindi il rate di trasmissione non raggiunge R/M (come sarebbe ideale)
