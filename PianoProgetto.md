# Gruppo 6 — Piano di lavoro per lunedì 12 ottobre 2026

Piano proposto il 9 ottobre. Le assegnazioni sono indicative e non presuppongono competenze specifiche dei componenti. La consegna è lunedì; l'orario non è stato indicato. Conviene avere tutto pronto domenica sera.

## 1. Situazione verificata

Sono stati letti la traccia originale `C:/Users/A847apulia/Desktop/Gruppo 6/Traccia 5.docx`, il README e tutti i JSON della copia locale `C:/Users/A847apulia/Desktop/EsameGENAI-main`.

- Il README riporta gli stessi requisiti della traccia originale; i cinque JSON delle due cartelle sono identici.
- I quattro notebook intestati ad Andrea, Antonio, Eduardo e Francesco sono file vuoti da 0 byte: il codice deve ancora essere sviluppato.
- Sono disponibili 3 clienti, 3 polizze, 4 sinistri e le regole su documenti e soglie.
- Non risultano codice, test, diagrammi o presentazione già realizzati nelle due cartelle esaminate.
- La copia locale non contiene `.git`: per collaborare su GitHub occorre partire da un clone della repository condivisa.
- Il JSON chiamato `requirements` contiene regole del dominio, non dipendenze Python.

## 2. Cosa costruire

Un unico assistente Python con LangGraph che riceve una richiesta, recupera cliente/polizza/sinistro, verifica copertura e documenti e indica il passo successivo con motivazione.

Il modello interpreta il linguaggio naturale e formula spiegazioni. Il codice decide le condizioni oggettive: polizza attiva, corrispondenza della copertura, documenti richiesti e confronto con le soglie.

La traccia include anche quattro richieste avanzate, da inserire nel piano: Claims Review Agent richiamato come tool; chiarimenti per informazioni mancanti; approvazione umana prima dell'approvazione finale; checkpoint per sospendere e riprendere una pratica. Ollama è una possibilità, non un obbligo; il modello è a scelta del gruppo.

### Flusso proposto

1. Interpretare la richiesta e identificare la pratica; chiedere chiarimenti se l'identificativo manca o è ambiguo.
2. Recuperare sinistro, polizza, cliente e regole tramite strumenti Python.
3. Verificare la copertura. Se la polizza è inattiva o l'evento non è coperto, restituire il motivo del blocco.
4. Controllare i documenti. Se mancano, sospendere e chiedere quelli necessari; dopo l'integrazione, ripetere il controllo.
5. Applicare le regole di valutazione e richiamare il Claims Review Agent come strumento di revisione. Il revisore non può annullare un vincolo deterministico.
6. Se serve un perito, indicare il passaggio a revisione umana. Altrimenti richiedere l'approvazione umana prima di marcare la pratica come approvata.
7. Produrre una risposta con esito, fatti verificati, motivazione e prossimo passo. Gestire il diniego umano senza approvare la pratica.

Gli errori tecnici vanno gestiti esplicitamente: un errore nel recupero non equivale a una polizza inesistente. Per la prima versione, il passaggio al perito può concludersi nello stato “richiede perito”; un eventuale esito successivo deve comunque rispettare il controllo umano prima dell'approvazione finale.

## 3. Divisione in quattro

