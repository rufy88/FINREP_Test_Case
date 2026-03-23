# 2_Matrice_requisiti_vs_test_case — Tracciabilità Requisiti/Test (Shadow Semantica)

Fonte ufficiale: `2_Matrice_requisiti_vs_test_case.xlsx`.

## Scopo
Questo documento descrive la **mappatura ufficiale** tra:
- requisiti funzionali FINREP
- casi di test associati

È utilizzato dagli agenti per:
- garantire tracciabilità end‑to‑end
- verificare copertura dei requisiti
- individuare GAP di test
- supportare audit e revisione

---

## Struttura della Matrice

La matrice utilizza le seguenti colonne:

1. **ID_REQUISITO**
   - Identificativo univoco del requisito
   - Formato osservato: `FR###`

2. **TIPO**
   - Tipo requisito
   - Valore osservato: `FR` (Functional Requirement)

3. **DESCRIZIONE REQUISITO**
   - Descrizione testuale del requisito funzionale

4. **PRIORITÀ**
   - Valori osservati:
     - Alta
     - Media

5. **ID_TEST ASSOCIATI**
   - Elenco dei casi di test che validano il requisito
   - Formato:
     - singolo (es. `TC001`)
     - multiplo (es. `TC014, TC015`)

6. **FASE TEST**
   - Fase di validazione prevista
   - Valori osservati:
     - `SIT` (System Integration Test)
     - `UAT` (User Acceptance Test)

7. **NOTE**
   - Campo opzionale per dettagli aggiuntivi (quando presente)

---

## Requisiti Funzionali – Ambiti Coperti

### A) Flussi Automatici Legacy (MFT / ETL / TWS / DWH)

Requisiti principali osservati:
- Acquisizione file da MFT
- Validazione naming convention
- Avvio schedulazione TWS
- Caricamento ODS
- Regole STG (duplicati, importi zero)
- Aggregazioni dati
- Calcoli (margine, incidenza)
- Scrittura tabelle target
- Consolidamento FACT
- Integrità referenziale
- Esposizione su Power BI

Esempi rappresentativi:
- **FR001** → TC001 (ricezione file CECORE01)
- **FR004** → TC004 (caricamento ODS)
- **FR007** → TC007 (aggregazioni)
- **FR011** → TC011 (consolidamento FACT)
- **FR013** → TC014, TC015 (visualizzazione Power BI)

---

### B) Flussi Manuali WebApp (Excel Budget)

Requisiti principali osservati:
- Upload file Budget
- Controlli strutturali Excel
- Controlli formato numerico
- Caricamento in ODS_EXCEL_MANUALI
- Generazione report di controllo
- Pubblicazione manuale Budget
- Consolidamento Budget
- Visualizzazione Budget su Power BI
- Gestione ricarico e versioning

Esempi rappresentativi:
- **FR014** → TC016 (upload Budget)
- **FR017** → TC019 (caricamento ODS Excel)
- **FR018** → TC020 (report di controllo)
- **FR020** → TC022 (pubblicazione Budget)
- **FR023** → TC025 (anti‑duplicazione ricarico)

---

## Regole di Tracciabilità (vincolanti)

Gli agenti devono garantire che:
- Ogni **requisito FR** abbia **almeno un test associato**
- I requisiti **Alta priorità** siano coperti almeno da:
  - 1 test positivo
  - 1 test negativo (se applicabile)
- Requisiti esposti a Power BI abbiano:
  - test di caricamento dati
  - test di visualizzazione
- I requisiti di ricarico/versioning abbiano test dedicati

---

## Uso Operativo da parte degli Agent

Questa matrice `.md` deve essere utilizzata per:
- verificare copertura dei requisiti in fase di generazione
- segnalare requisiti senza test
- validare coerenza tra:
  - AFU / ATE
  - Diagramma di flusso
  - Storico test case
- supportare calcolo percentuale di copertura

---

## Esempio minimo (strutturale)

| ID_REQUISITO | DESCRIZIONE | PRIORITÀ | ID_TEST | FASE |
|-------------|-------------|----------|--------|------|
| FR### | Descrizione requisito | Alta | TC### | SIT/UAT |
