# 📘 Important Terms in Data Modeling (SAP HANA & BI)

---

## 📊 Measure vs Attribute

- **Measure**: Reprezintă o valoare numerică (ex: preț, cantitate), peste care se aplică funcții de agregare.  
  ➤ Exemplu: număr produse vândute, preț unitar, total vânzări.

- **Attribute**: Descrie o măsură.  
  ➤ Exemplu: ID produs, nume client, țară organizație vânzări, monedă.  
  Împreună cu măsurile oferă informații de business.

---

## 🧱 Dimensions

- O **dimensiune** este o grupare de atribute semnatic similare.

### Exemple:
- **Dimensiunea Product**: `Product Key`, `Product Name`, `Category`, `Supplier`.
- **Dimensiunea Sales Organization**: `Org Key`, `Name`, `Region`.

---

## 🧊 Cube

- O combinație de dimensiuni și atribute.
- Oferă agregări ale măsurilor pe baza selecțiilor de atribute.

---

## ✨ Star Schema

- Organizare logică ce implică o **fact table** centrală și tabele de **dimensiuni** conectate.
- `Fact Table` conține: măsuri, chei străine și informații adiționale (ex: status factură).

---

## 🔹 Hierarchy

- Gruparea atributelor într-o structură ierarhică.

### Exemplu:
- Regiuni pe categorii:
  - `EMEA`: Germania, UK
  - `Americas`: SUA, Brazilia
  - `APJ`: Japonia, China

- **Root** = nod fără părinți

---

## 📃 Semantics

- Reprezintă "sensul" dat unei date.
- Ajută la formatare și interpretare (ex: semn pentru monedă).

### Exemplu:
- Câmpul `Currency` are semantica `EUR` sau `RON`, iar un tool de BI afișează automat simbolul valutei.

---

## 📊 Models & Virtual Data Model (VDM)

- Prin `Calculation View` într-un sistem SAP HANA se creează un **VDM** = Virtual Data Model

### Virtual Data Model:
- Ecosistem de view-uri virtuale care răspund la întrebări de business.
- Nu persistă datele — doar le interoghează și agregă **la runtime**.

#### Beneficii:
- Calcul la runtime cu `HANA database engine`
- Reutilizabil: obiectele pot fi refolosite
- Flexibil: permite ierarhii, parametri, semantici
- Adaptabil: poți selecta dimensiuni și măsuri expuse dinamic

#### Design Time vs Runtime:
- **Design Time**: definire în Web IDE
- **Build**: generează obiect runtime în baza de date

---

## 🤖 OLAP vs OLTP

| Termen | Ce face | Exemplu |
|--------|---------|----------------|
| **OLTP** (Online Transaction Processing) | Gestionează tranzacții frecvente și rapide (insert, update, delete). | Un client face o comandă online. |
| **OLAP** (Online Analytical Processing) | Analizează date, face rapoarte și interogări complexe. | Un manager analizează vânzările lunare pe regiuni. |

### 🔑 Pe scurt:
- **OLTP** = viteză pentru operațiuni zilnice  
- **OLAP** = analiză pentru decizii de business
