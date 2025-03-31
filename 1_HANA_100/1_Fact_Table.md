# 1. Ce este o Fact Table și cum funcționează cu Dimensiunile

## ✅ Ce este o **tabelă de fapte** (*fact table*)?

Este o tabelă centrală care **conține măsurile numerice** (faptele) ale unui proces de business — vânzări, comenzi, profit, cantitate etc.

Aceasta:
- **se leagă** de mai multe **tabele de dimensiuni**
- și împreună formează un model de tip **stea (star schema)** sau **fulg (snowflake schema)**

---

## 🧱 Structură simplificată:

```
                    +--------------------+
                    | Dimensiune PRODUS  |
                    |  product_id, name  |
                    +--------------------+
                            ▲
                            │
+--------------------------+▼--------------------------+
|                   FACT VÂNZĂRI (Sales)               |
|  sale_id, product_id, customer_id, quantity, amount  |
+--------------------------▲--------------------------+
                            │
                    +--------------------+
                    | Dimensiune CLIENT   |
                    | customer_id, name  |
                    +--------------------+
```

---

## 🔍 Concret:

### 🔸 **Fact Table (ZSALES_FACTS):**

| sale_id | product_id | customer_id | quantity | revenue |
|---------|------------|-------------|----------|---------|
| 1001    | P123       | C001        | 3        | 120.00  |
| 1002    | P456       | C002        | 1        | 59.99   |

🔸 Conține:
- ID-uri (chei străine către dimensiuni)
- Măsuri numerice: `quantity`, `revenue`

---

### 🔸 **Dimensiune PRODUS (ZPRODUCT_DIM):**

| product_id | name         | category    |
|------------|--------------|-------------|
| P123       | Smartphone X | Electronics |
| P456       | Headphones Z | Audio       |

---

### 🔸 **Dimensiune CLIENT (ZCUSTOMER_DIM):**

| customer_id | name       | region |
|-------------|------------|--------|
| C001        | John Doe   | Europe |
| C002        | Jane Smith | Asia   |

---

## 🎯 Scopul și utilizarea tabelelor de fapte și dimensiuni

Tabelele de fapte și dimensiuni sunt esențiale în modelarea datelor pentru:

- ✅ **Analize de business (BI / Analytics)**
- ✅ **Rapoarte de management și KPI-uri**
- ✅ **Dashboard-uri Fiori / SAC**
- ✅ **Machine Learning și Data Science** (pentru training sets structurate)

---

## 🔄 Surse de date pentru tabelele de fapte

Tabelele de fapte pot fi populate din:
- **Sisteme ERP (ex: SAP S/4HANA, SAP ECC)** – vânzări, comenzi, clienți
- **Sisteme CRM / marketing** – lead-uri, campanii
- **Fișiere externe / flat files** – .csv, Excel
- **API-uri externe** – sistem POS, IoT, marketplace-uri
- **SAP BW / HANA views** – prin Calculation View sau CDS

---

## 📌 Extragerea și integrarea datelor

Datele sunt adesea:
- 🔄 preluate prin **ETL/ELT** (Data Services, SLT, SDI, SAP Datasphere)
- 🗃 stocate în **HANA**, **SAP BW**, **Data Lake**, etc.
- 🧮 transformate în Calculation Views sau CDS Views pentru consum analitic

---

## 🧮 Exemplu de întrebare analitică:

> „Care este totalul vânzărilor pe produs în luna curentă?”

Se face un `JOIN` între `ZSALES_FACTS` și `ZPRODUCT_DIM`, apoi se grupează după `product.name` și se face `SUM(revenue)`.

---

## 🧠 Paralele

| Termen               | Explicație simplă                          | Echivalent                   |
|----------------------|--------------------------------------------|------------------------------|
| **Fact table**       | Tabel central cu numere, măsuri            | `Sales`, `Orders`, `Inventory Facts` |
| **Dimensiune**       | Tabel lateral cu detalii descriptive       | `Product`, `Customer`, `Date`        |
| **Star Schema**      | Fact + dimensiuni directe                  | rapid, simplu                       |
| **Snowflake Schema** | Dimensiuni normalizate (cu subdimensiuni)  | mai lent, mai flexibil              |

---

## ✅ TL;DR

> O **fact table** conține **măsurile numerice** importante (vânzări, cantități, venituri), și se **leagă** de tabele descriptive numite **dimensiuni** (produs, client, timp) pentru a permite **analize și rapoarte avansate**.

---

## 🔧 Următorii pași

Se poate construi acest model:
- 📊 În **SAP HANA** cu Calculation Views (dimensiuni și măsuri)
- 📦 În **CDS Views** folosind:
  - `@Analytics.dataCategory: #FACT`
  - `@Analytics.dataCategory: #DIMENSION`
- 🚀 Expus prin OData, folosit în SAP Analytics Cloud sau Fiori Elements
---
