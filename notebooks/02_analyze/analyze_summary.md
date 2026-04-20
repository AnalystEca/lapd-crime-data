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
- [ ] **Feature engineering** — notebook dedicato, da creare
- [ ] **EDA (Exploratory Data Analysis)** — notebook dedicato, da creare

## Decisioni chiave

1. **Colonne eliminate**: `Crm Cd 2/3/4` (>93% nulli, non rilevanti per le domande guida) e `Cross Street` (84% nulli, ridondante con LOCATION + LAT/LON). Totale: da 28 a 24 colonne.

2. **`TIME OCC` sostituito con `hour_occ`**: l'intero HHMM è stato convertito in ora intera (0-23), granularità sufficiente per le analisi previste. La colonna originale è stata eliminata perché ridondante e meno leggibile.

3. **Coordinate sentinella (0, 0) → NaN anziché eliminazione righe**: i 3.148 record mantengono tutta l'informazione non-geografica (tipo crimine, vittima, orario) e vengono esclusi automaticamente solo dalle analisi che richiedono le coordinate.

4. **Duplicati: drop_duplicates() semplice**: tutti i 57.809 duplicati su DR_NO sono risultati esatti (righe identiche su tutte le colonne), distribuiti su 122 tipi di crimine diversi. Errore generalizzato di caricamento nel dataset originale, non specifico per categoria.

5. **Vict Sex: solo M, F, X**: i valori rari H (185), N (17), - (2) sono stati ricodificati come X (Unknown) per semplicità statistica. Rappresentano lo 0.007% del dataset.

6. **Vict Age ≤ 0 → NaN anziché eliminazione righe**: stessa logica delle coordinate sentinella. Il valore 0 (631.621 record, 20.5%) è il sentinella LAPD per "età sconosciuta", i valori negativi (778 record) sono errori di inserimento. I record restano per analisi non-demografiche.

7. **Mocodes lasciati con NaN**: il 12.18% di nulli è un valore mancante legittimo (non tutti i crimini hanno modus operandi codificato). Verranno gestiti nel notebook di analisi dedicato.

8. **Nulli residui minimi (Status: 2, Crm Cd 1: 21) → eliminazione righe**: quantità trascurabile, non giustifica strategie di imputazione.

## Risultati principali

- **Dataset finale**: 3.079.424 righe × 24 colonne
- **Righe rimosse durante il cleaning**: 58.607 totali (57.809 duplicati + 775 Premis Desc nulli + 23 Status/Crm Cd 1 nulli)
- **Range temporale confermato**: DATE OCC dal 1 gennaio 2010 al 30 dicembre 2024. Il 2024 NON è troncato a metà anno come inizialmente ipotizzato — ha effettivamente meno crimini registrati (127k vs ~230k degli anni precedenti). Date Rptd arriva al 5 giugno 2025 (crimini 2024 denunciati nel 2025).
- **Range geografico verificato**: LAT 33.34–34.79, LON -118.83–-117.66 (coerente con l'area metropolitana di Los Angeles)
- **Conteggi chiave**: 15 anni coperti, 21 aree LAPD, 143 tipi di crimine distinti
- **Valori nulli residui** (tutti intenzionali e documentati):
  - `Vict Age`: 632.399 (20.54%) — sentinelle "età sconosciuta"
  - `Mocodes`: 375.192 (12.18%) — modus operandi non codificato
  - `LAT`/`LON`: 3.140 (0.10%) — ex coordinate sentinella (0, 0)
- **Nota su hour_occ = 0 (mezzanotte)**: 130k record. Probabile artefatto di registrazione (orario sconosciuto codificato come 0000), non picco reale di criminalità. Da documentare e annotare nelle visualizzazioni.
- **Distribuzione Vict Sex post-pulizia**: M 44.2%, F 40.0%, X 15.9%
- **Media Vict Age post-pulizia**: 38.8 anni (era 30.8 prima della rimozione dei sentinella 0)

## Problemi e soluzioni

- **Scoperta 2024 non troncato**: l'ispezione iniziale (fase Plan) ipotizzava che il 2024 fosse incompleto. La conversione delle date ha rivelato che DATE OCC arriva al 30 dicembre 2024: il calo di record è reale, non un troncamento. Aggiornata la strategia di gestione: il 2024 sarà incluso nei trend annuali con nota esplicativa.

- **Valori anomali in Vict Sex non previsti**: oltre a M, F, X sono emersi H (non-binary), N (not reported), - (errore). Ricodificati tutti come X per coerenza, data la quantità irrilevante (204 record).

- **Nulli in Status e Crm Cd 1 non emersi nell'ispezione iniziale**: scoperti solo nella verifica finale post-cleaning. Rimossi (23 righe). Lezione metodologica: eseguire sempre una verifica finale completa dopo tutte le trasformazioni, perché operazioni intermedie possono rivelare nulli precedentemente mascherati.

## Output prodotti
<!-- Formato: - `path/al/file.ext` ← prodotto da `nome_notebook.ipynb` -->
- `notebooks/02_analyze/02_cleaning.ipynb` — notebook completo di pulizia dati
- `data/processed/crimes_clean.parquet` — dataset pulito, 3.079.424 righe × 24 colonne ← prodotto da `02_cleaning.ipynb`

## Note per la fase successiva

I prossimi step della fase Analyze sono:

1. **Pausa decisionale**: raffinare le 4 domande guida del PACE doc e aggiungerne di nuove, ora che conosciamo il dataset in profondità
2. **Feature engineering** (notebook dedicato): colonne derivate come anno, mese, giorno settimana, fascia oraria, gruppi di età, macro-categorie di crimine, decodifica Mocodes
3. **EDA** (notebook dedicato): analisi univariata e bivariata, distribuzione crimini per tipologia/area/anno, profilo vittime, pattern temporali e geografici
4. **Indagine sul calo 2024**: verificare se il calo di record è un fenomeno reale o un artefatto del sistema di registrazione
5. **Indagine anomalie 2015-2016**: verificare la connessione con la transizione UCR → NIBRS del LAPD
