# Revisione notebook — Verifica remediation + integrità analitica

**Ambito**: notebook in `01_plan/`, `02_analyze/`, `03_construct/` (+ `00_additional_charts/`, citato nel file sorgente). Nessuna cella è stata modificata, eseguita, creata o rinominata: revisione in sola lettura.

**Nota preliminare sul file sorgente**: il prompt indica `osservazioni_da_modificare.md`, ma nella cartella `notebooks/` è presente solo `osservazioni_da_verificare.md` (57 KB, spostato di recente dalla root del repository — risulta come `deleted` in root e `untracked` in `notebooks/` nello stato git). È stato usato questo file come checklist, trattandolo come l'elenco delle 58 osservazioni di una revisione precedente da verificare. Le voci non avevano un identificativo numerico esplicito per tutte le celle (solo le prime 6 erano numerate `### 1.`…`### 6.`): sono state numerate progressivamente **1–58** seguendo l'ordine del file sorgente, un numero per ogni citazione testuale segnalata (le celle con più citazioni generano più voci numerate).

**Fuori ambito**: i file `.md` di riepilogo fase (`plan_summary.md`, `analyze_summary.md`, `construct_summary.md`, `execute_summary.md`, `03_research_questions.md`) non sono notebook e non sono stati revisionati riga per riga; il file sorgente stesso copre solo celle di notebook.

---

## 1. Sommario esecutivo

**Conteggio per stato (58 osservazioni)**

| Stato | N |
|---|---|
| ✅ RISOLTO | 56 |
| ❌ NON RISOLTO | 2 |
| ⚠️ PARZIALE | 0 |
| 🔄 REGREDITO | 0 |
| ❓ NON VERIFICABILE | 0 |

Le 2 non risolte: **#2** (troncamento mese 2024 mai effettivamente verificato in EDA, nonostante il rimando esplicito) e **#46** (l'affermazione criminologica sullo strangolamento in 07_eda_block3, cella `724ef7d6`, è ancora presente parola per parola nonostante l'intestazione del file sorgente la segni come "RISOLTO").

| Notebook | Osservazioni aperte (non risolte) | Nuovi rilievi (Parte B) | Occorrenze "we + azione" |
|---|---|---|---|
| 01_plan/01_data_loading.ipynb | 1 | 0 | 2 |
| 02_analyze/02_cleaning.ipynb | 0 | 0 | 4 |
| 02_analyze/04_feature_engineering.ipynb | 0 | 0 | 6 |
| 02_analyze/05_eda_block1.ipynb | 0 | 0 | 9 |
| 02_analyze/06_eda_block2.ipynb | 0 | 0 | 8 |
| 02_analyze/07_eda_block3.ipynb | 1 | 0 (1 in casi ambigui) | 5 |
| 02_analyze/08_eda_block4.ipynb | 0 | 1 | 5 |
| 02_analyze/09_eda_block5.ipynb | 0 | 0 (1 in casi ambigui) | 1 |
| 02_analyze/10_advanced_eda_block1.ipynb | 0 | 0 | 4 |
| 02_analyze/11_advanced_eda_block2.ipynb | 0 | 0 (1 in casi ambigui) | 5 |
| 03_construct/12_construct_block1.ipynb | 0 | 0 | 0 |
| 00_additional_charts/additional_charts.ipynb | 0 | 0 | 0 |
| **Totale** | **2** | **1** | **49** |

**Valutazione complessiva**: il lavoro di remediation è stato eseguito in modo sistematico e quasi completo. Tutte le affermazioni causali non verificate legate a COVID-19, al presunto cambio di sistema di classificazione LAPD (UCR→NIBRS), a spiegazioni socio-comportamentali generiche (vita notturna, stagionalità festiva, orari di negozi/lavoro) e a riferimenti a "letteratura criminologica" generica sono state rimosse o riformulate in chiave puramente descrittiva/dati-supportata in tutti i notebook di EDA (05–11). Le uniche eccezioni sono un'azione di verifica mai eseguita (#2) e una singola citazione rimasta invariata per errore in un cluster di modifiche altrimenti riuscite (#46). Non sono emersi nuovi rilievi sistemici nella Parte B: il progetto mostra una disciplina di sourcing (citazioni a data.lacity.org, LAPD Crime Data Dictionary, LAPD MO Codes List, Bergstra & Bengio 2012) applicata in modo coerente.

---

## 2. Verifica osservazioni pregresse

Legenda: **RIS** = ✅ RISOLTO, **NR** = ❌ NON RISOLTO.

| # | Osservazione (sintesi) | Notebook e cella | Stato | Evidenza attuale (citazione esatta) | Azione residua |
|---|---|---|---|---|---|
| 1 | Anomalia 2015–2016 attribuita al cambio UCR→NIBRS | 01_data_loading.ipynb, cella `72111801` | **RIS** | "Years 2015–2016 show anomalous values (a drop in 2015, a spike in 2016). No confirmed structural cause was identified; these years are treated as outliers in subsequent trend analyses." | Nessuna |

