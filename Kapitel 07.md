# **7. Indexstrukturen in Data Warehouses**

## **1. Einführung in Indexstrukturen**
Indexstrukturen sind essenziell für die Optimierung der Abfragegeschwindigkeit in Data Warehouses. Da Faktentabellen sehr groß sind, wäre ein vollständiger Tabellen-Scan ineffizient. Indexe minimieren die Anzahl der zu lesenden Datenseiten und verbessern so die Performance analytischer Abfragen.

### **Wichtige Eigenschaften von Indexen**
- **Schneller Datenzugriff**: Reduziert die Anzahl der zu durchsuchenden Datensätze.
- **Unterstützung von Bereichsabfragen**: Erlaubt effiziente Selektionen.
- **Vermeidung redundanter Berechnungen**: Erhöht die Performance von wiederholten Abfragen.

## **2. Klassifikation von Indexstrukturen**
Indexstrukturen lassen sich nach verschiedenen Kriterien klassifizieren:
- **Eindimensional vs. Mehrdimensional**: Betrifft die Anzahl der indizierten Attribute.
- **Clustering vs. Non-Clustering**: Ob die physische Datenanordnung mit dem Index übereinstimmt.
- **Dynamische vs. Statische Indexe**: Ob der Index sich automatisch mit den Daten ändert oder manuell aktualisiert werden muss.

## **3. Baum-basierte Indexstrukturen**
### **B-Baum-Index**
- Balancierte Baumstruktur, bei der alle Blattebene auf der gleichen Höhe liegen.
- Gut geeignet für **Punkt- und Bereichsabfragen**.
- Kann bei geringen Kardinalitäten ineffizient sein.

**Beispiel:**
```sql
CREATE INDEX sales_idx ON sales (datum);
```

### **B+-Baum-Index**
- Eine Variante des B-Baums mit verketteten Blättern für schnellere Bereichsabfragen.
- Besonders nützlich in Data Warehouses für **bereichsorientierte SQL-Abfragen**.

## **4. Bitmap-Indexstrukturen**
Bitmap-Indexe sind besonders effizient für **Attribute mit niedriger Kardinalität** (wenige verschiedene Werte, z. B. Geschlecht, Region).

### **Standard-Bitmap-Index**
- Speichert für jede mögliche Attributausprägung eine Bit-Vektor-Liste.
- Vorteile: **Sehr speicherplatzsparend** und effizient bei **OLAP-Abfragen**.
- Nachteile: Hohe **Kosten für Aktualisierungen**, daher besser für **selten veränderte Daten**.

**Beispiel:**
```sql
CREATE BITMAP INDEX region_idx ON sales(region);
```

### **Mehrkomponenten-Bitmap-Index**
- Optimierte Variante, die Speicherplatz spart, indem Werte in zwei Komponenten zerlegt werden.
- Beispiel: PLZ-Index kann in `erste 2 Ziffern` und `letzte 3 Ziffern` aufgeteilt werden.
- Reduziert den Speicherverbrauch erheblich.

### **Bereichskodierte Bitmap-Indexe**
- Nutzen eine **komprimierte Bitmap-Darstellung** für Bereichsabfragen.
- Ermöglichen schnelle **Vergleiche mit wenigen Bit-Operationen**.

**Beispiel für eine Bereichsanfrage:**
```sql
SELECT * FROM sales WHERE region BETWEEN 'A' AND 'C';
```

## **5. Mehrdimensionale Indexstrukturen**
Data Warehouses benötigen oft **mehrdimensionale Abfragen**. Dafür eignen sich spezielle Strukturen:

### **R-Baum**
- Baumstruktur zur Speicherung **mehrdimensionaler Datenbereiche**.
- Besonders effizient für **geografische Daten und räumliche Anfragen**.

### **Grid-Files**
- Mehrdimensionales **Gitter-basiertes Indexierungsverfahren**.
- Ermöglicht schnelles **Partitionieren von Daten entlang mehrerer Achsen**.

### **kd-Baum (k-dimensionaler Baum)**
- Baumstruktur für **mehrdimensionale Suchanfragen**.
- Besonders effizient für **OLAP-Operationen mit mehreren Filtern**.

## **6. Optimierung durch Materialisierte Sichten und Index-Sichten**
### **Materialisierte Sichten**
- Vorgespeicherte Abfrageergebnisse zur Reduzierung der Berechnungszeit.
- Besonders sinnvoll für komplexe Aggregationen in **Data Marts**.

**Beispiel:**
```sql
CREATE MATERIALIZED VIEW sales_summary AS
SELECT region, SUM(amount) FROM sales GROUP BY region;
```

### **Indexierte Sichten**
- Indizierte Datenbank-Sichten in **SQL Server**.
- Automatische Aktualisierung bei **Datenänderungen in der Basisrelation**.

## **7. Fazit**
Indexstrukturen spielen eine zentrale Rolle bei der **Optimierung von Abfragen in Data Warehouses**. Während **B-Bäume und B+-Bäume** für **bereichsbasierte Abfragen** geeignet sind, bieten **Bitmap-Indexe** signifikante Vorteile bei **mehrdimensionalen Filtern**. Mehrdimensionale Indexe wie **R-Bäume oder kd-Bäume** verbessern die Verarbeitung komplexer Analysen erheblich. Die Wahl der richtigen Indexstrategie hängt maßgeblich vom **Abfrageprofil und den Speicheranforderungen** ab.

