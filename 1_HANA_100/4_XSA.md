# 4. SAP HANA XSA – Extended Services, Advanced Model

## ✅ Ce este XSA?

**XSA (Extended Services Advanced)** este platforma de aplicații modernă integrată în SAP HANA, construită pentru:
- rularea aplicațiilor multi-language (Node.js, Java, Python),
- managementul containerelor,
- integrarea cu servicii cloud și microservicii.

📦 XSA este baza pentru **SAP Web IDE for HANA**, **HDI (HANA Deployment Infrastructure)** și dezvoltarea modernă în HANA.

---

## 🧱 Arhitectura XSA

XSA extinde SAP HANA cu:

| Componentă             | Descriere                                                       |
|------------------------|------------------------------------------------------------------|
| **XS Advanced Runtime**| Motorul de execuție pentru aplicații (înlocuiește XS Classic)    |
| **Multi-language Support** | Suport pentru Node.js, Java, Python (prin buildpacks)     |
| **HDI**                 | HANA Deployment Infrastructure – controlează obiectele DB       |
| **User-Provided Services** | Integrare cu servicii externe (ex: PostgreSQL, Redis, etc.) |
| **Microservices**       | Aplicațiile pot fi scalate ca servicii separate                 |

---

## 💻 SAP Web IDE for SAP HANA (cu XSA)

### 🔧 Funcționalități:
- Proiecte HANA (db, srv, ui)
- Modelare vizuală a Calculation Views
- Scriere de CDS, SQLScript, Node.js, Java
- Build & deploy direct în containerul HDI

📁 Structură proiect tipică:

```
my-project/
│
├── db/         → modele CDS, tabele, views
├── srv/        → logică business (Node.js/Java)
├── ui/         → aplicație SAPUI5/Fiori (opțional)
├── mta.yaml    → descriere aplicație multi-target
```

---

## 🧠 Ce este HDI (HANA Deployment Infrastructure)?

HDI este sistemul care:
- **gestionează obiectele HANA** (tabele, views, procedures),
- în mod izolat pe fiecare spațiu și container,
- permite controlul versiunilor și deploy-urilor.

🔐 Totul este desfășurat în **HDI Containers**, care izolează aplicațiile și protejează obiectele.

---

## ⚙️ Componente-cheie într-un proiect XSA

| Director | Ce conține                                    |
|----------|-----------------------------------------------|
| `db/`    | CDS, tabele, views, constraints, synonyms     |
| `srv/`   | Servicii REST/OData în Node.js sau Java       |
| `ui/`    | UI Fiori/SAPUI5 (fără ABAP)                   |
| `mta.yaml` | Configurația generală a aplicației          |

---

## 🔐 Autentificare și utilizatori

- XSA folosește **uși de securitate dedicate** față de utilizatorii clasici HANA
- Userii sunt gestionați cu `xs-security.json` + `roles`, `scopes`, `JWT`
- Aplicațiile folosesc serviciul `uaa` (User Account & Authentication)

---

## 🚀 Exemple de cazuri de utilizare

- Crearea unui serviciu REST în Node.js care accesează tabele HANA
- Model analitic expus în Calculation View + consumat de o aplicație UI
- Integrare cu SAP Analytics Cloud (prin exposed OData)

---

## 🧠 TL;DR

| Concept                     | Rol în XSA                                               |
|-----------------------------|----------------------------------------------------------|
| XS Advanced Runtime         | Platformă pentru microservicii în HANA                   |
| HDI Container               | Spațiu de lucru izolat pentru modele și cod              |
| SAP Web IDE for HANA        | IDE pentru dezvoltare HANA modernă (CDS, Node, UI5)      |
| mta.yaml                    | Structura și build-ul aplicației multi-layer             |
| Autentificare UAA           | Gestionare acces și roluri în aplicații XSA              |

---

## 🌐 Alternative moderne

- SAP Business Application Studio (succesorul Web IDE)
- SAP HANA Cloud + SAP BTP pentru dezvoltare full cloud