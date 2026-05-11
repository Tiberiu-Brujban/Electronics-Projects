# P01 — Detector de Paritate Impară pe 4 biți (TTL + DTL + IC)

<div align="center">

![Type](https://img.shields.io/badge/tip-Digital-blueviolet?style=flat-square)
![Status](https://img.shields.io/badge/status-Complet-brightgreen?style=flat-square)
![Sim](https://img.shields.io/badge/simulare-LTspice%20%7C%20Logisim-blue?style=flat-square)
![Tech](https://img.shields.io/badge/tehnologie-TTL%20%7C%20DTL%20%7C%20IC-orange?style=flat-square)

[← Înapoi la lista de proiecte](../../README.md)

</div>

---

## Descriere

Detector de paritate impară pe **4 biți de intrare (A, B, C, D)**, construit cu **trei tehnologii diferite de implementare XOR**, toate pe același breadboard:

- **XOR #1** — 4 porți NAND construite manual din tranzistori și rezistențe în tehnologie **TTL** (Transistor-Transistor Logic)
- **XOR #2** — 4 porți NAND construite manual din diode și rezistențe în tehnologie **DTL** (Diode-Transistor Logic)
- **XOR #3** — IC comercial **SN74HC86N**

Ieșirea este **LED aprins** când numărul de biți de `1` în intrare este **impar**, **LED stins** când este par.

Scopul proiectului a fost să înțeleg cum se construiesc porți logice de la zero și să compar cele trei tehnologii pe același circuit real.

---

## 📷 Circuit fizic

| Vedere generală | Detaliu TTL | Detaliu DTL |
|:--------------------------:|:------------------------:|:------------------------:|
| ![breadboard](./photos/breadboard_circuit.png) | ![ttl](./photos/breadboard_circuit_TTL.png) | ![dtl](./photos/breadboard_circuit_DTL.png) |

---

## 🔧 Componente folosite

### XOR #1 — Tehnologie TTL (NAND din tranzistori)

| Componentă | Valoare | Rol |
|------------|---------|-----|
| Tranzistori NPN | BC547 × 10 | Porțile NAND TTL |
| Tranzistori NPN | 2N2222 × 2 | Tranzistor multi-emitor la ultima poartă NAND TTL |
| Rezistențe bază | 4.7 kΩ × 4 | Limitare curent bază |
| Rezistențe colector | 1 kΩ × 4 | Pull-up colector |
| Buffer | 2N2222 × 2, 330 Ω × 1, 1 kΩ × 1 | Rezolvare problemă fanout |

### XOR #2 — Tehnologie DTL (NAND din diode + tranzistor)

| Componentă | Valoare | Rol |
|------------|---------|-----|
| Diode | 1N4148 × 16 | Porțile NAND DTL |
| Tranzistori NPN | 2N2222 × 4 | Porțile NAND DTL |
| Rezistențe pull-up | 4.7 kΩ × 4 | Pull-up intrări diode |
| Rezistențe bază | 4.7 kΩ × 4 | Limitare curent bază |
| Rezistențe colector | 1 kΩ × 4 | Pull-up colector |

### XOR #3 — IC comercial

| Componentă | Valoare | Rol |
|------------|---------|-----|
| SN74HC86N | IC XOR quad | Al treilea XOR (combinare parțiale) |
| LED verde | — | Indicator paritate impară |

### Alimentare

| Parametru | Simulare LTspice | Real (breadboard) |
|-----------|-----------------|-------------------|
| VCC | 5 V | ~7–8 V (baterie 9V sub sarcină) |

---

## 📐 Schema logică

![Schematic](./schematic/schematic.png)

---

## 📊 Tabel de adevăr (4 biți, paritate impară)

> Ieșire = 1 (LED aprins) când numărul de biți de `1` este **impar**

| A | B | C | D | Nr. de 1 | Paritate | Output (LED) |
|:-:|:-:|:-:|:-:|:---------:|:--------:|:------------:|
| 0 | 0 | 0 | 0 | 0 | Par | 0 ⚫ |
| 0 | 0 | 0 | 1 | 1 | **Impar** | **1 🟢** |
| 0 | 0 | 1 | 0 | 1 | **Impar** | **1 🟢** |
| 0 | 0 | 1 | 1 | 2 | Par | 0 ⚫ |
| 0 | 1 | 0 | 0 | 1 | **Impar** | **1 🟢** |
| 0 | 1 | 0 | 1 | 2 | Par | 0 ⚫ |
| 0 | 1 | 1 | 0 | 2 | Par | 0 ⚫ |
| 0 | 1 | 1 | 1 | 3 | **Impar** | **1 🟢** |
| 1 | 0 | 0 | 0 | 1 | **Impar** | **1 🟢** |
| 1 | 0 | 0 | 1 | 2 | Par | 0 ⚫ |
| 1 | 0 | 1 | 0 | 2 | Par | 0 ⚫ |
| 1 | 0 | 1 | 1 | 3 | **Impar** | **1 🟢** |
| 1 | 1 | 0 | 0 | 2 | Par | 0 ⚫ |
| 1 | 1 | 0 | 1 | 3 | **Impar** | **1 🟢** |
| 1 | 1 | 1 | 0 | 3 | **Impar** | **1 🟢** |
| 1 | 1 | 1 | 1 | 4 | Par | 0 ⚫ |

---

## 📊 Simulări

### Logisim Evolution

![Logisim Truth Table](./schematic/truth_table.png)

Fișier circuit Logisim: [`schematic/circuit.circ`](./schematic/circuit.circ)

### LTspice

![LTspice Simulation](./simulation/simulation_output.png)

Fișier LTspice: [`simulation/circuit.asc`](./simulation/circuit.asc)

---

## 💡 Cum funcționează

### Principiul detectării de paritate

Poarta **XOR** are proprietatea fundamentală:

```
A XOR B = 1  dacă  A ≠ B
A XOR B = 0  dacă  A = B
```

Prin înlănțuirea a trei XOR-uri:

```
Etapa 1:  P1 = A XOR B          (XOR #1 — TTL)
Etapa 2:  P2 = C XOR D          (XOR #2 — DTL)
Etapa 3:  OUT = P1 XOR P2       (XOR #3 — SN74HC86N)
```

Rezultatul final este `1` exact când numărul total de biți de `1` este impar.

### Construirea XOR din NAND-uri

Un XOR se poate construi din **4 porți NAND**:

```
XOR(A,B) = NAND( NAND(A, NAND(A,B)), NAND(B, NAND(A,B)) )
```

Această formulă a fost implementată fizic în TTL și DTL.

### Diferența între tehnologii

| Caracteristică | TTL | DTL | IC (74HC86N) |
|----------------|-----|-----|--------------|
| Componente active | Tranzistori NPN | Diode + Tranzistor | — (integrat) |
| Viteză | Medie | Lentă | Ridicată |
| Fanout | Mic ⚠️ | Mic | Mare |
| Consum | Ridicat | Mediu | Foarte mic |
| Complexitate montaj | Ridicată | Medie | Foarte mică |

---

## ⚠️ Probleme întâlnite

### Problemă: Fanout mic la TTL

**Simptom:** Ieșirea NAND #1 (TTL) nu putea comanda direct intrarea NAND #2 și NAND #3 — tensiunea de ieșire scădea sub pragul logic `HIGH`.

**Cauză:** Tranzistoarele BC547 în configurație TTL au un fanout mic — curentul disponibil la ieșire era insuficient pentru a menține nivelul logic corect la intrarea IC-ului.

**Soluție:** Am adăugat un **Buffer** (2 × 2N2222 în configurație emitor comun legate în serie) între ieșirea NAND #1 și intrarea NAND #2 și NAND #3, care a restabilit nivelul logic corect.

✅ După buffer, circuitul a funcționat corect.

### Problemă: Tensiune baterie sub sarcină

**Simptom:** Bateria de 9V a scăzut la ~7–8V când a fost conectată la breadboard.

**Cauză:** Rezistența internă a bateriei + rezistența parazită a firelor de breadboard.

**Observație:** Circuitul a funcționat corect și la această tensiune, deoarece TTL și DTL sunt tolerante la variații de alimentare, iar SN74HC86N funcționează în intervalul 2V–6V; doar în simulare a fost folosit 5V.

### Constrângere hardware

> ⚠️ Implementarea a fost limitată la 10 tranzistoare BC547 și 10 tranzistoare 2N2222, ceea ce a influențat direct alegerea topologiei și distribuția componentelor între cele două tehnologii (TTL și DTL).

---

## 📚 Ce am învățat

- **XOR din NAND-uri** — cum se derivă formula și se implementează fizic, atât în TTL cât și în DTL
- **Tehnologie TTL** — tranzistori NPN ca comutatori digitali, praguri logice, curenți de bază/colector
- **Tehnologie DTL** — diodele ca porți AND hardware, completate cu un tranzistor inversor
- **Problema fanout-ului** — de ce TTL are fanout mic și cum se rezolvă cu un buffer
- **Compararea tehnologiilor** — diferențele practice între TTL, DTL și IC pe același circuit
- **Rezistența internă a bateriei** — de ce tensiunea scade sub sarcină și cum afectează circuitul

---

<div align="center">

[← Înapoi](../../README.md) · [⬆ Sus](#p01--detector-de-paritate-impară-pe-4-biți-ttl--dtl--ic)

</div>
