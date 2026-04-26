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
- [x] **Fascia oraria**: `time_slot` con 6 fasce (Night, Early Morning, Morning, Afternoon, Evening, Late Night) tramite `pd.cut()` su `hour_occ` — `04_feature_engineering.ipynb`
- [x] **Fasce di età**: `age_group` con 5 categorie (Child 0-12, Adolescent 13-17, Young Adult 18-34, Adult 35-64, Senior 65+) tramite `pd.cut()` su `Vict Age` — `04_feature_engineering.ipynb`
- [x] **Macro-categoria crimine**: `crime_category` (Person / Property / Other) tramite `np.select()` e classificazione manuale dei 143 tipi di crimine in due liste — `04_feature_engineering.ipynb`
- [x] **Report delay**: `report_delay` (differenza in giorni tra `Date Rptd` e `DATE OCC`) — `04_feature_engineering.ipynb`
- [x] **Flag reati domestici**: `is_domestic` (booleana, True/False) basata su parole chiave in `Crm Cd Desc` (INTIMATE PARTNER, CHILD ABUSE, ecc.) — `04_feature_engineering.ipynb`
- [x] **Verifica finale e salvataggio `crimes_features.parquet`** — `04_feature_engineering.ipynb`

### EDA
- [ ] **EDA Blocco 1-5** — da creare
- [ ] **EDA fase avanzata Blocco 6-7** — da creare

## Decisioni chiave

### Cleaning
1. **Colonne eliminate**: `Crm Cd 2/3/4` (>93% nulli) e `Cross Street` (84% nulli, ridondante). Da 28 a 24 colonne.
2. **`TIME OCC` sostituito con `hour_occ`**: intero HHMM convertito in ora intera 0-23. Colonna originale eliminata.
3. **Coordinate sentinella (0, 0) → NaN**: i 3.148 record mantengono l'informazione non-geografica.
4. **Duplicati: drop_duplicates() semplice**: tutti i 57.809 duplicati erano esatti, distribuiti su 122 tipi di crimine. Errore generalizzato di caricamento.
5. **Vict Sex: solo M, F, X**: valori rari H (185), N (17), - (2) ricodificati come X.
6. **Vict Age ≤ 0 → NaN**: 631.621 sentinelle (valore 0) + 778 errori (valori negativi).
7. **Mocodes lasciati con NaN**: 12.18% di nulli è mancanza legittima.

### Feature Engineering
8. **Fasce orarie da 4 ore**: scelta di 6 fasce che riflettono i ritmi della giornata. L'artefatto mezzanotte (130k record con hour_occ = 0) è incluso nella fascia "Night" e verrà gestito in EDA.
9. **Fasce di età allineate al Blocco 3**: le fasce Child (0-12) e Adolescent (13-17) sono state scelte specificamente per supportare l'analisi sugli abusi domestici e la sicurezza dei minori.
10. **Classificazione Person/Property/Other manuale**: i 143 tipi di crimine sono stati classificati manualmente in due liste. Casi ambigui (TILL TAP, PICKPOCKET, ARSON, CRUELTY TO ANIMALS, ecc.) classificati secondo il criterio legale USA. In uno scenario reale, la classificazione andrebbe validata con il domain expert (es. responsabile LAPD). I crimini non classificabili (47.648 record, 1.5%) sono in "Other" (es. DISTURBING THE PEACE, FALSE POLICE REPORT, CONTEMPT OF COURT).
11. **Flag `is_domestic` basato su parole chiave**: 225.938 reati domestici identificati (7.3% del dataset). Parole chiave: INTIMATE PARTNER, CHILD ABUSE, CHILD NEGLECT, CHILD ABANDONMENT, CHILD STEALING, CHILD ANNOYING, CRM AGNST CHLD, INCEST.

## Risultati principali

