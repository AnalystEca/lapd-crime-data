# Plan — Summary

**Stato**: Completata
**Data inizio**: 10-04-2026
**Data chiusura**: 11-04-2026

## Obiettivi della fase
- Definire lo scope del progetto e le domande guida
- Identificare il dataset LAPD e verificarne disponibilità e licenza
- Valutare la struttura del dataset: colonne, tipologie di dati, periodo coperto
- Definire metodologia di analisi e strumenti
- Impostare struttura cartelle e inizializzare repository GitHub
- Redigere il PACE Strategy Document
- Definire i tre deliverable finali e i rispettivi destinatari

## Task completati
<!-- Formato: - [x] **Nome task** — `nome_notebook.ipynb` -->
- [x] **Identificazione e download dei dataset LAPD** (Crime Data 2010–2019 e 2020–2024 da data.lacity.org)
- [x] **Setup struttura cartelle del progetto** (data/raw, data/processed, notebooks, src, outputs, reports)
- [x] **Inizializzazione repository GitHub** con `.gitignore`, `README.md`, `requirements.txt`
- [x] **Riorganizzazione notebook secondo framework PACE** (sottocartelle 01_plan, 02_analyze, 03_construct, 04_execute)
- [x] **Caricamento iniziale dei due dataset in pandas** — `01_data_loading.ipynb`
- [x] **Verifica e allineamento delle colonne tra i due CSV** — `01_data_loading.ipynb`
- [x] **Concatenazione dei due dataset in un unico DataFrame** — `01_data_loading.ipynb`
- [x] **Ispezione iniziale**: shape, dtypes, duplicati, nulli, distribuzione per anno — `01_data_loading.ipynb`
- [x] **Documentazione delle anomalie e definizione delle decisioni operative** — `01_data_loading.ipynb`

## Decisioni chiave

1. **Periodo di analisi**: 2010–2024 completo, anziché solo 2015–2024 come da bozza iniziale del PACE doc. Motivazione: più dati permettono analisi temporali più ricche, e il dataset 2010–2019 è completo e di buona qualità.

2. **Gestione del 2024 incompleto**: il 2024 contiene solo 127.567 record (vs ~230k attesi). Verrà mantenuto nel dataset pulito ma escluso dalle analisi di trend annuali. La mensilità di troncamento verrà verificata in fase EDA.

3. **Formato dataset pulito**: Parquet anziché CSV. Motivazioni: ~10x più veloce in lettura/scrittura, file ~6x più piccoli, conserva i tipi di dato (no riconversione delle date). Aggiunta libreria `pyarrow` a `requirements.txt`.

4. **Granularità del cleaning**: completo, non minimale. Il PACE doc fornisce 4 domande guida ma altre verranno aggiunte durante il progetto. Un cleaning completo costruisce una base solida e riutilizzabile.

5. **Feature engineering separato dal cleaning**: notebook dedicato (`02_feature_engineering.ipynb`) eseguito dopo il cleaning, in modo da poter raffinare le domande analitiche tra una fase e l'altra.

6. **Colonne da eliminare**: `Crm Cd 2`, `Crm Cd 3`, `Crm Cd 4` (>93% nulli), `Cross Street` (84% nulli, ridondante con LOCATION + LAT/LON).

7. **Mocodes mantenuti**: per analisi successive sui pattern dei modus operandi, in particolare in tema di crimini violenti.

8. **Gestione duplicati DR_NO**: 57.809 duplicati identificati (~1.8%). Ispezione caso per caso in fase EDA prima di definire la strategia di rimozione.

## Risultati principali

- **Dimensioni dataset concatenato**: 3.138.031 righe × 28 colonne (~670 MB in memoria)
- **Duplicati su DR_NO**: 57.809 (~1.8%)
- **Colonne con nulli rilevanti**:
  - `Crm Cd 4` (99.99%), `Crm Cd 3` (99.81%), `Crm Cd 2` (93.27%)
  - `Cross Street` (83.71%)
  - `Weapon Used Cd` / `Weapon Desc` (66.73%) — semantica "nessuna arma"
  - `Mocodes` (12.15%)
  - `Vict Sex` / `Vict Descent` (10.92%)
- **Anomalie temporali identificate**:
  - Calo anomalo nel 2015 (168k record) e picco nel 2016 (284k record), probabilmente legati alla transizione del sistema di classificazione LAPD
  - Anno 2024 incompleto (127k record vs ~230k attesi)
- **Tipi di dato problematici**: `Date Rptd` e `DATE OCC` come stringhe, `LAT`/`LON` come object invece che float, `TIME OCC` come intero in formato HHMM

## Problemi e soluzioni

- **Disallineamento colonne tra i due CSV**: la colonna `AREA` aveva uno spazio finale nel dataset 2010–2019 (`'AREA '`). Risolto applicando `.str.strip()` ai nomi colonna di entrambi i DataFrame.

- **Path dei notebook dopo riorganizzazione PACE**: i notebook spostati in sottocartelle hanno richiesto l'aggiornamento dei path da `../data/raw/...` a `../../data/raw/...`.

## Output prodotti
<!-- Formato: - `path/al/file.ext` ← prodotto da `nome_notebook.ipynb` -->
- `notebooks/01_plan/01_data_loading.ipynb` — notebook di caricamento e ispezione iniziale del dataset

## Note per la fase successiva

La fase Analyze deve:

1. **Ricaricare i CSV grezzi** e ripetere il merge (il notebook 02 deve essere autosufficiente)
2. **Eseguire il cleaning completo**: eliminazione colonne inutili, conversione tipi di dato, gestione nulli, ricodifica categorie, gestione duplicati, gestione outlier
3. **Salvare il dataset pulito** in `data/processed/crimes_clean.parquet`
4. **Verificare il troncamento del 2024** (in che mese si ferma)
5. **Ispezionare alcuni casi di duplicati DR_NO** per definire la strategia di rimozione
6. **Indagare il problema di tipo su `LAT`/`LON`** (perché sono object invece che float)
7. **Documentare ogni decisione di pulizia** direttamente nel notebook con celle markdown