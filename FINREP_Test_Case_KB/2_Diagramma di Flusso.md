# 2_Diagramma_di_Flusso — Logica dei Flussi FINREP (Shadow Semantica)

Fonte ufficiale: `2_Diagramma di Flusso.docx`.

Questo documento descrive in forma testuale e strutturata
la logica dei flussi FINREP rappresentati nei diagrammi,
per consentire agli agenti di:
- comprendere le sequenze operative
- classificare correttamente i processi
- generare test end‑to‑end coerenti

---

## 1️⃣ Flussi Automatici da Sistemi Legacy

### File gestiti
I sistemi legacy generano i seguenti file:
- CECORE01
- CEBRNCH1
- GESTCST1
- RICAVNET
- RISKADJ1

---

## Step 1 – Ricezione File (MFT)

Sequenza logica:
1. Generazione file da sistema legacy
2. Trasferimento tramite MFT
3. Deposito in cartella di atterraggio
4. Trigger schedulazione TWS dedicata

Schedulazioni osservate:
- TWS_CECORE01_LOAD
- TWS_CEBRNCH1_LOAD
- TWS_GESTCST1_LOAD
- TWS_RICAVNET_LOAD
- TWS_RISKADJ1_LOAD

---

## Step 2 – Avvio ETL DataStage

Ogni schedulazione TWS avvia un ETL dedicato:
- ETL_CECORE01_PROC
- ETL_CEBRNCH1_PROC
- ETL_GESTCST1_PROC
- ETL_RICAVNET_PROC
- ETL_RISKADJ1_PROC

---

## Tipologie di Flusso ETL

### Tipologia A — ETL con WRK (3 file)
Applicata a:
- CECORE01
- GESTCST1
- RISKADJ1

Sequenza end‑to‑end:
1. LEGACY
2. MFT
3. TWS_xxx
4. ETL_xxx
5. ODS_xxx_RAW  
   (PRC_LOAD_ODS_xxx)
6. STG_xxx_AGG  
   (PRC_STG_xxx – filtri e aggregazioni)
7. WRK_xxx_CALC  
   (PRC_WRK_xxx – ricalcoli)
8. TGT_xxx  
   (PRC_TGT_xxx)

---

### Tipologia B — ETL diretto (2 file)
Applicata a:
- CEBRNCH1
- RICAVNET

Sequenza end‑to‑end:
1. LEGACY
2. MFT
3. TWS_xxx
4. ETL_xxx
5. ODS_xxx_RAW
6. STG_xxx  
   (filtri e aggregazioni)
7. TGT_xxx

(Nessun passaggio WRK)

---

## 2️⃣ Flusso Upload Manuale Excel (WebApp)

### Sequenza logica

1. **Utente**
2. **FINREP Web Portal**
3. Upload file Excel
4. Controlli strutturali e formali
   - se KO → blocco processo + report errori
5. Caricamento in:
   - ODS_EXCEL_MANUALI  
     (PRC_LOAD_ODS_EXCEL)
6. Generazione preview:
   - RPT_FINREP_PREVIEW
7. Generazione report:
   - FINREP_Control_Report.xlsx
8. **Conferma Pubblicazione**
9. Scrittura in tabelle target:
   - TGT_BUDGET
   - TGT_FORECAST
   - TGT_DRIVER
   - TGT_KPI

---

## 3️⃣ Consolidamento Notturno

Sequenza:
1. Avvio schedulazione:
   - TWS_FINREP_DAILY_CONSOL
2. Esecuzione procedura:
   - PRC_BUILD_FACT_FINREP
3. Lettura di tutte le tabelle TGT
4. Uniformazione chiavi dimensionali
5. Scrittura nella fact table finale:
   - FCT_FINREP_ECONOMICO

---

## Relazioni tra Flussi

- I flussi automatici e manuali **convergono** nelle tabelle target
- Tutti i target alimentano il consolidamento notturno
- Il consolidamento produce **un’unica FACT certificata**
- La FACT alimenta il modello a stella per Power BI

---

## Implicazioni per la Generazione Test

Per ogni flusso devono essere generati test che coprano:
- ricezione file
- attivazione TWS
- avvio ETL
- caricamento ODS
- regole STG
- ricalcoli WRK (se presenti)
- scrittura target
- consolidamento
- verifica Power BI

Per il flusso WebApp:
- test upload positivo
- test negativi su controlli
- test preview/report
- test pubblicazione
- test consolidamento

---

## Nota per gli Agent
Questo `.md` deve essere utilizzato:
- per classificare correttamente i requisiti
- per costruire catene di test end‑to‑end
- come riferimento di sequenza logica

Le immagini del `.docx` NON vanno interpretate direttamente dagli agenti.
