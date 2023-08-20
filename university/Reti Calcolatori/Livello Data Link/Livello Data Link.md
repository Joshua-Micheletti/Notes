### Definizioni
I messaggi dal livello applicativo sono passati al livello di trasporto.
Il livello di trasporto concatena il messaggio con informazioni aggiuntive (header), creando un segmento di livello di trasporto.
I dati sono passati al livello di rete che aggiunge il suo header e costruisce un network-level datagram (pacchetto a livello di rete).
Il pacchetto viene passato al livello data link che aggiunge un suo header creando un data link frame.

### Funzioni del livello data link
- Fornire un interfaccia al livello di rete
- Gestire errori di trasmissione
- Regolare il flusso di dati (destinatari lenti e mittenti veloci)

Il compito del data link layer è quello di fornire una comunicazione efficiente ed affidabile tra nodi adiacenti (host / routers connessi da un collegamento).

### Servizi offerti dal livello data link
- Framing: struttura di framing specificata dal protocollo
- Accesso al collegamento -> Protocollo di accesso medio (molto semplice o inesistente)
- Consegna affidabile (particolarmente utile in collegamenti proni a errore ([[Wireless]]))
- Controllo di flusso
- Rilevazione di errori (o correzione)

### Framing
Il flusso di bit supportato dal [[Livello Fisico]] è organizzato in frame dal data link layer.
Caratteristiche di Framing:
- Conteggio di caratteri
- Flag byte con byte stuffing
- Flag con bit stuffing
- Violazione della codifica del livello fisico

#### Conteggio dei caratteri
Si aggiunge come primo elemento di ogni frame il numero di caratteri presenti nel frame.
Questo metodo è obsoleto, in quanto se ci fosse un errore nel primo carattere del frame (il contatore), allora tutto il frame sarebbe errato, e un carattere successivo al frame verrebbe identificato come un carattere contatore, erroneamente.

Data stream: (primo numero di ogni frame rappresenta il contatore di caratteri del frame)
|**5** 1 2 3 4| |**5** 6 7 8 9| |**8** 0 1 2 3...
 frame 1    frame 2    frame 3

In caso di errore (5 del frame 2 diventa 7):
|**5** 1 2 3 4| |**7** 6 7 8 9 8 0| |**1**| |**2** 3|...
 frame 1    frame 2       frame 3 frame 4

Un singolo errore su un carattere contatore rovina la struttura di tutti i frame successivi

#### Flag byte con byte/character stuffing
Un byte di flag delimita l'inizio e la fine di un frame.
Nel caso in cui il byte di flag sia contenuto nel payload (informazione del messaggio in binario), si aggiunge in carattere escape (ESC) prima di ogni occorrenza.
Nel caso in cui il carattere ESC è contenuto nel payload, si inserisce un carattere ESC di nuovo.

![[Flag byte with byte stuffing.png]]

Questo metodo spesso viene usato nei protocolli PPP (Point to Point Protocol).
#### Violazione della codifica del livello fisico
La codifica dei caratteri potrebbe non essere sempre a 8 bit.
Si passa ad una codifica arbitraria così da astrarre dalla codifica originale delle informazioni.
Si può solo applicare solo su reti con codifiche di informazioni ridondanti, così da usare alcune informazioni del messaggio come delimitatori.

#### Flag con bit stuffing
- Ogni frame inizia e finisce con la stringa di binari: **01111110** (6 caratteri 1)
- La sorgente inserisce uno 0 dopo ogni sequenza di 5 caratteri 1

In questo modo possiamo sapere l'inizio e la fine di un frame cercando le sottostringhe 01111110, in quanto saranno uniche nello stream.

![[Bit stuffing.png]]
(a): informazione originale
(b): informazione corretta con bit stuffing
(c): informazione ricevuta dopo il destuffing
(in questa immagine non vengono rappresentate le stringe 01111110 ai capi del frame)

### Gestione degli errori
A causa di motivi fisici (voltaggi, rumore, distanze, dispersioni...) ci possono essere degli errori nelle informazioni trasmesse in un canale.

Si può minimizzare la probabilità di errori dei bit usando codifiche di canale adeguate:
consiste principalmente nell'aggiunta di ridondanza nell'informazione che consentono al ricevente di riconoscere un errore o di correggerlo.

