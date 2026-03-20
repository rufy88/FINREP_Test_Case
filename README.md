# FINREP Test Case Generator

## Overview

**FINREP Test Case Generator** è una soluzione **multi‑agent** progettata per automatizzare la generazione di **casi di test completi, tracciabili e audit‑ready** per sistemi di reportistica bancaria FINREP.

La soluzione è pensata per contesti **enterprise / banking**, caratterizzati da:
- flussi dati complessi
- processi ETL legacy
- requisiti normativi stringenti
- necessità di tracciabilità e ripetibilità

Il repository GitHub rappresenta la **single source of truth** sia per la **Knowledge Base** sia per gli **output di delivery**.

---

## Goals

La soluzione consente di:

- Generare test **funzionali, tecnici, di integrazione e data quality**
- Garantire tracciabilità: Requisito → Processo → Oggetti tecnici → Caso di test
- Standardizzare il formato dei test case su Excel
- Ridurre errori manuali e incoerenze
- Supportare audit, revisione e riuso
- Centralizzare output e versioning su GitHub

---

## Covered Architecture

L’agente copre i seguenti ambiti architetturali:

### Flussi automatici (legacy)
- Ricezione file
- Attivazione TWS
- ETL DataStage
- Scrittura ODS → STG → WRK → TARGET
- Ricalcoli e aggregazioni
- Esposizione dati a Power BI

### Flussi manuali (WebApp Excel)
- Upload Excel
- Controlli formali e sostanziali
- Manipolazioni dati
- Scrittura ODS / TARGET
- Report Excel di validazione
- Conferma utente

---

## Repository Structure

```text
FINREP_Test_Case/
│
├── FINREP_Test_Case_KB/
│   ├── 0_AFU_Progetto_BANKING.docx
│   ├── 1_ATE_PROGETTO_BANKING.docx
│   ├── 1_Storico_Test_Case.xlsx
│   ├── 2_Matrice_requisiti_vs_test_case.xlsx
│   ├── 3_Requisiti_non_funzionali_NFR.xlsx
│   └── 4_Struttura_tabelle.xlsx
│
├── FINREP_Test_Case_Output/
│   └── FINREP_TestCaseSupport_<Scenario>_<Ambito>_<YYYYMMDD>_vNN.xlsx
│
└── README.md
```

## FINREP_Test_Case_KB

Contiene la **Knowledge Base ufficiale del progetto**:

- AFU e ATE  
- Storico test case  
- Requisiti non funzionali  
- Matrici di tracciabilità  
- Struttura tabelle  

---

## FINREP_Test_Case_Output

Contiene gli **output generati automaticamente dall’agente**:

- File Excel dei casi di test  
- Versioning progressivo  
- Tracciabilità commit GitHub  

---

## Multi‑Agent Architecture

La soluzione è basata su **4 agenti distinti**, coordinati da un **Orchestrator**.

### 1. FINREP Orchestrator  
**Ruolo:** governo end‑to‑end  

**Responsabilità:**
- Gestione del flusso completo  
- Coordinamento degli specialisti  
- Controlli di qualità e coerenza  
- Scrittura output su GitHub  
- Generazione report DOCX  
- Invio email di notifica agli stakeholder  

---

### 2. KB / Requirement Parsing Specialist  
**Ruolo:** analisi requisiti  

**Responsabilità:**
- Parsing AFU e ATE  
- Identificazione flussi, tabelle, ETL, TWS  
- Costruzione matrice requisito → processo → oggetti tecnici  
- Individuazione GAP informativi  

---

### 3. Pattern & Test Design Specialist  
**Ruolo:** progettazione dei test  

**Responsabilità:**
- Analisi storico test case  
- Riconoscimento pattern  
- Classificazione processi  
- Progettazione:
  - Test positivi  
  - Test negativi  
  - Test di integrazione  
  - Test ETL / WRK  
  - Test Data Quality  
  - Test WebApp  
  - Test di riconciliazione  

---

### 4. Excel‑Builder Specialist  
**Ruolo:** delivery specialist  

**Responsabilità:**
- Utilizzo del template storico  
- Costruzione Excel finale  
- Rispetto domini e strutture  
- Conversione in Base64  
- Consegna file all’Orchestrator  
  *(senza scrivere su GitHub)*  

---

## Output Generated

### Excel – Test Case Support

File Excel conforme a **1_Storico_Test_Case.xlsx** contenente, per ogni test:

- ID Test  
- Ambito  
- Obiettivo Test  
- Descrizione Test  
- Esito Atteso  
- Owner del test  
- Stato del test  
  *(Da Avviare, In Corso, Chiuso, Annullato)*  
- Esito del test  
  *(N.A., OK, KO)*  
- Note Owner  
- Data inizio  
- Data fine  
- Note tecniche  
  *(processo, tipo test, tabelle, ETL/TWS)*  

---

### Documento DOCX di Resoconto

Generato dall’Orchestrator e inviato via email:

- Sintesi attività  
- Documenti KB utilizzati  
- Copertura requisiti  
- GAP individuati  
- Riferimento file Excel  
- Link alla cartella GitHub di output  

---

## Naming Convention

Gli output Excel seguono la convenzione:

```text
FINREP_TestCaseSupport_<Scenario>_<Ambito>_<YYYYMMDD>_vNN.xlsx
```

## GitHub as Source of Truth

GitHub viene utilizzato come:

- Repository di conoscenza ufficiale  
- Sistema di versioning  
- Tracciabilità delle delivery  
- Punto di accesso per audit e revisione  

Ogni generazione è:

- Ripetibile  
- Versionata  
- Verificabile via commit history  

---

## Compliance & Audit

La soluzione è progettata per garantire:

- Tracciabilità end‑to‑end  
- Auditabilità  
- Ripetibilità  
- Separazione delle responsabilità  
- Riduzione del rischio operativo  

---

## Status

✅ Architettura multi‑agent definita  
✅ Integrazione GitHub read/write  
✅ Output Excel standardizzato  
✅ Reporting DOCX e email delivery  

---

## Next Steps (optional)

- Dashboard di copertura requisiti  
- Integrazione CI/CD  
- Estensione a ulteriori domini regolamentari  
- KPI di qualità test  

---

## Maintainers

- **Solution Owner:** FINREP GenAI Team  
- **Repository Owner:** Project Team  
