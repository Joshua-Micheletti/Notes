1. Come oggi gli oggetti selezionabili su mappa saranno tutti e solo quelli appartenenti ai layers in quel momento attivi, cioè sarà possibile selezionare solo ciò che risulta visibile su mappa
    
2. Cliccando sulla mappa verranno eseguite le query sul DB, sia se il backend è Smallworld (chiamando un opportuno servizio GSS), sia se il backend non è Smallworld in modo che siano restituiti tutti gli elementi dei layer attivi sottostanti al punto selezionato.
    
3. L’attuale Popup di disambiguazione sarà sostituito da una Sidebar che si aprirà sulla sinistra del Client WEB, esattamente come per gli altri tool, in cui saranno mostrati, secondo una rappresentazione a Tree, gli elementi selezionati
    
4. Il formato dell’alberatura prevederà una gestione comune a prescindere dal backend
    
    1. Nel caso Smallworld l’alberatura sarà costruita gerarchicamente secondo: Dataset-->Tabella-->Record
        
    2. Nel caso non Smallworld l’alberatura sarà costruita gerarchicamente secondo: Layer-->Sotto-Layer-->Record
        
5. Alla selezione di uno dei diversi livelli gerarchici dovrà corrispondere l’evidenziazione (in giallo scuro) su mappa di tutti i record sottostanti
    
6. La Sidebar presenterà due bottoni, uno per attivare la scheda di dettaglio del singolo record, uno per eseguire il goto sul singolo record o sui bounbds dell’insieme dei record nel caso in cui sia selezionato un livello gerarchico superiore

## Punti già sviluppati
I punti 1 e 2 sono già presenti nel prodotto G4B. La parte della scheda di dettaglio nel punto 6 è uguale a ciò che è già implementato
## Punti da sviluppare
### 3
Necessità di sviluppare la componente Front End che ospiterà la nuova funzionalità.
Controllando su Jira, ho trovato diverse opzioni per l'implementazione:
- Menù verticale a sinistra fisso che sostituisce gli strumenti della sidebar (come specificato nella richiesta originale)
- Menù verticale a destra fisso, strutturato come gli strumenti della sidebar (come proposto dall'HLD di Filippo)
![[side_query_click.png]]
![[side_query_detail.png]]
![[side_sidebar sx_click.png]]
![[side_sidebar sx_detail.png]]
![[side_simple_click.png]]
![[side_simple_detail.png]]

- Menù fluttuante (inizialmente in alto a sinistra, sotto la barra di ricerca) (come proposto dall'HLD di Filippo)
![[div_query.png]]
![[div_sidebar_sx_click.png]]
![[div_sidebar_sx_detail.png]]
![[div_simple_click.png]]
![[div_simple_detail.png]]
#### Task
FE - Scelta ed implementazione del contenitore grafico (5g)
### 4
Serve costruire una struttura ad albero in cui sono contenuti i vari elementi trovati in seguito al click sulla mappa.
```JSON
{
	<dataset>: {
		<collection>: [
			{
				type: "feature",
				...
			},
			{
				type: "feature",
				...
			}
		],
		<collection>: [
			{
				type: "feature",
				...
			},
			{
				type: "feature",
				...
			}
		]
	},
	<dataset>: {
		...
	},
	<dataset>: {
		<collection>: [
			{
				type: "feature",
				...
			}
		]
	}
}
```

Nella richiesta originale menzionano il concetto di "Sotto-Layer" per quanto riguarda i dati da Postgres, ma non so a cosa si riferisca.
#### Task
FE - Raccogliere tutte le risposte delle varie ricerche dal click e costruire una struttura ad albero (1g)
### 5
Il highlight della feature sulla mappa avviene adesso in seguito ad una selezione (click su un qualsiasi elemento dell'albero). In caso di click su una foglia, bisogna evidenziare la feature associata, in caso di click su un ramo, bisogna evidenziare ogni feature contenuta nel ramo (ricerca).
Il highlight delle feature avviene pushando le single feature in un layer temporaneo, quindi si tratta solo di scorrere l'oggetto che rappresenta l'albero dal punto selezionato verso il basso e pushare tutte le feature presenti sul layer temporaneo.

Al momento non è richiesto ma in futuro sarebbe possibile consentire la selezione multipla di rami ed evidenziare tutti gli elementi presenti nei rami selezionati (per esempio con ctrl + click).
#### Task
FE - Implementare la selezione di foglie e rami e il relativo highlight delle feature associate (1g)
### 6
#### Centramento
Molto simile a quello che già è disponibile, in quanto OpenLayers contiene una funzione 'fit' che automaticamente esegue il fit della visuale su tutte le feature presenti in un Vector Source (layer temporaneo).

In caso di centramento su un ramo, basterà caricare le feature sul layer temporaneo e fare un fit. In caso si voglia fare in modo che si possa centrare la visuale su una feature senza fare l'highlight (non so perchè si possa volere una cosa del genere però), bisognerà caricare le feature sul layer con uno style che le rende invisibili (alpha = 0) e poi fare il fit come menzionato sopra.
##### Task
FE - Implementare il centramento sulle feature presenti nei rami (1g)