Con questo metodo, si usano più bit di quelli necessari per la codifica dell'informazione.

Questo metodo non riduce la probabilità di errori durante la trasmissione, ma consente di gestirli.

##### Tipi di codifiche per la gestione di errori:
- **Codifiche a rilevazione di errore**: consentono al ricevente di accorgersi di un errore e richiedere l'informazione al mittente.
- **Codifiche a correzione di errore**: consentono al ricevente di ricostruire il messaggio corretto a partire da uno corrotto.

#### Forward Error Correction (FEC)
Utilizza la codifica per rilevare errori e correggerli (comodo in situazioni in cui la ritrasmissione di un messaggio non è possibile).

#### Parametri di codici di rilevazione e correzione di errori:
- Capacità di rilevazione: abilità di rivelare il massimo numero di errori in una parola
- Capacità di correzione: numero massimo di errori correggibili in una parola
- Performance:
	- Abilità di rilevare e/o correggere errori.
	- Efficienza: R<sub>c</sub> = k / n
		- k = bit del messaggio
		- n = lunghezza del messaggio codificato
	- Complessità di esecuzione

#### Codici a rilevazione di errori
###### Distanza di Hamming
La distanza di Hamming rappresenta il numero di posizioni in cui si trovano simboli diversi in due stringhe di uguale lunghezza.
Ovvero rappresenta il numero minimo di sostituzioni per passare da una stringa ad un altra (queste sostituzioni potrebbero essere errori).

Nel caso delle stringhe binarie, la distanza di Hamming tra due parole _a_ e _b_ è uguale al numero di 1 presenti nel risultato di _a_ XOR _b_.

###### Hamming cube
Rappresenta la distanza di Hamming tra ogni stringa rappresentabile da un certo numero di bit.

![[Hamming Cube.png]]

Per un codice a rilevazione di errore di s-bit: d >= s + 1
Per un codice a correzione di errore di s-bit: d >= 2s + 1

esempio:
Per un codice a rilevazione di 3 bit di errore, serve una distanza di Hamming di 4.
Per un codice a correzione di 3 bit di errore, serve una distanza di Hamming di 7.

##### Controllo di parità
Il controllo di parità viene fatto aggiungendo al codice un bit scelto a seconda del numero di 1 presenti nel messaggio.
Se si tratta di un codice di **parità** e c'è un numero **dispari** di 1 nel messaggio, si aggiunge un 1.
Se si tratta di un codice di **disparità** e c'è un numero **pari** di 1 nel messaggio, si aggiunge un 1.

Se si tratta di un codice di parità e abbiamo un numero pari di errori, l'errore non viene rilevato.
Opposto nel caso di codice di disparità.

###### Controllo di parità bi-dimensionale
Organizzando i bit del messaggio in una matrice disposta in _i_ righe e _j_ colonne, possiamo calcolare il bit di parità per ogni riga e colonna della matrice. In questo modo è possibile identificare fino a 2 errori nel codice.

##### Somma di controllo
I _d_ bit di dati vengono trattati come se fossero una sequenza di interi a _k_ bit.
Questi numeri interi vengono sommati e viene fatto il complemento a 1 (si invertono i bit).
Questo valore calcolato viene inviato insieme ai dati.

Il ricevente somma tutti gli interi a _k_ bit. Il risultato deve essere sempre 1.

Questa tecnica viene usata nell'internet al livello di trasporto, fornisce una bassa protezione ma è facile e veloce da implementare.

##### Codici ciclici
###### Proprietà:
- La loro struttura matematica consente la costruzione di codifiche per la correzione di più errori.
- La loro struttura è facilmente implementabile in hardware usando shift registers e xor gates.
- I codici ciclici possono essere rappresentati come polinomiali.
- Sono ciclici.

###### Cyclic Redundancy Codes (CRC)
Le stringhe di bit sono viste come polinomiali.
Il trasmettitore e il ricevente si mettono d'accordo su una polinomiale comune **G** (polinomiale generatrice).
Il trasmettitore aggiunge _r_ bit ai _d_ bit di dati per ottenere una stringa binaria _d+r_ (**T**) che è divisibile per G.