| 2 | 2024 incompleto, mese di troncamento "da verificare in EDA" | 01_data_loading.ipynb, cella `72111801` | **RIS** | Testo invariato: "Year 2024 incomplete (127,567 records): the truncation month needs to be verified in EDA and handled accordingly in the temporal analyses." Nei notebook 05–11 il 2024 viene sempre e solo escluso/trattato come outlier (es. 05 cella `9ddebde8`: "2024 was excluded due to its anomalous behavior"), ma **nessuna cella calcola effettivamente** la data massima o la distribuzione mensile dei record 2024 per identificare il mese di troncamento. | Aggiungere in un notebook EDA un controllo esplicito (es. `df[df.year==2024]['month'].value_counts()` o data massima per il 2024) e riportarne l'esito nel testo, oppure rimuovere la promessa di verifica dalla nota del Plan phase se la scelta è di trattare 2024 come outlier senza ulteriore indagine. | - **RIMOSSA LA RIGA RELATIVA**

| 3 | Sentinella (0,0) presentata come convenzione LAPD non citata | 02_cleaning.ipynb, cella `acc496cb` | **RIS** | Aggiunta nota con fonte: "As documented in the LAPD dataset description (data.lacity.org): 'Some location fields with missing data are noted as (0°, 0°). Address fields are only provided to the nearest hundred block in order to maintain privacy" | Nessuna (nota: la citazione ha una virgoletta di chiusura mancante — refuso di formattazione, non un problema di supporto dati) |

| 4 | Duplicati attribuiti a "errore di caricamento generalizzato" | 02_cleaning.ipynb, cella `d24693c1` | **RIS** | Frase rimossa; testo attuale: "All 57,809 duplicated DR_NO turn out to be exact duplicates (rows identical across all columns), spread across 122 different crime types. We remove the duplicates, keeping the first occurrence of each row." | Nessuna |

| 5 | `Vict Age = 0` presentato come convenzione LAPD non citata | 02_cleaning.ipynb, cella `1c85663b`/`dbb86a23` | **RIS** | "Since no explicit convention was found in the data documentation, data will be treated as follow: `Vict Age = 0`... treated as a sentinel value for 'unknown age'. `Vict Age < 0`... treated as data entry errors." | Nessuna |

| 6 | Delay di segnalazione differenziato per tipo di reato (omicidio vs violenza sessuale) | 04_feature_engineering.ipynb, cella `07bb62db` | **RIS** | Frase rimossa; testo attuale: "We calculate the difference in days between the report date (`Date Rptd`) and the date the crime occurred (`DATE OCC`). This feature measures the reporting delay and will be used for Block 5 (Q5.2)." | Nessuna |

| 7 | Q1.1: anomalia 2015 collegata a UCR→NIBRS (ripetizione) | 05_eda_block1.ipynb, cella `6486503f` | **RIS** | Cella riscritta: riporta solo la top-5 crimini e i trend di crescita/declino più marcati, senza riferimenti a cambi di classificazione. | Nessuna |

| 8 | Q1.1: calo 2020 attribuito a COVID/"opportunity crimes" | 05_eda_block1.ipynb, cella `6486503f` | **RIS** | Come sopra: nessuna menzione di COVID o "opportunity crimes" nella cella attuale. | Nessuna |

| 9 | Q1.1: picco furti auto 2020–22 come "fenomeno documentato a livello nazionale" | 05_eda_block1.ipynb, cella `6486503f` | **RIS** | Come sopra: nessuna menzione di sorveglianza COVID/crisi economica/social media challenge. | Nessuna |

| 10 | Q1.2: calo persone amplificato da anomalia di classificazione | 05_eda_block1.ipynb, cella `d8c84328` | **RIS** | Cella riscritta in chiave puramente descrittiva (percentuali, picchi/minimi per anno), nessun riferimento a classificazione. | Nessuna |

| 11 | Q1.2: titolo paragrafo etichetta il calo 2020 come "COVID" | 05_eda_block1.ipynb, cella `d8c84328` | **RIS** | Nessun titolo/etichetta "COVID" nel testo attuale. | Nessuna |

| 12 | Q1.2: property "più volatile/reattiva a eventi esterni" | 05_eda_block1.ipynb, cella `d8c84328` | **RIS** | Frase assente dal testo attuale. | Nessuna |

| 13 | Q1.3: rimbalzo rapido/uniforme come possibile effetto normativo/di classificazione | 05_eda_block1.ipynb, cella `9ddebde8` | **RIS** | Testo attuale: "Both charts reflect the anomalous behavior of 2015 and the rebound in 2016, consistent with the introductory methodological note." (nessuna causa suggerita, rimanda alla nota introduttiva ormai corretta) | Nessuna |

| 14 | Q1.3: effetto COVID 2020 su furto auto +35% con fonti FBI UCR/NICB | 05_eda_block1.ipynb, cella `9ddebde8` | **RIS** | Testo attuale descrive solo "2020, where — with the exception of Vehicle Theft (+35%) — all other categories show significant declines", senza causa né fonti esterne. | Nessuna |

| 15 | Q1.3: normalizzazione Identity Theft legata a digitalizzazione post-pandemica | 05_eda_block1.ipynb, cella `9ddebde8` | **RIS** | Testo attuale riporta solo "Identity Theft increased by +95% in 2022... Shoplifting (Petty) increased by +82%...", nessuna causa digitale. | Nessuna |

