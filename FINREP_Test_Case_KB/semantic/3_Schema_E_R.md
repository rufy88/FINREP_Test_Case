# 3_Schema_E_R — Modello Dati FINREP (Shadow Semantica)

Fonte ufficiale: `3_Schema_E_R.docx`.

Questo documento descrive in forma testuale e strutturata
il **modello dati a stella** utilizzato dal sistema FINREP
per la reportistica Power BI.

È destinato esclusivamente al reasoning degli agenti
e NON sostituisce lo schema grafico ufficiale.

---

## Scopo
Il modello E/R FINREP supporta:
- reporting di Conto Economico
- reporting Gestionale
- analisi Actual / Budget / Forecast
- KPI direzionali
- confronti temporali e dimensionali

---

## Struttura Generale — Modello a Stella

Il modello è basato su:
- **1 tabella dei fatti centrale**
- **più tabelle dimensionali**
- relazioni **1:N** da dimensioni verso fact

---

## Tabella dei Fatti

### FCT_FINREP_ECONOMICO

**Chiavi esterne (FK):**
- ID_TEMPO
- ID_FILIALE
- ID_PRODOTTO
- ID_CONTO
- ID_CENTRO_COSTO
- ID_SCENARIO
- ID_KPI

**Misure principali:**
- IMPORTO_ACTUAL
- IMPORTO_BUDGET
- IMPORTO_FORECAST
- SCOSTAMENTO
- SCOSTAMENTO_PERC
- MARGINE
- KPI_VALUE

La fact rappresenta la **fonte certificata unica** per il reporting.

---

## Tabelle Dimensionali

### DIM_TEMPO
- ID_TEMPO (PK)
- DATA
- MESE
- TRIMESTRE
- ANNO
- FLAG_YTD

---

### DIM_FILIALE
- ID_FILIALE (PK)
- COD_FILIALE
- DESCRIZIONE
- AREA
- REGIONE
- DIREZIONE_TERRITORIALE

---

### DIM_PRODOTTO
- ID_PRODOTTO (PK)
- COD_PRODOTTO
- DESCRIZIONE
- LINEA
- SEGMENTO

---

### DIM_CONTO
- ID_CONTO (PK)
- COD_CONTO
- DESCRIZIONE_CONTO
- LIVELLO_1
- LIVELLO_2
- LIVELLO_3

---

### DIM_CENTRO_COSTO
- ID_CDC (PK)
- COD_CDC
- DESCRIZIONE
- RESPONSABILE

---

### DIM_SCENARIO
- ID_SCENARIO (PK)
- TIPO_SCENARIO
  - ACTUAL
  - BUDGET
  - FORECAST

---

### DIM_KPI
- ID_KPI (PK)
- COD_KPI
- DESCRIZIONE
- FORMULA_CALCOLO

---

## Relazioni

- Tutte le relazioni sono **1:N**
  - Dimensione → Fact
- Utilizzo di **chiavi surrogate numeriche**
- Integrità referenziale garantita in fase di caricamento ETL
- Modello ottimizzato per:
  - Power BI
  - modello Tabular
  - performance analitiche

---

## Vista Architetturale Complessiva

Sequenza end‑to‑end dei dati:

1. Sistemi LEGACY + WebApp
2. ODS
3. STAGING
4. WRK (solo per flussi complessi)
5. TARGET
6. FACT TABLE (FCT_FINREP_ECONOMICO)
7. MODELLO A STELLA
8. REPORT POWER BI

---

## Implicazioni per la Generazione dei Test

Gli agenti devono generare test che verifichino:
- corretta popolazione delle FK in FACT
- coerenza FK ↔ PK dimensionali
- correttezza delle misure
- coerenza tra FACT e Power BI
- corretta distinzione scenario (Actual/Budget/Forecast)
- corretto calcolo KPI e scostamenti
- comportamento YTD

---

## Nota per gli Agent
Questo documento deve essere utilizzato:
- per progettare test di integrità referenziale
- per test di coerenza reporting
- come riferimento strutturale del modello dati

Lo schema grafico del `.docx` NON deve essere interpretato direttamente.
