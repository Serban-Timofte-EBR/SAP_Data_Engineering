# 6. Write Operations & Multi-Tenancy în SAP HANA

---

## 📥 1. Write Operations și Delta Merge în SAP HANA

### 🔹 Column Store = eficient pentru citiri, mai lent pentru scrieri
Pentru a păstra viteza mare de citire și compresie, SAP HANA **nu scrie direct în zona ordonată (main store)**, ci folosește un sistem cu 2 niveluri:

---

### 🧱 Structura de scriere:

| Componentă       | Caracteristici                                     |
|------------------|----------------------------------------------------|
| **Delta Store**  | Neordonat, scriere rapidă, mici volume de date     |
| **Main Store**   | Ordonat, comprimat, optimizat pentru citire        |

➡️ Când scrii date (`INSERT`, `UPDATE`, `DELETE`), ele sunt salvate mai întâi în **Delta Store**.

---

### 🔄 Ce este Delta Merge?

**Delta Merge** este procesul care:
- transferă datele din **Delta Store** în **Main Store**
- optimizează datele (sortare, compresie)
- se face **în fundal**, automat sau manual

---

### ⚙️ Tipuri de Delta Merge:

| Tip                   | Descriere                                      |
|------------------------|-----------------------------------------------|
| **Auto Merge**         | Declanșat automat de sistem (bazat pe volum)  |
| **Smart Merge**        | Analizează accesul și costul (doar dacă e nevoie) |
| **Hard Merge (manual)**| Poate fi declanșat de admin prin `MERGE DELTA OF` |

---

### 🧠 De ce este important?

- 📈 Permite scriere rapidă fără a afecta performanța citirilor
- 🧮 Menține Column Store comprimat și performant
- 🔄 Poate fi monitorizat în `M_DELTA_MERGE_STATISTICS`

---

## 🏗️ 2. Multi-Tenancy în SAP HANA

### 🔍 Ce este o „mașină HANA”?

O **mașină HANA** este o instanță fizică sau virtuală unde rulează:
- un **proces HANA Database Server**
- resursele RAM + CPU dedicate motorului in-memory

---

### 🧱 Ce înseamnă Multi-Tenancy?

**Multi-Tenant Database Containers (MDC)** permite ca:
- Pe aceeași instanță HANA să ruleze **mai multe baze de date independente**
- Fiecare tenant DB are:
  - Propriul spațiu de date
  - Propriul catalog de obiecte
  - Proprii utilizatori, securitate, back-up

---

### ⚙️ Beneficii ale Multi-Tenancy

| Beneficiu             | Detalii                                      |
|------------------------|-----------------------------------------------|
| Izolare                | Datele dintr-o bază nu pot fi văzute din alta |
| Ușurință în management | Fiecare DB poate fi actualizată separat       |
| Securitate             | ACL și autentificare per tenant               |
| Scalabilitate          | Poți aloca resurse diferit pentru fiecare     |

---

### 🧠 Componente ale HANA Multi-Tenant

| Componentă             | Rol                                              |
|------------------------|--------------------------------------------------|
| **SystemDB**           | Baza principală de administrare (control)       |
| **TenantDBs**          | Baze izolate unde rulează aplicații și modele   |
| **HDBLCM**             | HANA lifecycle manager – gestionare instanțe    |
| **SQL Port dedicat**   | Fiecare tenant are propriul port TCP SQL        |

---

## 🧾 TL;DR

### ✅ Write Operations + Delta Merge:
- Scrierea se face în **Delta Store** pentru viteză
- Datele sunt apoi mutate în **Main Store** (ordonat)
- Merge-ul poate fi auto, smart sau manual

### ✅ Multi-Tenancy:
- Poți avea **mai multe baze HANA** pe o singură instanță
- Fiecare tenant e **complet izolat**
- Ideal pentru cloud, demo-uri, SaaS, partajare de resurse

---

## 🔍 Vizualizare logică:

```
        +-------------------+         +-------------------+
        |    SYSTEM DB      |         |    TENANT DB 1     |
        |  (Admin central)  | <-----> |  catalog, users    |
        +-------------------+         +-------------------+
                                         |
        +-------------------+            ▼
        |   TENANT DB 2     |       Aplicatii, modele,
        | catalog, views    |       utilizatori separați
        +-------------------+
```