# 5. Advanced Analytics Processing în SAP HANA

## 🎯 Ce înseamnă Advanced Analytics în SAP HANA?

SAP HANA nu este doar o bază de date, ci o **platformă de procesare analitică avansată**.  
Oferă mai multe **engines specializate**, integrate nativ, pentru a analiza date complexe în RAM – fără a muta datele în afara bazei.

---

## ⚙️ Componentele cheie ale Advanced Analytics Processing

SAP HANA include următoarele **engines dedicate**:

| Engine               | Descriere                                                    |
|----------------------|---------------------------------------------------------------|
| **Predictive Analysis Library (PAL)** | Funcții statistice și algoritmi ML (K-Means, Regression, Trees etc.) |
| **APL (Automated Predictive Library)** | AutoML – generare de modele predictive fără cod     |
| **Graph Engine**     | Analize pe grafuri (noduri + muchii), relații complexe       |
| **Spatial Engine**   | Analize geo-spațiale (coordonate, forme, distanțe etc.)      |
| **Text Analysis / Mining** | NLP, tokenizare, extragere entități din texte           |
| **Document Store**   | Procesare JSON / semi-structurat în HANA (ca NoSQL)          |

---

## 🔬 Predictive Analysis Library (PAL)

PAL oferă peste 100+ algoritmi ML optimizați pentru in-memory:

- ✅ Classification: Decision Trees, SVM, Naive Bayes
- ✅ Regression: Linear, Logistic
- ✅ Clustering: K-Means, DBSCAN
- ✅ Time Series: ARIMA, Exponential Smoothing
- ✅ Recommendation: Collaborative Filtering

### 🧪 Executare:
- Se bazează pe **proceduri SQLScript** sau **funcții table user-defined**
- Necesită definirea de tabele de input/output în HDI Container

---

## 🤖 APL – Automated Predictive Library

- Oferă AutoML: generează automat modele pe baza datelor
- Perfect pentru useri non-tehnici (Business Analysts)
- Rulează pe seturi HANA și expune rezultate (performanță, importanța variabilelor)

---

## 🗺️ Spatial Engine

Permite lucrul cu date geografice:

### 🔹 Tipuri de date:
- `ST_POINT`, `ST_POLYGON`, `ST_LINESTRING`

### 🔹 Funcții:
- Calcul de distanță (`ST_DISTANCE`)
- Intersecții geometrice (`ST_INTERSECTS`)
- Filtrare prin raze geografice (ex: „toți clienții la 5km de magazin”)

### 🔧 Use cases:
- Optimizare rută livrare
- Analiză locații clienți
- Rețele de distribuție

---

## 🔗 Graph Engine

Analizează date ca graf: **noduri (vertices)** și **relații (edges)**

### 🔹 Exemple:
- Detectarea fraudelor (legături între conturi)
- Rețele sociale (influenceri, centralitate)
- Producție/distribuție (lanțuri dependente)

### 🔹 Algoritmi incluși:
- Shortest Path
- Betweenness Centrality
- PageRank
- Strongly Connected Components

---

## 🧾 Text Analysis / Text Mining

Permite interpretarea conținutului textual neformatat:

### 🔹 Funcții:
- Tokenizare, stemming
- Recunoaștere entități (NLP): nume, locații, sume
- Extracție de insighturi din recenzii, e-mailuri, documente

### 🔧 Use cases:
- Clasificare de texte (ex: plângeri clienți)
- Analiză sentimente
- Automatizare documente

---

## 📂 Document Store

Stochează și procesează fișiere JSON în tabele HANA:

- Tip `DOCUMENT TABLE` – stochează JSON direct
- Permite interogare cu `SQL JSON PATH`
- Se folosește pentru aplicații NoSQL, flexibilitate de schemă

---

## 🚀 Executare & integrare

- Toate aceste engines funcționează **în același spațiu de date HANA**
- Rulează **în RAM**, fără export către Python/R
- Pot fi combinate în **Calculation Views**, **proceduri SQLScript**, **RAP apps**

---

## 🧠 TL;DR

| Engine        | Funcționalitate principală                              | Exemple utilizare                                  |
|---------------|----------------------------------------------------------|----------------------------------------------------|
| PAL           | Machine Learning manual                                  | Clustering, time-series, predictive maintenance    |
| APL           | AutoML                                                   | Forecast KPI fără cod                              |
| Graph Engine  | Analiză relațională                                      | Detectare fraudă, rețele sociale                   |
| Spatial       | Analize geo-spațiale                                     | Livrare, locații clienți, optimizări geografice    |
| Text Analysis | NLP & insight din texte                                  | Analiză recenzii, sentiment, clasificare e-mailuri |
| Document Store| Procesare JSON                                           | Aplicații flexibile, semi-structurate              |

---

## 📚 Resurse utile pentru certificare:

- SAP Learning: Advanced Analytics in SAP HANA Cloud
- PAL & APL Documentation: https://help.sap.com/viewer/product/SAP_HANA_PLATFORM
- Tutorials: developers.sap.com (Graph, Spatial, Text, PAL)