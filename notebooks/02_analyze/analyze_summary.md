# Analyze — Summary

**Stato**: Completato (tutte le fasi)
**Data inizio**: 20-04-2026
**Data chiusura**: 23-07-2026

## Obiettivi della fase
- Eseguire il cleaning completo del dataset combinato
- Analisi univariata: distribuzione crimini, anni, distretti, sesso/età vittime
- Analisi bivariata: correlazioni tra variabili chiave
- Identificare e documentare anomalie nel dataset
- Verificare la completezza per anno
- Feature engineering

## Task completati

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
- [x] **Feature temporali**: `year`, `month`, `day_of_week` estratte da `DATE OCC`
- [x] **Fascia oraria**: `hour_bins` con 6 fasce tramite `pd.cut()` su `hour_occ`
- [x] **Fasce di età**: `age_group` con 5 categorie tramite `pd.cut()` su `Vict Age`
- [x] **Macro-categoria crimine**: `crime_category` (Person / Property / Other) tramite `np.select()`
- [x] **Report delay**: `report_delay` (differenza in giorni tra `Date Rptd` e `DATE OCC`)
- [x] **Flag reati domestici**: `is_domestic` (booleana) basata su parole chiave in `Crm Cd Desc`
- [x] **Verifica finale e salvataggio `crimes_features.parquet`**

### EDA Blocco 1 — `05_eda_block1.ipynb`
- [x] **Q1.1 — Top 5 crimini più frequenti e trend YoY** (line chart)
- [x] **Q1.2 — Crimini Person vs Property trend YoY** (line chart)
- [x] **Q1.3 — Categorie con maggior crescita/decrescita YoY** (bar chart + heatmap top 15)
- [x] **Q1.4 Parte 1 — Pattern stagionali macro-categorie** (line chart media mensile)
- [x] **Q1.4 Parte 2 — Approfondimento stagionale top 5 Property** (line chart)
- [x] **Q1.5 — Distribuzione per fascia oraria e giorno della settimana** (2 heatmap affiancate)

### EDA Blocco 2 — `06_eda_block2.ipynb`
- [x] **Q2.1 — Distribuzione vittime per sesso** (donut chart)
- [x] **Q2.1 — Distribuzione vittime per fascia di età** (donut chart)
- [x] **Q2.1 — Distribuzione vittime per etnia** (bar chart orizzontale)
- [x] **Q2.2 — Profilo vittime per macro-categoria** (bar chart raggruppato Person vs Property)
- [x] **Q2.3 — Top 10 crimini contro vittime Senior 65+** (bar chart orizzontale)
- [x] **Q2.3 — Trend temporale top 5 crimini contro Senior** (line chart)

### EDA Blocco 3 — `07_eda_block3.ipynb`
- [x] **Q3.1 — Volume reati domestici** (donut chart domestici vs non-domestici)
- [x] **Q3.1 — Trend annuale reati domestici 2010-2024** (line chart)
- [x] **Q3.1 — Distribuzione reati domestici per area LAPD** (bar chart orizzontale)
- [x] **Q3.2 — Confronto profilo vittime per sesso** (2 donut affiancati)
- [x] **Q3.2 — Confronto profilo vittime per fascia di età** (2 donut affiancati)
- [x] **Q3.2 — Confronto profilo vittime per etnia** (2 bar chart affiancati)
- [x] **Q3.3 — Crimini domestici contro minori: Child vs Adolescent** (2 bar chart affiancati)
- [x] **Q3.4 — Pattern Mocodes nei reati domestici** (bar chart top 30 codici)

### EDA Blocco 4 — `08_eda_block4.ipynb`
- [x] **Q4.1 — Distribuzione reati con arma da fuoco per area LAPD** (bar chart)
- [x] **Q4.1 — Volume reati per tipo di arma** (bar chart orizzontale)
- [x] **Q4.1 — Trend annuale per tipo di arma** (2 line chart affiancati: armi reali vs simulate)
- [x] **Q4.2 — Distribuzione vittime per sesso** (donut chart)
- [x] **Q4.2 — Distribuzione vittime per fascia di età** (donut chart)
- [x] **Q4.2 — Distribuzione vittime per etnia** (bar chart orizzontale)

