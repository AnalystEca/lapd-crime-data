# 03 — Domande di Ricerca

**Fase PACE**: Plan  
**Obiettivo**: definire in modo strutturato le domande analitiche a cui il progetto risponderà, organizzate per tema e divise in fase core (analisi descrittive e diagnostiche) e fase avanzata (modelli predittivi e analisi spaziale avanzata).

---

## Premessa metodologica

Le domande sono organizzate in **blocchi tematici**. Ogni blocco segue una logica progressiva dal generale al particolare: prima si identifica il fenomeno, poi si approfondisce chi coinvolge, dove si concentra e come evolve.

Il progetto lavora esclusivamente sul dataset LAPD Crime Data (2010–2024). Ogni conclusione è limitata ai **reati denunciati**, non ai reati commessi: aree con basso livello di fiducia verso la polizia risultano sistematicamente sotto-rappresentate. Questa limitazione va richiamata esplicitamente nella relazione finale.

**Dataset**: `data/processed/crimes_clean.parquet` (3.079.424 righe × 24 colonne)  
**Periodo**: 2010–2024 (15 anni, con nota sul 2024 che mostra un calo significativo di record rispetto agli anni precedenti)

---

## FASE CORE — Analisi descrittive e diagnostiche

### Blocco 1 — Panoramica criminalità e trend temporali

**1.1 — Top crimini e trend generali**  
Identificare i 5 reati più frequenti nell'intero dataset (2010–2024) e calcolarne il trend anno per anno. Obiettivo: costruire la "storia" generale della criminalità a Los Angeles nell'ultimo quindicennio.

**1.2 — Crimini contro la persona vs contro la proprietà**  
Classificare i reati in due macro-categorie (contro la persona: assault, robbery, homicide, ecc. / contro la proprietà: furto, vandalismo, ecc.), identificare i top 5 di ciascuna, e calcolarne i trend separati. Obiettivo: capire se le due categorie si muovono insieme o in direzioni divergenti.

**1.3 — Categorie con maggior crescita e decrescita**  
Identificare le categorie di reato con il maggior tasso di variazione anno su anno (YoY) negli ultimi anni del dataset. Obiettivo: individuare reati emergenti e reati in calo, segnalando potenziali shift nelle priorità operative.

**1.4 — Pattern stagionali**  
Analizzare la distribuzione mensile dei crimini per verificare l'esistenza di pattern stagionali, distinguendo tra crimini contro la persona e contro la proprietà. Approfondire per tipologie specifiche (es. furti residenziali vs furti di veicoli). Obiettivo: capire se e come la criminalità si muove con le stagioni.

