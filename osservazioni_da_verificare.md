# Osservazioni da verificare — Report di fact-checking interno

Questo file raccoglie le affermazioni nelle celle markdown dei notebook che
introducono spiegazioni, cause o interpretazioni non direttamente
verificabili dai dati/output presenti nel notebook stesso (es. riferimenti a
eventi esterni, normative, cambi metodologici, fattori socioeconomici non
misurati nel dataset). Nessuna cella è stata modificata: questo è un report
di sola lettura/segnalazione.

---

## 01_plan/01_data_loading.ipynb

### 1. Cella `72111801-e4bf-46bd-a51c-6bc8cf4114b7` (## Conclusions — Plan Phase) - RISOLTO: cambiato il testo per togliere supposizioni non basate                                                                                              sui dati

> "Years 2015–2016 show anomalous values (a drop in 2015, a spike in 2016), likely related to the transition of the LAPD classification system (from UCR to NIBRS). To be verified and documented in the final report."

**Motivo della segnalazione**: introduce una causa esterna al dataset (un cambio nel sistema di classificazione dei reati da parte dell'LAPD, UCR→NIBRS) senza che nel notebook sia presente alcuna colonna, test statistico o fonte a supporto. Il testo stesso usa un linguaggio dubitativo ("likely related to") e dichiara esplicitamente "to be verified" — quindi è un'ipotesi non ancora testata.

### 2. Cella `72111801-e4bf-46bd-a51c-6bc8cf4114b7` (## Conclusions — Plan Phase) - DA NON RISOLVERE, VERIFICARE BLOCCO EDA
> "Year 2024 incomplete (127,567 records): the truncation month needs to be verified in EDA and handled accordingly in the temporal analyses."

**Motivo della segnalazione**: il conteggio inferiore di record nel 2024 è visibile nell'output della cella precedente (tabella "Records per year"), quindi il *pattern* è supportato dai dati. Tuttavia l'interpretazione della causa — "incomplete" per troncamento del dataset — non è dimostrata in questo notebook (nessun controllo sulla data massima disponibile o su una data di congelamento del dataset); il testo stesso rimanda a una verifica futura ("needs to be verified in EDA").

**Notebook 01_data_loading: 2 osservazioni segnalate**

---

## 02_analyze/02_cleaning.ipynb

### 3. Cella `acc496cb-c4ad-40f0-a8c1-04f82410b300` (### 3.3 Handling sentinel coordinates) - RISOLTO: documentato con: 
"As documented in the LAPD dataset description (data.lacity.org): 'Some location fields with missing data are noted as (0°, 0°). Address fields are only provided to the nearest hundred block in order to maintain privacy"

> "In notebook 01 we identified 3,148 records with `(0, 0)` coordinates, which the LAPD uses as a sentinel value to indicate 'unknown location'."

**Motivo della segnalazione**: presenta come fatto stabilito una convenzione di codifica dei dati da parte dell'LAPD, senza citare una fonte né un test nel notebook che lo dimostri (a differenza, ad esempio, dei codici "Vict Descent" più avanti nel progetto, per i quali viene citata esplicitamente la fonte "LAPD Crime Data Dictionary"). È un'ipotesi di dominio non verificata internamente.

### 4. Cella `d24693c1-b66d-470e-b344-10287ca9e8d6` (### Removing duplicates) - RISOLTO: rimossa la riga di testo indicata
> "This is therefore a generalized loading error in the original dataset, not a category-specific issue."

**Motivo della segnalazione**: i dati mostrati (duplicati esatti su tutte le colonne, distribuiti su 122 tipi di reato) supportano che i duplicati non siano concentrati in una categoria specifica, ma non dimostrano la causa specifica ("errore di caricamento"). È un'attribuzione causale sul processo di raccolta/caricamento dati non testata (es. nessun controllo su timestamp di inserimento o metadati di importazione).

### 5. Cella `1c85663b-3f26-4d37-a228-ac1f88bd2623` (## 6. Handling sentinel values in Vict Age) - RISOLTO: eseguito refactoring del testo
> "the LAPD uses the value `0` to indicate \"unknown age\" (same logic as the sentinel coordinates)"

**Motivo della segnalazione**: estende alla colonna `Vict Age` la stessa assunzione non verificata già segnalata per LAT/LON (voce 3), sempre senza citazione o test nel notebook.

**Notebook 02_cleaning: 3 osservazioni segnalate**

---

## 02_analyze/04_feature_engineering.ipynb

### 6. Cella `07bb62db-056d-4230-9b23-037edd2316e0` (## 6. Report delay) - RISOLTO: rimossa la riga di testo indicata
> "Different crimes have very different delays: a homicide is reported immediately, a sexual assault often years later."

**Motivo della segnalazione**: generalizzazione di dominio sui tempi di segnalazione per tipo di reato specifico (omicidio vs. violenza sessuale), presentata come fatto assodato. In questo notebook la colonna `report_delay` viene solo calcolata in aggregato (nessuna scomposizione per tipo di reato); non c'è alcun output che confronti il delay di omicidi rispetto a quello di violenze sessuali per verificare l'affermazione.

**Notebook 04_feature_engineering: 1 osservazione segnalata**

---

## 02_analyze/05_eda_block1.ipynb

### Cella `6486503f-c56d-4a7f-abf9-2cf3b5c6e08f` (### Observations Q1.1)

- > "consistent with the anomaly already identified in the Plan phase (168k records vs ~200k in the surrounding years). Likely related to the transition of the LAPD classification system."

**Motivo**: ripete l'ipotesi non verificata sul cambio di sistema di classificazione LAPD (UCR→NIBRS), già segnalata nel notebook 01.

- > "coincides with COVID and the lockdowns. Crimes against property decline more than crimes against the person — Simple Assault declines less than the other crimes during 2020 because it includes a component of interpersonal crimes (occurring between people who know each other, in enclosed settings) that doesn't depend on the presence of people on the street. Lockdowns reduce \"opportunity\" crimes (theft, pickpocketing) but not those that occur within personal relationships."

**Motivo**: attribuisce il calo del 2020 al COVID e ai lockdown, e introduce un meccanismo esplicativo ("opportunity crimes" vs "crimini interpersonali") mai testato nel dataset (nessuna variabile su lockdown, mobilità o relazione vittima-sospetto usata a supporto).

- > "A phenomenon documented at the national level, linked to less surveillance during COVID, the economic crisis, and viral social media challenges about how to steal certain car models."

**Motivo**: attribuisce il picco dei furti d'auto 2020-2022 a tre cause esterne (minore sorveglianza da COVID, crisi economica, challenge social virali) senza fonte citata né dato nel notebook a supporto; dichiara "documented at the national level" senza riferimento verificabile.

### Cella `d8c84328-02c8-472b-a818-aca369f0ee7e` (### Observations Q1.2)

- > "The decline is more pronounced for crimes against the person, likely amplified by that year's classification anomaly."

**Motivo**: ripete l'ipotesi non verificata sull'anomalia di classificazione 2015.

- > "**2020 decline (COVID)**: Property drops by -8.3% (from 133,268 to 122,225), Person by -9.8% (from 82,254 to 74,156)."

**Motivo**: etichetta il calo 2020 come "COVID" nel titolo stesso del paragrafo, senza alcuna verifica interna al notebook (nessuna variabile COVID nel dataset).

- > "crime against property is more volatile and reactive to external events, while crime against the person is more stable and structural."

**Motivo**: generalizzazione interpretativa su "eventi esterni" come causa della volatilità, non testata nei dati.

### Cella `9ddebde8-644a-4947-941a-a31dc19dae0b` (### Observations Q1.3)

**13.** > "The speed and uniformity of this rebound suggests a possible classification- or policy-related effect rather than an actual worsening of crime — a hypothesis that would deserve further investigation with external sources in the final report."

Motivo: linguaggio dubitativo esplicito ("suggests a possible... hypothesis") su un cambio normativo/di classificazione, senza fonte né verifica nel notebook.

**14.** > "2020 — COVID effect: a widespread downward trend, consistent with COVID-19 restrictions and the resulting reduction in urban mobility. The exception is vehicle theft, which increases by +35%. This seemingly counterintuitive figure is likely related to the restrictions themselves: emptier streets and vehicles parked for extended periods may have made this type of crime easier. The phenomenon has been documented at the national level (FBI UCR report, NICB data) and is not specific to Los Angeles."

Motivo: attribuzione causale a COVID-19/restrizioni, senza dato di mobilità o restrizioni nel dataset; cita fonti esterne (FBI UCR, NICB) non incluse/verificabili nel notebook.

**15.** > "Identity Theft normalizes with a -39% decline, suggesting a temporary phenomenon likely linked to accelerated post-pandemic digitalization"

Motivo: causa socioeconomica/tecnologica esterna ("digitalizzazione post-pandemica") non testata nel dataset.

### Cella `6b111396-ba9c-45ca-857e-870f9f822e3b` (### Observations Q1.4 — Part 1)

**16.** > "Shared minimum in February: both categories reach their lowest point in February, the shortest month, with reduced urban mobility for climate-related reasons."

Motivo: causa climatica/di mobilità urbana non misurata nel dataset (nessuna variabile meteo).

**17.** > "This trend is likely attributable to holiday-season theft — unattended homes, more goods in cars and crowded stores — phenomena that affect property without necessarily involving the person."

Motivo: spiegazione stagionale/sociale (periodo festivo) non testata (nessuna variabile calendario festività nel dataset).

### Cella `ad510cad-1c12-44d9-9aa2-56428b2cc102` (### Observations Q1.4 — Part 2)

**18.** > "Hypothesis: it could be linked to periods of higher online activity (holiday shopping, early-year tax filings). To be verified with external sources in the final report."

Motivo: linguaggio dubitativo esplicito ("Hypothesis... could be linked"), causa non verificata nel notebook.

**19.** > "Residential burglary, theft from vehicles, and petty theft rise for reasons related to the holiday season (unattended goods, crowded stores, empty homes). Vehicle theft follows the opposite direction: in December people use their cars less, park them in more supervised locations, and pay closer attention."

Motivo: spiegazione comportamentale/stagionale (abitudini natalizie) non testata nel dataset.

### Cella `bf146b2d-d5ac-43f5-923a-8958b48f060a` (## 6. Q1.5 intro)

**20.** > "records with `hour_occ = 0` (midnight) include a recording artifact — crimes with unknown time coded as 0000."

Motivo: assunzione sulla convenzione di codifica dei dati LAPD (0000 = ora sconosciuta) presentata come fatto, senza fonte né test nel notebook che distingua i crimini avvenuti realmente a mezzanotte da quelli con ora sconosciuta.

### Cella `c525a1a5-f9da-4c7b-a345-6a63b1a41580` (### Observations Q1.5)

**21.** > "the Night slot includes ~130k records with unknown time coded as 0000. Values for this slot should be read with caution."

Motivo: ripete l'assunzione non verificata della voce 20.

**22.** > "For Property, the weekend shows slightly lower values than weekdays (Saturday 19,681, Sunday 18,268 vs ~22,000 on weekdays), likely because stores open later and urban mobility is reduced."

Motivo: causa esplicativa (orari negozi, mobilità urbana) non testata nel dataset.

**23.** > "Fewer people at work, less urban traffic, fewer opportunities for property crimes during daytime hours."

Motivo: spiegazione socioeconomica (occupazione, traffico) non misurata nel dataset.

**24.** > "Friday records the absolute peaks for Property: Afternoon 70,800, Evening 73,995. End of the work week, crowded stores, packed public transit — maximum exposure to crime opportunities."

Motivo: spiegazione comportamentale (fine settimana lavorativo, affollamento) non testata.

**25.** > "The start and peak of the weekend, with more nighttime activity, alcohol consumption, and high-risk interactions."

Motivo: causa socio-comportamentale (consumo di alcol, vita notturna) non misurata nel dataset.

**26.** > "A pattern consistent with nightlife: more alcohol, less oversight, more high-risk interactions."

Motivo: ripete la stessa tipologia di spiegazione non verificata della voce 25.

**Notebook 05_eda_block1: 20 osservazioni segnalate**

---

## 02_analyze/06_eda_block2.ipynb

### Cella `dde6cfb1-f9b5-437b-af60-345d8829192a` (### Observations Q2.1 — Distribution by sex)

**27.** > "A more balanced distribution than one might expect, likely because the dataset includes all crime types, including domestic ones where women are overrepresented as victims."

Motivo: linguaggio dubitativo ("likely because") su una causa che viene effettivamente testata solo più avanti, nel Blocco 3 (Q3.2); a questo punto del progetto non è ancora verificata.

**28.** > "the male overrepresentation typical of criminological literature (public violence) appears attenuated in this aggregate dataset."

Motivo: riferimento a "criminological literature" generico, senza fonte citata né test nel notebook.

### Cella `d9976b1d-6c3a-44e5-9f5e-c43a44bad424` (### Observations Q2.1 — Distribution by age group)

**29.** > "abuse of minors is historically among the most underreported crimes — often discovered years later or never reported — which introduces significant selection bias."

Motivo: conoscenza di dominio criminologico (sottostima strutturale dell'abuso minorile) citata senza fonte né verifica nel notebook.

### Cella `5b9d21e6-9b6f-4d7f-9d56-ad9be1c05c6f` (### Observations Q2.2 — Victim profile by crime macro-category)

**30.** > "in the 35-64 group a significant difference emerges, likely related to greater male exposure in work and mobility contexts."

Motivo: causa socioeconomica (esposizione lavorativa/mobilità per genere) non misurata nel dataset.

**31.** > "A pattern consistent with the literature on sexual abuse and domestic violence, which disproportionately affects females already at the youngest age groups."

Motivo: riferimento a letteratura esterna generica, senza fonte citata.

**32.** > "This could reflect greater exposure of elderly men to street crime (robbery, assault)."

Motivo: linguaggio dubitativo ("could reflect"), ipotesi non testata nel dataset.

### Cella `9fec3bcd-8dd4-4c58-82c8-15120a272ea4` (### Observations Q2.3 — Top 10 crimes against Senior victims)

**33.** > "Physical vulnerability when defending the home and the higher likelihood of being home during the day make the elderly a preferred target for this type of offense."

Motivo: ipotesi esplicativa (vulnerabilità fisica, presenza in casa di giorno) non testata nel dataset (nessun dato su presenza in casa o vulnerabilità).

**34.** > "consistent with the well-known vulnerability of the elderly to digital scams and credential theft."

Motivo: riferimento a conoscenza di dominio ("well-known vulnerability") non verificata nel notebook.

**35.** > "The elderly's reduced physical reactivity makes them easy targets for this type of opportunistic offense."

Motivo: assunzione fisiologica/comportamentale non misurata nel dataset.

### Cella `886b849d-fe5a-4703-938b-b448ac5652e2` (### Observations Q2.3 — Time trend of top 5 crimes against Senior)

**36.** > "consistent with the LAPD classification anomaly already documented in Q1.3, not an actual improvement in safety for the elderly."

Motivo: ripete l'ipotesi non verificata sul cambio di classificazione LAPD.

**37.** > "Shows a significant decline from 2018 to 2020, likely related to the wider adoption of home security systems (cameras, alarms), which became more affordable in those years."

Motivo: causa tecnologica/socioeconomica (adozione di sistemi di sicurezza domestica) non misurata nel dataset.

**38.** > "Explosive rebound from 2020 to 2023: COVID lockdowns forced the elderly to increase their use of digital services without adequate preparation, creating ideal conditions for scammers."

Motivo: attribuzione causale al COVID e ai lockdown, non testata nel dataset (nessuna variabile COVID).

**39.** > "Petty Theft shows a peak in 2018 followed by a decline during COVID (less mobility, fewer opportunities for pickpocketing), then a partial rebound."

Motivo: attribuzione causale al COVID/mobilità ridotta, non testata nel dataset.

**Notebook 06_eda_block2: 13 osservazioni segnalate**

---

## 02_analyze/07_eda_block3.ipynb

### Cella `90aec861-5e44-4c87-9d69-95fe58879262` (### Observations Q3.1 — Volume of domestic crimes)

**40.** > "**Underreporting**: domestic crimes are historically among the most underreported. Victims often don't report out of fear of retaliation, economic dependence on the perpetrator, shame, or a desire to protect the children involved."

Motivo: conoscenza di dominio criminologico/sociologico su cause di mancata denuncia, senza fonte citata né test nel notebook.

### Cella `a6fa86d9-af23-4799-a254-db841d09b155` (### Observations Q3.1 — Yearly trend of domestic crimes)

**41.** > "The 2015 minimum is consistent with the LAPD classification anomaly already documented in previous blocks — it doesn't represent a real improvement."

Motivo: ripete l'ipotesi non verificata sul cambio di classificazione LAPD.

**42.** > "The speed and magnitude of the increase suggests a possible change in LAPD recording policies, or the effect of awareness campaigns that increased reporting during that period. To be verified with external sources in the final report."

Motivo: linguaggio dubitativo esplicito ("suggests a possible... to be verified"), due cause alternative non testate nel notebook.

**43.** > "International criminological literature has documented an increase in domestic crimes during COVID lockdowns in most cities. Los Angeles instead shows a declining trend."

Motivo: riferimento a letteratura internazionale generica, senza fonte citata né dato nel dataset che la testi.

**44.** > "Three possible explanations, not mutually exclusive: Changes in LAPD classification policies / Specific domestic violence intervention programs activated during COVID / Increased underreporting: with victims confined at home with the perpetrator, reporting was even harder than usual."

Motivo: tre ipotesi esplicitamente presentate come "possibili" e non verificate, nessuna delle quali testata con dati o metriche nel notebook.

### Cella `a4617670-1ee3-4a74-8565-6e2ab6c0f8af` (### Observations Q3.3 — minors)

**45.** > "in this context, minors recorded as victims were probably present during domestic violence and recorded as secondary victims, or are adolescents in romantic relationships"

Motivo: linguaggio dubitativo ("probably"), ipotesi non testata nel notebook (nessun dato su ruolo "vittima secondaria" o stato di relazione).

### Cella `724ef7d6-beac-4da4-b34e-5d0896393ddf` (### Observations Q3.4 — Mocodes)

**46.** > "strangulation is a criminological indicator of high risk of escalation and recidivism."

Motivo: riferimento a conoscenza criminologica specialistica, senza fonte citata nel notebook.

**47.** > "The suspect's intoxication is recorded in about 5% of cases. It's a factor correlated with domestic violence but not its cause."

Motivo: afferma una relazione di correlazione/causa con la violenza domestica senza alcun test statistico calcolato nel notebook (nessuna correlazione, nessun confronto tra gruppi).

**Notebook 07_eda_block3: 8 osservazioni segnalate**

---

## 02_analyze/08_eda_block4.ipynb

### Cella `57ce322f-4391-4d10-99b7-e8d570e66f58` (### Observations Q4.1 — Volume by firearm type)

**48.** > "**Hand Gun**: 53,523 (~46%) — the dominant weapon, easy to obtain and to conceal"

Motivo: assunzione su disponibilità/occultabilità dell'arma non misurata nel dataset (nessun dato su mercato armi o dimensioni).

**49.** > "the dataset confirms that firearm crimes predominantly involve common, compact weapons that are easy to obtain. More sophisticated weapons that are harder to conceal (assault rifles, automatic weapons) are almost absent."

Motivo: ripete la stessa assunzione non verificata della voce 48 ("easy to obtain"/"harder to conceal").

### Cella `9ac118ed-6f25-47eb-9c2f-e57bf395d327` (### Observations Q4.1 — Yearly trend of firearm crimes)

**50.** > "The 2015 decline is consistent with the LAPD classification anomaly already documented in previous blocks."

Motivo: ripete l'ipotesi non verificata sul cambio di classificazione LAPD.

**Notebook 08_eda_block4: 3 osservazioni segnalate**

Nota: in questo notebook la maggior parte dei confronti (es. quote di genere/età/descent rispetto ai blocchi precedenti, sovrapposizione geografica con i crimini domestici) sono confronti diretti tra percentuali già calcolate in altri notebook — quindi supportati da dati/output effettivi, non segnalati.

---

## 02_analyze/09_eda_block5.ipynb

### Cella `791e81eb-f40a-4b32-8864-310138ca34e7` (### Observations Q5.1 — Clearance Rate by crime category)

**51.** > "**Other**: 37.0% — a residual category with an intermediate rate, consistent with the fact that it includes crimes where the suspect is often already known to authorities (disturbing the peace, false reports, contempt of court)."

Motivo: assunzione sul fatto che il sospetto sia "già noto alle autorità" per la categoria "Other", non testata nel notebook (nessuna scomposizione del clearance rate per singolo tipo di reato entro "Other").

**52.** > "Theft, burglary, and vandalism are structurally harder to resolve due to the frequent absence of witnesses or direct evidence."

Motivo: causa esplicativa (assenza di testimoni/prove) non misurata nel dataset (nessuna variabile su testimoni o prove disponibili).

### Cella `965c19b5-a37f-4ed7-af1c-6bb01619e9b5` (### Observations Q5.2 — Report Delay by crime category)

**53.** > "This is consistent with the nature of the two types of offense: assaults and violent crimes are traumatic events that get reported immediately, while theft and burglary are often discovered with a delay."

Motivo: spiegazione psicologica/comportamentale ("eventi traumatici segnalati immediatamente") non testata nel dataset.

**Notebook 09_eda_block5: 3 osservazioni segnalate**

Nota: non ho segnalato l'ipotesi nella cella `8a49f1de` sulla composizione per tipo di reato come causa del range ristretto di clearance rate per area, perché viene di fatto verificata/supportata dal confronto numerico nella cella immediatamente successiva (`791e81eb`).

---

## 02_analyze/10_advanced_eda_block1.ipynb

### Cella `cf7283ea-721e-4bc8-9fd9-d87dfc24e846` (titolo / intro)

**54.** > "the LAT/LON coordinates in the dataset are often obfuscated to the block centroid (~100m precision), as documented in the project limitations."

Motivo: afferma una caratteristica tecnica della fonte dati (offuscamento a livello di centroide dell'isolato) come fatto documentato, ma nel notebook non compare alcun test o verifica che lo dimostri (es. nessun controllo di clustering delle coordinate su punti fissi).

### Cella `21b36b74-d603-4b66-807b-a15471615ee4` (### Observations Q6.2 — Hotspot persistence over time)

**55.** > "Exception: the 2015 period shows a peak above average, consistent with the LAPD classification anomaly already documented in previous blocks."

Motivo: ripete l'ipotesi non verificata sul cambio di classificazione LAPD.

**Notebook 10_advanced_eda_block1: 2 osservazioni segnalate**

Nota: la maggior parte delle affermazioni in questo notebook (Moran's I, Getis-Ord Gi*, confronto con le aree dominanti nei blocchi precedenti) sono supportate da test statistici calcolati nel notebook stesso o da confronti diretti tra dati già verificati altrove nel progetto — quindi non segnalate.

---

## 02_analyze/11_advanced_eda_block2.ipynb

### Cella `16454d22-785e-4ae4-ae6b-05b86ae88091` (### Observations Q7.1 — 7-day crime volume forecast)

**56.** > "**Anomalous peaks**: the peaks in the actual line (early January, February) likely correspond to particular days (New Year's, special events) that the model can't capture without holiday information."

Motivo: linguaggio dubitativo ("likely correspond to"), ipotesi su date/eventi specifici non verificata nel notebook (nessuna variabile calendario/festività usata).

### Cella `91f3ac70-e418-42dc-a027-cd06326b740f` (### Observations Q7.3 — Spatio-temporal co-occurrence)

**57.** > "The coordinates in the dataset are obfuscated to the block centroid (~100m precision), as documented in the project limitations, which in any case makes it impossible to distinguish events that occurred in adjacent residences."

Motivo: ripete (terza occorrenza nel progetto, dopo il notebook 10) l'affermazione non verificata sull'offuscamento delle coordinate al centroide dell'isolato, presentata come "documented" senza che nel notebook compaia il test/la verifica corrispondente.

### Cella `93bc35dc-410c-40a2-b1d3-c5496d7c8ab2` (## 5. Q7.4 — Re-victimization patterns, nota "not implemented")

**58.** > "the coordinates in the dataset are obfuscated to the block centroid (~100m precision), making the distinction between \"same address\" and \"same 500m×500m cell\" less meaningful than it seems."

Motivo: ulteriore ripetizione della stessa affermazione non verificata (voce 54, 57).

**Notebook 11_advanced_eda_block2: 3 osservazioni segnalate**

Nota: il resto delle "Observations" in questo notebook (MAPE di Prophet, classification report del Random Forest, lift delle association rules, la stessa auto-critica metodologica su Q7.3) è supportato da metriche/test effettivamente calcolati nelle celle di codice, quindi non segnalato.

---

## 03_construct/12_construct_block1.ipynb

Nessuna osservazione da segnalare. Le uniche celle markdown interpretative
riguardano l'ottimizzazione degli iperparametri (confronto Baseline vs
Optimized, tabella metriche) e sono supportate dai risultati calcolati nel
notebook. L'unico riferimento a conoscenza esterna — Bergstra & Bengio
(2012), "Random Search for Hyper-Parameter Optimization", JMLR — è citato
con fonte verificabile, quindi non rientra nei criteri di segnalazione.

**Notebook 12_construct_block1: 0 osservazioni segnalate**

---

## 00_additional_charts/additional_charts.ipynb

Notebook privo di celle markdown (solo 2 celle di codice: setup e grafico a
ciambella person/property). Nessuna osservazione interpretativa da
verificare.

**Notebook additional_charts: 0 osservazioni segnalate**

---

## Riepilogo finale

| Notebook | Osservazioni segnalate |
|---|---|
| 01_data_loading | 2 |
| 02_cleaning | 3 |
| 04_feature_engineering | 1 |
| 05_eda_block1 | 20 |
| 06_eda_block2 | 13 |
| 07_eda_block3 | 8 |
| 08_eda_block4 | 3 |
| 09_eda_block5 | 3 |
| 10_advanced_eda_block1 | 2 |
| 11_advanced_eda_block2 | 3 |
| 12_construct_block1 | 0 |
| additional_charts | 0 |
| **Totale** | **58** |

### Temi ricorrenti individuati
- **Anomalia di classificazione LAPD 2015-2016 (UCR→NIBRS)**: citata come causa non verificata in almeno 6 punti diversi del progetto (notebook 01, 05×2, 06, 07, 10).
- **Attribuzioni a COVID-19/lockdown**: presenti in almeno 7 punti (notebook 05×3, 06×2, 07×2), quasi sempre senza una variabile COVID nel dataset a supporto diretto.
- **Offuscamento delle coordinate al centroide dell'isolato (~100m)**: affermato come "documentato" in 3 punti (notebook 10, 11×2) senza che in nessun notebook compaia il test che lo dimostri.
- **Spiegazioni socio-comportamentali generiche** (vita notturna/alcol, periodo festivo, orari di lavoro/negozi, vulnerabilità fisica degli anziani): concentrate soprattutto nei notebook 05 e 06, tipicamente introdotte con linguaggio dubitativo ("likely", "probably", "could reflect").

Fine del report.