| Responsabile proposto | Lavoro concreto | Quando è completo |
|---|---|---|
| Andrea — Dati e strumenti | Caricamento e controllo dei JSON; funzioni per cercare sinistro, polizza, cliente e requisiti; gestione di ID assenti, dati incoerenti ed errori dei tool. | Gli altri possono recuperare dati con interfacce stabili; casi validi, non trovati e guasti simulati sono verificati. |
| Antonio — Regole e decisioni | Verifica polizza attiva/copertura; elenco documenti mancanti; confronto con soglia automatica; esito e motivazioni strutturate. | I quattro sinistri forniti producono gli esiti concordati; i confini delle soglie sono testati senza dipendere dal modello. |
| Eduardo — LLM e agente revisore | Interpretazione della richiesta; Claims Review Agent effettivamente invocabile come tool; risposta finale basata sui risultati verificati; gestione di output LLM non validi. | Richieste in italiano restituiscono informazioni strutturate o domande di chiarimento; la revisione è dimostrabile e non altera le regole. |
| Francesco — LangGraph e integrazione | Stato condiviso; nodi e diramazioni; chiarimenti, approvazione e rifiuto umani; checkpoint persistente e ripresa; notebook finale eseguibile. | Il flusso collega tutti i moduli, si sospende e riprende la stessa pratica anche dopo un riavvio e non approva senza consenso. |

Il lavoro sul grafo è il più impegnativo: Eduardo affianca Francesco nell'integrazione di agente revisore, chiarimenti e approvazione. Andrea e Antonio verificano in coppia dati e regole. Ogni persona scrive i test della propria parte e prepara una o due slide; tutti provano la demo e devono saper descrivere il flusso completo.

## 4. Accordi da chiudere prima di lavorare separatamente

Dedicate circa 30–45 minuti a questi accordi:

- **Stato comune:** richiesta, ID pratica, dati recuperati, documenti ricevuti/mancanti, controlli, esito, motivazioni, revisione, decisione umana ed errori.
- **Interfacce:** stessi nomi e formati per input e output dei tool e delle regole. Ogni risultato distingue successo, dato non trovato ed errore tecnico.
- **Esiti:** informazioni mancanti, non coperto, richiede perito, in attesa di approvazione, approvato, non approvato dall'operatore ed errore tecnico devono essere distinguibili.
- **Ordine dei controlli:** per questa proposta la copertura viene prima dei documenti, per non chiedere integrazioni inutili per un evento già non coperto.
- **Soglie:** proposta da dichiarare nei test: l'importo esattamente uguale alla soglia massima resta ammissibile alla revisione automatica; la soglia si confronta con il danno stimato lordo.
- **Franchigia e massimale:** i dati contengono questi campi, ma la traccia non specifica la formula dell'indennizzo. Non presentare formule inventate come requisiti del docente. Per il progetto potete documentare la convenzione che i casi sotto franchigia o oltre massimale vadano a revisione manuale, senza calcolare automaticamente un pagamento.
- **Altri dati:** non assegnare a `risk_tier` effetti sulle decisioni non previsti dalla traccia. Non inventare date di validità: il dataset offre solo il campo `active`.
- **Integrazione:** un responsabile del notebook finale; moduli Python separati importati dal notebook, se compatibile con le modalità di consegna. Se consegnate un solo notebook, riunite e provate tutto prima della consegna. I notebook individuali possono servire per esperimenti.
- **Ambiente:** stesso modello e stesse versioni delle dipendenze per tutti. Annotare come avviare e riprodurre la demo.

Per chiarimenti e approvazione, LangGraph offre interruzioni riprendibili; il checkpoint deve essere associato all'identificativo della conversazione/pratica. Usate un salvataggio persistente su disco per dimostrare la ripresa dopo riavvio, non soltanto memoria temporanea. Riferimenti: [interrupts](https://docs.langchain.com/oss/python/langgraph/interrupts) e [persistence](https://docs.langchain.com/oss/python/langgraph/persistence).

## 5. Casi di prova

Questi sono **esiti attesi ricavati dai dati e dal flusso proposto**, non risultati di un programma già eseguito.

