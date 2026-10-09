# Traccia 5: Assistente per la gestione di una pratica assicurativa

**Gruppo:** 6

---

## Requisiti Tecnici Generali

* **Linguaggio e Framework:** Il sistema deve essere implementato in Python utilizzando **LangGraph**.
* **LLM:** L'LLM può essere eseguito localmente tramite **Ollama**.
* **Tool:** I tool possono essere simulati con funzioni Python e dataset locali.
* **Logica Deterministica:** Le decisioni deterministiche e le regole non ambigue devono essere implementate nel codice e non delegate al modello.
* **Robustezza:** Il sistema deve gestire esplicitamente informazioni mancanti, errori dei tool e situazioni che richiedono intervento umano.
* **Documentazione & Test:** È richiesto almeno un diagramma dell'architettura e una suite minima di casi di test.

---

## Descrizione del Dominio

Un assicurato apre una richiesta per un sinistro.  
L'assistente deve raccogliere le informazioni disponibili, verificare la copertura, controllare se la documentazione è sufficiente e decidere se la pratica può proseguire automaticamente oppure richiede un perito.

### Informazioni disponibili:
* Dati dei clienti
* Dati sulle polizze
* Dati sui sinistri
* Policy di gestione dei sinistri

---

## Punti Chiave da Implementare

* **Agent in LangGraph**
* **Interpretazione della richiesta**
* **Recupero delle informazioni tramite tools**
* **Workflow Sequential**
* **Gestione dello Stato condiviso**
* **Conditional Routing**
* **Gestione di tool failure ed errori**
* **Regole deterministiche implementate nel codice e non delegate al modello**
* **Risposta finale con motivazione**

---

## Richieste Avanzate

* **Claims Review Agent:** Introdurre un Claims Review Agent come *Agent-as-Tool*.
* **Gestione Incompleta:** Prevedere una fase di chiarimento quando mancano informazioni (es. integrazioni documentali).
* **Human-in-the-Loop:** *Human approval* prima dell'approvazione finale.
* **Persistenza:** Salvare lo stato con un *checkpointer* per riprendere una pratica che rimane sospesa per ore/giorni.

---

## Deliverables

1. **Test case:** Un file (JSON o Markdown) con i Test Scenario da eseguire per verificare il funzionamento del sistema. Ogni scenario dovrà includere:
   * Identificativo del test
   * Input (richiesta iniziale al sistema)
   * Output del sistema
   * Reasoning, cioè il ragionamento atteso che dovrebbe seguire il sistema nel fornire la risposta
   * Eventuali altri messaggi da inserire per chiarimenti e/o approvazioni
2. **Notebook Python** (LLM a scelta del gruppo)
3. **Architettura del sistema**
4. **Presentazione PowerPoint**
