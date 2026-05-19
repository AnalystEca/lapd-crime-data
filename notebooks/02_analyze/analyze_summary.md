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
- [x] **Rimozione duplicati esatti** (57.809 righe rimosse) — `02_cleaning.ipynb`
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

### EDA Blocco 2 — `06_eda_block2.ipynb`
- [x] **Q2.1 — Distribuzione vittime per sesso** (donut chart) — `06_eda_block2.ipynb`
- [x] **Q2.1 — Distribuzione vittime per fascia di età** (donut chart) — `06_eda_block2.ipynb`
- [x] **Q2.1 — Distribuzione vittime per etnia** (bar chart orizzontale con lookup table codici LAPD) — `06_eda_block2.ipynb`
- [x] **Q2.2 — Profilo vittime per macro-categoria** (bar chart raggruppato Person vs Property per fascia età e sesso) — `06_eda_block2.ipynb`
- [x] **Q2.3 — Top 10 crimini contro vittime Senior 65+** (bar chart orizzontale con hue crime_category) — `06_eda_block2.ipynb`
- [x] **Q2.3 — Trend temporale top 5 crimini contro Senior** (line chart) — `06_eda_block2.ipynb`

### EDA Blocchi 3-5
- [ ] **Q3.1-3.4 — Abusi domestici e sicurezza minori** — `07_eda_block3.ipynb` (da creare)
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

### EDA Blocco 2
15. **Donut chart per distribuzioni a poche categorie**: usato per sesso (3 valori) e età (5 valori). Bar chart orizzontale per etnia (20 valori).
16. **Soglia 10.000 per raggruppamento etnie**: etnie con meno di 10.000 occorrenze aggregate in "Other/Small Groups" per leggibilità.
17. **Lookup table codici etnia LAPD**: mappatura dei codici lettera singola (H, B, W, ecc.) in etichette descrittive. Fonte: LAPD Crime Data Dictionary su data.lacity.org.
18. **Top 5 per trend Senior**: limitato ai 5 crimini più frequenti (su 10 del bar chart) per mantenere leggibilità del line chart.

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
- **Vehicle Theft**: unico crimine in crescita dal 2020. Da citare: FBI UCR, NICB.
- **Anomalie 2015-2016**: calo 2015, rimbalzo 2016. Ipotesi: transizione sistema classificazione LAPD.
- **Shoplifting (Petty) strutturale**: +87% nel 2023. Correlata alla Prop 47 California.
- **Identity Theft temporaneo**: +95% nel 2022, -39% nel 2023.
- **Stagionalità Property**: picchi a marzo (effetto aggregato) e ottobre (Vehicle Theft).
- **Picco notturno weekend (Person)**: domenica notte 34.062. Pattern movida notturna.
- **Venerdì picco Property**: Afternoon 70.800, Evening 73.995.
- **Early Morning fascia più sicura**: minimi per entrambe le categorie.

### EDA Blocco 2 — Scoperte principali
- **Distribuzione sesso**: M 44.2%, F 40.0%, X 15.9%. Gap contenuto, attenuato dalla presenza di crimini domestici dove le donne sono sovrarappresentate.
- **Distribuzione età**: Adult 47.4%, Young Adult 40.5% dei casi con età nota. Child 1.8%, Adolescent 3.4%. Nota: sottodenuncia elevata per i minori.
- **Distribuzione etnia**: Hispanic/Latin/Mexican prima categoria (32.7%), White seconda (22.9%), Black terza (15.1%). Senza dati di popolazione non è possibile valutare sovra/sottorappresentazione.
- **Pattern di genere invertito tra Property e Person**: nei crimini Property i maschi sono più colpiti nelle fasce adulte; nei crimini Person le femmine sono sovrarappresentate in tutte le fasce tranne Senior.
- **Child e Adolescent femmine sovrarappresentate nei crimini Person**: bambine 23.336 vs 16.436 maschi; adolescenti 41.717 vs 26.823 maschi. Pattern coerente con abusi sessuali e violenza domestica. Approfondimento nel Blocco 3.
- **Senior: 8 crimini su 10 sono Property**. Burglary (23.729) e Identity Theft (20.851) sono i primi due. Simple Assault unico crimine Person con trend strutturalmente crescente dal 2010 al 2023.
- **Identity Theft Senior**: calo 2017-2020, esplosione 2020-2023 correlata al COVID e all'aumento forzato dell'uso di servizi digitali da parte degli anziani.

## Problemi e soluzioni
- **2024 non troncato**: il calo è reale, non un artefatto del dataset.
- **SettingWithCopyWarning**: risolto con `.copy()` sui DataFrame filtrati.
- **FutureWarning groupby**: risolto con `observed=True` sui groupby su colonne `category`.
- **Notebook non salvato**: recuperato via git pull. Lezione: committare frequentemente.

## Output prodotti
- `notebooks/02_analyze/02_cleaning.ipynb`
- `notebooks/02_analyze/04_feature_engineering.ipynb`
- `notebooks/02_analyze/05_eda_block1.ipynb`
- `notebooks/02_analyze/06_eda_block2.ipynb`
- `data/processed/crimes_clean.parquet` — 3.079.424 × 24 colonne
- `data/processed/crimes_features.parquet` — 3.079.424 × 32 colonne
- `outputs/05_eda_block1/01_top5_crimes_trend.png`
- `outputs/05_eda_block1/02_person_vs_property_trend.png`
- `outputs/05_eda_block1/03_variation_top5_bottom5_2023.png`
- `outputs/05_eda_block1/04_heatmap_yoy_variation_top15.png`
- `outputs/05_eda_block1/05_seasonal_patterns_macro.png`
- `outputs/05_eda_block1/06_seasonal_patterns_property_top5.png`
- `outputs/05_eda_block1/07_heatmap_time_distribution.png`
- `outputs/06_eda_block2/08_donut_victim_sex.png`
- `outputs/06_eda_block2/09_donut_victim_age.png`
- `outputs/06_eda_block2/10_barh_victim_descent.png`
- `outputs/06_eda_block2/11_barplot_victim_age_sex_property.png`
- `outputs/06_eda_block2/12_barplot_victim_age_sex_person.png`
- `outputs/06_eda_block2/13_barh_top10_crimes_senior.png`
- `outputs/06_eda_block2/14_lineplot_top5_crimes_senior_trend.png`

## Note per la fase successiva

1. **EDA Blocco 3** (`07_eda_block3.ipynb`): abusi domestici e sicurezza minori (Q3.1-3.4)
2. **Scaricare lookup table Mocodes** (`MO_CODES.txt`) prima di affrontare Q3.4
3. **Grafici Blocco 3** salvati in `outputs/07_eda_block3/` con numerazione sequenziale da 15_
4. **Fonti esterne da raccogliere** per relazione finale: Vehicle Theft post-COVID (FBI UCR, NICB), anomalie 2015-2016, Prop 47 California, Identity Theft anziani e COVID
