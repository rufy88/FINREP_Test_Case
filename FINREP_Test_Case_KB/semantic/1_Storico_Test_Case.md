# 1_Storico_Test_Case — Storico Test Case (Shadow Semantica)

Fonte ufficiale: `1_Storico_Test_Case.xlsx`.

## Scopo
Questo documento descrive:
- la struttura del template storico dei test case
- i domini e le regole operative osservate nello storico
- i pattern ricorrenti (legacy e WebApp)
- esempi rappresentativi (minimi) per guidare la generazione dei nuovi test

---

## Struttura del Template (colonne)

Le colonne utilizzate nello storico sono:

1. **ID_TEST**
   - Formato osservato: `TC###` (es. TC001, TC025) 
   - Deve essere univoco

2. **AMBITO**
   - Valori osservati nello storico (esempi):  
     `MFT`, `Naming File`, `TWS`, `ETL`, `STG`, `WRK`, `TGT`, `Consolidamento`,  
     `Integrità FK`, `Performance`, `Power BI`, `WebApp`, `ODS`, `RPT`,  
     `Pubblicazione`, `Reprocessing` )

3. **OBIETTIVO TEST**
   - Sintesi dell’intento del test (es. “Verifica ricezione file”) 

4. **DESCRIZIONE TEST**
   - Azione operativa / scenario (cosa fare) (es. “Inserire file CECORE01 in cartella MFT”) 

5. **ESITO ATTESO**
   - Risultato verificabile (es. “Processo bloccato”, “Record presenti con valori coerenti”) 

6. **OWNER DEL TEST**
   - Ruoli osservati: `IT Operations`, `Batch Control`, `Data Engineer`,
     `Data Warehouse Team`, `BI Analyst`, `Business User`, `Business Controller` 

7. **STATO DEL TEST**
   - Nello storico compare “Da eseguire” come stato iniziale
   - Nel nuovo processo target (Orchestrator) lo stato deve essere mappato nel dominio:
     `Da Avviare | In Corso | Chiuso | Annullato`

8. **DATA INIZIO**
   - Formato osservato: gg/mm/aaaa (es. 03/01/2026) 

9. **DATA FINE**
   - Nello storico può essere assente su alcune righe (non sempre valorizzata)  
   - Nel nuovo processo target deve essere: `DATA INIZIO + 2 giorni`

10. **NOTE**
   - Campo note tecniche/operative (es. “SLA 30 min”) 

---

## Sezioni logiche presenti nello storico

