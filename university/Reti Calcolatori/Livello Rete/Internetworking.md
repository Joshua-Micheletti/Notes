In Internet esistono diversi tipi di indirizzi:
- Media Access Control (MAC) nel [[Livello Data Link]]
	- Associato alle schede di interfaccia di rete (NIC)
	- 48 o 64 bits
- Indirizzi IP per il [[Livello Rete]]
	- 32 bits per IPv4 e 128 bits per IPv6
- Indirizzi IP + porte per il [[Livello Trasporto]]
	- es. 123.4.56.7:80
- Nomi di domini per il [[Livello Applicativo]] / livello umano
	- es. www.google.com

Gli indirizzi IP e MAC lavorano insieme, in quanto l'indirizzo IP definisce la rete logica, mentre l'indirizzo MAC definisce l'indirizzo fisico del dispositivo.
I due indirizzi sono totalmente scollegati tra di loro, come definito dai principi di separazione di competenze dei livelli ISO/OSI.

# Traduzione di indirizzi
Le traduzioni di indirizzi possono avvenire tra indirizzi IP e MAC:
- Address Resolution Protocol (ARP) per IPv4
- Neighbor Discovery Protocol (NDP) per IPv6

Oppure tra indirizzi IP e nomi di domini (Domain Name System (DNS)).

## Address Resolution Protocol (ARP)
Generalmente viene considerato una parte del livello data link.
Il livello data link contiene indirizzi Ethernet a 6 byte mentre il livello di rete contiene indirizzi IP a 4 byte.

Generalmente viene usato per tradurre indirizzi IP ad indirizzi Ethernet MAC (il driver del dispositivo che usa la NIC Ethernet deve fare la traduzione per mandare un pacchetto).

Questo protocollo viene usato anche per la traduzione di indirizzi IP a tecnologie LAN diverse (802.11 wifi per esempio).

### Integrazione con Ethernet
La tecnologia Ethernet è definita nel [[Livello Fisico]] e nella sottoparte del Livello Data Link, nel protocollo IEEE 802.3.

Il pacchetto ARP (tradotto) viene incapsulato all'interno di un pacchetto Ethernet nel campo "Data".
Per indicare la presenza del protocollo ARP, si indica nel campo "Tipo" il valore 0x0806, che rappresenta comunicazione Ethernet.

### RARP
Se ARP si usa per tradurre indirizzi logici (IP) in indirizzi fisici (MAC), il procedimento inverso viene svolto dal protocollo RARP, ovvero si passa da un indirizzo fisico ad un indirizzo logico.

### Utilizzo di ARP
Immaginando di voler inviare un pacchetto attraverso Ethernet ad un altro dispositivo, di cui conosciamo solo l'indirizzo IP.
Il protocollo ARP consente di ottenere l'indirizzo fisico a partire dall'IP.

**Richiesta ARP**
Il procedimento per la traduzione consiste nell'invio da parte del mittente di un messaggio di richiesta dell'indirizzo fisico a partire da un indirizzo logico. Questo messaggio viene inviato in broadcast a tutti i computer della rete.

**Risposta ARP**
Quando un computer nella rete conosce la risposta, invia un messaggio in unicast al computer mittente con l'indirizzo MAC corrispondente. (generalmente l'host che risponde è colui che riconosce il proprio indirizzo IP indicato nella richiesta ARP e condivide il proprio indirizzo MAC).

Mandare un messaggio di richiesta ed aspettare una risposta ogni volta che si vuole inviare un pacchetto è molto inefficiente, richiede maggiore banda e consuma tempo.
Per evitare questa procedura ad ogni pacchetto, si usa una cache ARP che tiene in memoria l'associazione tra indirizzi IP e indirizzi MAC.
Questa cache ha una dimensione massima di 512 associazioni ed è presente in ogni nodo della rete.

Nel caso in cui un host cambi indirizzo IP, si invaliderebbe il valore salvato nella tavola di cache ARP. Per ovviare a questo problema, i valori salvati nella cache ARP vengono dimenticati dopo un certo tempo (circa 20 minuti).

### Formato pacchetti ARP
![[ARP Packet Format.png]]

### ARP Proxy
I messaggi ARP possono essere comunicati anche attraverso reti, e possono provenire da computer remoti. In quel caso, è il router che si occupa di gestire la richiesta ARP su IP di una rete esterna, e risponde personalmente con l'indirizzo fisico corrispondente

### Comandi per gestione di informazioni ARP
Per mostrare la tavola di cache ARP: **arp -a**
Per inserire un valore nella tavola: **arp -s \<IP> \<MAC>**
Per eliminare un valore nella tavola: **arp -d \<IP>**

