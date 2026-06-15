# Analyze — Summary

**Stato**: Completato (fase core)
**Data inizio**: 20-04-2026
**Data chiusura**: 15-06-2026

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
- [x] **Q2.1 — Distribuzione vittime per etnia** (bar chart orizzontale) — `06_eda_block2.ipynb`
- [x] **Q2.2 — Profilo vittime per macro-categoria** (bar chart raggruppato Person vs Property) — `06_eda_block2.ipynb`
- [x] **Q2.3 — Top 10 crimini contro vittime Senior 65+** (bar chart orizzontale) — `06_eda_block2.ipynb`
- [x] **Q2.3 — Trend temporale top 5 crimini contro Senior** (line chart) — `06_eda_block2.ipynb`

### EDA Blocco 3 — `07_eda_block3.ipynb`
- [x] **Q3.1 — Volume reati domestici** (donut chart domestici vs non-domestici) — `07_eda_block3.ipynb`
- [x] **Q3.1 — Trend annuale reati domestici 2010-2024** (line chart) — `07_eda_block3.ipynb`
- [x] **Q3.1 — Distribuzione reati domestici per area LAPD** (bar chart orizzontale) — `07_eda_block3.ipynb`
- [x] **Q3.2 — Confronto profilo vittime per sesso** (2 donut affiancati) — `07_eda_block3.ipynb`
- [x] **Q3.2 — Confronto profilo vittime per fascia di età** (2 donut affiancati) — `07_eda_block3.ipynb`
- [x] **Q3.2 — Confronto profilo vittime per etnia** (2 bar chart affiancati) — `07_eda_block3.ipynb`
- [x] **Q3.3 — Crimini domestici contro minori: Child vs Adolescent** (2 bar chart affiancati) — `07_eda_block3.ipynb`
- [x] **Q3.4 — Pattern Mocodes nei reati domestici** (bar chart top 30 codici) — `07_eda_block3.ipynb`

### EDA Blocco 4 — `08_eda_block4.ipynb`
- [x] **Q4.1 — Distribuzione reati con arma da fuoco per area LAPD** (bar chart) — `08_eda_block4.ipynb`
- [x] **Q4.1 — Volume reati per tipo di arma** (bar chart orizzontale) — `08_eda_block4.ipynb`
- [x] **Q4.1 — Trend annuale per tipo di arma** (2 line chart affiancati: armi reali vs simulate) — `08_eda_block4.ipynb`
- [x] **Q4.2 — Distribuzione vittime per sesso** (donut chart) — `08_eda_block4.ipynb`
- [x] **Q4.2 — Distribuzione vittime per fascia di età** (donut chart) — `08_eda_block4.ipynb`
- [x] **Q4.2 — Distribuzione vittime per etnia** (bar chart orizzontale) — `08_eda_block4.ipynb`

### EDA Blocco 5 — `09_eda_block5.ipynb`
- [x] **Q5.1 — Clearance rate per area LAPD** (bar chart orizzontale) — `09_eda_block5.ipynb`
- [x] **Q5.1 — Clearance rate per categoria di crimine** (bar chart) — `09_eda_block5.ipynb`
- [x] **Q5.2 — Report delay: media e mediana per categoria** (bar chart raggruppato) — `09_eda_block5.ipynb`

### EDA fase avanzata Blocchi 6-7
- [ ] **Analisi geospaziale avanzata e modelli predittivi** — da creare (opzionale)

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
23. **Firearm list con 25 categorie**: include SIMULATED GUN, AIR PISTOL/BB GUN e TOY GUN — non armi da fuoco reali ma usate per far percepire minaccia armata. Documentato nelle osservazioni.
24. **Trend split in armi reali vs simulate**: limitato a 7 armi reali ad alto volume e 3 simulate per leggibilità.

### EDA Blocco 5
25. **Clearance rate**: calcolato come (Adult Arrest + Juv Arrest + Adult Other + Juv Other) / totale. Invest Cont e UNK considerati "not cleared". Definizione coerente con FBI UCR.
26. **Report delay filtrato a ≤ 365 giorni**: 3.040.664 record inclusi. Delay estremi (max 5.407 giorni) esclusi come casi eccezionali.
27. **Visualizzazione report delay**: bar chart con media e mediana affiancate per categoria — preferito al box plot perché la distribuzione è troppo concentrata sui valori bassi (mediana 0-1 giorni).

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
- **Distribuzione sesso**: M 44.2%, F 40.0%, X 15.9%.
- **Pattern di genere invertito tra Property e Person**: nei crimini Person le femmine sono sovrarappresentate in tutte le fasce tranne Senior.
- **Child e Adolescent femmine sovrarappresentate nei crimini Person**: bambine 23.336 vs 16.436 maschi; adolescenti 41.717 vs 26.823 maschi.
- **Senior: 8 crimini su 10 sono Property**. Burglary e Identity Theft primi due. Simple Assault con trend crescente dal 2010 al 2023.

