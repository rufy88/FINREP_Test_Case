# 0_AFU_Progetto_BANKING — Analisi Funzionale (Shadow Semantica)

## Scopo del Documento
Questo documento rappresenta la versione **semantica e strutturata** dell’Analisi Funzionale FINREP.
Fonte ufficiale: `0_AFU_Progetto_BANKING.docx`.

È destinato al reasoning degli agenti e NON sostituisce il documento formale.

---

## Obiettivo del Sistema FINREP
Il sistema FINREP è finalizzato alla produzione di reportistica di:
- **Conto Economico**
- **Conto Gestionale**

tramite **Power BI**, utilizzando dati provenienti da:
- Flussi automatici da sistemi legacy
- File Excel caricati manualmente tramite Web Application

---

## Ambito Funzionale Coperto
L’analisi funzionale copre:
- Contesto applicativo
- Funzionalità della WebApp
- Processo di ingestione dati
- Processi ETL
- Consolidamento
- Modello dati
- Fruizione tramite Power BI

---

## Contesto Organizzativo
Il sistema supporta le seguenti funzioni aziendali:
- Controllo di gestione
- Direzione Amministrazione e Bilancio
- Controllo Filiali
- Pianificazione e Budgeting

### Benefici garantiti
- Tracciabilità
- Coerenza contabile
- Riconciliazione
- Performance analitiche elevate

---

## Architettura Logica di Riferimento
L’architettura FINREP è strutturata nei seguenti layer:

1. **Sorgenti Dati**
2. **ODS (Raw Layer)**
3. **Staging**
4. **Work (WRK – calcoli)**
5. **Target**
6. **Fact Consolidata**
7. **Modello a Stella per Power BI**

---

## Flussi Automatici da Sistemi Legacy

### Flusso di riferimento
- **CECORE01 – Conto Economico Core**

### Fase 1 – Ricezione File (MFT)
Il file viene:
- Generato dal sistema legacy
- Trasferito tramite MFT
- Depositato in cartella di atterraggio

**Controlli funzionali:**
- Naming convention corretta
- Unicità file per periodo
- Integrità fisica file

**Regola bloccante:**
- Se un controllo fallisce → processo bloccato

---

### Fase 2 – Avvio Batch (TWS)
Una schedulazione TWS dedicata:
- Verifica presenza file
- Avvia ETL DataStage
- Logga esito processo

---

### Fase 3 – Caricamento ODS
Il file viene caricato **integralmente** in ODS.

**Obiettivi funzionali ODS:**
- Conservazione dato originale
- Auditabilità
- Tracciabilità file

> In ODS NON vengono applicate trasformazioni.

---

### Fase 4 – Staging (STG)
Nel layer STG vengono applicate:

**Trasformazioni funzionali:**
- Filtri
- Normalizzazioni
- Aggregazioni

**Regole funzionali:**
- Esclusione importi pari a zero
- Esclusione centri di costo non attivi
- Verifica periodo contabile valido
- Eliminazione duplicati

**Aggregazioni per:**
- Periodo
- Filiale
- Prodotto
- Conto
- Centro di costo

---

### Fase 5 – Work Layer (WRK – Ricalcoli)
Applicabile ai flussi complessi.

**Calcoli funzionali principali:**
- Margine operativo
- Incidenza percentuale costi/ricavi
- Ribaltamento costi indiretti
- Calcolo YTD
- Conversione valuta (se presente)

**Obiettivo:**
Produrre dato già pronto per il reporting.

---

### Fase 6 – Target
I dati vengono scritti in tabelle target che rappresentano:
- Fonte ufficiale certificata
- Base per consolidamento notturno

---

## Flussi Manuali – File Excel Budget (WebApp)

### Fase 1 – Upload
L’utente utilizza la WebApp:
- **Upload Manual Data – FINREP**
- Caricamento file Budget Annuale

---

### Fase 2 – Controlli di Validazione (bloccanti)
La WebApp verifica:
- Presenza colonne obbligatorie
- Formato numerico importi
- Formato data corretto
- Duplicati
- Coerenza codici (filiale, conto, prodotto)
- Quadratura totale importi

**In caso di errore:**
- Blocco elaborazione
- Generazione report anomalie

---

### Fase 3 – Caricamento ODS Excel
Se i controlli sono superati:
- Inserimento in `ODS_EXCEL_MANUALI`
- Tracciamento utente e timestamp

---

### Fase 4 – Elaborazione e Preview
Il dato viene:
- Normalizzato
- Allineato al calendario contabile
- Confrontato con Actual

**Output generato:**
- `FINREP_Control_Report.xlsx`

L’utente verifica:
- Totali
- Scostamenti
- Indicatori

---

### Fase 5 – Pubblicazione
Dopo validazione:
- Click su **“Conferma Pubblicazione”**
- Scrittura dati in `TGT_BUDGET`

---

## Consolidamento Notturno
Ogni notte:
- TWS avvia procedura di consolidamento
- Lettura di tutte le tabelle target
- Uniformazione chiavi dimensionali
- Scrittura in:
  - `FCT_FINREP_ECONOMICO`

---

## Modello Dati per Power BI

### Modello a Stella
**Tabella dei Fatti**
- `FCT_FINREP_ECONOMICO`
- Misure:
  - Importo Actual
  - Importo Budget
  - Importo Forecast
  - Scostamento
  - Margine
  - KPI

**Tabelle Dimensionali**
- `DIM_TEMPO`
- `DIM_FILIALE`
- `DIM_PRODOTTO`
- `DIM_CONTO`
- `DIM_CENTRO_COSTO`
- `DIM_SCENARIO`
- `DIM_KPI`

Relazioni: **1:N** tra dimensioni e fact.

---

## Reportistica Power BI

### Tipologia Report
- Conto Economico mensile
- Analisi per filiale
- Analisi per prodotto
- Analisi scostamenti Budget vs Actual
- KPI direzionali
- Analisi YTD

### Funzionalità Analitiche
- Drill‑down gerarchico
- Filtri dinamici
- Confronto scenari
- Analisi temporale
- Esportazione Excel
- Visualizzazioni KPI

---

## Controlli Funzionali e Governance
Il sistema garantisce:
- Separazione layer dati
- Auditabilità completa
- Log esecuzione batch
- Integrità referenziale
- Controlli di qualità
- Versionamento scenari previsionali

---

## Gestione Errori
Gli errori possono verificarsi in:
- Fase upload
- Fase ETL
- Fase consolidamento

**Gestione:**
- Errore loggato
- Dati certificati non compromessi
- Generazione notifica tecnica

---

## Benefici Funzionali
- Centralizzazione dato gestionale
- Allineamento consuntivo/previsionale
- Riduzione errori manuali
- Velocità di analisi
- Coerenza tra funzioni aziendali

---

## Conclusioni
Il processo FINREP è un sistema integrato di:
- Data ingestion strutturata
- Validazione controllata
- Calcolo e consolidamento certificato
- Reporting avanzato

Garantisce **robustezza, tracciabilità e performance**, in linea con standard bancari di governance.