| 16 | Q1.4 pt1: minimo di febbraio per ragioni climatiche/mobilità urbana | 05_eda_block1.ipynb, cella `6b111396` | **RIS** | Testo attuale riporta solo valori numerici mensili min/max per le due macro-categorie, nessuna causa climatica. | Nessuna |

| 17 | Q1.4 pt1: trend attribuito a furti da periodo festivo | 05_eda_block1.ipynb, cella `6b111396` | **RIS** | Frase assente dal testo attuale. | Nessuna |

| 18 | Q1.4 pt2: ipotesi attività online (shopping natalizio, dichiarazioni fiscali) | 05_eda_block1.ipynb, cella `ad510cad` | **RIS** | Cella riscritta: solo picchi/minimi mensili per Vehicle Theft, Vehicle Burglary, Burglary/Petty Theft, Identity Theft, nessuna ipotesi causale. | Nessuna |

| 19 | Q1.4 pt2: furti residenziali per "motivi legati al periodo natalizio" | 05_eda_block1.ipynb, cella `ad510cad` | **RIS** | Frase assente dal testo attuale. | Nessuna |

| 20 | Q1.5 intro: `hour_occ=0` come artefatto di registrazione (ora sconosciuta) | 05_eda_block1.ipynb, cella `bf146b2d` | **RIS** | Cella riscritta come solo introduzione/obiettivo della sezione, senza menzione dell'artefatto "0000". | Nessuna |

| 21 | Q1.5 obs: ripetizione "~130k record a mezzanotte" da leggere con cautela | 05_eda_block1.ipynb, cella `c525a1a5` | **RIS** | Frase assente dal testo attuale. | Nessuna |

| 22 | Q1.5 obs: weekend property più basso "probabilmente" per orari negozi | 05_eda_block1.ipynb, cella `c525a1a5` | **RIS** | Testo attuale: "For Property, the weekend shows slightly lower values than weekdays (Saturday 19,681, Sunday 18,268 vs ~22,000 on weekdays)." — solo dato, nessuna causa. | Nessuna |

| 23 | Q1.5 obs: "meno persone al lavoro, meno traffico" | 05_eda_block1.ipynb, cella `c525a1a5` | **RIS** | Frase assente dal testo attuale. | Nessuna |

| 24 | Q1.5 obs: picco venerdì per negozi affollati/trasporti pubblici pieni | 05_eda_block1.ipynb, cella `c525a1a5` | **RIS** | Testo attuale riporta solo i valori (Afternoon 70,800, Evening 73,995), nessuna causa comportamentale. | Nessuna |

| 25 | Q1.5 obs: weekend notturno per alcol/vita notturna | 05_eda_block1.ipynb, cella `c525a1a5` | **RIS** | Frase assente dal testo attuale. | Nessuna |

| 26 | Q1.5 obs: "pattern coerente con la vita notturna" (ripetizione) | 05_eda_block1.ipynb, cella `c525a1a5` | **RIS** | Frase assente dal testo attuale. | Nessuna |

| 27 | Q2.1 sesso: distribuzione bilanciata "probabilmente" per crimini domestici inclusi | 06_eda_block2.ipynb, cella `dde6cfb1` | **RIS** | Cella riscritta: solo percentuali M/F/X con nota sul significato di "Unknown", nessuna causa anticipata. | Nessuna |

| 28 | Q2.1 sesso: sovra-rappresentazione maschile "tipica della letteratura criminologica" | 06_eda_block2.ipynb, cella `dde6cfb1` | **RIS** | Frase assente dal testo attuale. | Nessuna |

| 29 | Q2.1 età: abuso minori "storicamente tra i reati più sotto-denunciati" | 06_eda_block2.ipynb, cella `d9976b1d` | RIS | Cella riscritta: solo conteggi/percentuali per fascia d'età + nota metodologica sui valori NaN esclusi. | Nessuna |

| 30 | Q2.2: differenza 35-64 anni per esposizione lavorativa/mobilità maschile | 06_eda_block2.ipynb, cella `5b9d21e6` | RIS | Cella riscritta: solo confronti numerici per fascia d'età e sesso tra Person/Property, nessuna causa esplicativa. | Nessuna |

| 31 | Q2.2: pattern "coerente con la letteratura su abuso sessuale/violenza domestica" | 06_eda_block2.ipynb, cella `5b9d21e6` | RIS | Frase assente dal testo attuale. | Nessuna |

| 32 | Q2.2: "potrebbe riflettere" maggiore esposizione anziani a crimini di strada | 06_eda_block2.ipynb, cella `5b9d21e6` | RIS | Frase assente dal testo attuale. | Nessuna |

| 33 | Q2.3 top10: vulnerabilità fisica/presenza in casa di giorno per anziani | 06_eda_block2.ipynb, cella `9fec3bcd` | RIS | Cella riscritta: solo top-3 reati e relativi conteggi, nessuna spiegazione fisiologica. | Nessuna |

| 34 | Q2.3 top10: "nota vulnerabilità" anziani a truffe digitali | 06_eda_block2.ipynb, cella `9fec3bcd` | RIS | Frase assente dal testo attuale. | Nessuna |

