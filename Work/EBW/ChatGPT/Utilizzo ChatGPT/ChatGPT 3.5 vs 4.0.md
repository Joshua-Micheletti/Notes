
fonte: https://openai.com/pricing#language-models

| Modello                | Prezzo Input                              | Prezzo Output       | API    | Context Window    |
| ---------------------- | ----------------------------------------- | ------------------- | --- | --- |
| GPT 3.5                | Gratuito                                  | Gratuito            | No    | 4096    |
| GPT 3.5 Turbo          | $0.0005 / 1K tokens                       | $0.0015 / 1K tokens | Si    | 4,096/16,385    |
| GPT 3.5 Turbo Instruct | $0.0015 / 1K tokens                       | $0.0020 / 1K tokens | Si    | 4,096/16,385    |
| GPT 4                  | $0.03 / 1K tokens                         | $0.06 / 1K tokens   | Si    | 8,192    |
| GPT 4 32k              | $0.06 / 1K tokens                         | $0.12 / 1K tokens   | Si    | 32,768    |
| GPT 4 Turbo            | $0.01 / 1K tokens | $0.03 / 1K tokens                    | Si    | 128,000    |
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