# Revisione notebook v3 — Analisi indipendente in sola lettura

**Ambito**: tutti i notebook in `00_additional_charts/`, `01_plan/`, `02_analyze/`,
`03_construct/`. Nessuna cella è stata modificata, eseguita, creata, cancellata o
rinominata: revisione interamente in sola lettura, basata sulla lettura integrale
del codice sorgente, del testo markdown e degli output testuali già presenti in
ogni notebook (gli output immagine — PNG dei grafici — non sono stati rigenerati
né decodificati pixel per pixel; le verifiche sui grafici si basano sui dati
numerici sottostanti stampati nelle celle di codice).

**Documenti di riferimento letti integralmente prima dell'analisi**:
`osservazioni_da_verificare.md` (58 osservazioni originarie) e
`revisione_notebook.md` (revisione precedente, che verificava lo stato di
remediation delle 58 osservazioni più un controllo sulle forme "we + azione").
Questo documento non presuppone corrette le conclusioni di `revisione_notebook.md`:
ogni voce è stata riverificata in modo indipendente contro il contenuto attuale
dei notebook. In tre punti la riverifica ha prodotto un esito diverso da quello
di `revisione_notebook.md` (si veda la Sezione 2) — segno che i notebook sono
stati modificati ulteriormente dopo la stesura di quel documento.