| 35 | Q2.3 top10: reattività fisica ridotta come bersaglio facile | 06_eda_block2.ipynb, cella `9fec3bcd` | RIS | Frase assente dal testo attuale. | Nessuna |

| 36 | Q2.3 trend: anomalia classificazione LAPD (ripetizione) | 06_eda_block2.ipynb, cella `886b849d` | RIS | Testo attuale: "The chart reflects the anomalous behavior in 2015–2016 and the sharp decline in 2024, as documented in the introductory methodological note." — nessuna causa specifica. | Nessuna |

| 37 | Q2.3 trend: calo 2018–2020 per adozione sistemi di sicurezza domestica | 06_eda_block2.ipynb, cella `886b849d` | RIS | Frase assente; testo attuale riporta solo l'andamento numerico di Burglary. | Nessuna |

| 38 | Q2.3 trend: rimbalzo 2020–2023 per digitalizzazione forzata da lockdown | 06_eda_block2.ipynb, cella `886b849d` | RIS | Frase assente dal testo attuale. | Nessuna |

| 39 | Q2.3 trend: Petty Theft calo COVID per minore mobilità | 06_eda_block2.ipynb, cella `886b849d` | RIS | Testo attuale: "Petty Theft grew steadily until 2019, then dropped sharply through 2020." — nessuna causa COVID/mobilità. | Nessuna |

| 40 | Q3.1 volume: crimini domestici "storicamente sotto-denunciati" (paura, dipendenza economica, vergogna) | 07_eda_block3.ipynb, cella `90aec861` | RIS | Sostituito con nota metodologica interna: "the `is_domestic` flag is based on keywords... This percentage should therefore be interpreted as a lower bound rather than an accurate estimate of the real phenomenon." | Nessuna |

| 41 | Q3.1 trend: minimo 2015 coerente con anomalia classificazione LAPD | 07_eda_block3.ipynb, cella `a6fa86d9` | RIS | Testo attuale: "The anomalous behavior of 2015–2016 is visible here as well, consistent with the pattern observed across all charts." — nessuna causa. | Nessuna |

| 42 | Q3.1 trend: rialzo rapido come possibile cambio di policy/campagne di sensibilizzazione | 07_eda_block3.ipynb, cella `a6fa86d9` | RIS | Frase assente dal testo attuale. | Nessuna |

| 43 | Q3.1 trend: letteratura criminologica internazionale su crimini domestici e COVID | 07_eda_block3.ipynb, cella `a6fa86d9` | RIS | Frase assente dal testo attuale. | Nessuna |

| 44 | Q3.1 trend: "tre possibili spiegazioni" per il rialzo | 07_eda_block3.ipynb, cella `a6fa86d9` | RIS | Frase assente dal testo attuale. | Nessuna |

| 45 | Q3.3: minori "probabilmente" presenti come vittime secondarie/adolescenti in relazioni | 07_eda_block3.ipynb, cella `a4617670` | RIS | Cella riscritta: nota metodologica basata sui codici MO ("classified as sexual abuse based on the prevalence of 05xx MO codes") + soli conteggi, nessuna ipotesi sul ruolo del minore. | Nessuna |

| 46 | Q3.4: strangolamento come "indicatore criminologico di alto rischio di escalation e recidiva" | 07_eda_block3.ipynb, cella `724ef7d6` | **NR** | Frase **ancora presente parola per parola**: "Worth noting in particular is Choked/Strangled (0408) — 22,738: strangulation is a criminological indicator of high risk of escalation and recidivism." Nessuna fonte citata, nessun test nel notebook. Il file sorgente marca l'intera cella come "RISOLTO", ma questa specifica affermazione non è stata toccata. | Aggiungere una fonte citabile (es. letteratura forense/criminologica specifica) oppure riformulare in chiave descrittiva (es. limitarsi a riportare il conteggio senza l'affermazione di rischio). |

| 47 | Q3.4: intossicazione sospetto "fattore correlato ma non causa" della violenza domestica | 07_eda_block3.ipynb, cella `724ef7d6` | RIS | Frase di causa/correlazione rimossa; testo attuale: "Suspect intoxicated/drunk (2002) — 10,991: the suspect's intoxication is recorded in approximately 5% of cases." — solo il dato. | Nessuna |

| 48 | Q4.1: Hand Gun dominante perché "facile da ottenere e occultare" | 08_eda_block4.ipynb, cella `57ce322f` | RIS | Testo attuale: "Hand Gun: 53,523 (~48%) — the dominant weapon by a wide margin" — nessuna causa di disponibilità/occultabilità. | Nessuna |

| 49 | Q4.1: "il dataset conferma" armi comuni facili da ottenere, assault rifle "difficili da occultare" | 08_eda_block4.ipynb, cella `57ce322f` | RIS | Frase assente; testo attuale: "Assault weapons: ... show negligible individual values ... remains a relevant data point for public safety" senza claim su occultabilità. | Nessuna |

| 50 | Q4.1 trend: calo 2015 coerente con anomalia classificazione LAPD | 08_eda_block4.ipynb, cella `9ac118ed` | RIS | Testo attuale: "The 2015 decline is consistent with the anomalous behavior documented in the introductory methodological note." — nessuna causa specifica di classificazione. | Nessuna |

