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
- Consegna affidabile (particolarmente utile in collegamenti proni a errore (wireless))
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

