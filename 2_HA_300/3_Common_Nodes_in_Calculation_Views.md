# Working with Common Nodes in Calculaton Views

## Using Projection Nodes

- Projection Node is typically used in the following scenarios:

1. Filter measures based on attributes values

2. Extract only some columns from a data source

3. Define calculated columns, in particular when calculation must occur BEFORE an aggregation

## Working with Aggregation Nodes:

- Aggregation Nodes are used to apply aggregate functions to measures based on one or several attributes

- A graphical calculation view can support SUM, MIN, MAX, and COUNT.

|   Node    | Scope |   Exemple |
| --------- | ----- | --------- |
| Projection | Select, Filter, Calculate | Apply WHERE, Coloane calculate (Price * Quantity) |
| Aggregation | Aggregate data o measurable | GROUP BY, SUM, AVG, COUNT |

## Aggregations in Client Query

- Can execute SQL Query on top of a CUBE Calculation View but the behaviour has two cases:

1. SQL Query withOUT GROUP BY clause => each measure is aggregated as specified in the Semantics of the Calcaultaion View

2. SQL Query with GROUP BY => overwrites what we have in Semantics of a Calculation View

### Example

When you build the Calculation View:

- You create two measures:
  - `Revenue`
  - `Quantity`

- In the **Semantics Node**, you configure:
  - `Revenue` → Aggregation Type: **SUM**
  - `Quantity` → Aggregation Type: **SUM**

🔵 **Important**: In Semantics, you define **how measures must behave when grouped** — without needing the end-user to care about technicalities.

```sql
SELECT Product_ID, Revenue, Quantity
FROM CV_SALES_REPORT;
```

- HANA generates: Revenued SUM-ed by Product_ID AND Quantity SUM-ed by Product_ID

```sql
SELECT Product_ID, Region, SUM(Revenue)
FROM CV_SALES_REPORT
GROUP BY Product_ID, Region;
```

- HANA ignores the Semantics and uses the GROUP BY syntax

## Flags

1. Keep Flag

> **Keep Flag** controlează **dacă o coloană filtrată** mai rămâne sau nu disponibilă în fluxul de date după aplicarea filtrului.

---

### 🔹 Cum funcționează:

| Situație                                  | Ce se întâmplă                                                |
|-------------------------------------------|---------------------------------------------------------------|
| **Keep Flag = OFF** (default)              | Coloana filtrată dispare după aplicarea filtrului.            |
| **Keep Flag = ON**                        | Coloana rămâne disponibilă și după ce filtrul a fost aplicat. |

---

- Dacă vrei să **filtrezi** după o coloană, dar **să o mai folosești** mai departe (ex: la agregări, joins), trebuie să activezi **Keep Flag**.
- Altfel, HANA consideră că acea coloană a fost folosită doar pentru filtrare și o elimină din flux.

---

- Vrei doar produsele cu `Category = 'Electronics'`.
- Dar **vrei să păstrezi** `Category` și pentru raport.
- ➡️ **Activezi Keep Flag** pe coloana `Category`.

2. Transparent Filter

> **Transparent Filter** permite ca filtrele aplicate într-un view superior (consumator) să fie **propagate în jos** către subnivelele de modelare.

| Situație                               | Ce se întâmplă                                                    |
|----------------------------------------|-------------------------------------------------------------------|
| **Transparent Filter = OFF** (default) | Filtrul aplicat sus NU se transmite automat în structura internă. |
| **Transparent Filter = ON**            | Filtrul aplicat sus SE transmite către nodurile de mai jos.        |

---

- Îmbunătățește **performanța**: filtrele sunt aplicate mai devreme → mai puține date de procesat.
- Evită supraîncărcarea modelului cu date inutile.

---

- Ai un raport care cere doar `Sales in 2024`.
- Dacă Transparent Filter este ON:
  - HANA aplică direct filtrul `YEAR = 2024` jos, la sursele de date.
- Dacă este OFF:
  - Sistemul aduce toate datele, apoi filtrează sus — mai lent.

## Info:

1. Purpose of Projection Node: Apply filters on the data, Extract only the required columns from a data source

2. In Aggregation Node a calculated column is always computed **after** the aggregate function.