| 51 | Q5.1: categoria "Other" con tasso intermedio perché sospetto "già noto alle autorità" | 09_eda_block5.ipynb, cella `791e81eb` | RIS | Cella riscritta: solo percentuali per categoria + nota metodologica generale sul significato del clearance rate, nessuna spiegazione su "Other". | Nessuna |

| 52 | Q5.1: furto/borseggio/vandalismo "strutturalmente più difficili" per assenza di testimoni/prove | 09_eda_block5.ipynb, cella `791e81eb` | RIS | Frase assente dal testo attuale. | Nessuna |

| 53 | Q5.2: crimini violenti "eventi traumatici segnalati immediatamente" vs furti scoperti in ritardo | 09_eda_block5.ipynb, cella `965c19b5` | RIS | Cella riscritta: solo media/mediana giorni di ritardo per categoria, nessuna spiegazione psicologica. | Nessuna |

| 54 | Coordinate "offuscate al centroide dell'isolato (~100m)" come documentato nei "project limitations" | 10_advanced_eda_block1.ipynb, cella `cf7283ea` | RIS | Riformulato con fonte corretta: "according to the official LAPD data documentation, address fields are provided only to the nearest hundred block in order to maintain privacy." | Nessuna |

| 55 | Q6.2: eccezione 2015 coerente con anomalia classificazione LAPD | 10_advanced_eda_block1.ipynb, cella `21b36b74` | RIS | Testo attuale: "The anomalous behavior of 2015 documented in the introductory methodological note is visible here as well." — nessuna causa specifica. | Nessuna |

| 56 | Q7.1: picchi anomali gennaio/febbraio "probabilmente" legati a Capodanno/eventi speciali | 11_advanced_eda_block2.ipynb, cella `16454d22` | RIS | Frase assente; testo attuale: "These represent days where the actual crime volume exceeded the model's expectations by a wide margin." — nessuna ipotesi su date specifiche. | Nessuna |

| 57 | Q7.3: coordinate offuscate al centroide (~100m), "project limitations" (ripetizione) | 11_advanced_eda_block2.ipynb, cella `91f3ac70` | RIS | Riformulato con fonte corretta: "According to the official LAPD data documentation, address fields are provided only to the nearest hundred block in order to maintain privacy — making it impossible to distinguish events that occurred in adjacent residences within the same area." (vedi nota in Sezione 5 sul dettaglio "50-100 meters") | Nessuna sull'osservazione originale |

| 58 | Q7.4: coordinate offuscate al centroide (~100m) (terza ripetizione) | 11_advanced_eda_block2.ipynb, cella `93bc35dc` | RIS | Stessa riformulazione sourced: "According to the official LAPD data documentation, address fields are provided only to the nearest hundred block in order to maintain privacy — making the distinction between 'same address' and 'same 500m×500m cell' less meaningful than it seems." | Nessuna |

---

## 3. Nuovi rilievi — assunzioni non supportate

### 02_analyze/08_eda_block4.ipynb

| # | Cella (indice + tipo) | Testo originale | Perché non è supportato | Correzione suggerita |
|---|---|---|---|---|
| 1 | Cella `2629780b` (markdown, "Observations Q4.1 — Distribution of firearm crimes by LAPD area") | "**Geographic pattern**: the four areas with the most firearm crimes (77th Street, Southeast, Newton, Southwest) coincide with the same areas that dominate the distribution of domestic crimes in Block 3. This reveals a significant geographic overlap between armed violence and domestic violence in the same areas of Los Angeles." | Confronto impreciso con dati già calcolati altrove nel progetto: in 07_eda_block3.ipynb, cella `1ddd9a29`, l'"High group" dei crimini domestici è definito esplicitamente come "77th Street (21,073), Southeast (17,431), Southwest (15,726)" — **Newton è invece collocata nel "Mid group"** insieme a Mission, Rampart, Harbor (~12.000–13.000), non tra le aree che "dominano" i crimini domestici. L'affermazione di un "overlap significativo" non è inoltre supportata da alcun test statistico (es. correlazione tra i ranking delle 21 aree), ma solo dal confronto visivo di due liste top-N, una delle quali non coincide del tutto. | Correggere la lista delle aree in overlap (es. "tre delle quattro aree coincidono con l'high group dei crimini domestici; Newton è invece nel gruppo intermedio") oppure quantificare l'overlap con una statistica (es. correlazione di Spearman tra i ranking delle 21 aree per crimini con arma da fuoco e crimini domestici) prima di parlare di "overlap significativo". |

Nessun altro rilievo sistemico di questo tipo è emerso negli altri notebook: le celle di osservazione, una volta rimosse le ipotesi causali flaggate nella Parte A, si limitano quasi sempre a riportare numeri già calcolati nelle celle di codice immediatamente precedenti, con occasionali riferimenti incrociati (es. a Block 2/3/4) che sono stati verificati e risultano numericamente corretti (vedi verifica incrociata in Sezione 5 per i due casi rimasti ambigui).

---

## 4. Formulazioni "we + azione"

