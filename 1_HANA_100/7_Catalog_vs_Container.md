# 7. Catalog vs Container Approach în SAP HANA

## 🎯 Scopul capitolului

Să înțelegi **diferența dintre abordarea clasică (Catalog)** și cea modernă **bazată pe HDI Containers (Container Approach)** în gestionarea obiectelor de bază de date în SAP HANA — concept esențial în HA100 și certificarea SAP HANA.

---

## 🔍 1. Ce este CATALOG Approach?

### ✅ Definiție:
Este metoda **tradițională** de creare a obiectelor HANA (tabele, views, proceduri) **direct în schema DB activă**, prin SQL sau SAP HANA Studio.

### 📦 Exemplu:
```sql
CREATE COLUMN TABLE MY_SCHEMA.ZCUSTOMERS (
    ID INT PRIMARY KEY,
    NAME NVARCHAR(100)
);
```

### ⚠️ De ce este considerată OUTDATED?

1. Probleme de lock-uri (citire vs. scriere)
În Catalog Approach:

Obiectele sunt partajate global între dezvoltatori/admini.

Orice modificare (ALTER TABLE, DROP VIEW, etc.) poate impune lock-uri:

Write lock (exclusive) → blochează orice alt acces până la finalul operației

Read lock → blochează modificările dacă obiectul este folosit într-o altă sesiune

- Asta duce la:

Erori de tip resource locked by another user

Delay-uri în echipe mari

Inconsistență între versiuni de obiecte (nimeni nu știe „cine a modificat ce”)

2. Fără control de versiune
Nu există control GIT sau istoric asupra modificărilor

Obiectele sunt modificate direct în DB → greu de colaborat

3. Lipsa modularizării
Nu există concept de „proiect” → totul e global, în aceeași schemă

Nu poți avea două versiuni ale aceleiași proceduri sau view în același timp

Complicat pentru dezvoltare paralelă, CI/CD, testare

4. Probleme de securitate
Obiectele sunt vizibile global în MY_SCHEMA, deci trebuie gestionate cu ACL (Access Control Lists)

Asta înseamnă efort mare de securizare manuală pe fiecare obiect în parte

- Concluzie:

Catalog Approach este bun doar pentru interogări administrative sau prototipuri rapide.

### ⚙️ Caracteristici:

| Caracteristică       | Detalii                                                 |
|----------------------|----------------------------------------------------------|
| Se lucrează direct   | Obiectele sunt create direct în schema DB               |
| Accesibil imediat    | Alte utilizatori pot vedea obiectele dacă au privilegii |
| Creat prin SQL       | Manual, fără structură de proiect                       |
| Fără versionare      | Nu există izolare sau control al modificărilor          |

---

## 📦 2. Ce este CONTAINER (HDI) Approach?

### ✅ Definiție:
Este metoda **modernă, bazată pe proiecte** în care obiectele HANA sunt gestionate de infrastructura HDI (HANA Deployment Infrastructure) și **izolate într-un container logic**.

### 🔧 Exemple:
- Proiecte Web IDE / Business Application Studio
- Fișiere `.hdbtable`, `.hdbview`, `.hdbprocedure`

```hdbtable
table.schemaName = "undefined";
table.tableType = COLUMNSTORE;
table.columns = [
  { name = "ID"; sqlType = INTEGER; },
  { name = "NAME"; sqlType = NVARCHAR; length = 100; }
];
```

🔁 Obiectul este deployat → apare în HANA doar în **containerul HDI**

---

## ⚙️ Caracteristici Container Approach

| Caracteristică           | Detalii                                                     |
|--------------------------|--------------------------------------------------------------|
| HDI (Deployment Infra)   | Gestionează build, versionare, rollback                     |
| Izolare                  | Obiectele nu apar în CATALOG general – doar în container    |
| Dev-friendly             | Permite lucrul pe proiecte separate, paralel                |
| Deployment Controlat     | Orice modificare este buildată & validată                   |
| Role Mapping             | Necesită `container role` pentru acces                      |

---

## ✅ 1.1 Cum rezolvă Container Approach problemele Catalog Approach?

### 🔒 1. Elimină lock-urile globale

