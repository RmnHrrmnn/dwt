# **Musterlösung zur Probeklausur – Aufgabe 1**

## **Aufgabe 1: Verschiedenes (15 Punkte)**

### a) Kardinalitäten und Dimensionshierarchie
1. **Kardinalitäten in der ER-Diagramm-Notation**:
   - `Umsatz` ist mit `Tag` über eine 1:N-Beziehung verknüpft.
   - `Tag` ist mit `Monat` in einer 1:N-Beziehung verknüpft.
   - `Monat` ist mit `Jahr` über eine 1:N-Beziehung verbunden.
   
   **Erklärung:** Diese Hierarchie wird genutzt, um verschiedene Aggregationsebenen zu definieren. Beispielsweise kann man Umsätze pro Jahr, Monat oder Tag analysieren. Kardinalitäten beschreiben die Anzahl der Beziehungen zwischen Entitäten. Eine 1:N-Beziehung bedeutet, dass ein Element einer Entität mehreren Elementen einer anderen Entität zugeordnet ist.
   
   **Zusätzliches Beispiel:**
   - `Kunde` ist mit `Bestellung` über eine 1:N-Beziehung verknüpft, da ein Kunde mehrere Bestellungen aufgeben kann, aber jede Bestellung nur zu einem Kunden gehört.
   
2. **Warum gehört die Entität „Woche“ nicht zur Dimensionshierarchie?**
   - `Tag → Woche` ist eine alternative Hierarchie und nicht direkt aus `Monat` ableitbar.
   - Uneinheitliche Zuordnung: Eine Woche kann Tage aus zwei Monaten enthalten.
   
   **Erklärung:** Da eine Woche sich über zwei Monate erstrecken kann, passt sie nicht eindeutig in eine hierarchische Struktur, die auf Monatsdaten basiert. In Data Warehouses wird eine hierarchische Zeitdimension oft für aggregierte Analysen verwendet.
   
   **Zusätzliches Beispiel:**
   - Ein Finanzbericht nutzt eine Hierarchie von `Tag → Monat → Quartal → Jahr`, um Umsätze auszuwerten. Eine alternative Hierarchie könnte jedoch `Tag → Woche → Jahr` sein.

### b) Vier wesentliche Charakteristika nach Inmon:
1. **Themenorientierung** (*Subject-Oriented*): DWH ist auf Geschäftsbereiche fokussiert.
2. **Integriertheit** (*Integrated*): Vereinheitlichung von Daten aus verschiedenen Quellen.
3. **Zeitbezogenheit** (*Time-Variant*): Historische Speicherung ermöglicht Analysen über Zeiträume.
4. **Nicht-volatil** (*Non-volatile*): Daten werden nach Speicherung nicht verändert.

**Erklärung:** Diese vier Prinzipien definieren die Struktur eines Data Warehouses. Besonders wichtig ist die Zeitbezogenheit, da analytische Systeme häufig Vergleiche über verschiedene Zeiträume durchführen. Data Warehouses speichern nicht nur aktuelle Daten, sondern ermöglichen auch rückblickende Analysen.

**Zusätzliches Beispiel:**
- Eine Versicherung speichert Vertragsdaten im Data Warehouse, um Trends über mehrere Jahre hinweg zu analysieren. Im Gegensatz dazu ändern sich operative Vertragsdaten häufig, sodass ein OLTP-System hierfür besser geeignet ist.

# **Musterlösung zur Probeklausur – Aufgabe 2**

## **Aufgabe 2: SQL-Anfragen (15 Punkte)**

### a) Ausgabe der ROLLUP-Gruppierung

```sql
SELECT REGION.Filiale, PRODUKT.Artikel, SUM(VERKAUF.UMSATZ)
FROM PRODUKT, REGION, VERKAUF
WHERE PRODUKT.PNr = VERKAUF.PNr
AND REGION.RNr = VERKAUF.RNr
GROUP BY ROLLUP (REGION.Filiale, PRODUKT.Artikel);
```

**Erklärung:**
- `ROLLUP` erstellt eine hierarchische Gruppierung mit aggregierten Summen für jede Filiale, jede Produktkombination sowie für die gesamte Datenmenge.
- Die Hierarchie folgt der Reihenfolge der Gruppierung: `Filiale -> Produkt -> Gesamtsumme`.

**Erwartete Ausgabe:**

| Filiale  | Artikel     | Umsatz |
|----------|------------|--------|
| West     | Käse       | 50     |
| West     | Wurst      | 120    |
| West     | NULL       | 170    |
| Ost      | Käse       | 110    |
| Ost      | Wurst      | 180    |
| Ost      | NULL       | 290    |
| Süd      | Bonbon     | 50     |
| Süd      | NULL       | 50     |
| NULL     | NULL       | 510    |

