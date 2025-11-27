# 📘 README – Etapa 3: Analiza și Pregătirea Setului de Date pentru Detectarea Schimbărilor în Imagini

**Disciplina:** Rețele Neuronale
**Instituție:** POLITEHNICA București – FIIR
**Student:** Nicoară Vlad-Mihai
**Data:** [Data]

---

## Introducere

Acest document prezintă activitățile realizate în **Etapa 3**, care include analiza și preprocesarea setului de date necesar proiectului *„Compararea și Detectarea Schimbărilor din Imagini Aplicate Sălilor de Laborator”*.
Obiectivul etapei este pregătirea corectă a imaginilor înainte de antrenarea rețelelor neuronale (Siamese + UNet), respectând principiile de calitate, consistență și reproductibilitate a datelor.

---

## 1. Structura Repository-ului GitHub (versiunea Etapei 3)

```
change-detection-lab/
├── README.md
├── docs/
│   └── datasets/          # informații despre dataset + rezultate EDA
├── data/
│   ├── raw/               # imagini brute (neprocesate)
│   │   ├── before/        # imagini înainte
│   │   └── after/         # imagini după
│   ├── pairs/             # perechi before–after generate automat
│   ├── processed/         # imagini aliniate și normalizate
│   ├── train/             # set de antrenare
│   ├── validation/        # set de validare
│   └── test/              # set de testare
├── src/
│   ├── preprocessing/     # cod pentru preprocesarea imaginilor
│   ├── data_acquisition/  # generare dataset (dacă se extinde)
│   └── neural_network/    # arhitectura RN (Siamese + UNet)
├── config/                # fișiere configurare preprocesare
└── requirements.txt       # dependențe Python
```

---

## 2. Descrierea Setului de Date

### 2.1 Sursa datelor

* **Origine:** imagini cu o sală de laborator în două momente diferite (simulare/generare proprie).
* **Modul de achiziție:** imagini colectate manual (before/after) sau generate în cadrul proiectului.
* **Condițiile colectării:** imagini surprinse cu aceeași cameră, din unghi similar, cu diferențe introduse manual (obiect mutat, scaun deplasat, monitor oprit/pornit etc.).

### 2.2 Caracteristicile dataset-ului

* **Număr total de perechi de imagini:** [completat de student]
* **Număr imagini before:** [ ]
* **Număr imagini after:** [ ]
* **Tip de date:** imagini RGB
* **Format fișiere:** PNG / JPG
* **Dimensiune finală:** 256×256 px (după preprocesare)

### 2.3 Descrierea fiecărui element din dataset

| Componentă     | Tip     | Descriere                                                        |
| -------------- | ------- | ---------------------------------------------------------------- |
| Imagine_before | RGB     | Imaginea capturată la începutul orei                             |
| Imagine_after  | RGB     | Imaginea capturată la finalul orei                               |
| Mask_diff      | Imagine | Mască binară indicând zonele în care au apărut schimbări         |
| Score_diff     | Numeric | Un scor între 0 și 1 ce indică nivelul diferenței dintre imagini |

**Fișier recomandat:** `data/README.md`

---

## 3. Analiza Exploratorie a Datelor (EDA) – Sintetic

### 3.1 Analiză cantitativă

* număr total imagini before/after
* rezoluții originale
* distribuția valorilor pixelilor
* histogramă pe canale R, G, B
* verificarea consistenței perechilor A–B

### 3.2 Analiza calității imaginilor

* verificarea iluminării neuniforme
* diferențe de unghi sau perspectivă
* imagini nealiniate între before/after
* zgomot, blur, obiecte în mișcare

### 3.3 Probleme identificate

* variații de lumină între cadre → necesară normalizare
* aliniere imperfectă a imaginilor → necesară corecția geometrică (homografie)
* număr mic de imagini → risc de overfitting → utilizare augmentări
* diferențele pot fi subtile → nevoie de corecții pentru contrast

---

## 4. Preprocesarea Datelor

### 4.1 Curățarea imaginilor

* eliminarea imaginilor corupte
* conversie la format RGB
* uniformizare dimensiune: 256×256 px
* corecție de iluminare (normalizare histogramă)

### 4.2 Alinierea imaginilor în perechi

**Metodă utilizată:**
Detectare puncte cheie (ORB/SIFT) → potrivire → estimare Homography → deformare imagine after pentru a corespunde imaginii before.

### 4.3 Generarea etichetelor (labels)

* **Mask_diff:** calculată prin diferență dintre imagini aliniate + threshold adaptiv
* **Score_diff:** proporție pixeli modificați (0–1)

### 4.4 Normalizare și transformări

* scaling valorilor pixelilor în [0,1]
* augmentări (opțional):

  * flip orizontal
  * variație luminozitate
  * rotație ±10°

### 4.5 Structurarea seturilor

**Împărțire utilizată:**

* 70% — train
* 15% — validation
* 15% — test

**Principii respectate:**

* împărțirea se face pe perechi (before/after)
* datele sunt amestecate random
* nu se folosesc imagini din test în antrenare (evitare data leakage)

### 4.6 Salvarea rezultatelor preprocesării

* imagini procesate → `data/processed/`
* perechi → `data/pairs/`
* seturi → folderele train/validation/test
* parametri preprocesare → `config/preprocessing_config.yaml`

---

## 5. Fișiere Generate în Această Etapă

* `data/raw/` – imagini brute (before/after)
* `data/pairs/` – perechi aliniate
* `data/processed/` – imagini normalizate și corectate
* `data/train/`, `data/validation/`, `data/test/` – seturi pregătite pentru model
* `src/preprocessing/` – codul Python pentru aliniere și preprocesare
* `docs/datasets/` – rezultate EDA
* `data/README.md` – explicația dataset-ului

---

## 6. Stare Etapă (de completat de student)

* [ ] Structura repository configurată
* [ ] Setul de imagini colectat / generat
* [ ] EDA efectuată și documentată
* [ ] Imagini aliniate și preprocesate
* [ ] Seturi train/validation/test generate
* [ ] Documentație actualizată în README + `data/README.md`

