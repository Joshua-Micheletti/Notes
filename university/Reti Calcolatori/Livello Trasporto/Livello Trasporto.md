Il livello di trasporto deve fornire un servizio efficace, affidabile ed efficiente ai sui utenti, che sono i processi provenienti dal [[Livello Applicativo]].
Utilizza i servizi forniti dal [[Livello Rete]] ma il suo proprio codice viene eseguito interamente sull'host dell'utente.

Questo livello fornisce un canale di comunicazione logico di tipo end-to-end tra processi applicativi su diversi host.

La struttura sottostante rimane visibile ai processi applicativi, come se fossero collegati direttamente nonostante si possano trovare molto distanti tra loro.

Questo livello serve principalmente per il **trasporto di dati** da un host all'altro astraendo dall'architettura della rete o delle interfacce sottostanti, e fornendo una comunicazione efficace per il livello superiore (applicativo).

## Servizi forniti ai livelli superiori
Il software / hardware che implementa le funzionalità del livello di trasporto si chiama **Transport Entity** e le informazioni scambiate si chiamano **Transport Protocol Data Unit (TPDU)**.

Generalmente gli utenti possono interfacciarsi al livello di trasporto tramite le **Primitivi di Servizi di Trasporto**, spesso accessibili come funzioni di **API**.

Queste funzioni consentono al processo dell'applicazione di stabilire, usare e rilasciare connessioni.

![[Transport Primitives.png]]

I pacchetti nel livello di trasporto sono rappresentati dai TPDU, che sono formati da un **header** e da un **payload**.

Alcune primitive per l'utilizzo di socket:
![[Socket Primitives.png]]