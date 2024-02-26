fonte: https://openai.com/pricing#language-models

| Modello | Prezzo Input | Prezzo Output | API | Context Window | Data cutoff date | Tipo Input |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| GPT 3.5 | Gratuito | Gratuito | No | 4096 | 01/22 | Testo |
| GPT 3.5 Turbo | $0.0005 / 1K tokens | $0.0015 / 1K tokens | Si | 4,096/16,385 | 01/22 | Testo |
| GPT 3.5 Turbo Instruct | $0.0015 / 1K tokens | $0.0020 / 1K tokens | Si | 4,096/16,385 | 01/22 | Testo |
| GPT 4 | $0.03 / 1K tokens | $0.06 / 1K tokens | Si | 8,192 | 04/23 | Multimedia |
| GPT 4 32k | $0.06 / 1K tokens | $0.12 / 1K tokens | Si | 32,768 | 04/23 | Multimedia |
| GPT 4 Turbo | $0.01 / 1K tokens | $0.03 / 1K tokens | Si | 128,000 | 04/23 | Multimedia |
## Tokens
"You can think of tokens as pieces of words, where 1,000 tokens is about 750 words. This paragraph is 35 tokens."

I token sono delle unità di testo definite da OpenAI per la costruzione di parole e frasi. Generalmente un token corrisponde ad una parola, ma non sempre, in quanto alcune parole o caratteri possono richiedere più di un token.
## Instruct vs Chat
Questo concetto viene utilizzato nella versione Turbo di GPT 3.5
### Chat
I modelli Chat sono generalmente molto espressivi, tendono a spiegare in dettaglio un concetto anche se non necessariamente parte integrante della domanda posta.
Sono utili per utilizzi quali la risoluzione di problematiche mondane, risposta a domande informative, creazione di testo in linguaggio naturale e dialogo tra utente e modello.
### Instruct
I modelli Instruct rispondono alle domande in modo più mirato, meno discorsivo e più diretto all'obiettivo. Risultano più coerenti e corretti nella risposta ma tralasciano le informazioni che la circondano, concentrandosi principalmente sul risultato corretto. Questo tipo di modelli risulta vantaggioso nella risoluzione di problematiche specifiche, analisi di dettagli di un problema e anche a livello economico, in quanto riducendo il testo scritto che non appartiene strettamente alla soluzione del problema, si evita di usare token altrimenti sprecati.
## Finestra di Contesto
La finestra di contesto di un LLM (Large Language Model) consiste nel numero massimo di tokens che possono essere utilizzati all'interno di un contesto. Questo significa che se stiamo comunicando con un LLM, la quantità di testo che verrà presa in considerazione per generare la risposta corrisponderà agli ultimi X token utilizzati (comprende sia i token in input che i token in output).
La finestra di contesto funziona come un buffer FIFO.
## Data Cutoff Date
Rappresenta la data da cui le informazioni non sono più disponibili. GPT si basa su dati collezionati fino alla Cutoff Date, quindi ogni informazione più recente non è disponibile al modello.

Per esempio la versione gratuita di ChatGPT 3.5 ha un Cutoff Date impostato al 01/22. Ciò significa che non contiene informazioni aggiornate. Per esempio, secondo GPT 3.5, le ultime versioni dei vari software sono:
- Python
	- GPT 3.5: 3.10
	- Attuale (22/02/24): 3.12.2
- Node
	- GPT 3.5: 16
	- Attuale (22/02/24): 21.6.2
- Angular
	- GPT 3.5: 12
	- Attuale (22/02/24): 17.2.1
- Flutter
	- GPT 3.5: 2.10
	- Attuale (22/02/24): 3.19
- Magik
	- GPT 3.5: Sconosciuto
	- Attuale (22/02/24): 5.2
## Multimedia Input
I modelli ChatGPT 4 includono la possibilità di caricare file multimediali (immagini, documenti testuali, video, audio) in modo da poterli analizzare, estrarre informazioni, modificare e usare come base per costruire nuovi file.
## Lunghezza risposte
ChatGPT in genere contiene una lunghezza di output disponibile di 4096 token. Ogni dato superiore a questa quantità verrà rimosso dalla risposta. Un modo per aggirare questo limite consiste nel chiedere al modello di continuare la sua risposta precedente.

Il limite di larghezza della risposta varia dal modello utilizzato.
## Problemi con risposte lunghe
ChatGPT scrive il testo di risposta mano a mano per simulare una persona scrivendo. Questo porta però a potenziali ritardi nelle risposte particolarmente lunghe:

La risposta ha richiesto **62s** per essere mostrata interamente, e ha richiesto una conferma di proseguimento
![[slow.gif]]

Quando le risposte sono troppo lunghe, il modello può esaurire il numero di Token massimo disponibile nella risposta. Questo problema viene superato tramite l'utilizzo del pulsante "Continua" (controlla il nome effettivo) che consente al modello di riprendere la risposta da dove si era fermato.
Anche questa soluzione però risulta problematica, in quanto più è grande la risposta, più è instabile il modello, in quanto generalmente in seguito a proseguimenti il modello:
- diventa meno preciso
- inizia a perdere informazioni precedenti del context window (ormai saturo)
- tende a risultare in errore

La best practice per questo problema è quella di cercare di mantenere le domande e le risposte contenute, quindi evitare di copiare e incollare il source code intero, ma al massimo alcuni pezzi.