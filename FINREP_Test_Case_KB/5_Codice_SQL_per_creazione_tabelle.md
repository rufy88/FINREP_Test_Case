# 5_Codice_SQL_per_creazione_tabelle — DDL Database FINREP (Shadow Semantica)

Fonte ufficiale: `5_Codice_SQL_per_creazione_tabelle.docx`.

Questo documento descrive in forma **semantica e strutturata**
le regole di creazione delle tabelle FINREP,
i vincoli applicati e le implicazioni per i test.

È destinato esclusivamente al reasoning degli agenti
e NON sostituisce il codice SQL ufficiale.

---

## Regole Globali di Creazione Tabelle

Tutte le tabelle del modello FINREP condividono le seguenti colonne di audit:

- DATA_INSERIMENTO (TIMESTAMP, default SYSTIMESTAMP, NOT NULL)
- DATA_AGGIORNAMENTO (TIMESTAMP, opzionale)
- UTENTE_INS (VARCHAR2)
- UTENTE_AGG (VARCHAR2)

### Implicazioni di Test
Gli agenti devono generare test che verifichino:
- valorizzazione automatica di DATA_INSERIMENTO
- corretta popolazione UTENTE_INS
- assenza di NULL su campi obbligatori

---

## Chiavi e Strategie di Identificazione

### Dimensioni
- Utilizzo di **Surrogate Key numeriche**
- Popolate tramite sequence dedicate:
  - SEQ_DIM_TEMPO
  - SEQ_DIM_FILIALE
  - SEQ_DIM_PRODOTTO
  - SEQ_DIM_CONTO
  - SEQ_DIM_CDC
  - SEQ_DIM_SCENARIO
  - SEQ_DIM_KPI

### Fact
- Sequence dedicata:
  - SEQ_FACT_FINREP

### Implicazioni di Test
- test di univocità PK
- test di corretto incremento sequence
- test di assenza collisioni

---

## Layer ODS (Raw)

Caratteristiche:
- copia integrale dei file di input
- nessuna trasformazione
- tracciabilità file garantita (FILE_NAME)

Tabelle principali:
- ODS_CECORE01_RAW
- ODS_GESTCST1_RAW
- ODS_RISKADJ1_RAW
- ODS_CEBRNCH1_RAW
- ODS_RICAVNET_RAW
- ODS_EXCEL_MANUALI

### Implicazioni di Test
- conteggio record ODS = record file
- presenza FILE_NAME
- valorizzazione DATA_INSERIMENTO

---

## Layer STAGING (STG)

Caratteristiche:
- dati filtrati e aggregati
- applicazione regole funzionali

Tabelle principali:
- STG_CECORE01_AGG
- STG_GESTCST1_AGG
- STG_RISKADJ1_AGG
- STG_CEBRNCH1
- STG_RICAVNET

### Implicazioni di Test
- verifica filtri (importo = 0, duplicati)
- verifica aggregazioni per chiavi
- confronto STG vs ODS

---

## Layer WRK (Ricalcoli)

Caratteristiche:
- ricalcoli funzionali
- calcolo margini e incidenze

Tabelle principali:
- WRK_CECORE01_CALC
- WRK_GESTCST1_CALC
- WRK_RISKADJ1_CALC

### Implicazioni di Test
- correttezza formule
- coerenza valori WRK vs STG
- verifica precisione decimali

---

## Layer TARGET

Caratteristiche:
- dato certificato finale
- base per consolidamento

Tabelle principali:
- TGT_CE_CORE
- TGT_GEST_COSTI
- TGT_RISK_ADJ
- TGT_CE_FILIALI
- TGT_RICAVI_NETTI
- TGT_BUDGET / TGT_FORECAST / TGT_DRIVER / TGT_KPI

### Implicazioni di Test
- coerenza TARGET vs WRK/STG
- corretto mapping chiavi
- verifica scenari (Actual/Budget/Forecast)

---

## Dimensioni

Dimensioni principali:
- DIM_TEMPO
- DIM_FILIALE
- DIM_PRODOTTO
- DIM_CONTO
- DIM_CENTRO_COSTO
- DIM_SCENARIO
- DIM_KPI

Caratteristiche:
- PK surrogate
- codici di business UNIQUE
- supporto analisi gerarchiche

### Implicazioni di Test
- univocità codici business
- popolamento corretto dimensioni
- gestione valori non censiti

---

## Fact Table

### FCT_FINREP_ECONOMICO

Caratteristiche:
- PK: ID_FACT
- FK verso tutte le dimensioni
- misure Actual/Budget/Forecast
- scostamenti e margini

Vincoli:
- FK su DIM_TEMPO
- FK su DIM_FILIALE
- FK su DIM_PRODOTTO
- FK su DIM_CONTO
- FK su DIM_CENTRO_COSTO
- FK su DIM_SCENARIO
- FK su DIM_KPI

### Implicazioni di Test
- test di integrità referenziale
- test FK non valorizzate
- test coerenza FACT vs TARGET
- test coerenza FACT vs Power BI

---

## Indici di Performance

Indici principali su FACT:
- IDX_FACT_TEMPO
- IDX_FACT_FILIALE
- IDX_FACT_PRODOTTO
- IDX_FACT_CONTO

### Implicazioni di Test
- test performance su query PBI
- verifica piani di esecuzione
- supporto NFR di performance

---

## Uso da parte degli Agent

Questo documento deve essere utilizzato per:
- progettare test di integrità dati
- progettare test ETL/DDL
- coprire NFR di audit e performance
- validare schema vs reporting

Il codice SQL completo resta nel documento `.docx`.
``
