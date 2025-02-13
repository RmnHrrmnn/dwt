# **Musterlösung zur Probeklausur – Data Warehouse Technologien**

## **Aufgabe 1: Verschiedenes (15 Punkte)**

### a) Kardinalitäten und Dimensionshierarchie
1. **Kardinalitäten in der ER-Diagramm-Notation**:
   - `Umsatz` ist mit `Tag` über eine 1:N-Beziehung verknüpft.
   - `Tag` ist mit `Monat` in einer 1:N-Beziehung verknüpft.
   - `Monat` ist mit `Jahr` über eine 1:N-Beziehung verbunden.

2. **Warum gehört die Entität „Woche“ nicht zur Dimensionshierarchie?**
   - `Tag → Woche` ist eine alternative Hierarchie und nicht direkt aus `Monat` ableitbar.
   - Uneinheitliche Zuordnung: Eine Woche kann Tage aus zwei Monaten enthalten.

### b) Vier wesentliche Charakteristika nach Inmon:
1. **Themenorientierung** (*Subject-Oriented*): DWH ist auf Geschäftsbereiche fokussiert.
2. **Integriertheit** (*Integrated*): Vereinheitlichung von Daten aus verschiedenen Quellen.
3. **Zeitbezogenheit** (*Time-Variant*): Historische Speicherung ermöglicht Analysen über Zeiträume.
4. **Nicht-volatil** (*Non-volatile*): Daten werden nach Speicherung nicht verändert.

---

## **Aufgabe 2: SQL-Anfragen (15 Punkte)**

### a) Ausgabe der ROLLUP-Gruppierung
```sql
SELECT REGION.Filiale, PRODUKT.Artikel, SUM(VERKAUF.UMSATZ)
FROM PRODUKT, REGION, VERKAUF
WHERE PRODUKT.PNr = VERKAUF.PNr
AND REGION.RNr = VERKAUF.RNr
GROUP BY ROLLUP (REGION.Filiale, PRODUKT.Artikel);
```
**Ergebnis:**
- Einzelne Kombinationen von Filiale und Artikel.
- Teilaggregationen für Filialen.
- Gesamtsumme über alle Filialen und Artikel.

### b) Ausgabe der CUBE-Gruppierung
```sql
SELECT REGION.Filiale, PRODUKT.Artikel, SUM(VERKAUF.UMSATZ)
FROM PRODUKT, REGION, VERKAUF
WHERE PRODUKT.PNr = VERKAUF.PNr
AND REGION.RNr = VERKAUF.RNr
GROUP BY CUBE (REGION.Filiale, PRODUKT.Artikel);
```
**Ergebnis:**
- Alle Aggregationen von `ROLLUP`.
- Zusätzlich: Aggregationen für Artikel unabhängig von der Filiale und umgekehrt.

### c) Ausgabe der GROUPING SETS-Gruppierung
```sql
SELECT REGION.Filiale, PRODUKT.Artikel, SUM(VERKAUF.UMSATZ)
FROM PRODUKT, REGION, VERKAUF
WHERE PRODUKT.PNr = VERKAUF.PNr
AND REGION.RNr = VERKAUF.RNr
GROUP BY GROUPING SETS ((REGION.Filiale), (PRODUKT.Artikel));
```
**Ergebnis:**
- Separate Summen nach Filiale und Artikel, jedoch keine vollständige Kombination.

---

## **Aufgabe 3: Analytische SQL-Funktionen (15 Punkte)**

### a) Ergänzung der SQL-Abfrage
```sql
SELECT REGION.Filiale, PRODUKT.Artikel, SUM(VERKAUF.UMSATZ)
FROM PRODUKT, REGION, VERKAUF
WHERE PRODUKT.PNr = VERKAUF.PNr
AND REGION.RNr = VERKAUF.RNr
GROUP BY GROUPING SETS ((REGION.Filiale, PRODUKT.Artikel), (REGION.Filiale), (PRODUKT.Artikel), ());
```
### b) Window Function – Summierte Umsätze
```sql
SELECT ZEIT.MONAT, ZEIT.JAHR, VERKAUF.UMSATZ,
       SUM(VERKAUF.UMSATZ) OVER (ORDER BY ZEIT.MONAT) AS SumGes
FROM ZEIT, VERKAUF
WHERE ZEIT.ZNr = VERKAUF.ZNr AND ZEIT.JAHR = 2010;
```
**Ergebnis:**
- Laufende Summe der Umsätze über alle Monate von 2010.

### c) Window Function – Gleitender Durchschnitt
```sql
SELECT ZEIT.TAG, ZEIT.MONAT, ZEIT.JAHR, VERKAUF.UMSATZ,
       AVG(VERKAUF.UMSATZ) OVER (ORDER BY ZEIT.JAHR, ZEIT.MONAT, ZEIT.TAG
       ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS PlusMinus1
FROM ZEIT, VERKAUF
WHERE ZEIT.ZNr = VERKAUF.ZNr
AND ZEIT.JAHR = 2010
AND ZEIT.MONAT = 1
AND ZEIT.TAG = 12;
```
**Ergebnis:**
- Durchschnittswert des Umsatzes basierend auf dem aktuellen Tag, dem Vortag und dem Folgetag.

---

## **Aufgabe 4: Datenmodellierung (15 Punkte)**

### a) Snowflake-Schema für Produkt
1. **Produkt-Tabelle aufteilen in:**
   - **Produkt:** `PNr, Artikel, KategorieID`
   - **Kategorie:** `KategorieID, GruppenID, Kategoriename`
   - **Gruppe:** `GruppenID, Gruppenname`

### b) Vorteil des Snowflake-Schemas
- **Reduzierung von Redundanzen:** Geteilte Attribute werden ausgelagert.

### c) Nachteil des Snowflake-Schemas
- **Komplexere Abfragen:** Zusätzliche Joins erhöhen die Verarbeitungszeit.

---

## **Aufgabe 5: kd-Bäume (15 Punkte)**

### a) Konstruktionsregel für einen kd-Baum
1. **Sortiere Punkte nach X-Koordinate.**
2. **Teile in zwei Gruppen, links/rechts.**
3. **Wechsel zur Y-Koordinate für nächste Ebene.**
4. **Rekursiv wiederholen.**

### b) Aufbau des kd-Baums
```
            (33,40) Frankfurt
           /       \
  (25,80) Köln      (45,45) Mainz
   /         \        /         \
(10,75) Hamburg (20,65) München (35,35) Hannover (55,80) Bielefeld
```
---

## **Aufgabe 6: Bitmap-Index (15 Punkte)**

### a) Anzahl der Spalten im Mehrkomponenten-Bitmap-Index
- Bei 36 Teams mit **2 Komponenten**: 6 Spalten (z. B. 3 für jede Komponente).

### b) Erstellung des Bitmap-Index für Teams
| TeamNr | B1 | B2 | B3 | B4 | B5 | B6 |
|--------|----|----|----|----|----|----|
| 5      | 0  | 1  | 0  | 1  | 0  | 0  |
| 6      | 0  | 1  | 0  | 1  | 0  | 1  |
| …      | …  | …  | …  | …  | …  | …  |

### c) Vorteile des bereichskodierten Bitmap-Index
- **Effiziente Bereichsanfragen** (z. B. alle Teams zwischen 5 und 15).
- **Reduzierung der benötigten Bitmap-Vektoren.**

---
