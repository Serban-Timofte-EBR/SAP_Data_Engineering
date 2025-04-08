# Dimension Calculation View

## Ce este un Data View?

- O reprezentare logică a datelor extrase dintr-un model sau din tabelele HANA, pregătită pentru a fi consumată de aplicații, rapoarte sau alte modele analitice

- ✅ Calculation Views pot consuma baze de date relaționale sau alte views.

- ✅ Calculation Views sunt cele expuse către SAP Analytics Cloud, RAP apps, și alte consumatoare de date.

- Un Calculation View este orice model de date construit în HANA pentru a combina, procesa și expune date.

1. de tip Dimension (fără măsuri) - atribute descriptive (ex: nume client, adresă, categorie produs) - Nu are masuri

2. de tip Cube (cu măsuri și agregări)

```txt
+-------------------------------------------------+
| Source Tables (ex: SALES, CUSTOMERS)            |
+-------------------------------------------------+
            ↓
+----------------+   +-----------------+
| Projection Node|   | Join Node         |
| (filter cols)  |   | (link tables)     |
+----------------+   +-----------------+
            ↓
+-------------------------------------------------+
| Aggregation Node (ex: SUM(quantity), KPIs)      |
+-------------------------------------------------+
            ↓
+-------------------------------------------------+
| Semantics (nume final, measure, attributes)     |
+-------------------------------------------------+

```

## Checking the Output of a Cacultation

- SAP Web IDE for SAP HANA allows to preview the data of the view

### Dimension

- Grupeaza logic mai multe atribute

- Se modeleaza pe unul sau mai multe tabele de **master data**

- Nu contin masuri => nu pot face raporturi (nu pot vedea in SAC)

### Types of Data Preview

| Type  |   Explanation |
| ----- | ------------- |
| Standard Data Preview | Select all the columns that are included in the semnatics of the calculation view |
| Custom SQL Query | Execute custom SQL Query or modify default SQL statements |

### Data Preview Features

- Not a reporting tool, but offers analysis functionality

### Deferred Default Query Execution

- A setting to not execute the query at preview OR the user wants to apply additional criteria

- Setted to *Require* at least one filter should be set before fetching view => could decrease the complexity of the query

## Creating Dimension Calculation Views

- **Dimensions provide context by master data tables**. Master Data Tabeles = date statice sau rar schimbate despre entități de business, cum ar fi clienți, produse sau furnizori

- **IMPORTANT:** One Dimension Calculation View -> Several CUBE Calculation View

- For example, the product attribute view can be used both in a purchase order CUBE calculation view and in sales order CUBE calculation view

## Creating CUBE calcultaion View

- Ex: Cube Calculation View = vânzări (Product ID, Sales Amount, Quantity Sold)

- By default, all measures in this type of calculation view will always be aggregated by the attributes requested by the query

```txt
+----------------------------------------------------------------------------+
| CUBE View provides measure: revenue + attributes: country, city            |
+----------------------------------------------------------------------------+

                                    Request Revenue with attribute country
+------------------------------------------------+          +------------------------------------------------+          
| The query requests only the country            | ------>  | The revenue is summed by country               |
+------------------------------------------------+          +------------------------------------------------+


                                    Request Revenue with attribute country + city
+------------------------------------------------+          +------------------------------------------------+          
| The query requests only the country            | ------>  | The revenue is summed by country and city      |
+------------------------------------------------+          +------------------------------------------------+
```

- This type of calculation view is optimized for ad-hoc analysis, where unpredictable slice-anddice is required over the measures by any combination of attributes within the model.

- **Star Join:** Nu face dubplicari (spre deosebire de JOIN-urile normale) si optimizeaza automat relatiile intre CUBE si DIMENSION join

```txt
               DIMENSION: Product
                      ▲
               DIMENSION: Customer
                      ▲
               DIMENSION: Date
                      ▲
                  CUBE (Sales Data)
             (CUBE with Star Join)

```

- Ai un CUBE central care consumă direct mai multe DIMENSION views, ca într-un Star Schema

## Creating SQL Access Only Calculation Views

- Graphucal Calculation View defined with SQL Access Only data category

- To be consumes by other calculation views

- Unlike CUBE or CUBE with star join, the resulting data set is not automatically aggregated to  the columns requested in the external query => No extra smart aggregations, only what you ask for

- As the name suggests, it is possible to access SQL Access Only calculation views directly 
using SQL. For example, they can be accessed from a table function or procedure.

## Choosing a Data Source for a Calculation View

- Supported Data Source Types in SAP HANA Calculation Views: Column Tables, Row Tables, Calculation Views, SQL Views, Table Functions (located in the same database if using a multi-tenant system), Virtual Tables

- To chcek if a table is row or column - Check HDI Container or Catalog View OR via SQL Console: M_TABLES

## Working with Common Features of Calculation Views

- Every calcultation view has a *Semantics Node* and will always be the top node

- In the Semnatics Node, we assign semantics to each column in order to define its behavior and meaning

- Semantics Node este ultimul nod dintr-un Calculation View care definește cum sunt interpretate și expuse coloanele modelului => coloanele care sunt atribute/masuri, seteaza unitati de masura, configureaza tipul de output (CUBE/DIMENSION/SQL ACCESS ONLY), mapeaza alias-uri

- In a calculation view, we can set the default value of null (Null Handling check-box)

## Defining the Top View Node

- **IMPORTANT:** There are always two odes automatically provided: Semantic Node + Top View Node

- Top View Node is under the Semantic Node => it is the last node from logical processing

### Top View Node Roles:

1. Reunește toată logica modelului: join-uri, unions, projections, aggregations.

2. Este nodul final de modelare care livrează datele către Semantics Node.

3. Determină structura exactă a setului de date: ce coloane ies din Calculation View.

## Info:

1. Valid Output Column Types: Measure, Attribute

2. SAP Recommandation to check calculation view result: Custom SQL Queries using SQL Console

3. A Dimension can have attributes, but NOT measures

4. SQL Access Only Calcultation Views: Do NOT expose their metadata to reporting tools, Mainly used for reusing inside other calculation views

5. Hide Columns: When the columns are not required or allowed for client consumtation. **IMPORTANT:** A hidden column is still exposed to consuming calculation views
