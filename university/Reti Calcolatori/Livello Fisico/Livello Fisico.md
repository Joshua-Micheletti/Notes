### Funzioni d'onda
I dati analogici vengono rappresentati da funzioni sinusoidali che possono essere semplici o composte (composte significa che sono costituite dalla composizione di più onde seno).

Un'onda seno può essere rappresentata nel dominio della frequenza come una singola punta, in quanto la frequenza non cambia, ed è sufficiente per definire le caratteristiche dell'onda.
Analizzare le funzioni d'onda nel dominio delle frequenza risulta più utile quando stiamo analizzando più onde contemporaneamente. Altrimenti la rappresentazione nel dominio del tempo diventa problematica.

#### Analisi di Fourier
Ogni segnale composto è la combinazione di onde seno con diverse frequenze, ampiezze e fasi.

Informazioni binarie rappresentate da segnali digitali possono essere approssimate con la composizione di più funzioni seno.

![[Analog signal 1.png]]
![[Analog signal 2.png]]

#### Problemi della trasmissione analogica
I segnali viaggiano attraverso un mezzo fisico, che non è perfetto. Ciò causa un danno nel segnale.
il segnale inviato non corrisponde al segnale ricevuto.
Ci sono 3 cause principali di danni:
- Attenuazione
- Distorsione
- Rumore

##### Attenuazione
Accade quando il segnale perde energia nel trasferimento e l'ampiezza delle onde si riduce.
Per risolvere questo problema, bisogna usare un amplificatore, che fornisce energia all'onda mantenendo le componenti intatte e aumentando l'ampiezza.

![[Attenuation.png]]

##### Distorsione
Avviene quando le componenti di un onda si sfasano durante il trasferimento, causando un segnale distorto dalla parte del ricevente.

![[Distortion.png]]

##### Rumore
Se nel mezzo di trasferimento c'è già un onda (rumore), questa viene aggiunta alle componenti del segnale, trasformandolo in un'onda diversa da quella inviata.

![[Noise.png]]


#### Teorema di Nyquist
Un segnale inviato attraverso un canale con banda H può essere correttamente ricostruito prendendo 2H sample/sec.
Se il segnale ha V livelli discreti:
- max bit rate = 2H log<sub>2</sub>V (bit/sec)
- esempio: un canale a 3KHz senza rumore supporta fino a 6000 bps

#### Teorema di Shannon
Il teorema di Nyquist non prende in considerazione il rumore termico.
S = potenza segnale
N = potenza rumore
S/N = rapporto tra il segnale e il rumore (10 log<sub>10</sub> S/N (dB))
- max bit rate = H log<sub>2</sub> (1 + S/N)  (bit/sec)
- esempio: un canale rumoroso a 3KHz con S/N = 30dB, supporta indipendentemente dal numero dei livelli e dalla frequenza di campionamento, fino a 30000 bps


### Banda
Nelle definizioni di reti, usiamo il concetto di bandwidth in 2 modi:
- Bandwidth in hertz: intervallo di frequenze in un segnale composto o intervallo di frequenze trasferibili da un canale
- Bandwidth in bits per secondo: velocità di trasmissione in un canale o connessione

Il prodotto tra banda e delay definisce il numero di bit che ci stanno in un collegamento.
Possiamo immaginare un collegamento come un tubo, in cui la lunghezza del tubo rappresenta il delay, e la sezione del tubo rappresenta la banda. Il volume del tubo è dato dal prodotto bandwidth * delay

### Trasmissione Dati
Categorie:
- Guidata
	- Mezzi magnetici
	- Coppie incrociate
	- Cavo coassiale
	- Fibra ottica
- Wireless
	- Trasmissione radio
	- Trasmissione microonde
	- Onde infrarossi e millimetriche
	- Trasmissione a onda di luce


#### Guidata
##### Mezzi magnetici (rimovibili)
Non rappresentano esattamente un mezzo di una rete, ma tecnicamente spostare fisicamente un mezzo di memoria da un punto ad un altro consiste nello scambio di informazioni.
Caratteristiche:
- Densità di capacità: 200GB
- 1000 memorie (1600 terabit) possono essere trasportati da un servizio di spedizione in 24h all'interno di una nazione
- Bandwidth = 1600 10<sup>12</sup>/86400 = 19 Gbps
- Costi molto ridotti
- Delay troppo alto per molte applicazioni e generalmente offline

##### Coppie incrociate (cavo ethernet)
Vastamente utilizzati per collegare utenti a reti telefoniche
Bandwidth:
- 16 MHz (cat.3)
- 100 MHz (cat.5)
- 250 MHz (cat.6)
- 500 MHz (cat.6a)

![[Twisted Pair.png]]