### EDA Blocco 5 — `09_eda_block5.ipynb`
- [x] **Q5.1 — Clearance rate per area LAPD** (bar chart orizzontale)
- [x] **Q5.1 — Clearance rate per categoria di crimine** (bar chart)
- [x] **Q5.2 — Report delay: media e mediana per categoria** (bar chart raggruppato)

### EDA Avanzata Blocco 6 — `10_advanced_eda_block1.ipynb`
- [x] **Q6.1 — Mappa hotspot su griglia 500m × 500m** (mappa folium interattiva HTML)
- [x] **Q6.2 — Persistenza hotspot nel tempo** (line chart overlap mese su mese)
- [x] **Q6.3 — Clustering spaziale statistico** (Moran's I + Getis-Ord Gi*, mappa folium)

### EDA Avanzata Blocco 7 — `11_advanced_eda_block2.ipynb`
- [x] **Q7.1 — Previsione volume reati a 7 giorni** (Prophet, line chart forecast vs actual)
- [x] **Q7.2 — Modello predittivo di clearance** (Random Forest, bar chart feature importance)
- [x] **Q7.3 — Co-occorrenza spazio-temporale** (documentata e scartata — vedi note)
- [x] **Q7.4 — Pattern di ri-vittimizzazione** (documentata e scartata — vedi note)

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
15. **Donut chart per distribuzioni a poche categorie**: usato per sesso e età. Bar chart per etnia (20 valori).
16. **Soglia 10.000 per raggruppamento etnie**: etnie minori aggregate in "Other/Small Groups".
17. **Lookup table codici etnia LAPD**: fonte: LAPD Crime Data Dictionary su data.lacity.org.
18. **Top 5 per trend Senior**: limitato ai 5 crimini più frequenti per leggibilità del line chart.

### EDA Blocco 3
19. **Mocodes esplosi con `.str.split().explode()`**: ogni codice trattato come unità indipendente. 1.004.602 codici totali da 224.201 record.
20. **Top 30 Mocodes per frequenza**: filtro applicato prima della visualizzazione.
21. **Lookup table Mocodes**: fonte: LAPD MO Codes Numerical List, rev. 05/18.
22. **Codice 2000 (Domestic Violence)**: flag amministrativo LAPD, non indicatore comportamentale.

### EDA Blocco 4
23. **Firearm list con 25 categorie**: include SIMULATED GUN, AIR PISTOL/BB GUN e TOY GUN.
24. **Trend split in armi reali vs simulate**: limitato a 7 armi reali ad alto volume e 3 simulate.

### EDA Blocco 5
25. **Clearance rate**: calcolato come (Adult Arrest + Juv Arrest + Adult Other + Juv Other) / totale.
26. **Report delay filtrato a ≤ 365 giorni**: 3.040.664 record inclusi.
27. **Visualizzazione report delay**: bar chart media/mediana affiancate — preferito al box plot.

### EDA Avanzata Blocco 6
28. **Griglia 500m × 500m**: 62.790 celle totali, 4.706 celle attive (7.5% della griglia).
29. **Sistema di coordinate**: EPSG:4326 (gradi) → EPSG:32611 (metri UTM Zone 11N) per calcoli spaziali.
30. **Queen contiguity**: matrice di pesi W con media 7.95 vicini per cella.
31. **Hotspot threshold**: top 5% delle celle attive per volume — soglia standard in criminologia.
32. **Overlap mese su mese**: calcolato su finestre di 48 ore per area, 180 mesi totali (2010-2024).

### EDA Avanzata Blocco 7
33. **Prophet vs ARIMA**: scelto Prophet per gestione nativa stagionalità settimanale e robustezza agli outlier.
34. **Train/test split temporale**: train 2010-2022, test 2023 (anno completo). 2024 escluso dal test per incompletezza.
35. **Random Forest con class_weight='balanced'**: compensa sbilanciamento 75%/25% (not cleared/cleared).
36. **Q7.3 scartata**: co-occorrenza a livello area LAPD troppo grossolana — lift max 1.11, associazioni non actionable.
37. **Q7.4 scartata**: ridondante con Q6.1/Q6.2, stessa limitazione di granularità geografica.
38. **Nota per relazione finale**: il modello Prophet usa intervallo di confidenza simmetrico — per la pianificazione operativa delle risorse di polizia si raccomanda di usare il limite superiore dell'intervallo, data l'asimmetria del costo tra under-staffing e over-staffing.

## Risultati principali

### Post-cleaning
- **Dataset pulito**: 3.079.424 righe × 24 colonne
- **Range DATE OCC**: 1 gennaio 2010 → 30 dicembre 2024
- **15 anni**, **21 aree LAPD**, **143 tipi di crimine**

### Post-feature engineering
- **Dataset arricchito**: 3.079.424 righe × 32 colonne
- **Distribuzione crime_category**: Property 63%, Person 35.5%, Other 1.5%
- **Reati domestici**: 225.938 (7.3%)
- **Report delay**: mediana 1 giorno, media 21 giorni, max 5.407 giorni

### EDA Blocco 1 — Scoperte principali
- **Vehicle Theft**: unico crimine in crescita dal 2020. Da citare: FBI UCR, NICB.
- **Anomalie 2015-2016**: calo 2015, rimbalzo 2016. Ipotesi: transizione sistema classificazione LAPD.
- **Shoplifting (Petty) strutturale**: +87% nel 2023. Correlata alla Prop 47 California.
- **Identity Theft temporaneo**: +95% nel 2022, -39% nel 2023.
- **Stagionalità Property**: picchi a marzo (effetto aggregato) e ottobre (Vehicle Theft).
- **Picco notturno weekend (Person)**: domenica notte 34.062. Pattern movida notturna.
- **Venerdì picco Property**: Afternoon 70.800, Evening 73.995.

### EDA Blocco 2 — Scoperte principali
- **Distribuzione sesso**: M 44.2%, F 40.0%, X 15.9%.
- **Pattern di genere invertito tra Property e Person**.
- **Child e Adolescent femmine sovrarappresentate nei crimini Person**.
- **Senior: 8 crimini su 10 sono Property**.

### EDA Blocco 3 — Scoperte principali
- **7.3% reati domestici**: limite inferiore per sottodenuncia.
- **Trend domestici contro-intuitivo post-2020**: calo durante COVID.
- **Concentrazione geografica**: 77th Street, Southeast, Southwest dominano.
- **Profilo vittime domestiche**: donne 75.8%.
- **Minori sovrarappresentati**: Child dal 1.8% al 10.4% nei domestici.
- **Choked/Strangled (22.738)**: indicatore alto rischio escalation.

### EDA Blocco 4 — Scoperte principali
- **Concentrazione geografica reati armati**: stesse aree dei reati domestici.
- **Hand Gun dominante**: 53.523 casi (~46% del totale).
- **Revolver in declino costante**; Semi-Auto Pistol in crescita dal 2015.
- **Profilo vittime armati**: Male 70.3%, Young Adult 52.3%.

### EDA Blocco 5 — Scoperte principali
- **Clearance rate**: Person 45.2%, Property 9.0% — divario 5x.
- **Report delay**: Person mediana 0 giorni, Property mediana 1 giorno.

### EDA Avanzata Blocco 6 — Scoperte principali
- **Q6.1**: 5% delle celle attive (235 su 4.706) concentra il 32.9% dei crimini contro la persona.
- **Cluster principale**: Downtown LA → South LA → Inglewood, diviso dalla Harbor Transitway.
- **Q6.2**: overlap medio 49.4% — hotspot cronici e dinamici in egual misura. Post-2016 overlap stabilmente sopra media (hotspot più cronici).
- **Q6.3**: Moran's I = 0.7359 (p=0.001) — clustering non casuale confermato. Getis-Ord: 908 celle Hot Spot al 99%, 285 al 95%.

### EDA Avanzata Blocco 7 — Scoperte principali
- **Q7.1 Prophet**: MAPE 10.74% sul 2023 — sotto soglia 15%. Sovrastima sistematica di ~46 crimini/giorno coerente con calo strutturale post-2022.
- **Q7.2 Random Forest**: accuracy 71%, recall Cleared 82%, precision Cleared 45%. Feature importance: crime_category_property (0.35), crime_category_person (0.29), No Weapon (0.13), report_delay (0.06).
- **Q7.3 e Q7.4**: scartate per limitazioni metodologiche documentate nel notebook.

## Problemi e soluzioni
- **2024 apparentemente completo ma sottorappresentato**: DATE OCC arriva al 30 dicembre 2024 ma il dataset è stato congelato a giugno 2025. I crimini del 2024 denunciati con ritardo non sono inclusi, causando il calo anomalo visibile in tutti i grafici. Il dato 2024 va sempre interpretato con cautela.
- **SettingWithCopyWarning**: risolto con `.copy()` sui DataFrame filtrati.
- **FutureWarning groupby**: risolto con `observed=True` sui groupby su colonne `category`.
- **Notebook non salvato**: recuperato via git pull. Lezione: committare frequentemente.
- **Box plot illeggibile per report delay**: risolto con bar chart media/mediana affiancate.
- **Getis-Ord warning**: risolto con `fill_diagonal()` e `star=None`.
- **Q7.3 lift troppo basso**: area LAPD troppo grande come unità spaziale per association rules significative.

## Output prodotti
- `notebooks/02_analyze/02_cleaning.ipynb`
- `notebooks/02_analyze/04_feature_engineering.ipynb`
- `notebooks/02_analyze/05_eda_block1.ipynb`
- `notebooks/02_analyze/06_eda_block2.ipynb`
- `notebooks/02_analyze/07_eda_block3.ipynb`
- `notebooks/02_analyze/08_eda_block4.ipynb`
- `notebooks/02_analyze/09_eda_block5.ipynb`
- `notebooks/02_analyze/10_advanced_eda_block1.ipynb`
- `notebooks/02_analyze/11_advanced_eda_block2.ipynb`
- `data/processed/crimes_clean.parquet` — 3.079.424 × 24 colonne
- `data/processed/crimes_features.parquet` — 3.079.424 × 32 colonne
- `outputs/05_eda_block1/` — 7 grafici (01-07)
- `outputs/06_eda_block2/` — 7 grafici (08-14)
- `outputs/07_eda_block3/` — 8 grafici (15-22)
- `outputs/08_eda_block4/` — 6 grafici (23-28)
- `outputs/09_eda_block5/` — 3 grafici (29-31)
- `outputs/10_advanced_eda_block1/` — 4 output (32-34: PNG + 2 HTML mappe folium)
- `outputs/11_advanced_eda_block2/` — 3 grafici (35-37)

## Note per la fase successiva

La fase Analyze è completamente conclusa (core + avanzata).

1. **Fase Construct**: integrata nella Analyze — vedi `construct_summary.md`
2. **Fase Execute**: 
   - Relazione management (documento Word/PDF)
   - Presentazione PowerPoint con 8-10 grafici selezionati
3. **Fonti esterne da raccogliere**: Vehicle Theft post-COVID (FBI UCR, NICB), anomalie 2015-2016 LAPD, Prop 47 California, Identity Theft anziani e COVID, picco reati domestici 2016-2018
4. **Nota Prophet per relazione**: usare limite superiore intervallo di confidenza per pianificazione risorse operative