### Post-cleaning
- **Dataset pulito**: 3.079.424 righe × 24 colonne
- **Righe rimosse**: 58.607 totali (57.809 duplicati + 775 Premis Desc nulli + 23 Status/Crm Cd 1 nulli)
- **Range DATE OCC**: 1 gennaio 2010 → 30 dicembre 2024. Il 2024 NON è troncato (calo reale di crimini registrati)
- **Range Date Rptd**: fino al 5 giugno 2025 (dataset congelato a quel punto)
- **15 anni**, **21 aree LAPD**, **143 tipi di crimine**

### Post-feature engineering
- **Dataset arricchito**: 3.079.424 righe × 32 colonne (8 feature aggiunte)
- **Distribuzione crime_category**: Property 63% (1.938.752), Person 35.5% (1.093.024), Other 1.5% (47.648)
- **Distribuzione age_group**: Adult 37.6%, Young Adult 32.2%, NaN 20.5%, Senior 5.5%, Adolescent 2.7%, Child 1.4%
- **Distribuzione time_slot**: Evening (690k) > Afternoon (678k) > Late Night (631k) > Morning (499k) > Night (351k) > Early Morning (229k)
- **Reati domestici**: 225.938 (7.3% del totale)
- **Report delay**: mediana 1 giorno, media 21 giorni, max 5.407 giorni (~14.8 anni). Distribuzione fortemente asimmetrica (la maggior parte dei crimini si denuncia entro 2 giorni, con coda lunga di denunce tardive)

## Problemi e soluzioni

- **Scoperta 2024 non troncato**: l'ispezione iniziale ipotizzava troncamento a metà anno. La conversione delle date ha rivelato che DATE OCC arriva al 30 dicembre 2024: il calo è reale.
- **Valori anomali in Vict Sex**: oltre a M, F, X emersi H, N, -. Ricodificati come X (204 record).
- **Nulli in Status e Crm Cd 1 scoperti solo nella verifica finale**: non emersi nell'ispezione iniziale, rivelati dopo le trasformazioni. Lezione: eseguire sempre verifica finale post-cleaning.
- **Notebook non salvato (incidente locale)**: una sessione di lavoro persa per mancato salvataggio. Recuperata tramite git pull dalla versione committata. Lezione: salvare frequentemente e committare ogni step significativo.

## Output prodotti
<!-- Formato: - `path/al/file.ext` ← prodotto da `nome_notebook.ipynb` -->
- `notebooks/02_analyze/02_cleaning.ipynb` — notebook completo di pulizia dati
- `notebooks/02_analyze/04_feature_engineering.ipynb` — notebook di creazione feature derivate
- `data/processed/crimes_clean.parquet` — dataset pulito, 3.079.424 righe × 24 colonne ← prodotto da `02_cleaning.ipynb`
- `data/processed/crimes_features.parquet` — dataset arricchito, 3.079.424 righe × 32 colonne ← prodotto da `04_feature_engineering.ipynb`

## Note per la fase successiva

I prossimi step della fase Analyze sono:

1. **EDA strutturata per blocchi tematici** seguendo le domande di ricerca definite in `03_research_questions.md`:
   - Blocco 1: Panoramica e trend temporali
   - Blocco 2: Profilo demografico vittime
   - Blocco 3: Abusi domestici e sicurezza minori
   - Blocco 4: Reati con arma da fuoco
   - Blocco 5: Efficacia della risposta

2. **Scaricare la lookup table dei Mocodes** (`MO_CODES.txt`) dal portale LAPD prima di affrontare il Blocco 3 (Q3.4)

3. **Ogni grafico va esportato come PNG** con `plt.savefig('nome_descrittivo.png', dpi=150, bbox_inches='tight')` prima di `plt.show()`, come richiesto dal Piano di Sviluppo Professionale per la costruzione della presentazione PowerPoint

4. **Gestire l'artefatto mezzanotte** nelle analisi temporali: confronto con e senza record a hour_occ = 0

5. **Indagare il calo 2024** e le **anomalie 2015-2016** con ricerca esterna per documentarle nella relazione management