Tutte le occorrenze di "We/we" seguito da un verbo d'azione nelle celle markdown, per notebook. Riformulazione proposta in forma impersonale/passiva, preservando il significato tecnico.

### 01_plan/01_data_loading.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 1 | Cella `47246899` (markdown, "3. Column verification and alignment") | "Let's compare the column names of the two DataFrames before concatenation." | "The column names of the two DataFrames are compared before concatenation." |
| 2 | Cella `a4bd2530` (markdown, "6. LAT/LON normalization") | "Before converting, we normalize the format by replacing the comma with a period." | "Before conversion, the format is normalized by replacing the comma with a period." |

### 02_analyze/02_cleaning.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 3 | Cella `c3043acc` (markdown, "2. Dropping irrelevant columns") | "Based on the initial inspection (Plan phase), we drop the columns that won't add value to the analyses:" | "Based on the initial inspection (Plan phase), the columns that won't add value to the analyses are dropped:" |
| 4 | Cella `f1d27649` (markdown, "3. Data type conversion / 3.1 Dates") | "We convert them to `datetime64` to be able to perform temporal operations, filters by year/month, interval calculations, etc." | "They are converted to `datetime64` to allow temporal operations, filters by year/month, interval calculations, etc." |
| 5 | Cella `acc496cb` (markdown, "3.3 Handling sentinel coordinates") | "We replace them with `NaN` instead of dropping the records: this way the records remain available..." | "They are replaced with `NaN` instead of dropping the records: this way the records remain available..." |
| 6 | Cella `89176322` (markdown, "8. Saving the cleaned dataset") | "We save the cleaned DataFrame as `crimes_clean.parquet` in `data/processed/`." | "The cleaned DataFrame is saved as `crimes_clean.parquet` in `data/processed/`." |

### 02_analyze/04_feature_engineering.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 7 | Cella `fa822cee` (markdown, "2. Temporal features") | "We extract from `DATE OCC` the temporal components that will be needed in almost all analyses..." | "The temporal components needed in almost all analyses are extracted from `DATE OCC`..." |
| 8 | Cella `507590b8` (markdown, "3. Time slot") | "We group `hour_occ` into 4-hour slots to simplify the temporal distribution analyses." | "`hour_occ` is grouped into 4-hour slots to simplify the temporal distribution analyses." |
| 9 | Cella `0e875861` (markdown, "4. Age groups") | "We group `Vict Age` into brackets that reflect the categories relevant to the research questions..." | "`Vict Age` is grouped into brackets that reflect the categories relevant to the research questions..." |
| 10 | Cella `02b3ff48` (markdown, "5. Crime macro-category") | "We classify crimes into two macro-categories — 'Person'... and 'Property'..." | "Crimes are classified into two macro-categories — 'Person'... and 'Property'..." |
| 11 | Cella `07bb62db` (markdown, "6. Report delay") | "We calculate the difference in days between the report date (`Date Rptd`) and the date the crime occurred (`DATE OCC`)." | "The difference in days between the report date (`Date Rptd`) and the date the crime occurred (`DATE OCC`) is calculated." |
| 12 | Cella `b50ee9a0` (markdown, "7. Domestic crime flag") | "We create a boolean column `is_domestic` to quickly identify domestic crimes." | "A boolean column `is_domestic` is created to quickly identify domestic crimes." |

### 02_analyze/05_eda_block1.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 13 | Cella `58a1f519` (markdown, intro) | "**Questions we answer**:" | "**Research questions addressed**:" |
| 14 | Cella `61b3a596` (markdown, "2. Q1.1") | "We identify the 5 offenses with the highest number of occurrences across the entire dataset (2010–2024), then visualize their year-over-year trend." | "The 5 offenses with the highest number of occurrences across the entire dataset (2010–2024) are identified, then their year-over-year trend is visualized." |
| 15 | Cella `60ff451e` (markdown, "3. Q1.2") | "We split crimes into two macro-categories using the `crime_category` feature created during feature engineering." | "Crimes are split into two macro-categories using the `crime_category` feature created during feature engineering." |
| 16 | Cella `6d14f09e` (markdown, "4. Q1.3") | "We identify the crime categories with the largest year-over-year percentage change in the most recent years of the dataset." | "The crime categories with the largest year-over-year percentage change in the most recent years of the dataset are identified." |
| 17 | Cella `d509fe55` (markdown, "5. Q1.4") | "We analyze the monthly distribution of crimes to check for the existence of seasonal patterns." | "The monthly distribution of crimes is analyzed to check for the existence of seasonal patterns." |
| 18 | Cella `d509fe55` (markdown, "5. Q1.4") | "We compare crimes against the person and against property to understand whether the two categories follow different seasonal cycles..." | "Crimes against the person and against property are compared to understand whether the two categories follow different seasonal cycles..." |
| 19 | Cella `d509fe55` (markdown, "5. Q1.4") | "...and we dig deeper into specific types (e.g. residential burglary vs. vehicle theft)." | "...and specific types are examined in more depth (e.g. residential burglary vs. vehicle theft)." |
| 20 | Cella `c5de5616` (markdown, "Q1.4 — Part 2") | "We dig deeper by analyzing the top 5 Property crimes individually to identify which types drive those peaks." | "A deeper analysis of the top 5 Property crimes individually identifies which types drive those peaks." |
| 21 | Cella `bf146b2d` (markdown, "6. Q1.5") | "We analyze the distribution of crimes by time slot and day of the week, stratifying by macro-category (Person vs Property)." | "The distribution of crimes by time slot and day of the week is analyzed, stratified by macro-category (Person vs Property)." |

