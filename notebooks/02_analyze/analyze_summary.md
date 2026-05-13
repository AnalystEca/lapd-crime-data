# Analyze — Summary

**Stato**: In corso
**Data inizio**: 20-04-2026
**Data chiusura**: 

## Obiettivi della fase
- Eseguire il cleaning completo del dataset combinato
- Analisi univariata: distribuzione crimini, anni, distretti, sesso/età vittime
- Analisi bivariata: correlazioni tra variabili chiave
- Identificare e documentare anomalie nel dataset
- Verificare la completezza per anno
- Feature engineering

## Task completati
<!-- Formato: - [x] **Nome task** — `nome_notebook.ipynb` -->

### Cleaning — `02_cleaning.ipynb`
- [x] **Caricamento dataset da `crimes_merged.parquet`** — `02_cleaning.ipynb`
- [x] **Eliminazione colonne non rilevanti** (`Crm Cd 2/3/4`, `Cross Street`: 28 → 24 colonne) — `02_cleaning.ipynb`
- [x] **Conversione date** (`DATE OCC`, `Date Rptd` → datetime64) — `02_cleaning.ipynb`
- [x] **Conversione orari** (`TIME OCC` intero HHMM → `hour_occ` 0-23, colonna originale eliminata) — `02_cleaning.ipynb`
- [x] **Gestione coordinate sentinella** (3.148 record con (0,0) → NaN) — `02_cleaning.ipynb`
- [x] **Rimozione duplicati esatti** (57.809 righe rimosse, tutti duplicati identici su tutte le colonne, distribuiti su 122 tipi di crimine) — `02_cleaning.ipynb`
- [x] **Ricodifica nulli Weapon** (`Weapon Desc` → "No Weapon", `Weapon Used Cd` → 0) — `02_cleaning.ipynb`
- [x] **Ricodifica nulli Vict Sex/Descent** (NaN → "X") — `02_cleaning.ipynb`
- [x] **Pulizia valori anomali Vict Sex** (H, N, - → "X", 204 record) — `02_cleaning.ipynb`
- [x] **Gestione sentinelle Vict Age** (632.399 valori ≤ 0 → NaN) — `02_cleaning.ipynb`
- [x] **Rimozione righe con Premis Desc nullo** (775 righe) — `02_cleaning.ipynb`
- [x] **Pulizia residua** (2 nulli in Status, 21 in Crm Cd 1 → drop, 23 righe) — `02_cleaning.ipynb`
- [x] **Verifica finale e salvataggio `crimes_clean.parquet`** — `02_cleaning.ipynb`

### Feature Engineering — `04_feature_engineering.ipynb`
- [x] **Feature temporali**: `year`, `month`, `day_of_week` estratte da `DATE OCC` — `04_feature_engineering.ipynb`
- [x] **Fascia oraria**: `hour_bins` con 6 fasce tramite `pd.cut()` su `hour_occ` — `04_feature_engineering.ipynb`
- [x] **Fasce di età**: `age_group` con 5 categorie tramite `pd.cut()` su `Vict Age` — `04_feature_engineering.ipynb`
- [x] **Macro-categoria crimine**: `crime_category` (Person / Property / Other) tramite `np.select()` — `04_feature_engineering.ipynb`
- [x] **Report delay**: `report_delay` (differenza in giorni tra `Date Rptd` e `DATE OCC`) — `04_feature_engineering.ipynb`
- [x] **Flag reati domestici**: `is_domestic` (booleana) basata su parole chiave in `Crm Cd Desc` — `04_feature_engineering.ipynb`
- [x] **Verifica finale e salvataggio `crimes_features.parquet`** — `04_feature_engineering.ipynb`

### EDA Blocco 1 — `05_eda_block1.ipynb`
- [x] **Q1.1 — Top 5 crimini più frequenti e trend YoY** (line chart) — `05_eda_block1.ipynb`
- [x] **Q1.2 — Crimini Person vs Property trend YoY** (line chart) — `05_eda_block1.ipynb`
- [x] **Q1.3 — Categorie con maggior crescita/decrescita YoY** (bar chart + heatmap top 15) — `05_eda_block1.ipynb`
- [x] **Q1.4 Parte 1 — Pattern stagionali macro-categorie** (line chart media mensile) — `05_eda_block1.ipynb`
- [x] **Q1.4 Parte 2 — Approfondimento stagionale top 5 Property** (line chart) — `05_eda_block1.ipynb`
- [x] **Q1.5 — Distribuzione per fascia oraria e giorno della settimana** (2 heatmap affiancate) — `05_eda_block1.ipynb`

### EDA Blocchi 2-5
- [ ] **Q2.1-2.3 — Profilo demografico vittime** — `06_eda_block2.ipynb` (da creare)
- [ ] **Q3.1-3.4 — Abusi domestici e sicurezza minori** — da creare
- [ ] **Q4.1-4.2 — Reati con arma da fuoco** — da creare
- [ ] **Q5.1-5.2 — Efficacia della risposta** — da creare

### EDA fase avanzata Blocchi 6-7
- [ ] **Analisi geospaziale avanzata e modelli predittivi** — da creare

## Decisioni chiave