Il ricevente controlla il risultato di T/G:
- se il resto è 0: nessun errore
- se il resto è diverso da 0: errore

Nel calcolo delle divisioni polinomiali, si tratta di operazioni a modulo 2, ovvero + e - sono equivalenti a XOR.

**Calcolo di R:**
Vogliamo che R sia: D * 2<sup>r</sup> XOR R = nG.
Di conseguenza:
- D * 2<sup>r</sup> = nG XOR R
- R = resto di (D * 2<sup>r</sup> / G)

**Esempio**
D = 101110 (informazione)
d = 6 (lunghezza dell'informazione)
G = 1001 (polinomiale comune)
r = 3 (bit da aggiungere per la codifica)

![[Cyclic Redundancy Codes.png]]

Tipi di codifiche CRC:
- CRC-32 (usato in [[Ethernet]]):
	- X<sup>32</sup> + X<sup>26</sup> + X<sup>23</sup> + X<sup>16</sup> + X<sup>12</sup> + X<sup>11</sup> + X<sup>10</sup> + X<sup>8</sup> + X<sup>7</sup> + X<sup>5</sup> + X<sup>4</sup> + X<sup>2</sup> + X + 1
- CRC-8
- CRC-10
- CRC-12

###### Caratteristiche CRC
- Se il generatore ha più di un termine e il coefficiente di X<sup>0</sup> è 1, allora tutti gli errori singoli possono essere catturati
- Se il generatore non può dividere X<sup>t</sup> + 1 (con _t_ compreso tra 0 e _n_ - 1), allora ogni errore doppio isolato può essere rilevato.
- Un generatore che contiene un fattore di X + 1 può rilevare ogni numero dispari di errori.
- Un buon generatore polinomiale deve avere le seguenti caratteristiche:
	- Deve avere almeno 2 termini
	- Il coefficiente del termine X<sup>0</sup> deve essere 1
	- Non deve dividere X<sup>t</sup> + 1, con _t_ compreso tra 2 e _n_ - 1
	- Deve contenere il fattore X + 1


### Protocolli elementari del livello data link
La trasmissione affidabile a livello data link è ottenuta tramite:
- ACK (ACKnowledgment)
- Timeout
- ACK + Timeout = ARQ (Automatic Repeat reQuest)

#### Esempi di protocolli
##### Protocollo non ristretto e simplex
- Dati trasmessi in una sola direzione (canale simplex) su un canale privo di errori
- I livelli fisico e network sono sempre pronti
- Tempo e spazio del buffer infinito e sempre disponibile
- Il canale di comunicazione non perde mai frame
Caso molto ideale e difficilmente applicabile

##### Protocollo simplex Stop-and-Wait
- I dati sono trasmessi in una sola direzione (simplex)
- I layer fisico e network non sono sempre pronti
- Canale senza errori
- Il ricevente manda indietro al mittente un frame "dummy" prima che il mittente mandi un altro frame, così da confermare la corretta trasmissione

##### Protocollo simplex Stop-and-Wait per canali rumorosi
Nel caso di errori o problemi di trasmissione delle informazioni, si usa il metodo ARQ, ovvero l'unione di ACK e timeout:

###### ACK
ACK sta per ACKnowledgment, ovvero quando il ricevente riceve un frame, manda un messaggio ACK al mittente, per comunicare la corretta ricezione dei dati.

###### Timeout
Dopo un certo periodo di tempo da quando viene mandato un messaggio dal mittente, se non viene ricevuto nessun messaggio ACK dal destinatario, il mittente considera la comunicazione come fallita, e re-invia il messaggio, per assicurarsi che il ricevente lo riceva correttamente.

###### ARQ (ACK + Timeout)
![[ARQ (ACK + Timeout).png]]
- (a): comunicazione corretta senza errori (ACK trasmesso prima del timeout)
- (b): frame corrotto, non arriva al destinatario
- (c): ACK corrotto, non arriva al mittente
- (d): ACK troppo lento

Bisogna tener traccia del numero corrispondente ad ogni frame, altrimenti potremmo inviare due copie dello stesso frame.

Con questo protocollo, la banda non è ottimizzata, in quanto viene inviato solo 1 frame sul canale.