---

### b) Ausgabe der CUBE-Gruppierung

```sql
SELECT REGION.Filiale, PRODUKT.Artikel, SUM(VERKAUF.UMSATZ)
FROM PRODUKT, REGION, VERKAUF
WHERE PRODUKT.PNr = VERKAUF.PNr
AND REGION.RNr = VERKAUF.RNr
GROUP BY CUBE (REGION.Filiale, PRODUKT.Artikel);
```

**Erklärung:**
- `CUBE` berechnet alle möglichen Kombinationen von Gruppierungen.
- Dadurch werden nicht nur Filial- und Produkt-spezifische Summen berechnet, sondern auch aggregierte Werte für alle Kombinationen.

**Erwartete Ausgabe:**

| Filiale  | Artikel     | Umsatz |
|----------|------------|--------|
| West     | Käse       | 50     |
| West     | Wurst      | 120    |
| West     | NULL       | 170    |
| Ost      | Käse       | 110    |
| Ost      | Wurst      | 180    |
| Ost      | NULL       | 290    |
| NULL     | Käse       | 160    |
| NULL     | Wurst      | 300    |
| NULL     | NULL       | 510    |
| Süd      | Bonbon     | 50     |
| Süd      | NULL       | 50     |

---

### c) Ausgabe der GROUPING SETS-Gruppierung

```sql
SELECT REGION.Filiale, PRODUKT.Artikel, SUM(VERKAUF.UMSATZ)
FROM PRODUKT, REGION, VERKAUF
WHERE PRODUKT.PNr = VERKAUF.PNr
AND REGION.RNr = VERKAUF.RNr
GROUP BY GROUPING SETS ((REGION.Filiale, PRODUKT.Artikel), (REGION.Filiale), (PRODUKT.Artikel), ());
```

**Erklärung:**
- `GROUPING SETS` ermöglicht eine flexible Definition von Aggregationsebenen, indem nur bestimmte Gruppierungen berücksichtigt werden.
- Diese Abfrage erstellt separate Aggregationen für jede Kombination von `Filiale` und `Artikel`, `Filiale` allein, `Artikel` allein und eine Gesamtsumme.

**Erwartete Ausgabe:**

| Filiale  | Artikel     | Umsatz |
|----------|------------|--------|
| West     | Käse       | 50     |
| West     | Wurst      | 120    |
| West     | NULL       | 170    |
| Ost      | Käse       | 110    |
| Ost      | Wurst      | 180    |
| Ost      | NULL       | 290    |
| Süd      | Bonbon     | 50     |
| NULL     | Käse       | 160    |
| NULL     | Wurst      | 300    |
| NULL     | Bonbon     | 50     |
| NULL     | NULL       | 510    |

---

# **Musterlösung zur Probeklausur – Aufgabe 3**

## **Aufgabe 3: SQL-Anfragen (15 Punkte)**

### a) Ergänzen Sie den folgenden Befehl so, dass das Ergebnis identisch mit dem aus Aufgabe 2a ist.

```sql
SELECT REGION.Filiale, PRODUKT.Artikel, SUM(VERKAUF.UMSATZ)
FROM PRODUKT, REGION, VERKAUF
WHERE PRODUKT.PNr = VERKAUF.PNr
AND REGION.RNr = VERKAUF.RNr
GROUP BY GROUPING SETS ((REGION.Filiale, PRODUKT.Artikel), (REGION.Filiale), (PRODUKT.Artikel), ());
```

**Erklärung:**
- `GROUPING SETS` erlaubt es, gezielt Aggregationsmengen zu definieren.
- Diese Definition repliziert das Verhalten von `ROLLUP`, indem sowohl Kombinationen von `Filiale` und `Artikel` als auch ihre Einzelaggregationen sowie die Gesamtsumme berechnet werden.

**Erwartete Ausgabe:**

| Filiale  | Artikel     | Umsatz |
|----------|------------|--------|
| West     | Käse       | 50     |
| West     | Wurst      | 120    |
| West     | NULL       | 170    |
| Ost      | Käse       | 110    |
| Ost      | Wurst      | 180    |
| Ost      | NULL       | 290    |
| Süd      | Bonbon     | 50     |
| NULL     | Käse       | 160    |
| NULL     | Wurst      | 300    |
| NULL     | Bonbon     | 50     |
| NULL     | NULL       | 510    |

---

### b) Welche Ausgabe bewirkt der folgende SQL-Befehl?

```sql
SELECT ZEIT.MONAT, ZEIT.JAHR, VERKAUF.UMSATZ AS UMSATZ,
       SUM(VERKAUF.UMSATZ) OVER (ORDER BY ZEIT.MONAT) AS SumGes
FROM ZEIT, VERKAUF
WHERE ZEIT.ZNr = VERKAUF.ZNr
AND ZEIT.JAHR = 2010;
```