### Cleaning
1. **Colonne eliminate**: `Crm Cd 2/3/4` e `Cross Street`. Da 28 a 24 colonne.
2. **`TIME OCC` → `hour_occ`**: intero HHMM convertito in ora intera 0-23.
3. **Coordinate sentinella (0, 0) → NaN**: 3.148 record corretti.
4. **Duplicati esatti rimossi**: 57.809 righe, errore generalizzato di caricamento.
5. **Vict Sex: solo M, F, X**: valori rari ricodificati.
6. **Vict Age ≤ 0 → NaN**: 632.399 valori sentinella/errore.
7. **Mocodes lasciati con NaN**: mancanza legittima.

### Feature Engineering
8. **Fasce orarie da 4 ore**: 6 fasce. Artefatto mezzanotte incluso in "Night", gestito in EDA.
9. **Fasce età allineate al Blocco 3**: Child (0-12) e Adolescent (13-17) per analisi abusi domestici.
10. **Classificazione Person/Property/Other manuale**: 143 crimini classificati. Da validare con domain expert in scenario reale.
11. **Flag `is_domestic`**: 225.938 reati domestici (7.3%).

### EDA Blocco 1
12. **Filtro soglia 500 occorrenze (Q1.3)**: esclude crimini rari per evitare variazioni percentuali distorte.
13. **Media mensile su 15 anni (Q1.4)**: conteggio per anno+mese → media per mese. Evita bias da anni con più dati.
14. **Heatmap Q1.5 con cmap RdYlGn_r**: rosso = più crimini, verde = meno crimini.

## Risultati principali

### Post-cleaning
- **Dataset pulito**: 3.079.424 righe × 24 colonne
- **Range DATE OCC**: 1 gennaio 2010 → 30 dicembre 2024
- **15 anni**, **21 aree LAPD**, **143 tipi di crimine**

### Post-feature engineering
- **Dataset arricchito**: 3.079.424 righe × 32 colonne
- **Distribuzione crime_category**: Property 63%, Person 35.5%, Other 1.5%
- **Reati domestici**: 225.938 (7.3%)
- **Report delay**: mediana 1 giorno, media 21 giorni, max 5.407 giorni (~14.8 anni)

### EDA Blocco 1 — Scoperte principali
- **Vehicle Theft**: unico crimine in crescita dal 2020. Fenomeno nazionale documentato (da citare: FBI UCR, NICB).
- **Anomalie 2015-2016**: calo generalizzato 2015, rimbalzo brusco 2016. Ipotesi: transizione sistema classificazione LAPD. Da verificare con fonti esterne.
- **Shoplifting (Petty) strutturale**: +87% nel 2023, tendenza non in rientro. Correlata alla Prop 47 California.
- **Identity Theft temporaneo**: +95% nel 2022, -39% nel 2023. Fenomeno post-pandemia.
- **Stagionalità Property**: picchi a marzo (effetto aggregato post-febbraio) e ottobre (guidato da Vehicle Theft).
- **Identity Theft stagionalità inversa**: alta in inverno, bassa in estate. Ipotesi: attività online (acquisti natalizi, dichiarazioni fiscali).
- **Divergenza dicembre**: tutti i Property aumentano tranne Vehicle Theft.
- **Picco notturno weekend (Person)**: domenica notte 34.062, sabato notte 28.763. Pattern movida notturna.
- **Venerdì picco Property**: Afternoon 70.800, Evening 73.995 — valori massimi assoluti.
- **Early Morning fascia più sicura**: minimi per entrambe le categorie in tutti i giorni.

## Problemi e soluzioni
- **2024 non troncato**: il calo è reale, non un artefatto del dataset.
- **SettingWithCopyWarning**: risolto con `.copy()` sui DataFrame filtrati.
- **FutureWarning groupby**: risolto con `observed=True` sui groupby su colonne `category`.
- **Notebook non salvato**: recuperato via git pull. Lezione: committare frequentemente.

## Output prodotti
- `notebooks/02_analyze/02_cleaning.ipynb`
- `notebooks/02_analyze/04_feature_engineering.ipynb`
- `notebooks/02_analyze/05_eda_block1.ipynb`
- `data/processed/crimes_clean.parquet` — 3.079.424 × 24 colonne
- `data/processed/crimes_features.parquet` — 3.079.424 × 32 colonne
- `outputs/05_eda_block1/01_top5_crimes_trend.png`
- `outputs/05_eda_block1/02_person_vs_property_trend.png`
- `outputs/05_eda_block1/03_variation_top5_bottom5_2023.png`
- `outputs/05_eda_block1/04_heatmap_yoy_variation_top15.png`
- `outputs/05_eda_block1/05_seasonal_patterns_macro.png`
- `outputs/05_eda_block1/06_seasonal_patterns_property_top5.png`
- `outputs/05_eda_block1/07_heatmap_time_distribution.png`

## Note per la fase successiva

1. **EDA Blocco 2** (`06_eda_block2.ipynb`): profilo demografico vittime (Q2.1-2.3)
2. **Scaricare lookup table Mocodes** (`MO_CODES.txt`) prima del Blocco 3
3. **Grafici Blocco 2** salvati in `outputs/06_eda_block2/` con numerazione sequenziale da 08_
4. **Fonti esterne da raccogliere** per relazione finale: Vehicle Theft post-COVID (FBI UCR, NICB), anomalie 2015-2016, Prop 47 California
