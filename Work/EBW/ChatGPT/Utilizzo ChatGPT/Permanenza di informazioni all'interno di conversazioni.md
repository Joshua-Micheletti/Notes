ChatGPT prende in considerazione le informazioni precedentemente condivise all'interno di una conversazione. Il numero di informazioni disponibili è però limitato. Questo limite è definito come Context Window, e viene calcolato in Token ([[ChatGPT 3.5 vs 4.0]]).
Nel caso di ChatGPT 3.5, questo limite è di 4096 tokens.

Ciò vuol dire che per quanto la permanenza di informazioni nelle conversazioni sia un fattore influente sulle risposte fornite, le informazioni troppo vecchie vengono scartate dal modello (lista FIFO).

## Esempio limite Context Window
https://chat.openai.com/share/15c230bc-0b05-4adc-ad20-d9f14f3feb8a

In questo esempio, la conversazione inizia con l'informazione: "i miei calzini sono viola".
In seguito vengono fatte delle domande per tentare di evidenziare il limite di dimensione delle risposte, seguito dalla ripetizione del primo canto della Divina Commedia (un qualsiasi testo per riempire la Context Window).

Una volta saturato il Context Window, viene chiesto a ChatGPT: "di che colore sono i miei calzini?".

Prima di riempire il contesto, ChatGPT era in grado di rispondere correttamente alla domanda, ma dopo aver saturato il buffer, il modello ha rimosso l'informazione, e risponde alla domanda dicendo di non poter sapere quel dato.

"I'm sorry, but I don't have access to personal information about individuals unless it has been shared in the course of our conversation. I am designed to respect user privacy and confidentiality. If you have any concerns about privacy or data security, please let me know, and I will do my best to address them."