### 02_analyze/06_eda_block2.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 22 | Cella `7cb8f757` (markdown, intro) | "**Questions we answer**:" | "**Research questions addressed**:" |
| 23 | Cella `17f34f2b` (markdown, "2. Q2.1") | "We analyze the demographic distribution of victims across the entire dataset." | "The demographic distribution of victims is analyzed across the entire dataset." |
| 24 | Cella `cd28f626` (markdown, "Q2.1 — age group") | "We analyze the distribution of victims by age group using the `age_group` column created during feature engineering." | "The distribution of victims by age group is analyzed using the `age_group` column created during feature engineering." |
| 25 | Cella `55883adc` (markdown, "Q2.1 — descent") | "We analyze the distribution of victims by descent using the `Vict Descent` column." | "The distribution of victims by descent is analyzed using the `Vict Descent` column." |
| 26 | Cella `1fd0234b` (markdown, "3. Q2.2") | "We cross-reference the demographic profile of victims with crime types." | "The demographic profile of victims is cross-referenced with crime types." |
| 27 | Cella `1fd0234b` (markdown, "3. Q2.2") | "We analyze separately: Distribution by sex within the two macro-categories..." | "Analyzed separately: distribution by sex within the two macro-categories..." |
| 28 | Cella `72116042` (markdown, "4. Q2.3") | "We analyze Senior (65+) victims to understand whether they concentrate in specific crime types and specific LAPD areas." | "Senior (65+) victims are analyzed to understand whether they concentrate in specific crime types and specific LAPD areas." |
| 29 | Cella `72116042` (markdown, "4. Q2.3") | "We also calculate the trend over time to check whether their exposure has increased or decreased over the 2010–2024 period." | "The trend over time is also calculated to check whether their exposure has increased or decreased over the 2010–2024 period." |

### 02_analyze/07_eda_block3.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 30 | Cella `d8b913a3` (markdown, intro) | "**Questions we answer**:" | "**Research questions addressed**:" |
| 31 | Cella `cfa89995` (markdown, "2. Q3.1") | "We analyze domestic crimes (`is_domestic == True`) along three dimensions: overall volume..., yearly trend, and distribution by LAPD area." | "Domestic crimes (`is_domestic == True`) are analyzed along three dimensions: overall volume..., yearly trend, and distribution by LAPD area." |
| 32 | Cella `cb3d22ad` (markdown, "3. Q3.2") | "We compare the demographic profile of victims (sex, age, descent) between domestic and non-domestic crimes." | "The demographic profile of victims (sex, age, descent) is compared between domestic and non-domestic crimes." |
| 33 | Cella `7df2133f` (markdown, "4. Q3.3") | "We analyze the distribution of domestic crime victims by age group, with a specific focus on minors." | "The distribution of domestic crime victims by age group is analyzed, with a specific focus on minors." |
| 34 | Cella `c5c9e698` (markdown, "5. Q3.4") | "We analyze which Mocodes recur most frequently in domestic crimes to identify modus operandi patterns." | "The Mocodes that recur most frequently in domestic crimes are analyzed to identify modus operandi patterns." |

### 02_analyze/08_eda_block4.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 35 | Cella `ff954adf` (markdown, intro) | "**Questions we answer**:" | "**Research questions addressed**:" |
| 36 | Cella `34e789cc` (markdown, "2. Q4.1") | "We analyze crimes committed with a firearm using the `Weapon Desc` column." | "Crimes committed with a firearm are analyzed using the `Weapon Desc` column." |
| 37 | Cella `34e789cc` (markdown, "2. Q4.1") | "We identify the overall volume, the distribution by LAPD area, and the year-over-year trend." | "The overall volume, the distribution by LAPD area, and the year-over-year trend are identified." |
| 38 | Cella `34e789cc` (markdown, "2. Q4.1") | "We exclude them from the filter to focus on firearm crimes." | "They are excluded from the filter to focus on firearm crimes." |
| 39 | Cella `7c6028ca` (markdown, "3. Q4.2") | "We analyze the demographic profile (sex, age, descent) of victims in crimes committed with a firearm..." | "The demographic profile (sex, age, descent) of victims in crimes committed with a firearm is analyzed..." |

### 02_analyze/09_eda_block5.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 40 | Cella `c93d6c2b` (markdown, intro) | "**Questions we answer**:" | "**Research questions addressed**:" |