**1.5 — Fasce orarie e giorni della settimana**  
Analizzare la distribuzione dei crimini per ora del giorno (con nota sull'artefatto di registrazione a mezzanotte) e giorno della settimana, stratificando per area LAPD e per macro-categoria di reato. Obiettivo: identificare le finestre temporali più critiche.

---

### Blocco 2 — Profilo demografico delle vittime

**2.1 — Profilo generale delle vittime**  
Analizzare la distribuzione delle vittime per sesso, fasce di età e etnia sull'intero dataset. Integrare i risultati per area LAPD per evidenziare differenze territoriali.

**2.2 — Profilo vittime per tipo di reato**  
Incrociare il profilo demografico con le tipologie di crimine dei blocchi 1.1 e 1.2. Obiettivo: capire non solo *cosa* succede ma *a chi* succede.

**2.3 — Focus vittime anziane (65+)**  
Analizzare se le vittime anziane si concentrano in specifiche tipologie di reato (truffe, scippi, furti) e in specifiche aree LAPD. Calcolare il trend post-2020. Obiettivo: fornire dati utili per campagne di sensibilizzazione mirate.

---

### Blocco 3 — Abusi domestici e sicurezza minori

Questo blocco merita un'analisi dedicata e approfondita data la gravità del tema e la ricchezza di informazioni disponibili nel dataset (codici di crimine specifici, dati demografici delle vittime, Mocodes).

**3.1 — Panoramica reati domestici**  
Identificare tutti i reati classificabili come domestici (tramite `Crm Cd Desc`: "INTIMATE PARTNER", "CHILD ABUSE", "DOMESTIC VIOLENCE", ecc.). Calcolarne il volume complessivo, il trend anno per anno, e la distribuzione per area.

**3.2 — Confronto profilo vittime: reati domestici vs non-domestici**  
Confrontare il profilo demografico delle vittime (sesso, età, etnia) tra reati domestici e non-domestici per area LAPD. Obiettivo: dimensionare la domanda di servizi sociali e shelter.

**3.3 — Distribuzione per fasce di età: bambini, adolescenti, adulti**  
Suddividere le vittime di reati domestici in tre fasce:
- **Bambini (0-12 anni)**
- **Adolescenti (13-17 anni)**
- **Adulti (18+ anni)**

Analizzare la distribuzione, il trend nel tempo e le differenze per area LAPD. Obiettivo: porre attenzione specifica sulla sicurezza dei minori e identificare aree dove il fenomeno è più concentrato.

**3.4 — Pattern nei Mocodes per reati domestici**  
Decodificare i Mocodes (utilizzando la lookup table ufficiale LAPD `MO_CODES.txt`) e analizzare i pattern di modus operandi nei reati domestici. Identificare combinazioni ricorrenti e verificare se differiscono tra le tre fasce di età delle vittime. Obiettivo: comprendere le dinamiche degli abusi per informare strategie di prevenzione.

**Requisito**: scaricare la lookup table `MO_CODES.txt` dal portale LAPD prima di iniziare questo blocco.

---

### Blocco 4 — Reati con arma da fuoco

**4.1 — Distribuzione e trend armi da fuoco**  
Calcolare la quota di reati commessi con arma da fuoco (tramite `Weapon Desc`) per area LAPD e il trend anno per anno. Obiettivo: mappare la distribuzione geografica e temporale della violenza armata.

**4.2 — Profilo vittima nei reati con arma da fuoco**  
Analizzare il profilo demografico delle vittime nei reati che coinvolgono armi da fuoco, confrontandolo con il profilo generale. Obiettivo: identificare chi è più esposto alla violenza armata.

---

### Blocco 5 — Efficacia della risposta

**5.1 — Tasso di clearance per categoria e area**  
Calcolare il tasso di chiusura dei casi (tramite colonna `Status`: AA = Adult Arrest, JA = Juvenile Arrest, IC = Invest Cont, ecc.) per categoria di reato e per area LAPD. Obiettivo: identificare dove e per quali crimini il sistema è più o meno efficace.

**5.2 — Report delay**  
Calcolare la differenza `Date Rptd - DATE OCC` per tipo di reato e area. Analizzare la distribuzione del ritardo di denuncia e identificare i reati con il maggior delay sistematico. Obiettivo: capire dove il tempo tra crimine e denuncia è più lungo e perché.

---

## FASE AVANZATA — Modelli predittivi e analisi spaziale

Queste domande richiedono competenze e strumenti da acquisire durante il progetto. Verranno affrontate dopo il completamento della fase core.

### Blocco 6 — Analisi geospaziale avanzata

**6.1 — Mappa hotspot su griglia**  
Costruire una griglia 500m × 500m sull'area di Los Angeles e identificare le celle che concentrano oltre il 20% dei reati violenti pur rappresentando meno del 5% della superficie. Strumenti: geopandas, folium.

**6.2 — Persistenza hotspot nel tempo**  
Misurare la stabilità degli hotspot calcolando la percentuale di overlap mese su mese. Distinguere hotspot cronici (persistenti) da hotspot emergenti (nuovi). Obiettivo: informare la scelta tra interventi strutturali e tattici.

**6.3 — Clustering spaziale statistico**  
Applicare indici di autocorrelazione spaziale (Moran's I, Getis-Ord Gi*) per identificare cluster significativi di reati legati ad armi da fuoco. Identificare anche le aree cold spot come benchmark positivi. Strumenti: PySAL.

### Blocco 7 — Modelli predittivi

**7.1 — Previsione volume reati a 7 giorni**  
Costruire un modello di serie temporali per prevedere il volume atteso di reati per area e categoria con 7 giorni di anticipo. Target: MAPE < 15%. Strumenti: ARIMA, Prophet, o simile.

**7.2 — Modello predittivo di clearance**  
Costruire un modello di classificazione che predica la probabilità di chiusura di un caso in base alle feature disponibili (area, tipo di reato, tipo di arma, profilo vittima, report delay). Strumenti: sklearn (Random Forest, Logistic Regression).

**7.3 — Co-occorrenza spazio-temporale**  
Analizzare se esistono associazioni significative tra tipologie di reato nella stessa cella spazio-temporale (es. furto di veicolo seguito da assault nello stesso blocco entro 48h). Strumenti: association rules, definizione di celle spazio-temporali.

**7.4 — Pattern di ri-vittimizzazione**  
Identificare pattern di ripetuta vittimizzazione analizzando la concentrazione di reati allo stesso indirizzo o blocco entro finestre di 30, 60 e 90 giorni. Obiettivo: individuare vittime a rischio di re-victimization.

---

## Riepilogo

| Fase | Blocchi | Domande | Focus |
|------|---------|---------|-------|
| Core | 1-5 | 15 domande | Descrittiva e diagnostica |
| Avanzata | 6-7 | 7 domande | Predittiva e geospaziale |
| **Totale** | **7** | **22 domande** | |

---

## Limitazioni da richiamare nella relazione finale

1. **Dark figure of crime**: il dataset riflette reati denunciati, non commessi. Aree con bassa fiducia verso la polizia sono sotto-rappresentate.
2. **Cambiamenti classificatori**: riforme procedurali (es. Prop 47 in California) possono alterare i confronti longitudinali senza normalizzazione.
3. **Missing rate non casuale (MNAR)**: i campi arma, etnia vittima e descrittori demografici hanno tassi di missing elevati e non casuali.
4. **Granularità geografica**: le coordinate sono spesso offuscate al centroide del blocco (~100m di precisione).
5. **Assenza dati di esposizione**: il dataset non contiene dati di popolazione. I tassi per 1.000 residenti richiedono integrazione con dati census/ACS esterni.
6. **Artefatto mezzanotte**: ~130k record con `hour_occ = 0`, probabilmente orario sconosciuto codificato come 0000.
7. **Anomalie 2015-2016**: calo anomalo nel 2015 e picco nel 2016, probabilmente legati alla transizione del sistema di classificazione LAPD. Da documentare e annotare nei grafici.