### EDA Blocco 3 — Scoperte principali
- **7.3% reati domestici**: dato da considerare limite inferiore per sottodenuncia e limitazioni classificatorie.
- **Trend domestici contro-intuitivo post-2020**: calo invece dell'atteso aumento durante i lockdown COVID.
- **Picco 2016-2018**: +38% in un anno. Da investigare con fonti esterne.
- **Concentrazione geografica**: 77th Street (21.073), Southeast (17.431), Southwest (15.726).
- **Profilo vittime domestiche**: donne 75.8% (vs 37.1% nei non-domestici).
- **Minori sovrarappresentati nei domestici**: Child dal 1.8% al 10.4% (6x). Adolescent dal 3.4% al 6.7%.
- **Mocodes dominanti**: Hit with weapon (115.673), Boyfriend/Girlfriend (70.790), Victim knew Suspect (69.305), Spouse/Cohabitant (54.293). Choked/Strangled (22.738) — indicatore alto rischio escalation.

### EDA Blocco 4 — Scoperte principali
- **Concentrazione geografica reati armati**: 77th Street (17.036), Southeast (12.121), Newton (9.879), Southwest (8.908).
- **Hand Gun dominante**: 53.523 casi (~46% del totale). Top 4 armi coprono ~81% dei reati.
- **Armi simulate significative**: ~10.000 casi totali.
- **Revolver in declino costante** dal 2010; Semi-Auto Pistol in crescita strutturale dal 2015.
- **Profilo vittime armati**: Male 70.3%, Young Adult 52.3%, Adolescent 6.2% — tutti significativamente più alti rispetto al dataset generale.
- **Etnia**: Hispanic/Latin/Mexican prima (51.770), Black seconda (29.611, ~27% vs 15.1% generale).

### EDA Blocco 5 — Scoperte principali
- **Clearance rate per area**: range 15.4% (Pacific) — 28.2% (West Valley). Range ristretto (~13 punti) suggerisce che il tasso è influenzato dalla tipologia di crimine presente nell'area più che dall'efficacia investigativa.
- **Clearance rate per categoria**: Person 45.2%, Other 37.0%, Property 9.0%. Divario enorme — i crimini Property sono 5 volte meno risolti di quelli Person.
- **Report delay Person**: media 1.0 giorno, mediana 0 giorni — denunciati lo stesso giorno.
- **Report delay Property**: media 2.5 giorni, mediana 1 giorno — leggero ritardo coerente con la natura del reato (spesso scoperto il giorno dopo).

## Problemi e soluzioni
- **2024 non troncato**: il calo è reale, non un artefatto del dataset.
- **SettingWithCopyWarning**: risolto con `.copy()` sui DataFrame filtrati.
- **FutureWarning groupby**: risolto con `observed=True` sui groupby su colonne `category`.
- **Notebook non salvato**: recuperato via git pull. Lezione: committare frequentemente.
- **Box plot illeggibile per report delay**: distribuzione troppo concentrata sui valori bassi. Risolto con bar chart media/mediana affiancate.

## Output prodotti
- `notebooks/02_analyze/02_cleaning.ipynb`
- `notebooks/02_analyze/04_feature_engineering.ipynb`
- `notebooks/02_analyze/05_eda_block1.ipynb`
- `notebooks/02_analyze/06_eda_block2.ipynb`
- `notebooks/02_analyze/07_eda_block3.ipynb`
- `notebooks/02_analyze/08_eda_block4.ipynb`
- `notebooks/02_analyze/09_eda_block5.ipynb`
- `data/processed/crimes_clean.parquet` — 3.079.424 × 24 colonne
- `data/processed/crimes_features.parquet` — 3.079.424 × 32 colonne
- `outputs/05_eda_block1/` — 7 grafici (01-07)
- `outputs/06_eda_block2/` — 7 grafici (08-14)
- `outputs/07_eda_block3/` — 8 grafici (15-22)
- `outputs/08_eda_block4/` — 6 grafici (23-28)
- `outputs/09_eda_block5/` — 3 grafici (29-31)

## Note per la fase successiva

La fase Analyze (core) è completata. I prossimi step sono:

1. **Fase Construct**: raffinamento visivo dei grafici, eventuale aggiunta di annotazioni, preparazione per la presentazione
2. **Fase Execute**: redazione della relazione management e del PowerPoint con i grafici esportati
3. **Fonti esterne da raccogliere** per la relazione finale: Vehicle Theft post-COVID (FBI UCR, NICB), anomalie 2015-2016 LAPD, Prop 47 California, Identity Theft anziani e COVID, picco reati domestici 2016-2018
4. **EDA avanzata Blocchi 6-7** (opzionale): analisi geospaziale e modelli predittivi — da valutare in base al tempo disponibile