**Nota metodologica**: dove non diversamente specificato, "testo attuale" indica
il contenuto della cella così come letto direttamente dal file `.ipynb` sul
disco al momento di questa revisione (verificato anche via lettura diretta del
JSON, non solo tramite l'estratto testuale usato per la lettura).

---

## 1. Sintesi esecutiva

I notebook di cleaning e feature engineering (01–04) sono metodologicamente
solidi: le decisioni sono giustificate, testate nel notebook stesso e
riproducibili. Il lavoro di rimozione delle ipotesi causali non supportate dai
dati (COVID, cambio di classificazione LAPD, riferimenti a "letteratura
criminologica" generica) è stato quasi ovunque completato, spesso in modo più
aggressivo di quanto richiesto: il rilievo più grave emerso da questa revisione
è che la rimozione del paragrafo sull'anomalia 2015–2016/2024 dal notebook
[01_data_loading.ipynb](01_plan/01_data_loading.ipynb) ha lasciato **almeno
5 notebook a valle** (05, 06, 07, 08, 10) con un riferimento pendente a "the
introductory methodological note" che non esiste più in nessun notebook del
progetto — un'incoerenza narrativa concreta, verificabile con un semplice
grep, che nessuna revisione precedente aveva segnalato. Il secondo rilievo
più rilevante riguarda [12_construct_block1.ipynb](03_construct/12_construct_block1.ipynb):
la narrazione descrive due tentativi falliti di `GridSearchCV` (">4 ore",
"1.200 fit") per cui non esiste **alcuna cella di codice corrispondente** nel
notebook — esattamente il tipo di affermazione non verificabile dal contenuto
del notebook che l'intero esercizio di revisione dovrebbe intercettare. Sono
inoltre emersi: una selection bias non dichiarata nel modello di clearance
(Random Forest addestrato dopo un `dropna()` che rimuove il 20,5% dei record,
in gran parte proprio i reati "property" a bassissimo tasso di risoluzione);
test di autocorrelazione spaziale (Moran's I, Getis-Ord Gi*) eseguiti senza
seed, con un numero riportato nel testo che non coincide con l'output di
codice attualmente salvato; e prove dirette, nei metadati di esecuzione delle
celle, che più notebook non sono stati eseguiti linearmente dall'inizio alla
fine in un'unica sessione. Nessun problema di join, duplicazione residua o
data leakage temporale è emerso nella pipeline dati (01→04); le pulizie dei
sentinel, dei duplicati e dei tipi sono corrette e ben verificate nel
notebook stesso.

---

## 2. Esito delle osservazioni da verificare

Tutte le 58 osservazioni originarie segnalavano un'affermazione interpretativa
non supportata da dati/test presenti nel notebook. La riverifica diretta del
contenuto attuale mostra che, per 57 delle 58 voci, il testo segnalato è stato
rimosso o riformulato in chiave puramente descrittiva/dati-supportata: lo
stato assegnato è quindi **SMENTITA** (l'osservazione originaria non descrive
più il contenuto reale del notebook). Tre voci meritano una nota a parte
rispetto a quanto riportato in `revisione_notebook.md`:

- **#1** e **#2** (anomalia 2015–2016 e troncamento 2024, notebook 01): il
  paragrafo intero è stato **rimosso**, non riformulato. `revisione_notebook.md`
  riportava per queste due voci un testo "attuale" (rispettivamente sull'esito
  di UCR→NIBRS e sulla frase "the truncation month needs to be verified") che
  **non è più presente nel notebook**: la cella `72111801` oggi contiene solo
  dimensioni del dataset, null value e decisioni operative, senza alcun
  riferimento a 2015, 2016 o 2024. Lo stato SMENTITA riflette la sparizione
  del testo originario, ma la rimozione crea un problema nuovo (riferimenti
  pendenti a valle, vedi #BLOCCANTE in Sezione 3) e lascia comunque senza
  risposta la domanda sostanziale sollevata da #2 (in quale mese si tronca il
  2024) — riportata come rilievo autonomo in Sezione 3.
- **#46** (strangolamento come "indicatore criminologico", notebook 07,
  cella `724ef7d6`): `revisione_notebook.md` lo marcava **NON RISOLTO**
  ("frase ancora presente parola per parola, nessuna fonte citata"). La
  riverifica odierna mostra che la cella ora recita: *"Note: strangulation is
  documented as a high-risk escalation indicator in intimate partner violence
  (Glass et al., 2008, Journal of Emergency Medicine)."* — è stata aggiunta
  una fonte con autore/anno/rivista, nello stesso stile già usato altrove nel
  progetto per le fonti LAPD. Stato: **SMENTITA** (l'osservazione originaria,
  che lamentava l'assenza di fonte, non descrive più la situazione attuale).
  Non è stato possibile verificare esternamente l'esistenza/esattezza della
  citazione (fuori dall'ambito di una revisione in sola lettura sul solo
  repository) — segnalato come caso da chiudere in Sezione 4.
- I tre "casi ambigui" aperti in `revisione_notebook.md` (Sezione 5 di quel
  documento: correlazione volume/clearance rate per area in 09; conversione
  "(50-100 meters)" in 11, cella `91f3ac70`; frase "expected pattern...
  gender violence" in 07, cella `21a6b9a7`) risultano **tutti e tre chiusi**:
  la cella `8a49f1de` di 09 ora riporta un test di Spearman calcolato (ρ =
  -0.4638, p = 0.0342); la cella `91f3ac70` di 11 non contiene più la cifra
  "(50-100 meters)"; la cella `21a6b9a7` di 07 termina con una frase
  puramente descrittiva, senza riferimento a un "expected pattern".

| # | Osservazione (sintesi) | Notebook/cella | Stato | Evidenza |
|---|---|---|---|---|
| 1 | Anomalia 2015–2016 attribuita a UCR→NIBRS | 01_data_loading, `72111801` | SMENTITA | Paragrafo rimosso interamente (non riformulato); cfr. Sezione 3 per il problema di coerenza che questo crea a valle |
| 2 | 2024 incompleto, mese "da verificare in EDA" | 01_data_loading, `72111801` | SMENTITA | Frase rimossa interamente; nessun notebook calcola comunque il mese di troncamento — rilievo autonomo in Sezione 3 |
| 3 | Sentinella (0,0) come convenzione LAPD non citata | 02_cleaning, `acc496cb` | SMENTITA | Fonte aggiunta: data.lacity.org |
| 4 | Duplicati come "errore di caricamento generalizzato" | 02_cleaning, `d24693c1` | SMENTITA | Frase causale rimossa, resta solo il dato |
| 5 | `Vict Age = 0` come convenzione LAPD non citata | 02_cleaning, `1c85663b`/`dbb86a23` | SMENTITA | Riformulato: "no explicit convention was found..." |
| 6 | Delay differenziato per tipo di reato (omicidio/violenza sessuale) | 04_feature_engineering, `07bb62db` | SMENTITA | Frase rimossa |
| 7 | Q1.1: anomalia 2015 ↔ UCR→NIBRS | 05_eda_block1, `6486503f` | SMENTITA | Cella riscritta, solo top-5 e trend |
| 8 | Q1.1: calo 2020 ↔ COVID/"opportunity crimes" | 05_eda_block1, `6486503f` | SMENTITA | Nessuna menzione COVID |
| 9 | Q1.1: picco furti auto 2020-22 "documentato a livello nazionale" | 05_eda_block1, `6486503f` | SMENTITA | Frase assente |
| 10 | Q1.2: calo persone ↔ anomalia classificazione | 05_eda_block1, `d8c84328` | SMENTITA | Cella riscritta in chiave descrittiva |
| 11 | Q1.2: titolo paragrafo etichetta 2020 come "COVID" | 05_eda_block1, `d8c84328` | SMENTITA | Nessuna etichetta "COVID" |
| 12 | Q1.2: property "più volatile/reattiva a eventi esterni" | 05_eda_block1, `d8c84328` | SMENTITA | Frase assente |
| 13 | Q1.3: rimbalzo ↔ possibile effetto normativo | 05_eda_block1, `9ddebde8` | SMENTITA | Testo rimanda solo alla nota introduttiva |
| 14 | Q1.3: +35% furto auto 2020 ↔ COVID, fonti FBI/NICB | 05_eda_block1, `9ddebde8` | SMENTITA | Solo dato numerico, nessuna fonte esterna |
| 15 | Q1.3: normalizzazione Identity Theft ↔ digitalizzazione post-pandemica | 05_eda_block1, `9ddebde8` | SMENTITA | Solo dati numerici |
| 16 | Q1.4 pt1: minimo febbraio ↔ clima/mobilità | 05_eda_block1, `6b111396` | SMENTITA | Solo valori mensili |
| 17 | Q1.4 pt1: trend ↔ furti da periodo festivo | 05_eda_block1, `6b111396` | SMENTITA | Frase assente |
| 18 | Q1.4 pt2: ipotesi attività online (shopping/tasse) | 05_eda_block1, `ad510cad` | SMENTITA | Cella riscritta, solo picchi/minimi |
| 19 | Q1.4 pt2: furti residenziali ↔ periodo natalizio | 05_eda_block1, `ad510cad` | SMENTITA | Frase assente |
| 20 | Q1.5 intro: `hour_occ=0` come artefatto di registrazione | 05_eda_block1, `bf146b2d` | SMENTITA | Cella riscritta come solo introduzione |
| 21 | Q1.5: "~130k record a mezzanotte", da leggere con cautela | 05_eda_block1, `c525a1a5` | SMENTITA | Frase assente |
| 22 | Q1.5: weekend property più basso ↔ orari negozi | 05_eda_block1, `c525a1a5` | SMENTITA | Solo dato numerico |
| 23 | Q1.5: "meno persone al lavoro, meno traffico" | 05_eda_block1, `c525a1a5` | SMENTITA | Frase assente |
| 24 | Q1.5: picco venerdì ↔ negozi/trasporti affollati | 05_eda_block1, `c525a1a5` | SMENTITA | Solo dato numerico |
| 25 | Q1.5: weekend notturno ↔ alcol/vita notturna | 05_eda_block1, `c525a1a5` | SMENTITA | Frase assente |
| 26 | Q1.5: "pattern coerente con la vita notturna" (bis) | 05_eda_block1, `c525a1a5` | SMENTITA | Frase assente |
| 27 | Q2.1 sesso: distribuzione bilanciata ↔ crimini domestici inclusi | 06_eda_block2, `dde6cfb1` | SMENTITA | Cella riscritta, solo percentuali |
| 28 | Q2.1 sesso: sovra-rappresentazione maschile ↔ "letteratura criminologica" | 06_eda_block2, `dde6cfb1` | SMENTITA | Frase assente |
| 29 | Q2.1 età: abuso minori "storicamente sotto-denunciato" | 06_eda_block2, `d9976b1d` | SMENTITA | Cella riscritta, solo conteggi |
| 30 | Q2.2: differenza 35-64 anni ↔ esposizione lavorativa maschile | 06_eda_block2, `5b9d21e6` | SMENTITA | Solo confronti numerici |
| 31 | Q2.2: pattern ↔ "letteratura su abuso sessuale/violenza domestica" | 06_eda_block2, `5b9d21e6` | SMENTITA | Frase assente |
| 32 | Q2.2: "potrebbe riflettere" esposizione anziani a crimini di strada | 06_eda_block2, `5b9d21e6` | SMENTITA | Frase assente |
| 33 | Q2.3: vulnerabilità fisica/presenza in casa anziani | 06_eda_block2, `9fec3bcd` | SMENTITA | Cella riscritta, solo top-3 |
| 34 | Q2.3: "nota vulnerabilità" anziani a truffe digitali | 06_eda_block2, `9fec3bcd` | SMENTITA | Frase assente |
| 35 | Q2.3: reattività fisica ridotta ↔ bersaglio facile | 06_eda_block2, `9fec3bcd` | SMENTITA | Frase assente |
| 36 | Q2.3 trend: anomalia classificazione LAPD (bis) | 06_eda_block2, `886b849d` | SMENTITA | Rimanda solo alla nota introduttiva |
| 37 | Q2.3 trend: calo 2018-20 ↔ sistemi di sicurezza domestica | 06_eda_block2, `886b849d` | SMENTITA | Frase assente |
| 38 | Q2.3 trend: rimbalzo 2020-23 ↔ digitalizzazione forzata | 06_eda_block2, `886b849d` | SMENTITA | Frase assente |
| 39 | Q2.3 trend: Petty Theft ↔ COVID/mobilità ridotta | 06_eda_block2, `886b849d` | SMENTITA | Solo andamento numerico |
| 40 | Q3.1: crimini domestici "storicamente sotto-denunciati" | 07_eda_block3, `90aec861` | SMENTITA | Sostituito con nota su limite del flag `is_domestic` |
| 41 | Q3.1 trend: minimo 2015 ↔ anomalia classificazione | 07_eda_block3, `a6fa86d9` | SMENTITA | Rimanda solo al pattern generale |
| 42 | Q3.1 trend: rialzo ↔ possibile cambio policy/campagne | 07_eda_block3, `a6fa86d9` | SMENTITA | Frase assente |
| 43 | Q3.1 trend: "letteratura criminologica internazionale" e COVID | 07_eda_block3, `a6fa86d9` | SMENTITA | Frase assente |
| 44 | Q3.1 trend: "tre possibili spiegazioni" | 07_eda_block3, `a6fa86d9` | SMENTITA | Frase assente |
| 45 | Q3.3: minori "probabilmente" vittime secondarie/adolescenti in coppia | 07_eda_block3, `a4617670` | SMENTITA | Sostituito con nota sui codici MO 05xx |
| 46 | Q3.4: strangolamento "indicatore criminologico" | 07_eda_block3, `724ef7d6` | SMENTITA | Fonte aggiunta (Glass et al., 2008) — cfr. nota sopra e Sezione 4 |
| 47 | Q3.4: intossicazione sospetto "correlata ma non causa" | 07_eda_block3, `724ef7d6` | SMENTITA | Frase di causa/correlazione rimossa |
| 48 | Q4.1: Hand Gun dominante ↔ "facile ottenere/occultare" | 08_eda_block4, `57ce322f` | SMENTITA | Nessuna causa di disponibilità |
| 49 | Q4.1: "il dataset conferma" armi facili da ottenere | 08_eda_block4, `57ce322f` | SMENTITA | Frase assente |
| 50 | Q4.1 trend: calo 2015 ↔ anomalia classificazione | 08_eda_block4, `9ac118ed` | SMENTITA | Rimanda solo al pattern generale (cfr. rilievo BLOCCANTE Sez. 3) |
| 51 | Q5.1: "Other" tasso intermedio ↔ sospetto "già noto" | 09_eda_block5, `791e81eb` | SMENTITA | Cella riscritta, solo percentuali |
| 52 | Q5.1: furto/vandalismo "strutturalmente più difficili" | 09_eda_block5, `791e81eb` | SMENTITA | Frase assente |
| 53 | Q5.2: violenti "traumatici, segnalati subito" vs furti | 09_eda_block5, `965c19b5` | SMENTITA | Solo media/mediana |
| 54 | Coordinate offuscate al centroide, "project limitations" | 10_advanced_eda_block1, `cf7283ea` | SMENTITA | Fonte corretta: documentazione ufficiale LAPD |
| 55 | Q6.2: eccezione 2015 ↔ anomalia classificazione | 10_advanced_eda_block1, `21b36b74` | SMENTITA | Rimanda solo alla nota introduttiva (cfr. Sez. 3) |
| 56 | Q7.1: picchi gennaio/febbraio ↔ Capodanno/eventi speciali | 11_advanced_eda_block2, `16454d22` | SMENTITA | Nessuna ipotesi su date specifiche |
| 57 | Q7.3: coordinate offuscate, "project limitations" (bis) | 11_advanced_eda_block2, `91f3ac70` | SMENTITA | Fonte corretta; cifra "(50-100 meters)" anche rimossa |
| 58 | Q7.4: coordinate offuscate (tris) | 11_advanced_eda_block2, `93bc35dc` | SMENTITA | Fonte corretta |

---

## 3. Revisione per notebook

### [01_plan/01_data_loading.ipynb](01_plan/01_data_loading.ipynb)

- **[ALTA]** (Verrà inserita nella publicazione su kaggle) *Cella `72111801` ("Conclusions — Plan Phase")* — La cella non
  contiene più alcun riferimento all'anomalia 2015–2016 né al troncamento del
  2024 (rimossa interamente, non riformulata: verificato leggendo il JSON
  sorgente della cella). Almeno 6 celle markdown a valle, in 5 notebook
  diversi, rimandano però esplicitamente a questa nota come fonte:
  05_eda_block1 cella `9ddebde8` ("consistent with the introductory
  methodological note"), 06_eda_block2 cella `886b849d` ("as documented in
  the introductory methodological note"), 07_eda_block3 cella `a6fa86d9`
  ("consistent with the pattern observed across all charts" — riferimento
  implicito allo stesso pattern), 08_eda_block4 cella `9ac118ed`
  ("consistent with the anomalous behavior documented in the introductory
  methodological note"), 10_advanced_eda_block1 cella `21b36b74` ("documented
  in the introductory methodological note"). → **Impatto**: un lettore che
  segue il progetto dal notebook 01 in poi non trova mai la nota a cui questi
  cinque notebook rimandano; l'unica evidenza dell'anomalia resta il dato
  grezzo nella tabella "Records per year" (cella `b14e45ec`, non interpretata).
  Il progetto perde tracciabilità sul motivo per cui 2015, 2016 e 2024 sono
  trattati sistematicamente come outlier in tutti i blocchi EDA. →
  **Azione suggerita**: ripristinare in `72111801` una nota puramente
  fattuale e verificabile dai dati già presenti nel notebook (es.: "Years
  2015 (168,076 records) and 2016 (283,798 records) deviate from the
  ~200-230k/year range of surrounding years; year 2024 (127,567 records) is
  partial. No cause is established here; these three years are treated as
  outliers in the trend analyses that follow."), senza reintrodurre
  l'ipotesi UCR→NIBRS né la promessa di verifica del mese di troncamento.

- **[MEDIA]** *Cella `72111801`, questione di fondo* — Anche a prescindere
  dal problema di coerenza sopra, la domanda sostanziale posta dall'
  osservazione originaria #2 (in quale mese si interrompe la rilevazione
  2024) non risulta mai calcolata in nessuno dei 12 notebook: nessuna cella
  produce `df[df.year==2024]['month'].value_counts()` o equivalente. →
  **Impatto**: il 2024 viene sistematicamente escluso/trattato come outlier
  (05, 09-Q7.1, 11) senza che sia mai stato accertato se si tratti di un
  troncamento a un mese preciso (rendendo il confronto anno-su-anno
  potenzialmente ingannevole per gennaio-giugno 2024) o di una raccolta
  discontinua nel corso dell'anno. → **Azione suggerita**: aggiungere un
  controllo esplicito sulla distribuzione mensile 2024 in un notebook EDA (o
  in 01 stesso), oppure dichiarare esplicitamente che la verifica non è mai
  stata fatta e che 2024 è escluso per precauzione senza ulteriore indagine.

- **[BASSA]** *Riproducibilità — numeri di esecuzione delle celle* — La
  cella `b14e45ec` (tabella "Records per year") ha `execution_count = 18`,
  mentre le celle immediatamente circostanti nel notebook hanno
  `execution_count` 10 e 12: la cella è stata rieseguita molto più tardi
  rispetto alle celle vicine, fuori sequenza. → **Impatto**: nessun impatto
  sul risultato numerico in questo caso specifico (la cella calcola solo un
  conteggio da `DATE OCC`, non dipende da stato di celle successive), ma è
  un'evidenza diretta che il notebook non è stato eseguito con un singolo
  "Restart & Run All" lineare. → **Azione suggerita**: eseguire "Restart &
  Run All" prima della consegna finale, per garantire che gli output
  visibili corrispondano a un'esecuzione lineare verificabile.

- **[BASSA]** *Cella `b14e45ec`, output stderr* — Il warning di sistema
  mostra un percorso Windows (`C:\Users\Alessandro\AppData\Local\Temp\
  ipykernel_12580\...`), mentre il warning equivalente in 02_cleaning.ipynb
  (cella `6e389510`) mostra un percorso macOS (`/var/folders/wl/...`). →
  **Impatto**: conferma che i notebook sono stati eseguiti in momenti/macchine
  diversi, coerente con il rilievo di riproducibilità sopra; nessun impatto
  sui dati. → **Azione suggerita**: nessuna azione necessaria oltre a quanto
  già raccomandato sopra; opzionale, ripulire l'output per evitare di
  esporre un nome utente locale in un deliverable pubblico.

### [02_analyze/02_cleaning.ipynb](02_analyze/02_cleaning.ipynb)

Nessun rilievo metodologico rilevante. Le operazioni di pulizia (drop
colonne, conversione tipi, gestione sentinel su LAT/LON e Vict Age, gestione
duplicati, encoding null) sono tutte giustificate nel testo e verificate con
controlli prima/dopo nella stessa cella o nella cella successiva. Unico
dettaglio cosmetico: nella cella `acc496cb` la citazione dalla documentazione
LAPD ha una virgoletta di apertura senza chiusura ("'Some location fields...
maintain privacy" senza `'` finale) — refuso di formattazione, non un
problema di supporto ai dati. **[BASSA]**.

### [02_analyze/04_feature_engineering.ipynb](02_analyze/04_feature_engineering.ipynb)

Nessun rilievo metodologico rilevante. Le feature derivate (`year`, `month`,
`day_of_week`, `hour_bins`, `age_group`, `crime_category`, `report_delay`,
`is_domestic`) sono tutte costruite in modo trasparente, con la lista
`person_crimes`/`property_crimes` costruita manualmente e ispezionabile per
intero nel notebook (nessuna "black box").

- **[BASSA]** *Cella `07bb62db`/`52e101d5` (Report delay)* — `report_delay`
  ha una distribuzione fortemente asimmetrica (media 21,06 giorni, mediana 1
  giorno, max 5.407 giorni ≈ 14,8 anni) mai discussa nel notebook. →
  **Impatto**: minimo qui, ma il valore massimo di 5.407 giorni è
  implausibile per la maggior parte dei tipi di reato e non viene mai
  ispezionato (nessun controllo su outlier o valori aberranti nel delay). →
  **Azione suggerita**: aggiungere una nota o un controllo sui casi con
  `report_delay` estremo, anche solo per verificare che non riflettano un
  errore nei dati sorgente (es. `Date Rptd` != anno del report reale).

### [02_analyze/05_eda_block1.ipynb](02_analyze/05_eda_block1.ipynb)

- **[BASSA]** *Cella `c5de5616` (Q1.4 — Part 2 intro)* — Il testo afferma
  "Crimes against property show peaks in March and October, a
  counterintuitive pattern", ma dai valori medi mensili calcolati nella
  cella precedente (`ba39eeb0`: gennaio 11.635,87; febbraio 10.208,93; marzo
  10.928,20; ...; ottobre 10.976,93) il vero massimo assoluto è **gennaio**
  e il minimo **febbraio**; marzo e ottobre sono rimbalzi locali dopo un
  calo rispetto al mese precedente, non i picchi annuali. Il paragrafo
  precedente (`6b111396`, Part 1) aveva correttamente identificato gennaio
  come massimo. → **Impatto**: il lettore che legge solo la Part 2 rischia
  di concludere erroneamente che marzo/ottobre siano i mesi di punta per i
  reati contro il patrimonio, quando lo è gennaio. → **Azione suggerita**:
  riformulare come "local rebounds in March and October, following the
  post-January and post-September dips" oppure richiamare esplicitamente
  che gennaio resta il mese di massimo assoluto.

- **[MEDIA]** *Riproducibilità — numeri di esecuzione delle celle* — La
  cella `d3945475` (preparazione dati Q1.2, `person_vs_property_yearly_df`)
  ha `execution_count = 21`, il valore più alto dell'intero notebook (20
  celle di codice, max = 21): è stata eseguita per **ultima** nell'intera
  sessione, pur trovandosi all'inizio del notebook (subito dopo Q1.1). Le
  celle `6cff5475` e `ba39eeb0` (preparazione dati Q1.4, usate anche nel
  rilievo precedente) hanno invece `execution_count` 2 e 3 — tra le **prime**
  eseguite in assoluto nella sessione, pur trovandosi a metà notebook. →
  **Impatto**: gli output visibili in questo notebook non derivano da
  un'unica esecuzione lineare dall'alto verso il basso; non è possibile
  escludere che, durante lo sviluppo, celle successive abbiano
  temporaneamente alterato variabili con lo stesso nome (es. `df`,
  `top5_grouped`) usate più avanti, producendo output non riproducibili con
  un semplice "Run All". → **Azione suggerita**: eseguire "Restart & Run
  All" e ri-salvare prima della consegna finale.

### [02_analyze/06_eda_block2.ipynb](02_analyze/06_eda_block2.ipynb)

Nessun rilievo metodologico rilevante oltre a quanto già in Sezione 2. I
confronti percentuali (sesso, età, descent) sono stati riverificati a
campione contro gli output numerici delle celle di codice corrispondenti e
risultano corretti (es. Male 44,2%/1.359.841, Female 40,0%/1.230.565,
Adult 47,4%/1.158.297 su 2.447.032 con età nota — tutti coerenti).

### [02_analyze/07_eda_block3.ipynb](02_analyze/07_eda_block3.ipynb)

Nessun rilievo aggiuntivo oltre a quanto già in Sezione 2. Il confronto
domestic/non-domestic (sesso, età, descent) è internamente coerente con i
totali di 06_eda_block2 (es. il totale Female non-domestic 1.059.290 +
domestic 171.275 = 1.230.565, uguale al totale Female dell'intero dataset
riportato in 06).

### [02_analyze/08_eda_block4.ipynb](02_analyze/08_eda_block4.ipynb)

- **[MEDIA]** *Cella `2629780b` (Observations Q4.1 — Distribution by LAPD
  area)* — Il testo riporta "Southeast: 12,121" e "Southwest: 8,908" tra le
  aree con il maggior volume di reati con arma da fuoco. L'output della
  cella di codice immediatamente precedente (`ab6594d2`) mostra invece
  Southeast = **12.222** e Southwest = **8.938**. → **Impatto**: differenza
  piccola (101 e 30 casi, <1%) che non cambia il ranking né le conclusioni
  qualitative, ma è un disallineamento verificabile e concreto tra prosa e
  output di codice nella stessa sezione. → **Azione suggerita**: correggere
  i due valori in prosa per farli coincidere con l'output della cella
  `ab6594d2`. Da notare che, a differenza di quanto segnalato in una
  revisione precedente, il confronto di ranking con i crimini domestici
  presente nella stessa cella (77th Street e Southeast stesso rango; Newton
  5°→3°; Southwest 3°→4°) è invece **corretto** e coerente con i dati di
  07_eda_block3 cella `1ddd9a29`.

- **[BASSA]** *Cella `9ac118ed` (Observations Q4.1 — Yearly trend)* — Il
  testo rimanda genericamente a "the anomalous behavior documented in the
  introductory methodological note" e a "the already documented incomplete
  data" (per il 2024): stesso problema di riferimento pendente descritto nel
  rilievo ALTA su 01_data_loading.ipynb.

### [02_analyze/09_eda_block5.ipynb](02_analyze/09_eda_block5.ipynb)

- **[BASSA]** *Cella `ae0558ae`/`e500b393` (Q5.2 — filtro report delay)* —
  Il commento della cella `e500b393` dice "Exclude negative delays (data
  entry errors)" e il codice applica `q52_df['report_delay'] >= 0`. Tuttavia,
  dal calcolo di `report_delay` in 04_feature_engineering (cella
  `52e101d5`), il valore minimo su tutti i 3.079.424 record è già **0.00**
  (`Date Rptd` non è mai antecedente a `DATE OCC`): non possono quindi
  esistere valori negativi nel dataset, e il filtro `>= 0` non può rimuovere
  alcuna riga. → **Impatto**: nessuno sul risultato numerico (il filtro è
  innocuo), ma il commento suggerisce l'esistenza di un problema di qualità
  dati (valori negativi) che di fatto non esiste in questo dataset. →
  **Azione suggerita**: rimuovere il filtro ridondante o correggere il
  commento per chiarire che è un controllo difensivo, non un'evidenza di
  errori riscontrati.

- **[BASSA]** *Riproducibilità — numeri di esecuzione delle celle* — La
  cella `bc7b3867` (grafico clearance rate per area, con annotazione
  Spearman ρ) ha `execution_count = 14`, mentre la cella successiva nel
  notebook (`decb5b17`, grafico clearance rate per categoria) ha
  `execution_count = 9`: la prima è stata eseguita **dopo** la seconda,
  fuori sequenza. Questo è coerente con l'ipotesi che l'annotazione
  Spearman sia stata aggiunta in un secondo momento, per chiudere il caso
  ambiguo #1 di `revisione_notebook.md` — coerente con quanto verificato in
  Sezione 2, ma resta un'ulteriore prova di esecuzione non lineare.

### [02_analyze/10_advanced_eda_block1.ipynb](02_analyze/10_advanced_eda_block1.ipynb)

- **[MEDIA]** *Cella `a35b67d4` vs cella `8ecf7579` (Moran's I)* — La cella
  di codice stampa `z-score: 378.3961`, ma la cella di osservazioni
  immediatamente seguente riporta "**z-score = 376.82**" — un valore
  diverso da quello effettivamente presente nell'output salvato. → **Causa
  più probabile**: la chiamata `Moran(grid_with_counts['crime_count'], w)`
  (cella `a35b67d4`) non imposta alcun parametro `seed`; il p-value e lo
  z-score restituiti (`p_sim`, `z_sim`) derivano da un test di permutazione
  (999 permutazioni di default) intrinsecamente stocastico. È quindi
  plausibile che il testo sia stato scritto sulla base di una esecuzione
  precedente della cella, e che una successiva riesecuzione (senza seed)
  abbia prodotto un valore leggermente diverso, senza che il testo venisse
  risincronizzato. → **Impatto**: la conclusione qualitativa ("clustering
  spaziale fortemente significativo") non cambia, ma il numero specifico
  citato nel testo non corrisponde a ciò che un lettore ottiene rieseguendo
  la cella, e in generale il risultato non è esattamente riproducibile a
  ogni nuova esecuzione. Lo stesso problema di assenza di seed si applica a
  `G_Local(...)` nella cella `543155e7` (Getis-Ord Gi*, `permutations=999`,
  nessun `seed`). → **Azione suggerita**: impostare un `seed` fisso in
  entrambe le chiamate (`Moran(..., permutations=999, seed=42)` e
  `G_Local(..., permutations=999, seed=42)`, se supportato dalla versione di
  `esda` in uso) e riallineare i numeri citati nel testo con l'output
  effettivo dopo la fissazione del seed.

- **[BASSA — da verificare, ipotesi]** *Cella `543155e7` (Getis-Ord Gi*)* —
  La chiamata è `G_Local(grid_with_counts['crime_count'], w, star=None,
  permutations=999)`. In Python, `None` è "falsy": se il codice sorgente di
  `esda.getisord.G_Local` verifica il parametro con un semplice `if star:`,
  passare `star=None` produrrebbe lo stesso comportamento di `star=False`,
  cioè il calcolo della statistica **Gi** (che esclude la cella stessa dal
  proprio vicinato) e non **Gi\*** (che la include) — nonostante il notebook
  si riferisca sempre e solo a "Getis-Ord Gi\*" nel testo, nei titoli di
  sezione e nei commenti. Non è stato possibile verificare il comportamento
  esatto della versione di `esda` installata senza eseguire codice, quindi
  questo rilievo resta un'ipotesi da verificare, non un fatto accertato —
  riportato anche come caso ambiguo in Sezione 4.

### [02_analyze/11_advanced_eda_block2.ipynb](02_analyze/11_advanced_eda_block2.ipynb)

- **[ALTA]** *Cella `c69393b5` (Q7.2 — preparazione dati per il Random
  Forest)* — `q72_df = q72_df.dropna()` applicato dopo aver selezionato le
  feature (incluso `age_group`) riduce il dataset da 3.079.424 a 2.447.032
  record (**-20,5%**), esattamente lo stesso numero di record con
  `Vict Age`/`age_group` nullo già documentato in 02_cleaning e
  04_feature_engineering. Questi record non sono un campione casuale: sono
  in larga parte reati senza vittima diretta identificabile (furto
  d'auto, vandalismo — categoria "property"), la stessa categoria con il
  clearance rate più basso dell'intero dataset (9,0%, cella `3851d172` in
  09_eda_block5). Il tasso di "cleared" nel sottoinsieme usato per il
  modello è infatti 598.847/2.447.032 = 24,5% (cella `c69393b5`), un valore
  intermedio tra il 45,2% di "person" e il 9,0% di "property" sull'intero
  dataset — segno che il filtro sbilancia la composizione rispetto alla
  popolazione totale. → **Impatto**: il modello (e le sue metriche di
  precision/recall, e l'analisi di feature importance) è addestrato e
  valutato su una popolazione sistematicamente diversa da quella descritta
  nell'obiettivo dichiarato ("predict the probability of a case being
  cleared" per l'intero dataset); i risultati non si generalizzano ai
  ~632k reati con vittima non identificata, che sono proprio il segmento
  "property" a bassa risoluzione più rilevante per la domanda di ricerca.
  Il notebook non menziona questa esclusione né la relativa distorsione. →
  **Azione suggerita**: dichiarare esplicitamente la popolazione coperta dal
  modello ("the model covers only crimes with an identified/aged victim"),
  oppure imputare `age_group` con una categoria esplicita "Unknown" invece
  di scartare i record, per includere l'intera popolazione nel training e
  nella valutazione.

- **[MEDIA]** *Cella `c69393b5` e seguenti (Q7.2) — variabili demografiche
  come feature* — Il modello usa `Vict Sex`, `Vict Descent` e `age_group`
  (attributi della vittima, non del reato) come feature dirette per
  prevedere la probabilità di risoluzione del caso, con l'obiettivo
  dichiarato di "support investigation prioritization" (cella `fc634042`).
  Nessuna cella discute l'implicazione di derivare un punteggio operativo
  da attributi demografici della vittima, un tema centrale nella
  letteratura su equità e bias nei modelli di *predictive policing*. →
  **Impatto**: non è un errore tecnico, ma una scelta di design con
  ricadute etiche non discusse, rilevante se il modello venisse davvero
  usato per prioritizzare indagini. → Riportato anche come caso ambiguo in
  Sezione 4, trattandosi di una scelta di giudizio più che di un difetto
  tecnico.

- **[MEDIA]** *Cella `f51149d9` (Q7.3 — costruzione delle "transazioni"
  spazio-temporali)* — Il testo descrive la finestra come "same LAPD area,
  within 48 hours" (cella `0541a3cb`), ma il codice usa
  `q73_df['DATE OCC'].dt.floor('2D')`, che crea bin **fissi** di 2 giorni
  ancorati all'epoca (1970-01-01), non una finestra scorrevole di 48 ore
  centrata su ciascun evento. Due reati a pochi minuti di distanza ma a
  cavallo del confine di un bin finiscono in due "transazioni" diverse
  (falso negativo di co-occorrenza), mentre due reati distanti fino a quasi
  48 ore ma entrambi vicini al centro dello stesso bin vengono associati
  correttamente. → **Impatto**: la descrizione metodologica non corrisponde
  esattamente all'implementazione; l'impatto pratico sulle conclusioni è
  limitato dal fatto che il notebook stesso, nella cella `91f3ac70`, dichiara
  già i risultati di Q7.3 "not used in the project's conclusions" per altri
  limiti (granularità geografica). → **Azione suggerita**: correggere la
  descrizione testuale in "same fixed 2-day calendar window" oppure
  implementare una vera finestra scorrevole (es. rolling join per timestamp)
  se l'analisi venisse ripresa in futuro.

### [03_construct/12_construct_block1.ipynb](03_construct/12_construct_block1.ipynb)

- **[ALTA]** *Cella `10e33775` ("GridSearchCV — not completed")* — Il testo
  descrive due tentativi di `GridSearchCV` sul dataset completo, entrambi
  "interrupted after more than 4 hours of processing", con "1,200 fits (240
  combinations × 5 folds)". Il notebook importa `GridSearchCV` (cella
  `c358b681`) ma **non lo istanzia né lo esegue mai**: non esiste nessuna
  cella con `GridSearchCV(...)`, nessun `param_grid` a 240 combinazioni,
  nessun output parziale o traceback di interruzione. → **Impatto**: è
  esattamente il tipo di affermazione non verificabile dal contenuto del
  notebook che questa revisione è incaricata di intercettare — non c'è modo,
  leggendo solo questo file, di confermare che il tentativo sia realmente
  avvenuto, né con quali parametri esatti. Le conclusioni operative del
  notebook ("RandomizedSearchCV is the recommended approach") restano
  ragionevoli sulla base dei risultati che *sono* effettivamente presenti,
  ma il paragrafo sul perché GridSearchCV sia stato scartato non è
  supportato da alcuna evidenza nel notebook stesso. → **Azione suggerita**:
  aggiungere la cella (anche solo con `param_grid` e il comando, commentata
  se non più eseguibile) che documenti il tentativo, oppure riformulare la
  spiegazione come stima a priori ("a full grid of 240 combinations × 5
  folds was judged computationally infeasible and not attempted") invece di
  descrivere un'esecuzione realmente avvenuta.

- **[BASSA]** *Cella `13483b7e` (intro) vs cella `10e33775` (conclusioni)* —
  L'introduzione definisce "**Approach A**: GridSearchCV..." e "**Approach
  B**: RandomizedSearchCV...", ma la sezione finale scrive "Parameters
  identified via **RandomizedSearchCV (Approach A)**" — invertendo
  l'etichetta rispetto alla propria definizione iniziale. → **Impatto**:
  solo di chiarezza espositiva, il contenuto tecnico non è ambiguo dal
  contesto. → **Azione suggerita**: correggere l'etichetta in "Approach B"
  nella sezione finale, coerentemente con l'introduzione.

- **[BASSA]** *Cella `c358b681` (import)* — `RandomizedSearchCV` viene
  importato due volte (una volta insieme a `train_test_split` e
  `GridSearchCV`, una seconda volta da solo). Innocuo, solo pulizia del
  codice.

Per il resto, il confronto Baseline vs Optimized (accuracy 71%→72%, recall/
precision/F1 su "Cleared" invariati) è coerente tra i due notebook (11 e 12)
e supportato dagli output di codice in entrambi. La scelta di
RandomizedSearchCV su un campione al 20% con la citazione di Bergstra &
Bengio (2012) resta correttamente sourced.

### [00_additional_charts/additional_charts.ipynb](00_additional_charts/additional_charts.ipynb)

Nessun rilievo. Notebook privo di celle markdown interpretative (3 celle di
codice: import/setup, aggregazione, grafico a ciambella person/property/
other). Il grafico salvato su disco è disabilitato via commento
(`#plt.savefig(...)`) con una nota esplicita nel commento stesso ("Left
disabled: the output file already exists in the repo") — comportamento
intenzionale, non un errore.

---

## 4. Casi ambigui / da chiarire

1. **Nota metodologica introduttiva su 2015-2016/2024 (01_data_loading,
   cella `72111801`)** — È stata rimossa del tutto, lasciando pendenti i
   riferimenti a valle in 05, 06, 07 (implicito), 08, 10. **Domanda**: la
   nota va ripristinata in forma neutra (solo dato, nessuna causa) per
   sanare i riferimenti a valle, oppure i riferimenti a valle vanno rimossi
   uno per uno per allinearli alla scelta di non discutere più il tema nel
   notebook 01? **Opzioni**: (a) ripristinare una versione fattuale della
   nota in 01; (b) eliminare tutti i rimandi "as documented in the
   introductory methodological note" dai notebook 05/06/08/10 e sostituirli
   con la semplice osservazione del pattern nel grafico corrente, senza
   rimando incrociato.

2. **Mese di troncamento del 2024** — Mai calcolato in nessun notebook.
   **Domanda**: vale la pena investire il tempo per calcolarlo (una singola
   cella `value_counts()` per mese), dato che il 2024 viene comunque sempre
   escluso dalle analisi di trend? **Opzioni**: (a) calcolarlo e riportarlo,
   utile anche per contestualizzare eventuali confronti parziali 2024 in
   sede di reportistica finale; (b) dichiarare esplicitamente che non
   verrà calcolato perché 2024 è escluso a priori da ogni confronto
   quantitativo nel progetto.

3. **`G_Local(..., star=None, ...)` in 10_advanced_eda_block1 — Gi vs Gi\*** —
   Non verificabile senza eseguire codice o consultare la documentazione di
   `esda` per la versione installata. **Domanda**: il parametro `star=None`
   produce la statistica Gi\* (con self-inclusione) come dichiarato nel testo,
   o la statistica Gi standard? **Opzioni**: (a) verificare nella
   documentazione/sorgente di `esda` e, se necessario, correggere il codice
   in `star=True`; (b) se il comportamento attuale è quello desiderato,
   aggiungere un commento esplicito che lo chiarisca, dato che il default
   della libreria e il valore passato (`None`) non coincidono banalmente con
   `True`.

4. **Citazione "Glass et al., 2008, Journal of Emergency Medicine"
   (07_eda_block3, cella `724ef7d6`)** — Aggiunta per sanare l'osservazione
   #46, ma non verificabile dall'interno del repository (fuori ambito per
   una revisione in sola lettura sui soli notebook). **Domanda**: la
   citazione è stata verificata esternamente da chi l'ha aggiunta?
   **Opzioni**: (a) confermare la fonte con una verifica bibliografica
   puntuale prima della pubblicazione del report finale; (b) in assenza di
   verifica, attenuare a una formulazione puramente descrittiva senza
   riferimento bibliografico specifico.

5. **Uso di attributi demografici della vittima come feature nel modello di
   clearance (11_advanced_eda_block2, Q7.2)** — Scelta di design legittima
   in un contesto esplorativo, ma non discussa eticamente nonostante
   l'obiettivo dichiarato di "support investigation prioritization".
   **Domanda**: il modello è inteso come puro esercizio esplorativo/EDA
   avanzata, o è pensato come prototipo di uno strumento operativo?
   **Opzioni**: (a) se puramente esplorativo, aggiungere una nota che lo
   chiarisca esplicitamente e discuta il rischio di bias se mai
   operazionalizzato; (b) se prototipale, ripetere l'addestramento escludendo
   le feature demografiche e confrontare il calo di performance, per
   quantificare quanto il modello dipenda da esse.

---

## 5. Problemi trasversali

- **Riferimenti incrociati "pendenti"**: la rimozione di contenuto in un
  notebook senza aggiornare i notebook a valle che vi rimandano esplicitamente
  è il pattern più significativo emerso in questa revisione (vedi 01 → 05/06/
  08/10) e un rischio strutturale ogni volta che si modifica un notebook
  "early" nella pipeline senza un controllo grep sui notebook successivi.

- **Affermazioni narrative non verificabili dal contenuto del notebook**:
  oltre alle 58 osservazioni originarie (tutte relative a spiegazioni
  causali esterne al dataset), questa revisione ne ha trovata una categoria
  concettualmente identica ma di natura diversa — affermazioni sul *processo
  di lavoro* stesso (i tentativi di GridSearchCV in 12_construct_block1) prive
  di riscontro nel codice. Lo stesso principio di verificabilità applicato
  alle ipotesi causali andrebbe esteso alle affermazioni procedurali.

- **Test statistici stocastici senza seed**: Moran's I e Getis-Ord Gi* in
  10_advanced_eda_block1 sono gli unici test/modelli del progetto a non
  fissare un seed (a differenza di `train_test_split`, `RandomForestClassifier`
  e `RandomizedSearchCV`, che usano sistematicamente `random_state=42`).
  Conseguenza diretta e osservabile: un numero riportato nel testo (z-score
  Moran's I) non coincide con l'output di codice attualmente salvato.

- **Esecuzione non lineare delle celle**: i metadati `execution_count`
  mostrano celle rieseguite fuori sequenza in almeno 3 notebook (01, 05, 09),
  inclusi due casi (05, 09) in cui una cella collocata all'inizio del
  notebook è stata rieseguita per ultima o quasi-ultima nella sessione. Non
  ci sono evidenze che questo abbia alterato risultati numerici già
  verificati in questa revisione, ma è una pratica che rende gli output
  vulnerabili a stato nascosto non riproducibile con un "Run All" lineare.

- **Piccoli disallineamenti numerici prosa/codice**: oltre al caso di
  08_eda_block4 (Southeast/Southwest) e 10_advanced_eda_block1 (z-score
  Moran's I), non ne sono emersi altri nella lettura integrale — la
  disciplina di riportare nel testo esattamente i numeri stampati dal
  codice è buona nel resto del progetto.

- **Nessun problema di leakage, join o duplicazione residua**: la pipeline
  01→04 è stata riverificata riga per riga: niente target leakage nei
  modelli (Q7.2 usa solo feature note al momento della segnalazione;
  `report_delay` è calcolato correttamente come `Date Rptd - DATE OCC` con
  valore minimo 0), nessun duplicato residuo dopo `02_cleaning`, nessun join
  esterno nel progetto (i modelli di 11 e 12 lavorano tutti sullo stesso
  `crimes_features.parquet`).

Fine del report.
