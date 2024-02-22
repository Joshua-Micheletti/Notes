**

HLD query postgres

  

[1. Introduzione 2](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.ajvpm08l6e9l)

[2. Strumenti 2](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.bokmwoeui225)

[3. Funzionalità 2](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.oa8293xulb0y)

[3.0 Lista dataset 2](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.n95h76xhbyv5)

[3.1 Lista tabelle 2](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.wg7jbm4hlp8l)

[GSS 3](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.qjgi3yjffulg)

[PostgreSQL 3](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.iatl8ymq0xk1)

[3.2 Lista colonne 3](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.maz121bcvwx2)

[GSS 4](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.pwysas21d8zl)

[PostgreSQL 4](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.8xpvurmtdesj)

[3.3 Esecuzione query 4](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.fbsqhys6thx9)

[GSS 5](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.7pow0gido6bk)

[PostgreSQL 5](https://docs.google.com/document/d/1a4z2u4I2WDkFeSOPvC-eoxPr05ZuC6-J9gFraeZYClI/edit#heading=h.rv8cfzd4fbba)

# 1. Introduzione

Questo file conterrà l’analisi per la realizzazione della funzionalità di query anche su database PostgreSQL, in aggiunta al supporto per GSS/Smallworld.

# 2. Strumenti

Per la realizzazione di questa funzionalità sarà necessario implementare delle API che faranno interrogazioni sul database per ottenere in risposta le informazioni utili al frontend per popolare gli input di tabelle e colonne ed ottenere le informazioni da popolare nella tabella dopo l’esecuzione della query.

# 3. Funzionalità

Questo HLD ha lo scopo di introdurre il supporto per la funzionalità query anche per dabatase PostgreSQL. Dovranno essere gestite quindi entrambe le tipologie di query (GSS e PostgreSQL). L’idea è di creare delle API con una signature che permetta di capire dove recuperare i dati ed effettuare le query. La parte di compilazione delle tabelle asset_query, asset_query_cond, asset_query_ord rimane invariata poichè la struttura dei dati consente di creare le opportune richieste per entrambe le finalità.

## 3.0 Lista dataset

Per poter gestire entrambe le funzionalità è necessario modificare la modalità di popolamento della lista dei dataset aggiungendo per ogni voce anche la modalità di selezione a cui appartiene. Inoltre per rendere la funzionalità ancora più elastica la configurazione di tale elenco andrebbe portata nella tabella del database. E’ quindi necessario creare una nuova voce nella tabella configuration dentro la quale posizionare un array di oggetti che hanno le seguenti proprietà:

- dataset: contiene il nome del dataset (water, waste_water, shape, …)
    
- label: contiene la label del dataset
    
- type: contiene la tipologia GSS o PostgreSQL (in futuro anche altre eventualmente)
    
- ~~params: oggetto contenente host, port, user, password per la connessione a db PostgreSQL mentre conterrà url, (aggiungere quello che serve) per chiamare il GSS~~
    

## 3.1 Lista tabelle

Per ottenere la lista delle tabelle di un dataset sarà necessario realizzare un’API che interrogherà opportunamente il backend (GSS o query Postgres) restituendo il risultato. L’api 

riceverà in ingresso il parametro dataset e restituirà l’elenco delle tabelle presenti all’interno del db. Per capire in che database effettuare la query si possono prevedere due modalità:

- il portale invierà anche il parametro type ricevuto dalla chiamata precedente usata per popolare la lista dei datasets
    
- il backend una volta ricevuto il nome del dataset andrà a verificare nella configurazione la tipologia del dataset tramite l’opportuna chiave utilizzata nella sezione 3.0
    

Esempio (la presenza di type cambia in base all’implementazione):

- /saas/query/creation?dataset=water&type=gss
    
- /saas/query/creation?dataset=shape&type=pg
    

La risposta di questa api sarà composta dalla solita struttura:

{

  error: string

  erroDescription: string

  message: string

  data: any

  traceback: any

}

dove in caso di risposta affermativa il valore della chiave data sarà un array di oggetti che rappresentano le tabelle cosi strutturati:

- name: nome della tabella
    
- label: label della tabella
    
### GSS

Nel backend nel caso di tipologia GSS la chiamata da effettuare è la seguente 

http://attach-component-service:30443/gss/schema?dataset_name=water&json=true

dove il risultato va poi modellato per essere conforme alla signature definita sopra.

### PostgreSQL

Nel backend nel caso di tipologia PostgreSQL va effettuata la seguente senza aver selezionato uno specifico database e poi va anche in questo caso modellato il risultato per essere conforme alla signature definita sopra.

  

SELECT * 

FROM pg_catalog.pg_tables 

WHERE schemaname != 'pg_catalog' AND

tablename != 'spatial_ref_sys' AND

    schemaname != 'information_schema';

## 3.2 Lista colonne

Per ottenere la lista delle colonne di una tabella sarà necessario utilizzare un’API che interrogherà opportunamente il backend (GSS o query Postgres) restituendo il risultato. L’api 

riceverà in ingresso il parametro table (insieme al parametro dataset) e restituirà l’elenco delle colonne presenti all’interno della tabella. Per capire quale query effettuare si possono prevedere due modalità:

- il portale invierà anche il parametro type oltre che dataset e table
    
- il backend una volta ricevuto il nome del dataset e della tabella e della tabella andrà a verificare nella configurazione la tipologia del dataset tramite l’opportuna chiave utilizzata nella sezione 3.0
    

Esempio (la presenza di type cambia in base all’implementazione):

- /saas/query/creation?dataset=water&table=wa_pipe&type=gss
    
- /saas/query/creation?dataset=shape&table=bk_ssarf&type=pg
    

La risposta di questa api sarà composta dalla solita struttura:

{

  error: string

  erroDescription: string

  message: string

  data: any

  traceback: any

}

dove in caso di risposta affermativa il valore della chiave data sarà un array di oggetti che rappresentano le colonne cosi strutturati:

- name: nome della colonna
    
- label: label della colonna
    
- type: tipo di dato (string, geom, …)
    

### GSS

Nel backend nel caso di tipologia GSS la chiamata da effettuare è la seguente 

http://attach-component-service:30443/gss/schema?dataset_name=water&json=true

dove il risultato va poi modellato per essere conforme alla signature definita sopra.

  

### PostgreSQL

Nel backend nel caso di tipologia PostgreSQL va effettuata la seguente query nel database e poi va anche in questo caso modellato il risultato per essere conforme alla signature definita sopra. La query va eseguita nel database selezionato.

  

SELECT column_name, data_type

FROM information_schema.columns

WHERE table_name = 'nome_tabella_selezionata';

  

## 3.3 Esecuzione query

Per quanto riguarda l’esecuzione della query sarà necessario modificare l’attuale API che viene chiamata passando come parametri l’identificativo della query e i due valori offset e limit relativi alla paginazione.

  

Per quanto rigurda la struttura e la modalità di esecuzione saranno da creare helper diversi in base alla tipologia.

Una volta ricevuta la richiesta di esecuzione vengono recuperati i valori delle tre tabelle relative alle query (asset_query, asset_query_cond, asset_query_ord) dove all’interno è specificato il dataset. Per capire quale query effettuare si possono prevedere due modalità:

- il portale invierà anche il parametro type oltre che dataset e table
    
- il backend una volta ricevuto il nome del dataset e della tabella e della tabella andrà a verificare nella configurazione la tipologia del dataset tramite l’opportuna chiave utilizzata nella sezione 3.0
    

Esempio (la presenza di type cambia in base all’implementazione):

- /saas/query/exec?id=14&table=wa_pipe&type=gss
    
- /saas/query/exec?id=14&type=pg
    

  

La risposta di questa api sarà composta dalla solita struttura:

{

  error: string

  erroDescription: string

  message: string

  data: any

  traceback: any

}

dove in caso di risposta affermativa il valore della chiave data sarà un array contenente un solo oggetto cosi strutturato:

- "tab_name": "query"
    
- "icon"
    

- "type": "material"
    
- "name": "table"
    

- data: contenente un array di oggetti che hanno la struttura di GeoJSON feature
    

I valori evidenziati in magenta sono fissi. Il tab name indica il utilizzato dal frontend per mostrare l’attuale visualizzazione mentre nella sezione icon type e name indicano la tipologia e quale icona deve essere visualizzata come identificativo della tabella.

  

### GSS

Per quanto riguarda la struttura e l’esecuzione delle query GSS rimane quanto già fatto.

### PostgreSQL

Per quanto riguarda la struttura e l’esecuzione delle query PostgreSQL va creato un helper in grado di creare query sia per quelle “guidate” (unendo tabella, condizioni e ordinamento) sia per quelle “avanzate” (dove abbiamo il query script).  
  

Nella gestione dei confronti comportarsi in questo modo:

- equal: utilizzare il confronto con il carattere = e ricordarsi che nel caso di campo stringa il valore va “wrappato” da apici singoli. Se la stringa contiene un apostrofo questo va raddoppiato per fare l’escape
    
- like: utilizzare il confronto con like e wrappare il valore con il carattere % (es. %palo%). Se la stringa contiene un apostrofo questo va raddoppiato per fare l’escape
    
- within: utilizzare l’estensione spaziale postgis per questo confronto scegliendo la funzione ST_Contains (dove l’elemento ricercato deve essere completamente contenuto dalla geometria passata come filtro). La geometria nel db è salvata in 3857. Le geometrie delle tabelle potrebbero non essere in questo formato
    

  

Per quanto riguarda la paginazione vengono gestiti i valori LIMIT e OFFSET.

**