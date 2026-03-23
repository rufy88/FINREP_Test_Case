# 3_Requisiti_non_funzionali_NFR — Requisiti Non Funzionali (Shadow Semantica)

Fonte ufficiale: `3_Requisiti_non_funzionali_NFR.xlsx`.

## Scopo
Questo documento descrive i **Requisiti Non Funzionali (NFR)** del sistema FINREP
e la loro **tracciabilità verso i casi di test**.

È utilizzato dagli agenti per:
- integrare i NFR nella generazione dei test
- garantire copertura di performance, audit e qualità
- evitare che i test si limitino ai soli aspetti funzionali

---

## Struttura dei Requisiti NFR

Ogni requisito NFR è descritto tramite:

1. **ID_REQUISITO**
   - Formato osservato: `NFR###`

2. **TIPO**
   - Valore osservato: `NFR`

3. **DESCRIZIONE REQUISITO**
   - Descrizione del vincolo non funzionale

4. **PRIORITÀ**
   - Valore osservato: `Alta`

5. **ID_TEST ASSOCIATI**
   - Elenco dei test che validano il requisito

6. **FASE TEST**
   - Fase di validazione prevista:
     - `SIT`
     - `UAT`
     - `Performance Test`

7. **NOTE**
   - Dettagli operativi (quando presenti)

---

## Elenco Requisiti Non Funzionali

### NFR001 — Performance
- **Descrizione:**  
  Il sistema deve processare **1 milione di record entro 30 minuti**
- **Test associato:** TC013
- **Fase:** Performance Test
- **Nota:** SLA 30 min

---

### NFR002 — Audit & Tracciabilità
- **Descrizione:**  
  Il sistema deve garantire la **tracciabilità del file caricato**
- **Test associati:** TC004, TC019
- **Fase:** SIT
- **Nota:** Audit columns

---

### NFR003 — Consistenza Dati
- **Descrizione:**  
  Il sistema deve garantire la **consistenza dei dati tra Target e Fact**
- **Test associato:** TC011
- **Fase:** SIT

---

### NFR004 — Coerenza Reporting
- **Descrizione:**  
  Il sistema deve garantire la **coerenza dei dati tra Fact e Power BI**
- **Test associati:** TC015, TC024
- **Fase:** UAT

---

### NFR005 — Logging & Error Handling
- **Descrizione:**  
  Il sistema deve **loggare gli errori di processo**
- **Test associati:** TC002, TC017
- **Fase:** SIT

---

## Classificazione dei NFR (per uso Agent)

I NFR devono essere considerati come:

- **Performance**
  - Volumi elevati
  - SLA temporali

- **Audit & Compliance**
  - Tracciabilità file
  - Audit columns
  - Logging errori

- **Data Quality & Coerenza**
  - Consistenza Target ↔ Fact
  - Coerenza Fact ↔ Reporting

---

## Regole operative per la Generazione Test

Gli agenti devono garantire che:
- Ogni NFR abbia **almeno un test dedicato**
- I NFR di tipo **Performance** generino test:
  - con volumi significativi
  - con verifica esplicita dello SLA
- I NFR di tipo **Audit** includano test su:
  - colonne di tracciamento
  - log applicativi
- I NFR di tipo **Coerenza Reporting** includano:
  - confronti tra FACT e Power BI

---

## Integrazione con la KB

Questo documento deve essere utilizzato insieme a:
- `0_AFU_Progetto_BANKING.md`
- `1_ATE_PROGETTO_BANKING.md`
- `1_Storico_Test_Case.md`
- `2_Diagramma_di_Flusso.md`
- `2_Matrice_requisiti_vs_test_case.md`

per garantire:
- copertura funzionale + non funzionale
- tracciabilità completa
- supporto ad audit e revisione
``
