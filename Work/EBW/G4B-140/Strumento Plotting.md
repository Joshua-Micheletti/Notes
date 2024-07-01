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
- Interfaccia per la selezione di layer attivi, layout e area di stampa

### Renderi