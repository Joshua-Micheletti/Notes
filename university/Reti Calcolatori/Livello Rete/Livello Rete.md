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