##### Cavo Coassiale
Buona schermatura per la propagazione del segnale. Usato molto per televisione cablata.
Per distanze maggiori, viene rimpiazzato da fibra ottica.
Bandwidth: 1 GHz (dipende dal S/N e dalle distanze)

##### Fibra Ottica
La luce viene riflessa all'interno del cavo per trasmettere il segnale.
Indici di rifrazione e angolo di incidenza determinano la rifrazione.
α<sub>c</sub> = angolo critico
Se α > α<sub>c</sub> -> riflessione interna totale

Fibre multimodali: consentono più angoli (raggi), ma aumentano la dispersione su lunghe distanze.
Fibre monomodali: 1 sola possibile propagazione, più costoso ma consente l'uso di laser ed è più adeguato per lunghe distanze.

Si usano intervalli di frequenze nello spettro infrarosso, in quanto hanno caratteristiche particolari (per esempio una bassa attenuazione)

###### Cavo Fibra
Formato da 3 componenti:
- Un rivestimento di plastica esterno
- Un rivestimento di vetro intermedio, con indice di rifrazione minore rispetto al nucleo
- Un nucleo di vetro interno dove si trasmette la luce, il nucleo può essere di diametro:
	- 50 μm (fibre multimodali)
	- 8-10 μm (fibre monomodali)

Il ricevente è un diodo fotosensibile.
La conversione da segnale ottico a elettrico determina i limiti di rate dei dati (un tempo di risposta di 1 ns -> max data rate: 1 Gbps).

Il rumore termico limita l'energia trasferita e la frequenza di errori.

###### Reti di fibre ottiche
Per collegarsi con fibre ottiche servono dei ripetitori (attivi o passivi) che trasformano il segnale da ottico ad elettrico, così da poterci interfacciare un computer, e poi viene ripropagato da elettrico a ottico.

Le topologie classiche di reti ottiche sono ad anello o a stella. La configurazione a stello però divide l'energia tra tutti i canali.

###### Cavo elettrico VS Fibra ottica

| Proprietà | Cavo Elettrico | Fibra Ottica |
| ---------- | --------------- | ------------- |
| Distanza | Corta (100-1000 m) | Lunga (10-100 Km)|
| Bandwidth | Moderata | Molto alta|
| Costo | Economico | Moderato |
| Convenienza | Facile da usare | Meno facile |
| Sicurezza | Facile immettersi | Difficile immettersi |



#### Wireless
L'informazione viene trasmessa tramite onde elettromagnetiche modulando la loro ampiezza, frequenza e fase

Alcune bande di frequenze sono limitate in potenza, per esempio bluetooth (2.4Ghz) e WiFi (5Ghz) non possono superare 1 Watt di potenza.

##### Radio
Le onde radio possono essere di 2 tipi:
- Seguono la curvatura della terra
	- VLF
	- LF
	- MF
- Rimbalzano contro la ionosfera
	- HF
	- VHF

##### Satellitare
La comunicazione satellitare è molto efficace per la distribuzione ampia di informazioni e per comunicazioni indipendenti dalla posizione geografica.

| Banda | Downlink | Uplink | Bandwidth | Problemi |
| --- | --- | --- | --- | --- |
| L | 1.5 GHz | 1.6 GHz | 15 MHz | Banda bassa |
| S | 1.9 GHz | 2.2 GHz | 70 MHz | Banda bassa |
| C | 4.0 GHz | 6.0 GHz | 500 MHz | Interferenza terrestre |
| Ku | 11 GHz | 14 GHz | 500 MHz | Pioggia |
| Ka | 20 GHz | 30 GHz | 3500 MHz | Pioggia e costi |

###### Satelliti GEO
Satelliti geostazionari con un periodo di rivoluzione uguale alla terra (rimangono fermi rispetto alla terra)

Orbitano ad un altitudine di 36000 Km sopra un punto fisso.
Computer VSAT possono comunicare con l'aiuto di un hub.
Tempo di comunicazione up e down è di 250 millisecondi.
Problematici per la comunicazione vocale.

###### Satelliti MEO
Satelliti intermedi che orbitano tra i 5000 e i 20000 Km di altitudine.
###### Satelliti LEO
Satelliti Low-Earth Orbit (LEO) orbitano ad altitudini inferiori (200-2000 Km) e consentono una comunicazione molto più rapida e con molto meno delay.

Le informazioni possono essere trasmesse da satellite a satellite, oppure da satellite a base terrestre.

###### Satellitare VS Fibra
Principali differenze:
- Satellite:
	- Si può impostare una comunicazione in modo rapido ovunque ed in ogni momento (dopo aver lanciato il satellite in orbita)
	- Può comunicare contemporaneamente con regioni molto ampie
	- Banda limitata e molte interferenze da gestire
- Fibra:
	- Banda enorme su lunghe distanze
	- Installazione costosa e difficile
	- Non funziona in aree remote o in mare