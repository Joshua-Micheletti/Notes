Elementi del livello rete:
- Protocollo IP
- Indirizzi IP
- Protocolli di controllo Internet
- IPv6

Principi di Internet:
- Assicurati che funzioni
- Keep it simple
- Fai scelte chiare
- Sfrutta la modularità
- Aspettati eterogeneità
- Evita opzioni e parametri statici
- Usa design buoni (anche se non perfetti)
- Sii duro quando invii e tollerante quando ricevi
- Tieni in mente la scalabilità
- Considera performance e costi

# Protocollo IP
![[IP Protocol.png]]

Alcune opzioni IP:
- Sicurezza: specifica quando il datagram è segreto
- Indirizzamento di sorgente stretto: fornisce la strada completa da seguire
- Indirizzamento di sorgente allentato: fornisce una lista di router per cui passare
- Salvataggio di indirizzi: costringe ogni router ad aggiungere il sui IP
- Tempo: costringe ogni router ad aggiungere il suo indirizzo e il tempo al momento del routing

![[IP Address Format.png]]
(Questo tipo di IP non viene più usato, rimpiazzato da CIDR)

Un problema grosso di questo formato è il fatto che viene allocato lo stesso spazio per host e per network, i quali possono essere molto diversi in dimensioni tra loro.

## Indirizzi IP Speciali
- 0.0.0.0: Host corrente, senza specificare l'indirizzo
- 255.255.255.255: Indirizzo per broadcast limitati
- 127.x.y.z: Indirizzo di loopback (i datagram (pacchetti) vengono inviati ai livelli superiori dello stesso host)
- 169.254.x.y: Auto-configurazione host

## Indirizzi IP Privati
- Classe A: 10.0.0.0
- Classe B: 172.16.0.0 (consente 16 reti contigue)
- Classe C: 192.168.0.0 (consente 255 reti contigue)

| Classe A | 10.0.0.0 | a 10.255.255.255 |
| --- | --- | --- |
| Classe B | 172.16.0.0 | a 172.31.255.255 |
| Classe C | 192.168.0.0 | a 10.255.255.255 |

## Sottoreti
Invece di avere 1 rete con 65536 host può essere utile avere 64 reti con 1022 hosts.
Questo si può ottenere assegnando un certo numero di bit dei host per definire la sottorete, ed utilizzare i restanti per definire gli host nella sottorete.

### Maschera
La maschera indica il limite tra host e sottorete.
(255.255.252.0 esempio di maschera (/22 in CIDR)).

## Classless InterDomain Routing
Gli indirizzi IP sono assegnati in base alla generalizzazione degli indirizzi della sottorete.
Gli indirizzi IP sono divisi in due parti: a.b.c.d/x (x specifica il numero di bit del **prefisso**).

Il routing è gerarchico:
- Il router esterno alla rete dell'organizzazione deve solo salvare indirizzi di tipo a.b.c.d/x
- I router interni alla rete si occupano di usare i bit restanti per consegnare pacchetti
- I bit restanti possono anche essere usati per denotare sottoreti

## IPv6
Cambiamenti dal IP(v4):
- Gli indirizzi sono quadruplicati a 16 bytes
- Il formato dell'header viene semplificato:
	- Lunghezza fissa, header opzionali vengono concatenati
	- Gli header IPv6 sono il doppio più lunghi (40 bytes) degli header IPv4 senza opzioni (20 bytes)
- Non viene fatto il controllo checksum al livello IP del livello di rete
- Non avviene la segmentazione hop-by-hop (Path MTU discovery)
- 64 bit allineati
- Abilità di autenticazione e privatezza (IPsec è necessario)
- Non supportano broadcast

Gli indirizzi IPv6 sono di lunghezza fissa di 128-bit

### Differenze tra header IPv4 e IPv6
![[IPv4-IPv6 header.png]]

### Header di estensione
Visto che alcune informazioni sono ancora necessarie negli header IPv6, ma l'area di opzioni è stata rimossa rispetto ad IPv4, si usano header di estensione:

| Header di estensione | Descrizione |
| --- | --- |
| Opzioni Hop-by-Hop | Informazioni generiche per i router |
| Opzioni di destinazione | Informazioni aggiuntive per la destinazione |
| Routing | Lista leggera di router da visitare |
| Frammentazione | Gestione dei frammenti di datagrammi |
| Autenticazione | Verifica dell'identità del mittente |
| Sicurezza del payload codificata | Informazioni sul contenuto codificato |

### Dimensione IPv6
Per accontentare tutti, si decise di definire la dimensione degli indirizzi IPv6 di 128-bit fissi (non variabili). (128 bit = 16 Byte).

I benefici di un indirizzo a 128 bit sono:
- Spazio per molti livelli di gerarchia e aggregazioni di routing
- Facile gestione degli indirizzi e delegazione ad IPv4
- Auto-configurazione degli indirizzi facile
- Abilità di deployare IPsec end-to-end (NAT rimossi in quanto non più necessari)

### Indirizzamento IPv6
Gli indirizzi IPv6 sono definiti da multipli RFC (Request For Comments, documenti di specifica prodotti dalla IETF (Internet Engineering Task Force)).

L'architettura generale è definita in RFC 3513.

I tipi di indirizzi sono:
- Unicast: indirizzo uno ad uno (globale, collegamento locale, sito locale)
- Anycast: indirizzo uno al più vicino (allocato a partire da Unicast)
- Multicast: indirizzo uno a molti
- Inverso

Una singola interfaccia di rete può avere assegnati più indirizzi IPv6 di tipi diversi.

### Rappresentazione indirizzi IPv6
Sono divisi in campi da 16bit in rappresentazione esadecimale:
- 2031:0000:130F:0000:0000:09C0:876A:130B

Il carattere ':' separa i campi (8 campi in totale).
I caratteri non sono case sensitive, e gli zeri ripetuti sono opzionali.

Sequenze di :0000: possono essere rappresentate tramite '::'
- 0:0:0:0:0:0:0:1 => ::1
- 0:0:0:0:0:0:0:0 => ::

Rappresentazione di indirizzi IP compatibili con IPv4:
- 0:0:0:0:0:0:192.168.30.1 => ::192.168.30.1 => ::C0A8:1E01

### Formati indirizzi Unicast
##### Link Local
| FP (10 bits) | Riservati (54 bits) | ID Interfaccia (64 bits) |
| --- | --- | --- |
| 1111111010 | Devono essere 0 | Derivato dal MAC |

##### Site Local
| FP (10 bits) | Sottorete (38 bits) | Sottorete (16 bits) | ID Interfaccia (64 bits) |
| --- | --- | --- | --- |
| 1111111011 | Gestito Localmente | Gestito Localmente | Derivato dal MAC o Gestito Localmente |

##### Globale
| FP (3 bits) | Assegnati dal registro / provider (48 bits) | Sottorete (16 bits) | ID Interfaccia (64 bits) |
| --- | --- | --- | --- |
| 001 | Amministrato dal provider | Amministrato localmente | Derivato dal MAC o Gestito Localmente o Random |

#### Indirizzi Unicast Speciali
Indirizzo utilizzato come prestanome quando non è disponibile un indirizzo vero:
0:0:0:0:0:0:0:0

Indirizzo di Loopback, per inviare pacchetti a se stessi:
0:0:0:0:0:0:0:1