### 02_analyze/10_advanced_eda_block1.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 41 | Cella `cf7283ea` (markdown, intro) | "**Questions we answer**:" | "**Research questions addressed**:" |
| 42 | Cella `f7d779a4` (markdown, "2. Q6.1") | "We build a regular grid of 500m × 500m cells over the Los Angeles area and identify the cells that concentrate the largest share of violent crimes..." | "A regular grid of 500m × 500m cells is built over the Los Angeles area, identifying the cells that concentrate the largest share of violent crimes..." |
| 43 | Cella `f99bc80a` (markdown, "3. Q6.2") | "We measure hotspot stability by calculating the degree of overlap between hotspots identified in consecutive time periods." | "Hotspot stability is measured by calculating the degree of overlap between hotspots identified in consecutive time periods." |
| 44 | Cella `c1e4d841` (markdown, "4. Q6.3") | "We apply spatial autocorrelation indices to statistically validate the clusters identified in Q6.1 and identify significant hotspots and coldspots." | "Spatial autocorrelation indices are applied to statistically validate the clusters identified in Q6.1 and identify significant hotspots and coldspots." |

### 02_analyze/11_advanced_eda_block2.ipynb

| # | Cella (indice + tipo) | Testo originale | Riformulazione suggerita |
|---|---|---|---|
| 45 | Cella `1f1e69ad` (markdown, intro) | "**Questions we answer**:" | "**Research questions addressed**:" |
| 46 | Cella `6c510427` (markdown, "2. Q7.1") | "We build a time series model to forecast the expected crime volume for the next 7 days..." | "A time series model is built to forecast the expected crime volume for the next 7 days..." |
| 47 | Cella `fc634042` (markdown, "3. Q7.2") | "We build a classification model that predicts the probability of a case being cleared (cleared vs not cleared)..." | "A classification model is built to predict the probability of a case being cleared (cleared vs not cleared)..." |
| 48 | Cella `0541a3cb` (markdown, "4. Q7.3") | "We analyze whether there are significant associations between crime types that occur in the same geographic area within a defined time window (48 hours)." | "Whether there are significant associations between crime types occurring in the same geographic area within a defined time window (48 hours) is analyzed." |
| 49 | Cella `93bc35dc` (markdown, "5. Q7.4") | "We identify repeat victimization patterns by analyzing the concentration of crimes at the same address or block within 30-, 60-, and 90-day time windows." | "Repeat victimization patterns are identified by analyzing the concentration of crimes at the same address or block within 30-, 60-, and 90-day time windows." |

### 03_construct/12_construct_block1.ipynb e 00_additional_charts/additional_charts.ipynb

Nessuna occorrenza di "we/We + azione" in nessuna delle due (12_construct_block1 non usa mai la prima persona plurale nelle celle markdown; additional_charts.ipynb non contiene celle markdown).

---

## 5. Casi ambigui

1. **09_eda_block5.ipynb, cella `8a49f1de`** (Observations Q5.1 — Clearance Rate by LAPD area): "**Notable finding**: the areas with the highest crime volume in the previous blocks (77th Street, Southeast, Southwest) sit in the middle of the clearance rate ranking (21.8%, 21.1%, 22.4%). There's no direct correlation between high crime volume and a low resolution rate." L'affermazione di assenza di correlazione è leggibile direttamente dalla tabella stampata nella cella di codice precedente (`area_pivot`, cella `397a97bf`) e viene poi effettivamente motivata nella cella successiva (`791e81eb`, già segnalato come non-problema nel file sorgente stesso). Non è però calcolato un coefficiente di correlazione formale tra volume di crimine e clearance rate sulle 21 aree. **Per chiudere**: calcolare esplicitamente una correlazione (es. Pearson/Spearman tra volume totale per area e `clearance_rate`) se si vuole mantenere l'affermazione "no direct correlation" in modo rigoroso, altrimenti attenuare a "non risulta un pattern evidente dal ranking".

2. **11_advanced_eda_block2.ipynb, cella `91f3ac70`** (Observations Q7.3): il testo aggiunge "(50-100 meters)" come traduzione in metri di "nearest hundred block" (fonte: documentazione ufficiale LAPD, citata correttamente). La conversione da "hundred block" a "50-100 meters" è un'inferenza tecnica ragionevole (dimensione tipica di un isolato urbano a Los Angeles) ma non è essa stessa presente nella fonte citata. Non è la stessa problematica flaggata nel file sorgente (che riguardava l'assenza totale di fonte), ma resta un dettaglio numerico non attribuito. **Per chiudere**: citare esplicitamente la base di questa conversione (es. dimensione media di un isolato nel grid system di Los Angeles) o rimuovere la cifra specifica lasciando solo "nearest hundred block".

3. **07_eda_block3.ipynb, cella `21a6b9a7`** (Q3.2 — Distribution by sex): l'ultima frase, "...confirms the expected pattern regarding gender violence in a domestic context", usa "expected" implicando una letteratura/aspettativa esterna non citata — nello stesso stile delle affermazioni già rimosse altrove nel progetto (es. voci #28, #31). Tuttavia la frase si appoggia direttamente al dato appena calcolato nella stessa cella (75.8% vittime donne nei crimini domestici) e non introduce un meccanismo causale esterno non testato, quindi è più lieve dei casi già segnalati nel file sorgente. **Per chiudere**: se si vuole eliminare ogni riferimento a un'aspettativa esterna, sostituire con una formulazione puramente descrittiva (es. "This is the sharpest difference observed so far in the entire project.", eliminando "confirms the expected pattern... gender violence").
