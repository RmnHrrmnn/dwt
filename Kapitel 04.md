# **4. SQL-Anfragen in Data Warehouses**

## **1. Einführung in SQL für Data Warehouses**
SQL ist die zentrale Abfragesprache für relationale Datenbanken und wird im Data Warehousing zur effizienten Analyse und Aggregation großer Datenmengen eingesetzt. Abfragen in Data Warehouses unterscheiden sich von traditionellen OLTP-Anwendungen, da sie komplexe Analysen und Aggregationen über große Datensätze erfordern.

### **Hauptunterschiede zwischen OLTP- und OLAP-Abfragen:**
- **OLTP (Online Transaction Processing):** Viele kleine, transaktionsbasierte Abfragen mit schnellem Zugriff.
- **OLAP (Online Analytical Processing):** Wenige komplexe, analytische Abfragen mit hoher Aggregationstiefe.

## **2. Aggregationsfunktionen und Gruppierungsoperatoren**
In Data Warehouses sind Aggregationsfunktionen essenziell für die Berechnung von Kennzahlen. SQL bietet verschiedene Gruppierungsmechanismen für mehrdimensionale Analysen:

### **GROUPING SETS**
- Definiert mehrere Aggregationsebenen innerhalb einer Abfrage.
- Vermeidet die Notwendigkeit mehrerer UNION-Abfragen.
- Beispiel:
  ```sql
  SELECT r.land, p.artikel, to_char(z.jahr,'9999'), SUM(v.umsatz)
  FROM verkauf v, zeit z, region r, produkt p
  WHERE v.znr = z.znr AND v.rnr = r.rnr AND v.pnr = p.pnr
  GROUP BY GROUPING SETS ((r.land, p.artikel, z.jahr), (r.land, p.artikel), (r.land), ())
  ORDER BY r.land, p.artikel, to_char(z.jahr,'9999');
  ```

### **ROLLUP**
- Hierarchische Aggregation entlang einer Dimension.
- Beispiel:
  ```sql
  SELECT r.land, p.artikel, to_char(z.jahr,'9999'), SUM(v.umsatz)
  FROM verkauf v, zeit z, region r, produkt p
  WHERE v.znr = z.znr AND v.rnr = r.rnr AND v.pnr = p.pnr
  GROUP BY ROLLUP (r.land, p.artikel, z.jahr);
  ```

### **CUBE**
- Generiert alle möglichen Gruppierungskombinationen für die angegebenen Dimensionen.
- Beispiel:
  ```sql
  SELECT r.land, p.artikel, to_char(z.jahr,'9999'), SUM(v.umsatz)
  FROM verkauf v, zeit z, region r, produkt p
  WHERE v.znr = z.znr AND v.rnr = r.rnr AND v.pnr = p.pnr
  GROUP BY CUBE (r.land, p.artikel, z.jahr);
  ```

## **3. Analytische Funktionen (OLAP-Funktionen)**
SQL bietet erweiterte Funktionen zur Analyse von Daten über Zeiträume hinweg oder zur Berechnung von Anteilen.

### **PARTITION BY**
Ermöglicht Berechnungen innerhalb bestimmter Gruppen, ohne die Aggregation auf die gesamte Tabelle anzuwenden.
- Beispiel:
  ```sql
  SELECT z.tag, z.monat, z.jahr, v.umsatz,
         SUM(v.umsatz) OVER (PARTITION BY z.jahr, z.monat) AS m_umsatz
  FROM verkauf v, zeit z
  WHERE v.znr = z.znr;
  ```

### **Gleitende Durchschnitte (Moving Averages)**
Erlaubt die Berechnung von Durchschnittswerten über einen gleitenden Zeitraum.
- Beispiel:
  ```sql
  SELECT z.tag, z.monat, z.jahr, v.umsatz,
         AVG(v.umsatz) OVER (ORDER BY z.jahr, z.monat, z.tag ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS avg_umsatz
  FROM verkauf v, zeit z
  WHERE v.znr = z.znr;
  ```

### **Ranking-Funktionen**
- **RANK():** Vergibt Rangnummern mit möglichen Lücken bei gleichen Werten.
- **DENSE_RANK():** Vergibt fortlaufende Rangnummern ohne Lücken.
- **ROW_NUMBER():** Weist jeder Zeile eine eindeutige Nummer zu.
- Beispiel:
  ```sql
  SELECT r.land, p.artikel, v.umsatz,
         RANK() OVER (PARTITION BY r.land ORDER BY v.umsatz DESC) AS rank_umsatz
  FROM verkauf v, region r, produkt p
  WHERE v.rnr = r.rnr AND v.pnr = p.pnr;
  ```

## **4. Optimierungstechniken für SQL-Abfragen**
Um eine hohe Performance bei großen Datenmengen zu gewährleisten, gibt es verschiedene Optimierungsansätze:

### **Materialisierte Sichten**
- Speicherung voraggregierter Abfrageergebnisse zur Vermeidung wiederholter Berechnungen.
- Beispiel:
  ```sql
  CREATE MATERIALIZED VIEW matview AS
  SELECT r.land, p.artikel, SUM(v.umsatz) AS gesamtumsatz
  FROM verkauf v, region r, produkt p
  WHERE v.rnr = r.rnr AND v.pnr = p.pnr
  GROUP BY r.land, p.artikel;
  ```

### **Indexierung**
- **B+-Bäume:** Effiziente Indexierung für Suchanfragen.
- **Bitmap-Indexe:** Geeignet für Attribute mit geringer Kardinalität (z. B. Geschlecht, Region).
- Beispiel:
  ```sql
  CREATE INDEX idx_umsatz ON verkauf (rnr, pnr);
  ```

### **Partitionierung**
- Aufteilung großer Tabellen in kleinere, leichter zu verarbeitende Abschnitte.
- Beispiel:
  ```sql
  CREATE TABLE verkauf (
      rnr NUMBER,
      pnr NUMBER,
      umsatz NUMBER,
      jahr NUMBER
  ) PARTITION BY RANGE (jahr) (
      PARTITION p1 VALUES LESS THAN (2020),
      PARTITION p2 VALUES LESS THAN (2025)
  );
  ```

## **5. Fazit**
SQL-Abfragen in Data Warehouses sind komplexer als in traditionellen Datenbanksystemen. Durch erweiterte Gruppierungsoperatoren, analytische Funktionen und Optimierungstechniken lassen sich große Datenmengen effizient verarbeiten und auswerten. Der gezielte Einsatz von Indexen, Partitionierung und materialisierten Sichten trägt wesentlich zur Performance-Steigerung bei.