| Caso | Input o situazione | Esito atteso |
|---|---|---|
| T01 — CL001 | “Controlla la pratica CL001.” Collisione, 1.200, polizza attiva, documenti completi, soglia 3.000. | Ammissibile alla prosecuzione automatica, poi attesa di approvazione umana. Diventa approvata solo dopo un consenso esplicito. |
| T02 — CL002 | “Controlla CL002.” Danno acqua, 7.000, foto presente, fattura assente, soglia 5.000. | Richiesta della fattura; dopo l'integrazione, invio al perito perché sopra soglia. |
| T03 — CL003 | “Controlla CL003.” Polizza inattiva. | Blocco motivato per mancata copertura; nessuna approvazione automatica. |
| T04 — CL004 | “Controlla CL004.” Furto su polizza che copre collisioni. | Evento non coperto. Nel flusso proposto non si chiedono prima i documenti mancanti. |
| T05 — Richiesta ambigua | “Vorrei sapere come procede il sinistro.” | Chiedere l'identificativo senza inventarlo; riprendere dopo la risposta. |
| T06 — ID inesistente | “Controlla CL999.” | Segnalare che la pratica non è stata trovata e chiedere verifica dell'ID. |
| T07 — Tool guasto | Simulare un errore di lettura. | Gestire l'errore, spiegare che il controllo non è completato e impedire approvazioni basate su dati assenti. |
| T08 — Rifiuto umano | L'operatore rifiuta una CL001 pronta per approvazione. | Nessuna approvazione; registrare l'esito e la motivazione disponibile. |
| T09 — Ripresa | Sospendere CL002, riavviare il processo/kernel, riprendere con lo stesso identificativo. | Recuperare lo stato corretto e continuare senza perdere documenti o decisioni precedenti. |
| T10 — Confini | Creare casi sintetici con danno uguale alla soglia e appena superiore. | Rispettare la convenzione dichiarata; tenere separati questi dati dai casi originali. |

Ogni scenario deve contenere ID, input, output atteso, motivazione verificabile e gli eventuali messaggi successivi. Dopo l'esecuzione registrate anche l'output osservato e se coincide con quello atteso. Aggiungete verifiche mirate per gli altri rami effettivamente implementati, inclusi output LLM non validi e le convenzioni su franchigia/massimale.

## 6. Calendario

| Quando | Obiettivo condiviso |
|---|---|
| Venerdì 9 | Concordare interfacce e regole; clonare la repository; preparare ambiente comune e grafo minimo. Entro sera collegare almeno il percorso di CL001, anche con componenti provvisori dichiarati. |
| Sabato 10 | Completare i moduli e integrare progressivamente i quattro casi reali, l'agente revisore, gli errori, chiarimenti e controllo umano. Provare il salvataggio e la ripresa. |
| Domenica 11 | Eseguire scenari, correggere errori, provare da ambiente riavviato; completare diagramma e slide. Fare una simulazione dell'esposizione. Consegna pronta entro sera. |
| Lunedì 12 | Ultimo controllo dei file e consegna entro l'orario stabilito dal docente. |

Per GitHub: branch distinti per le quattro aree, contributi piccoli e integrazione almeno giornaliera. Evitate modifiche contemporanee allo stesso notebook finale; aggiornate le interfacce comuni insieme prima di cambiarle nei singoli moduli.

## 7. Checklist di consegna

- [ ] Notebook Python finale eseguibile dall'inizio alla fine, con gli eventuali moduli e dati necessari.
- [ ] Istruzioni per dipendenze, modello, avvio e riproduzione della demo.
- [ ] File JSON o Markdown con scenari, motivazioni, chiarimenti/approvazioni e risultati osservati.
- [ ] Diagramma che mostri anche diramazioni, revisore, sospensioni e intervento umano.
- [ ] PowerPoint: problema, dati, architettura, ruolo del modello e delle regole, demo, test e limiti.
- [ ] Dimostrazione di almeno un chiarimento, una richiesta al perito, un'approvazione/rifiuto umano e una ripresa dopo riavvio.

Il progetto richiesto è un prototipo didattico. Per questa consegna concentrate il lavoro sul notebook e sui requisiti espliciti; un sito, un'applicazione web o servizi assicurativi reali non sono richiesti.
