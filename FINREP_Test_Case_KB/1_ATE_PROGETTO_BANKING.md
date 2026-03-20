# 1_ATE_PROGETTO_BANKING — Architettura Tecnica (Shadow Semantica)

## Scopo del Documento
Questo documento descrive l’architettura tecnica e i processi di alimentazione dati
del sistema FINREP per la produzione di report di **Conto Economico** e
**Conto Gestionale** tramite **Power BI**.

Fonte ufficiale: `1_ATE_PROGETTO_BANKING.docx`.

È destinato al reasoning degli agenti e NON sostituisce il documento formale.

---

## Architettura Generale

### Componenti Principali
Il sistema FINREP è composto da:

- **Sistemi legacy**
- **MFT (Managed File Transfer)**
- **IBM DataStage (ETL)**
- **IBM Tivoli Workload Scheduler (TWS)**
- **Database Oracle**
- **FINREP Web Portal (Web Application)**
- **Power BI** (modello tabulare a stella)

---

## Flussi Automatici da Sistemi Legacy

Sono previsti **5 flussi automatici**, identificati da un codice file a 8 caratteri.

### Flussi e contenuto

| Codice | Significato | Contenuto |
|------|-----------|----------|
| CECORE01 | Conto Economico Core | Scritture contabili core banking |
| CEBRNCH1 | CE Branch | Redditività per filiale |
| GESTCST1 | Gestione Costi | Costi operativi e allocazioni |
| RICAVNET | Ricavi Netti | Margini da prodotti bancari |
| RISKADJ1 | Risk Adjustment | Rettifiche rischio |

---

## Schedulazioni TWS

Per ogni file è prevista una schedulazione dedicata:

| File | TWS |
|----|----|
| CECORE01 | TWS_CECORE01_LOAD |
| CEBRNCH1 | TWS_CEBRNCH1_LOAD |
| GESTCST1 | TWS_GESTCST1_LOAD |
| RICAVNET | TWS_RICAVNET_LOAD |
| RISKADJ1 | TWS_RISKADJ1_LOAD |

---

## Processi ETL DataStage

### Tipologia A — ETL con WRK (3 flussi)
Applicata a:
- CECORE01
- GESTCST1
- RISKADJ1

#### ODS
- Tabelle:
  - T_ODS_CECORE01_RAW
  - T_ODS_GESTCST1_RAW
  - T_ODS_RISKADJ1_RAW
- Contenuto: copia integrale del file
- Nessuna trasformazione

#### STG
- Tabelle:
  - STG_CECORE01_AGG
  - STG_GESTCST1_AGG
  - STG_RISKADJ1_AGG

**Regole applicate:**
- Esclusione importi = 0
- Esclusione centri di costo dismessi
- Controllo periodo contabile
- Eliminazione duplicati (chiave composta)

**Aggregazioni per:**
- Periodo
- Centro di costo
- Prodotto
- Filiale

#### WRK (ricalcoli)
- Tabelle:
  - WRK_CECORE01_CALC
  - WRK_GESTCST1_CALC
  - WRK_RISKADJ1_CALC

**Calcoli principali:**
- Margine operativo lordo
- Incidenza percentuale costi/ricavi
- Ribaltamento costi indiretti
- Rettifiche cumulative YTD
- Conversione valuta (se prevista)

#### Target
- Tabelle:
  - TGT_CE_CORE
  - TGT_GEST_COSTI
  - TGT_RISK_ADJ

---

### Tipologia B — ETL diretto (2 flussi)
Applicata a:
- CEBRNCH1
- RICAVNET

#### ODS
- ODS_CEBRNCH1_RAW
- ODS_RICAVNET_RAW

#### STG
- STG_CEBRNCH1
- STG_RICAVNET

**Regole:**
- Verifica codice filiale
- Verifica prodotto censito
- Validazione periodo
- Controllo formato numerico

#### Target
- TGT_CE_FILIALI
- TGT_RICAVI_NETTI

(Nessun ricalcolo WRK)

---

## Caricamento File Excel via WebApp

### File supportati
La WebApp consente il caricamento di:
- Budget Annuale
- Forecast
- Piano Industriale
- Driver di Allocazione
- Mappatura Centri di Costo
- Parametri KPI
- Note Gestionali

### Controlli WebApp (bloccanti)
- Struttura colonne
- Obbligatorietà campi
- Duplicati
- Formato date
- Formato numerico importi
- Verifica chiavi (filiale, prodotto, conto)
- Quadratura importi

### ODS Excel
- Tabella: ODS_EXCEL_MANUALI
- Procedura: PRC_LOAD_ODS_EXCEL

### Elaborazioni
- Normalizzazione codici
- Allineamento calendario
- Delta Budget vs Actual
- KPI derivati

### Report di Controllo
- FINREP_Control_Report.xlsx
- Contiene:
  - Riconciliazioni
  - Scostamenti
  - KPI
  - Anomalie residue

### Pubblicazione
- Azione utente: “Conferma Pubblicazione”
- Tabelle target:
  - TGT_BUDGET
  - TGT_FORECAST
  - TGT_DRIVER
  - TGT_KPI

---

## Consolidamento Notturno

Ogni notte:
- TWS: TWS_FINREP_DAILY_CONSOL
- Procedura: PRC_BUILD_FACT_FINREP
- Output:
  - FCT_FINREP_ECONOMICO

---

## Modello Dati Power BI

### Fact
- FCT_FINREP_ECONOMICO
- Misure:
  - Importo Actual
  - Importo Budget
  - Importo Forecast
  - Scostamento
  - Margine
  - KPI

### Dimensioni
- DIM_TEMPO
- DIM_FILIALE
- DIM_PRODOTTO
- DIM_CONTO
- DIM_CENTRO_COSTO
- DIM_SCENARIO
- DIM_KPI

Relazioni **1:N**, chiavi surrogate, modello ottimizzato per Power BI.

---

## Flusso Complessivo (end‑to‑end)
- Ricezione file automatici
- ETL → ODS → STG → WRK (se previsto) → Target
- Upload manuale WebApp
- Validazione utente
- Consolidamento notturno
- Alimentazione modello a stella
- Reportistica Power BI

---

## Benefici Architetturali
- Tracciabilità completa del dato
- Separazione dei layer
- Controllo qualità
- Governance contabile
- Flessibilità scenari
- Performance PBI ottimizzate