### ARP con ponti
Nel caso nella rete siano presenti dei ponti ([[Dispositivi di Rete]]), la richiesta ARP viene mandata al ponte (in caso si sta cercando l'indirizzo di un dispositivo in un altra rete). Il ponte risponde al posto dell'host di destinazione (ARP proxy), però invece di rispondere con l'indirizzo di destinazione, risponde con il proprio indirizzo fisico.

Quando successivamente l'host mittente invia il messaggio con IP dell'host di destinazione e indirizzo MAC del bridge, il bridge capta questo messaggio e lo invia all'host di destinazione.

Questo processo risulta invisibile agli host, che pensano di essere collegati alla stessa rete (bridging trasparente).

Nel caso però in cui un host di una rete cerca di comunicare con 2 host di una rete collegata con bridge, risulterà che i due indirizzi IP sono collegati allo stesso indirizzo MAC (in quanto il bridge risponde sempre con il proprio indirizzo MAC, piuttosto che con quello del destinatario).
Questo problema viene risolto in IEEE 802.1d con lo standard per Ethernet Bridging.

### ARP Spoofing (ARP Poisoning)
L'idea è quella di mandare messaggio ARP falsi in una LAN Ethernet. In questo modo il resto degli host nella rete associano l'IP di altri host con l'indirizzo MAC dell'attaccante.

Alcune difese contro questo attacco possono essere:
- Tavole ARP statiche
- Snooping DHCP (la rete si assicura che gli host possano rispondere ai messaggi solo con il proprio indirizzo IP)
- Identificazione: Arpwatch (quando avviene un aggiornamento, notifica l'utente (email)).

Questo metodo può essere usato legittimamente, per esempio per ridirezionare un utente alla pagina di registrazione prima di poter usare la rete.


## Internet Control Message Protocol (ICMP)
Definito nel RFC 792 (IETF credo).

L'obiettivo di messaggi di controllo è quello di fornire un feedback riguardo problemi nell'ambiente della comunicazione.
Generalmente i messaggi ICMP riportano errori nel processo di parsing dei datagram.

Alcuni usi per ICMP:
- Un datagram non può raggiungere la destinazione
- Quando il gateway non ha la capacità di buffering per inviare un datagram
- Quando il gateway può ridirezionare l'host per mandare il traffico in una strada più corta

### Formato messaggi ICMP
| Header IPv4 | Dimensione | Descrizione |
| --- | --- | --- |
| Tipo | 8 bytes | Tipo di servizio fornito, esiste un numero per ogni errore o informazione |
| Codice | 8 bytes | Codice di errore, può anche indicare informazioni sull'errore |
| Checksum ICMP | 16 bytes | Complemento a uno del messaggio ICMP, si usa solo per problemi ICMP |
| Parametri | variabile | Usato in messaggi specifici per scambiare dati |
| Informazioni aggiuntive | variabile | Usato in messaggi di errore, include le parti del datagram con errori |

![[ICMP Types.png]]

#### Destination Unreachable (Tipo 3)
In questo messaggio ICMP, il tipo viene impostato a 3, il codice dipende dal tipo di errore, i parametri vengono lasciati vuoti e le informazioni aggiuntive vengono riempite con l'header Internet + i primi 64 bit del campo dati del datagram originale.

Nel caso di un messaggio ICMP di tipo 3 (destination unreachable), il codice ci indica il motivo per cui non è possibile raggiungere la destinazione:
- 0: rete irraggiungibile
- 1: host irraggiungibile
- 2: protocollo irraggiungibile
- 3: porta irraggiungibile
- 4: frammentazione necessaria e il flag DF (don't fragment) è stato impostato
- 5: fallimento nella strada sorgente

Esempi di uso di questo messaggio:
- Quando secondo le tavole di routing del gateway, la rete specificata risulta irraggiungibile (codice 0)
- Quando il modulo IP non può consegnare il datagram, perchè il protocollo o la porta indicata non sono attivi (codice 2-3)
- Quando il datagram deve essere frammentato per essere inviato dal gateway e il flag DF (don't fragment) è attivo (codice 4)

#### Source Quench (tipo 4)
Parametri vuoti e informazioni aggiuntive contengono (come nel tipo 3) l'header Internet + i primi 64 bit del campo data del datagram originale.

Il codice viene impostato a 0, ma non fornisce informazioni particolari.

Questo messaggio viene usato quando:
- un gateway non ha spazio nel buffer necessario per mettere in coda dei datagram in uscita per la prossima rete nella strada

#### Redirect Message (tipo 5)
Nel campo dei parametri viene inserito l'indirizzo Internet del gateway di destinazione.
Informazioni aggiuntive compilate come nei precedenti 2 tipi.

Il codice può indicare:
- 0: ridirigi il datagram per la rete
- 1: ridirigi il datagram per l'host
- 2: ridirigi il datagram per il tipo di servizio e rete
- 3: ridirigi il datagram per il tipo di servizio e host

Un esempio di uso di questo messaggio può essere, per esempio, quando un gateway, dopo aver controllato la sua tabella di routing, scopre che esiste una strada più corta per raggiungere l'host di destinazione.

#### Echo Request / Echo Reply (tipi 0-8)
Il tipo di questo messaggio può essere di tipo 8, e rappresenta un messaggio di risposta echo, altrimenti di tipo 0, e rappresenta un messaggio di risposta echo.

Il campo di codice viene impostato a 0 senza particolari significati attribuiti.

Il campo parametri viene riempito con un identificatore e una sequenza di numeri. Questi parametri servono per aiutare la sincronizzazione degli echo, per esempio incrementando la sequenza di numeri per tenere traccia delle risposte echo.

Il campo di informazioni aggiuntive viene compilato come nei precedenti 3 casi.

Questo tipo di messaggio viene utilizzato per esempio quando un host o gateway vuole verificare la raggiungibilità di una comunicazione con un host (comando ping).

I messaggi echo consistono in una richiesta echo indicando l'indirizzo del mittente e l'indirizzo del destinatario, un identificatore "5350" ed un numero di sequenza "40".
La risposta echo consiste nello stesso messaggio (stesso identificatore e numero di sequenza) ma con gli indirizzi di mittente e destinatario invertiti.

#### Time Exceeded (Tipo 11)
Messaggio con parametro vuoto e informazioni aggiuntive come nei casi precedenti.

Il codice indica il tipo di errore:
- 0: tempo di vita superato durante il transito
- 1: riassemblamento dei frammenti supera il tempo di vita

Alcuni esempi di utilizzo di questo messaggio sono:
- Quando il gataway che sta processando un datagram scopre che il campo Time-To-Live è 0 (codice 0)
- Quando un host ricomponendo un frammento di un datagram non può completare la ricomposizione in quanto mancano dei frammenti prima del terminare del tempo di vita (codice 1)

#### Parameter Problem (Tipo 12)
Messaggio con tipo 12, codice impostato a 0, informazioni aggiuntive come nei casi precedenti e con un puntatore al bite dove è stato incontrato un errore nel parametro.

Un esempio di uso di questo messaggio è quando un gateway o host trova un problema con l'header di parametri e non può completare il processamento del datagram.

#### Timestamp / Timestamp Reply (Tipi 13-14)
Messaggio con tipi 13 o 14 (richiesta e risposta rispettivamente).

Il timestamp come dato è un numero a 32bit di millisecondi dalla mezzanotte.

#### Information Request / Reply (Tipi 15-16)
Messaggio con tipi 15 o 16 (richiesta e risposta rispettivamente).

Messaggio utilizzato per richiedere informazioni sulla rete di appartenenza (indirizzi principalmente).

### ICMP v6
Specifica di ICMP per IPv6 (piuttosto che IPv4).
Principalmente simile a ICMP per IPv4 ma con alcune modifiche:
- Contiene uno pseudo-header nella computazione di checksum (in quanto alcune parti dell'IPv6 non sono coperte da checksum a [Livello Internet]).
- ICMDv6 deve essere totalmente implementato da ogni nodo che utilizza indirizzi IPv6
- Per indicare la versione di ICMP, si usa il valore 58 nell'header del messaggio ICMP (58 = ICMPv6)

I messaggi ICMPv6 sono raggruppati in due classi:
- messaggi di errore
- messaggi informativi

I messaggi di errore iniziano sempre per 0 nel bit più significativo del campo tipo (quindi i messaggi di errore vanno da 0 a 127 e i messaggi informativi vanno da 128 a 255)

**I messaggi ICMPv6 devono determinare l'IPv6 di sorgente e destinatario nell'header prima di calcolare il checksum**

Nel caso in cui il nodo abbia più di un indirizzo unicast, deve scegliere l'indirizzo in base a:
- Se il messaggio è una risposta ad una richiesta proveniente da un indirizzo unicast del nodo, allora l'indirizzo di destinazione sarà sempre **lo stesso indirizzo della richiesta**
- Se il messaggio è una risposta ad una richiesta proveniente da un nodo in multicast o anycast, l'indirizzo di destinazione **sarà un indirizzo unicast appartenente all'interfaccia del mittente multi/anycast**
- Se il messaggio è una risposta ad una richiesta proveniente da un indirizzo che non appartiene al nodo, l'indirizzo di destinazione **sarà un indirizzo unicast di un nodo che meglio può aiutare a diagnosticare e risolvere il problema**
- Altrimenti bisogna consultare la tavola di routing del nodo per determinare l'interfaccia di trasmissione del messaggio e scegliere un indirizzo disponibile nell'interfaccia.

#### Regola di processamento dei messaggi ICMPv6
Le implementazioni di ICMPv6 devono seguire le seguenti regole nel processamento dei messaggi:
- Se un messaggio ICMPv6 viene ricevuto con un codice di errore sconosciuto, deve essere passato al livello superiore
- Se un messaggio informazionale viene ricevuto con un tipo sconosciuto, deve essere scartato
- Ogni messaggio di errore deve contenere tante informazioni quante ci stanno riguardo al pacchetto che ha causato l'errore.
- Quando un messaggio di errore viene passato al livello superiore, viene estratto il tipo di errore per selezionare il processo nel livello superiore adeguato per risolvere il problema
- Un messaggio di errore NON deve essere inviato in seguito alla ricevuta di:
	- un messaggio di errore
	- un pacchetto destinato ad un indirizzo IPv6 multicast
	- un pacchetto inviato nel [Livello Data Link] multicast
	- un pacchetto inviato nel [Livello Data Link] broadcast
	- un pacchetto il cui indirizzo di destinazione non indica necessariamente un solo nodo
- Un nodo IPv6 deve limitare il numero di messaggi di errore che possono essere inviati nel tempo (per non saturare la banda di errori)

#### Formato messaggi ICMPv6
![[ICMPv6 types.png]]

##### Destination Unreachable (Tipo 1)
Utilizzato in cui non si riesca a raggiungere l'indirizzo di destinazione.

Codice di errore integrato:
- 0: nessuna strada per la destinazione
- 1: comunicazione con la destinazione proibita amministrativamente
- 2: vuoto
- 3: indirizzo irraggiungibile
- 4: porta irraggiungibile

##### Packet Too Big (Tipo 2)
Messaggio più grosso del MTU.
Nel messaggio di errore viene indicato l'MTU al mittente per correggere il problema.

##### Time Exceeded (Tipo 3)
Quando si riceve un pacchetto con Hop Limit impostato a 0 (pacchetto si è mosso troppo e non può più muoversi (morto)).

Codice di errore integrato:
- 0: superato il Hop Limit durante il transito
- 1: superato il tempo di reassemblamento del fragment

##### Parameter Problem (Tipo 4)
Problemi negli header o campi di un pacchetto.
Il messaggio di errore include un puntatore offset per indicare il parametro errato che causa il problema.

Codice di errore integrato:
- 0: campo header errato
- 1: tipo Next Header sconosciuto
- 2: opzione IPv6 sconosciuta

##### Echo Request (Tipo 128)
Utilizzato per determinare la possibile comunicazione tra host e node.
Messaggio informativo.

##### Echo Reply (Tipo 129)
Risposta ad un echo request.

## Dynamic Host Configuration Protocol (DHCP)
Protocollo per il gestore di una rete per l'assegnazione di indirizzi IP ai nodi della rete.
Definito in RFC 1531 ma poi reso obsoleto da RFC successivi.

DHCP è un estensione del protocollo Bootstrap (BOOTP): un protocollo nato per la pre configurazione manuale delle informazioni di host in un server database.

DHCP server per inviare parametri di configurazione da un server DHCP ad un host.

Il DHCP è un protocollo a [Livello Applicativo] nel modello TCP/IP e supporta 3 meccanismi di allocazione di indirizzi IP:
- allocazione automatica
- allocazione dinamica
- allocazione manuale

Il protocollo DHCP è costituito da 3 componenti:
- Server DHCP (fornisce le informazioni)
- Client DHCP (riceve le informazioni)
- Relay DHCP/BOOTP (riparte le informazioni ai client)

DHCP deve essere sviluppato in maniera che sia un meccanismo che non richiede nessuna configurazione da parte del Client, che non richiede server in sottoreti e deve essere compatibile con relay BOOT e fornire servizi anche ai client BOOTP.

Il protocollo **deve**:
- garantire indirizzi di rete **unici**
- mantenere le configurazioni DHCP dei client anche dopo un reboot del client
- consentire la configurazione automatica per nuovi client
- supportare allocazioni fisse per client specifici (riservare l'indirizzo IP per un certo client)

### Messaggi DHCP
![[DHCP message.png]]

#### DHCPDISCOVER
Messaggio broadcast usato da client per localizzare server disponibili

#### DHCPOFFER
Messaggio da parte del server verso il client in risposta a DHCPDISCOVER, vengono offerti dei parametri di configurazione.

#### DHCPREQUEST
Messaggio dai client al server per:
- Accettare parametri offerti da un server (e declinare quelli offerti da altri server)
- Confermare la correttezza dei parametri allocati in seguito ad un reboot
- Rinnova le impostazioni DHCP scadute

#### DHCPACK
Messaggio server a client con parametri di configurazione, inclusi gli indirizzi dedicati.

#### DHCPNAK
Messaggio server a client per notificare il client di un indirizzo errato (a causa di un movimento ad una sottorete o a causa delle informazioni scadute).

#### DHCPDECLINE
Messaggio di declino dal client al server, indicando che il client è già impostato correttamente

#### DHCPRELEASE
Messaggio da client a server chiedendo di ritirare le informazioni di configurazione e il termine del prestito dell'indirizzo IP

#### DHCPINFORM
Messaggio da client a server per ottenere le informazioni di configurazione interna (configurazione esterna già avvenuta).

#### Procedura di configurazione di un client tramite DHCP
![[DHCP Configuration.png]]

- Client broadcast DHCPDISCOVER
- Servers risponde con DHCPOFFER
- Server controlla gli indirizzi
- Client broadcast DHCPREQUEST
- Il server selezionato risponde con DHCPACK
- Client invia DHCPRELEASE quando si spegne

I server e client DHCP contengono informazioni sul proprio stato per determinare il comportamento in seguito ad ogni messaggio in fase di configurazione.

Il server tiene anche traccia del tempo di esaurimento di un indirizzo IP concesso, ed il client richiede una rinnovazione (DHCPREQUEST).

Per ovviare i problemi di performance, i server DHCP devono selezionare tempi di prestito di indirizzi in modo appropriato:
- aumenta il tempo di prestito di un indirizzo IP per reti larghe e fisse
- riduci il tempo di prestito di un indirizzo IP per reti piccole e variabili
- riserva indirizzi specifici
- integra il protocollo DHCP con altri servizi

## Network Address Translation (NAT)
Definito in RFC 1631, consiste in una soluzione a breve termine per ovviare la mancanza di indirizzi IP (l'alternativa a lungo termine è IPv6).

NAT consente di conservare indirizzi IP nascondendo multipli host in una rete privata, così da utilizzare un solo indirizzo IP.

Rappresenta una funzionalità del router dove l'indirizzo IP di datagrammi IP sono rimpiazzati all'interno di una rete privata. Ciò consente a host all'interno di una rete privata di comunicare con host tramite Internet.

I dispositivi NAT contengono una tavola di indirizzi per tradurre indirizzi Internet in indirizzi privati.

### Pooling
Avviene quando si ha una rete privata con molti host ma con pochi indirizzi IP pubblici assegnati (meno del numero di host).
Quando un host nella rete privata vuole comunicare con Internet, l'indirizzo di sorgente viene rimpiazzato con uno degli indirizzi pubblici disponibili e spedito verso un host remoto.

Questa soluzione rende facile anche il trasferimento ad un altro Internet Service Provider:
nel momento in cui cambiano gli indirizzi pubblici disponibili, solo c'è bisogno di aggiornare la tavola NAT.

### IP Masquerading
Conosciuto anche come **Network Address and Port Translation (NAPT)** o **Port Address Translation (PAT)**.

Consiste nel mappare porte con indirizzi specifici, così da poter comunicare con host all'interno della rete privata in modo specifico.

| Indirizzo Privato | Indirizzo Pubblico |
| --- | --- |
| 10.0.1.2/2001 | 128.143.71.21/2100 |
| 10.0.1.3/3020 | 128.143.71.21/4444 |

Vengono mappate le porte interne a due indirizzi privati diversi, così da poterci comunicare tramite l'uso di porte esterne.

### Bilanciamento del carico tra server
Per bilanciare il carico tra più server in una rete privata ma con un solo indirizzo pubblico, il NAT si mette in mezzo come proxy per intercettare le richieste provenienti dalla rete pubblica. Il NAT smista le richieste tra gli indirizzi privati disponibili con metodo round-robin