**Erklärung:**
- Diese Abfrage berechnet eine laufende Summe (`SUMGes`) der Umsätze innerhalb des Jahres 2010.
- `ORDER BY ZEIT.MONAT` stellt sicher, dass die Werte nach Monat aufsteigend kumuliert werden.

**Erwartete Ausgabe:**

| Monat | Jahr | Umsatz | SumGes |
|-------|------|--------|--------|
| 1     | 2010 | 50     | 50     |
| 1     | 2010 | 120    | 170    |
| 1     | 2010 | 80     | 250    |
| 1     | 2010 | 50     | 300    |
| 1     | 2010 | 110    | 410    |
| 1     | 2010 | 180    | 590    |
| 5     | 2010 | 40     | 630    |
| 5     | 2010 | 150    | 780    |

---

### c) Welche Ausgabe bewirkt der folgende SQL-Befehl?

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

**Erklärung:**
- Diese Abfrage berechnet den Durchschnitt der Umsätze für ein Fenster von drei Tagen (`PlusMinus1`).
- `ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING` berücksichtigt den vorherigen, aktuellen und nächsten Tag.

**Erwartete Ausgabe:**

| Tag | Monat | Jahr | Umsatz | PlusMinus1 |
|-----|-------|------|--------|------------|
| 12  | 1     | 2010 | 50     | 83.33      |
| 12  | 1     | 2010 | 120    | 93.33      |
| 12  | 1     | 2010 | 80     | 83.33      |
| 12  | 1     | 2010 | 50     | 83.33      |
| 12  | 1     | 2010 | 110    | 113.33     |

---

# **Musterlösung zur Probeklausur – Aufgabe 4**

## **Aufgabe 4: Datenmodellierung (15 Punkte)**

### a) Snowflake-Schema für Produkt

Die folgende Tabelle zeigt die Normalisierung der **Produktdimension** im **Snowflake-Schema**:

#### **Datenmodell**

**Produkt-Tabelle:**
| PNr | Artikel    | KategorieID |
|-----|-----------|-------------|
| 1   | Käse      | 10          |
| 2   | Wurst     | 10          |
| 3   | Bonbon    | 11          |
| 4   | Schokolade| 11          |
| 5   | Gardine   | 12          |
| 6   | Kissen    | 12          |

**Kategorie-Tabelle:**
| KategorieID | GruppenID | Kategoriename |
|------------|----------|--------------|
| 10         | 100      | Food         |
| 11         | 100      | Süßes        |
| 12         | 101      | NonFood      |

**Gruppen-Tabelle:**
| GruppenID | Gruppenname |
|-----------|------------|
| 100       | Lebensmittel |
| 101       | Haushaltswaren |

**Erklärung:**
- Die **Produkt-Tabelle** enthält eine `KategorieID`, anstatt den vollständigen Namen der Kategorie zu speichern.
- Die **Kategorie-Tabelle** enthält eine `GruppenID`, um die Gruppenzugehörigkeit der Kategorien darzustellen.
- Die **Gruppen-Tabelle** sorgt für eine weitere Normalisierung.
- Dies reduziert **Redundanz** und spart Speicherplatz.

---

### b) Vorteile des Snowflake-Schemas

1. **Reduzierung von Datenredundanzen**: Da Informationen in separaten Tabellen gespeichert sind, gibt es keine doppelten Einträge.
2. **Verbesserte Datenintegrität**: Änderungen an einer Kategorie wirken sich automatisch auf alle zugehörigen Produkte aus.
3. **Speicherplatzersparnis**: Wiederholte Werte (z. B. Kategorienamen) werden ausgelagert.

**Zusätzliches Beispiel:**
Ein `Land`-Attribut könnte in einer separaten **Land-Tabelle** gespeichert werden, anstatt bei jeder `Filiale` wiederholt zu werden.

---

### c) Nachteile des Snowflake-Schemas

1. **Erhöhte Anzahl an Joins**: Abfragen erfordern zusätzliche Joins, was die **Performance** verschlechtern kann.
2. **Komplexere Abfragen**: Benutzer müssen mehr Tabellen verbinden, was die **SQL-Abfrage komplizierter** macht.
3. **Längere Abfragezeiten**: Da die Daten auf mehrere Tabellen verteilt sind, kann die Verarbeitung langsamer sein.

**Zusätzliches Beispiel:**
Im Gegensatz zum **Star-Schema**, bei dem alle Informationen in einer **einzigen Dimensionstabelle** gespeichert sind, benötigt das Snowflake-Schema mehrere Joins für dieselbe Abfrage.

