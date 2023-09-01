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