- Fiecare dezvoltator sau aplicație lucrează în **propriul container HDI izolat**.
- Obiectele nu sunt partajate global – **nu există coliziuni de scriere**.
- Nu se mai aplică `read/write locks` la nivel de sistem, deoarece totul este gestionat **în containerul local**.

➡️ Rezultatul: fără erori de tip `resource is locked`, fără conflicte între echipe.

---

### 🧩 2. Obiectele sunt versionate și controlate

- Fiecare fișier `.hdbtable`, `.hdbview`, etc. se salvează în proiectul tău HDI
- Codul este stocat în GIT sau în workspace-ul proiectului
- Poți face rollback, versionare, revizuiri de cod

➡️ Rezultatul: dezvoltare colaborativă profesionistă

---

### 🚀 3. Modularitate și CI/CD-ready

- Proiectele sunt structurate (db/, srv/, ui/)
- Build-ul este automatizat cu `mta.yaml`
- Suportă **deployment controlat și reproductibil**
- Poți crea mai multe instanțe, pentru testare/QA/producție

➡️ Rezultatul: scalabilitate, flexibilitate, rapiditate

---

### 🛡️ 4. Securitate granulară și controlată

- Accesul la obiectele HDI se face doar prin **external service bindings**
- Utilizatorii nu pot „vedea” obiectele dintr-un container fără role explicite
- Nu mai ai nevoie să aplici GRANT direct pe tabele

➡️ Rezultatul: securitate robustă, minimă expunere

---

### 📚 TL;DR

| Problemă în Catalog        | Soluție în Container Approach            |
|----------------------------|------------------------------------------|
| Lock-uri globale           | ✅ Izolare completă per container        |
| Lipsă versionare           | ✅ Control cu Git + rollback             |
| Lipsă modularitate         | ✅ Structură MTA, build logic            |
| Securitate slabă (ACL)     | ✅ Acces doar prin service bindings      |
| Nu e cloud/dev-friendly    | ✅ Optimizat pentru SAP BTP & DevOps     |

---

## 🔄 Diferențe cheie: Catalog vs Container

| Aspect                  | CATALOG Approach                   | CONTAINER (HDI) Approach                |
|-------------------------|-------------------------------------|-----------------------------------------|
| Acces obiecte           | Vizibile în SCHEMA directă          | Vizibile doar în container              |
| Securitate              | ACL clasic pe schema DB             | Acces doar prin `external service` role |
| Versionare              | Nu                                  | ✅ Git + HDI                             |
| Flexibilitate Dev       | Limitată                            | ✅ Mare – modularitate, CI/CD ready      |
| Recomandat pentru...    | Admini, SQL clasic                  | ✅ Dezvoltare aplicații moderne (CAP, XSA, RAP) |

---

## 🧱 Exemplu vizual

```
 CATALOG Approach:

  +------------------------+
  | Schema: MY_SCHEMA      |
  |  - ZCUSTOMERS          |  ← Vizibil direct
  |  - ZORDERS             |
  +------------------------+

 CONTAINER Approach (HDI):

  +------------------------+
  | HDI Container: app-db  |
  |  - ZCUSTOMERS          |  ← Accesibil doar în container
  |  - ZORDERS             |
  +------------------------+
```

---

## 🧠 Recomandări SAP (din HA100)

- Pentru **aplicații noi** (SAP CAP, Fiori, UI5):  
  → folosește **HDI Containers** (container approach)

- Pentru **interogări administrative, reporting ad-hoc**:  
  → poți folosi încă **Catalog Approach** dacă e cazul

- Pentru **deployment profesionist & controlat**:  
  → **HDI + Git + mta.yaml** este standardul

---

## 📚 În contextul certificării SAP HANA

| Ce trebuie să știi                                         | Examinare                |
|------------------------------------------------------------|---------------------------|
| HDI = container logic controlat de un descriptor de proiect| ✅                        |
| Obiectele în HDI nu apar în catalogul clasic               | ✅                        |
| Necesită role pentru acces (external hdi container role)   | ✅                        |
| Catalog Approach NU este recomandat pentru aplicații noi   | ✅                        |

---

## ✅ TL;DR

- **Catalog Approach** = clasic, direct, fără izolare
- **Container Approach** = modern, izolat, versionat, CI/CD
- SAP recomandă **HDI** pentru orice dezvoltare modernă (CAP, RAP, XSA)