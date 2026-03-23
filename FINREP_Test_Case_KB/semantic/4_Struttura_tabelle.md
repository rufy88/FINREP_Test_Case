# 4_Struttura_tabelle — Struttura Tabelle FINREP (Shadow Semantica)

Fonte ufficiale: `4_Struttura_tabelle.docx`.

Questo documento descrive la **struttura logica delle tabelle**
utilizzate nei vari layer del sistema FINREP
(ODS, STG, WRK, TARGET, DIM, FACT).

È destinato esclusivamente al reasoning degli agenti
e NON sostituisce la documentazione tecnica formale.

---

## 📌 1️⃣ Layer ODS (Raw Data)

Le tabelle ODS contengono **copia integrale dei file di input**.
Non sono applicate trasformazioni.

### ODS_CECORE01_RAW
Campi principali:
- ID_ODS (PK)
- COD_CONTO
- COD_FILIALE
- COD_PRODOTTO
- COD_CDC (centro di costo)
- DATA_MOVIMENTO
- PERIODO_RIF (YYYYMM)
- IMPORTO
- VALUTA
- DATA_INSERIMENTO
- FILE_NAME

### ODS_GESTCST1_RAW
- Struttura **analoga** a ODS_CECORE01_RAW

### ODS_RISKADJ1_RAW
- Struttura **analoga** a ODS_CECORE01_RAW

### ODS_CEBRNCH1_RAW
Campi principali:
- ID_ODS (PK)
- COD_FILIALE
- COD_PRODOTTO
- PERIODO_RIF
- RICAVO_FILIALE
- DATA_INSERIMENTO
- FILE_NAME

### ODS_RICAVNET_RAW
Campi principali:
- ID_ODS (PK)
- COD_PRODOTTO
- PERIODO_RIF
- RICAVO_NETTO
- DATA_INSERIMENTO
- FILE_NAME

### ODS_EXCEL_MANUALI
Campi principali:
- ID_ODS (PK)
- TIPO_FILE
- COD_RIF
- PERIODO_RIF
- IMPORTO
- SCENARIO
- DATA_INSERIMENTO
- UTENTE_UPLOAD

---

## 📌 2️⃣ Layer STAGING (STG)

Il layer STG contiene dati **filtrati e aggregati**.

### STG_CECORE01_AGG
Campi principali:
- COD_CONTO
- COD_FILIALE
- COD_PRODOTTO
- COD_CDC
- PERIODO_RIF
- IMPORTO_TOT

### STG_GESTCST1_AGG
- Struttura **analoga** a STG_CECORE01_AGG

### STG_RISKADJ1_AGG
- Struttura **analoga** a STG_CECORE01_AGG

### STG_CEBRNCH1
Campi principali:
- COD_FILIALE
- COD_PRODOTTO
- PERIODO_RIF
- RICAVO_FILIALE

### STG_RICAVNET
Campi principali:
- COD_PRODOTTO
- PERIODO_RIF
- RICAVO_NETTO

---

## 📌 3️⃣ Layer WRK (Ricalcoli)

Il layer WRK contiene i **dati ricalcolati** (solo per flussi complessi).

### WRK_CECORE01_CALC
Campi principali:
- COD_CONTO
- COD_FILIALE
- PERIODO_RIF
- IMPORTO
- MARGINE
- INCIDENZA_PERC

### WRK_GESTCST1_CALC
- Struttura **analoga** a WRK_CECORE01_CALC

### WRK_RISKADJ1_CALC
- Struttura **analoga** a WRK_CECORE01_CALC

---

## 📌 4️⃣ Layer TARGET

Le tabelle TARGET rappresentano il **dato certificato finale**.

### TGT_CE_CORE
Campi principali:
- COD_CONTO
- COD_FILIALE
- COD_PRODOTTO
- COD_CDC
- PERIODO_RIF
- IMPORTO
- MARGINE

### TGT_GEST_COSTI
- Struttura **coerente** con TGT_CE_CORE

### TGT_RISK_ADJ
- Struttura **coerente** con TGT_CE_CORE

### TGT_CE_FILIALI
Campi principali:
- COD_FILIALE
- COD_PRODOTTO
- PERIODO_RIF
- RICAVO_FILIALE

### TGT_RICAVI_NETTI
Campi principali:
- COD_PRODOTTO
- PERIODO_RIF
- RICAVO_NETTO

### TGT_BUDGET / TGT_FORECAST / TGT_DRIVER / TGT_KPI
Campi principali:
- COD_RIF
- PERIODO_RIF
- IMPORTO
- SCENARIO

---

## 📌 5️⃣ Dimensioni

### DIM_TEMPO
- ID_TEMPO (PK)
- DATA_RIF
- MESE
- TRIMESTRE
- ANNO
- FLAG_YTD

### DIM_FILIALE
- ID_FILIALE (PK)
- COD_FILIALE
- DESCRIZIONE
- AREA
- REGIONE

### DIM_PRODOTTO
- ID_PRODOTTO (PK)
- COD_PRODOTTO
- DESCRIZIONE
- LINEA
- SEGMENTO

### DIM_CONTO
- ID_CONTO (PK)
- COD_CONTO
- DESCRIZIONE
- LIVELLO_1
- LIVELLO_2
- LIVELLO_3

### DIM_CENTRO_COSTO
- ID_CDC (PK)
- COD_CDC
- DESCRIZIONE

### DIM_SCENARIO
- ID_SCENARIO (PK)
- COD_SCENARIO

### DIM_KPI
- ID_KPI (PK)
- COD_KPI
- DESCRIZIONE

---

## 📌 6️⃣ Fact Table

### FCT_FINREP_ECONOMICO
Campi principali:
- ID_FACT (PK)
- ID_TEMPO (FK)
- ID_FILIALE (FK)
- ID_PRODOTTO (FK)
- ID_CONTO (FK)
- ID_CDC (FK)
- ID_SCENARIO (FK)
- ID_KPI (FK)
- IMPORTO_ACTUAL
- IMPORTO_BUDGET
- IMPORTO_FORECAST
- SCOSTAMENTO
- SCOSTAMENTO_PERC
- MARGINE

---

## Implicazioni per la Generazione dei Test

Gli agenti devono generare test che verifichino:
- corretto popolamento delle tabelle ODS
- applicazione delle regole STG
- correttezza dei ricalcoli WRK
- integrità referenziale tra FACT e DIM
- coerenza tra TARGET, FACT e Power BI
- gestione corretta degli scenari (Actual/Budget/Forecast)

---

## Nota per gli Agent
Questo documento è il riferimento strutturale per:
- test di integrità dati
- test ETL
- test di riconciliazione
- test di reporting

Le definizioni DDL complete restano nel documento `.docx`.