---

# **Musterlösung zur Probeklausur – Aufgabe 5**

## Siehe kdB-Aufgaben.pdf

# **Musterlösung zur Probeklausur – Aufgabe 6**

## **Aufgabe 6: Bitmap-Index (15 Punkte)**

### a) Anzahl der benötigten Spalten für den 2-Komponenten-Bitmap-Index

Ein **Mehrkomponenten-Bitmap-Index** speichert Werte in einer komprimierten Bitmap-Darstellung, indem er Werte in mehrere Komponenten zerlegt. Angenommen, wir haben **36 Teams (1 bis 36)**, die mit einem **2-Komponenten-Bitmap-Index** kodiert werden sollen, müssen wir folgende Überlegungen anstellen:

- Eine Zahl **x** kann durch zwei Komponenten dargestellt werden: **x = n * y + z**.
- Um **36 Teams** zu kodieren, benötigen wir eine geeignete Wahl für `n` und `m`.
- Die Anzahl der Spalten entspricht der Summe der für `y` und `z` notwendigen Bitmap-Spalten.

Daher müssen die Komponenten so gewählt werden, dass **die Anzahl der Spalten minimal bleibt**.

**Antwort:** Es werden **12 Spalten** benötigt, um den 2-Komponenten-Bitmap-Index aufzubauen.

---

### b) Darstellung des 2-Komponenten-Bitmap-Index

Ein **2-Komponenten-Bitmap-Index** zerlegt Werte in zwei Teilwerte `y` und `z`.

| TeamNr  | B1 | B2 | B3 | B4 | B5 | B6 | B7 | B8 | B9 | B10 | B11 | B12 |
|---------|----|----|----|----|----|----|----|----|----|-----|-----|-----|
| 1       |  0 |  0 |  0 |  0 |  0 |  1 |  0 |  0 |  0 |  0  |  0  |  0  |
| 2       |  0 |  0 |  0 |  0 |  1 |  0 |  0 |  0 |  0 |  0  |  0  |  0  |
| ...     | ...| ...| ...| ...| ...| ...| ...| ...| ...| ... | ... | ... |
| 36      |  1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0  |  0  |  0  |

**Erklärung:**
- Jede Spalte repräsentiert eine Bitmaske für eine Komponente.
- Durch logische Verknüpfungen (`AND`, `OR`) können effiziente Suchoperationen durchgeführt werden.

---

### c) Eintragung der Bitmap-Werte für bestimmte Teams

Für die Teams **5, 6, 7, 12, 15, 22, 24, 36**, tragen wir die **korrekten Bitmuster** in die Tabelle ein:

| TeamNr  | B1 | B2 | B3 | B4 | B5 | B6 | B7 | B8 | B9 | B10 | B11 | B12 |
|---------|----|----|----|----|----|----|----|----|----|-----|-----|-----|
| 5       |  0 |  0 |  1 |  0 |  0 |  0 |  0 |  1 |  0 |  0  |  0  |  0  |
| 6       |  0 |  0 |  1 |  0 |  0 |  0 |  0 |  0 |  1 |  0  |  0  |  0  |
| 7       |  0 |  0 |  1 |  0 |  0 |  0 |  0 |  0 |  0 |  1  |  0  |  0  |
| 12      |  0 |  1 |  0 |  0 |  0 |  0 |  0 |  1 |  0 |  0  |  0  |  0  |
| 15      |  0 |  1 |  0 |  0 |  0 |  0 |  0 |  0 |  1 |  0  |  0  |  0  |
| 22      |  1 |  0 |  0 |  0 |  0 |  0 |  0 |  1 |  0 |  0  |  0  |  0  |
| 24      |  1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  1 |  0  |  0  |  0  |
| 36      |  1 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0  |  0  |  0  |

---

### d) Anwendung des bereichskodierten Bitmap-Index

Ein **bereichskodierter Bitmap-Index** ist besonders effizient für **Bereichsabfragen**, da er nur wenige Bit-Vektoren für die Speicherung benötigt.

#### **Vorteile des bereichskodierten Bitmap-Index:**
- Erfordert nur **2 Bitmap-Vektoren** zur Beantwortung einer Bereichsanfrage.
- Besonders effizient für **sequentielle Bereichsabfragen**, z. B. **„Finde alle Teams zwischen 5 und 15“**.
- **Geringer Speicherverbrauch**, da weniger Bitmap-Vektoren benötigt werden als bei traditionellen Bitmap-Indizes.

**Beispiel:**
- **Abfrage:** `WHERE TeamNr BETWEEN 5 AND 15`
- **Ergebnis:** Kombination von `B1` und `B5` mit einem logischen `AND`.

---
