## Requisito
ACDA ha sostituito la versione del SIAS risalente a più di 10 anni fa con Geo4B Water e sta richiedendo un Tool di Plotting che superi gli attuali limiti del Tool di Stampa di Geo4B e che sia il più possibile simile a quello del SIAS.

Uno strumento di plotting identico a quello del SIAS non è implementabile per cui quello che si propone di fare è uno strumento avente le seguenti caratteristiche:

1. Utilizzi il servizio di stampa in PDF del GSS che sulla base di opportuni parametri produca un PDF vettoriale con il livello di risoluzione adatto alle esigenze del cliente
    
2. Sia predisposta una interfaccia utente di configurtazione del Plot che permetta all’utente di:
    
    1. Selezionare il Template di Stampa tra quelli costruiti mediante lo strumento Layout Designer del Client Desktop e memorizzati nel repository condiviso dal Client WEB. In fase di HLD verrà definito un numero massimo di template di stampa da configurare e anche una composizione degli stessi che contenga elementi standard e non sia troppo complessa.
        
    2. Selezionare l’area di mappa da stampare. L’area di stampa sarà impostata dall’utente con una funzionalità di drag sulla mappa. Da valutare in fase di HLD se sarà possibile vincolare il drag alla forma della viewport del template (quindi permettere il disegno di un area che abbia la stessa ratio), oppure se adattare in post-elaborazione l’area definita sulla mappa alla vieport stessa
        
    3. I layer da stampare tra quelli al momento selezionati come visibili su mappa
        
    4. Altri eventuali parametri (Titolo, Logo, ecc.) che dovranno essere concordati e che dovranno comunque trovare esatta corrispondenza nei template costruiti
        
3. Al momento non è previsto che il sistema produca una Anteprima di Stampa perché i servizi GSS non lo permettono
    
4. La stampa chiamerà il servizio GSS con i parametri precedentemente impostati, avviserà l’utente al completamento della stessa, depositerà il PDF prodotto in un repository opportunamente predisposto in modo da permettere all’utente stesso il download del file
    

**Limiti:**

1. Non sarà possibile stampare elementi che non fanno parte dei dataset di Smallworld (es. Layer aggiuntivi da servizi o da file)
    
2. Non sarà possibile stampare elementi dei dataset di Smallworld se non opportunamente configurati nei layer visibili da Geo4B WEB
    
3. Non sarà possibile stampare basi cartografiche da servizi di terzi, quali ad esempio le mappe di Google
    
4. Non sarà possibile impostare alcun parametro che permetta di stampare elementi che non siano visibili in mappa al momento della stampa stessa
    

**Riferimenti:** A seguire alcuni link alla documentazione di GE in cui è descritto come configurare il servizio di Plotting da WEB

1. Plotting Configuration → [https://smallworld.gedigitalenergy.com/documentation/sw53/en/swDocs5.htm#../Subsystems/WebCommon/Content/Config/PlottingConfig.htm?Highlight=plotting configuration](https://smallworld.gedigitalenergy.com/documentation/sw53/en/swDocs5.htm#../Subsystems/WebCommon/Content/Config/PlottingConfig.htm?Highlight=plotting%20configuration "https://smallworld.gedigitalenergy.com/documentation/sw53/en/swDocs5.htm#../Subsystems/WebCommon/Content/Config/PlottingConfig.htm?Highlight=plotting%20configuration")
## Analisi Requisito
Il requisito può essere scomposto in N componenti principali:
- Rendering delle immagini tramite engine GSS
- Parsing dei template ed estrazione delle informazioni importanti (dati da mostrare in output, viewport)
- Selezione dell'area di stampa tramite uno strumento di disegno di rettangoli
- Creazione del PDF secondo le informazioni del layout e il risultato del rendering delle immagini
- Interfaccia per la selezione di layer attivi, layout e area di stampa

### Rendering delle Immagini
Serve una analisi delle capacità dell'API per la creazione di immagini da parte del GSS in modo da capire come poter strutturare le chiamate e capire quali possono essere le limitazioni a livello di risoluzione, DPI, stili, dimensione del testo e layer selezionabili.

Il modo in cui questo engine accetta le coordinate dei tile da renderizzare avrà conseguenze anche su come viene implementato lo strumento di selezione dell'area di stampa.

Controllare se richiede un proxy e in caso, si può inserire dentro il portale-saas-service come api_plotting.

### Parsing dei Template
I template sono dei file XML che contengono alcune informazioni relative al formato di stampa del PDF finale. Alcune di queste informazioni sono alcuni campi parametrizzabili con dei valori predefiniti o arbitrari (titolo, descrizione etc...) e il viewport della mappa nel PDF, informazione importante sia per la creazione del PDF che per la creazione dello strumento di selezione dell'area di stampa (in quanto dovrà mantenere lo stesso aspect-ratio della viewport del template).

I parametri disponibili nei template devono essere un numero finito e predefinito. La lista di template disponibili è fissa al momento e tecnicamente composta solo da elementi elementari. Questo rende sia il parsing che la rappresentazione dei campi dinamici più semplice e specifica.

I template disponibili sono presenti in una repository condivisa con il cliente, bisogna verificare se i dati sono accessibili direttamente dalla pagina web o se serve un proxy per recuperarli. In quel caso il backend si potrebbe sviluppare nel portale-saas-service come api_plotting.

### Selezione dell'Area di Stampa
La selezione dell'area di stampa avviene tramite disegno su mappa, OpenLayers contiene delle funzionalità per il disegno di rettangoli su mappa.

Questi rettangoli devono però rispettare l'aspect ratio della viewport del template. Ci sono due opzioni per implementare questa cosa:
- Limitare le dimensioni del rettangolo in fase di disegno in modo che rispetti l'aspect ratio della viewport (richiede un analisi di fattibilità). Soluzione preferibile
- Non limitare le dimensioni del disegno ma poi estrapolare il rettangolo inscritto al rettangolo disegnato che rispetta l'aspect ratio della viewport definita nel template

In caso di cambiamento di template dopo la selezione dell'area di stampa, bisogna decidere se:
- Far ri-disegnare l'area di stampa al cambiamento della viewport
- Adattare l'area di stampa con il nuovo viewport, mantenendo un area più o meno simile a quella originale (mantiene la dimensione dell'area di stampa uguale) oppure calcolando il rettangolo inscritto al rettangolo originale che rispetta l'aspect ratio della nuova viewport (area di stampa si riduce ad ogni cambio di viewport)

Lo strumento di selezione dell'area di stampa deve ritornare un centro e delle dimensioni di altezza e lunghezza, oppure il bounding box dell'area selezionata (a seconda di come funziona l'engine di rendering d)