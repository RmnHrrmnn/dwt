# **11. Spaltenorientierte Datenbanksysteme (Column Stores) in Data Warehouses**

## **1. Einführung in spaltenorientierte DBMS**
Spaltenorientierte Datenbanksysteme (Column Stores) sind eine Weiterentwicklung klassischer, zeilenorientierter Datenbanken (Row Stores). Sie wurden speziell für analytische Workloads in **Data Warehouses und OLAP-Anwendungen** entwickelt und bieten erhebliche Performancevorteile bei der Verarbeitung großer Datenmengen.

### **Motivation für Column Stores**
- **Optimierung für OLAP-Abfragen**: Aggregationen wie SUM(), AVG(), COUNT() benötigen oft nur bestimmte Spalten.
- **Effiziente Speichernutzung**: Durch Spaltenkompression wird weniger Speicherplatz benötigt.
- **Reduzierung des I/O-Aufwands**: Es werden nur die relevanten Spalten gelesen, nicht ganze Zeilen.

## **2. Unterschiede zwischen Row Stores und Column Stores**
### **Row Stores (Zeilenorientierte Speicherung)**
- Speichert Daten **zeilenweise** in Blöcken.
- Effizient für **OLTP (Online Transaction Processing)**, da alle Attribute eines Tupels zusammen abgelegt werden.
- Nachteile: Hoher I/O-Aufwand bei analytischen Anfragen, da unnötige Spalten geladen werden müssen.

### **Column Stores (Spaltenorientierte Speicherung)**
- Speichert Daten **spaltenweise** in Blöcken.
- Geeignet für **OLAP (Online Analytical Processing)**, da nur benötigte Spalten gelesen werden.
- Vorteile:
  - Bessere **Datenkompression**.
  - **Schnellere Abfragen** durch parallele Verarbeitung.
  - Effizientere Nutzung des Hauptspeichers.

## **3. Architektur von Column Stores**
### **Wichtige Eigenschaften**
- **Spaltenorientierte Speicherung**: Daten einer Spalte sind zusammen gespeichert und nicht über viele Seiten verteilt.
- **Vertikale Partitionierung**: Jede Spalte ist eine eigene physische Einheit.
- **Tupelrekonstruktion**: Bei komplexen Anfragen müssen Daten oft wieder zu vollständigen Tupeln zusammengesetzt werden.

### **Beispiel für Speicherung**
**Row Store (Zeilenweise Speicherung)**
| ID | Name | Stadt  | Umsatz  |
|----|------|--------|---------|
| 1  | Max  | Berlin | 5000    |
| 2  | Anna | München | 7000   |

**Column Store (Spaltenweise Speicherung)**
- ID: `[1, 2]`
- Name: `[Max, Anna]`
- Stadt: `[Berlin, München]`
- Umsatz: `[5000, 7000]`

## **4. Kompressionstechniken in Column Stores**
Column Stores nutzen verschiedene Kompressionstechniken zur Optimierung der Speichernutzung und Abfragegeschwindigkeit:

### **Lauflängenkodierung (Run-Length Encoding - RLE)**
- Wiederholte Werte werden als **(Wert, Häufigkeit)** gespeichert.
- Beispiel:
  - Unkomprimiert: `[A, A, A, B, B, C, C, C, C]`
  - RLE-kodiert: `[(A,3), (B,2), (C,4)]`

### **Wörterbuchkodierung**
- Ein Wörterbuch speichert eindeutige Werte und ersetzt Originalwerte durch Indexwerte.
- Beispiel:
  - Unkomprimiert: `[Berlin, München, Berlin, Hamburg]`
  - Wörterbuch: `{1: Berlin, 2: München, 3: Hamburg}`
  - Kodiert: `[1, 2, 1, 3]`

### **Bitvektor-Encoding**
- Ideal für Attribute mit wenigen unterschiedlichen Werten (geringe Kardinalität).
- Beispiel für `Kategorie`-Spalte mit Werten `[A, B, C]`:
  - A: `100`
  - B: `010`
  - C: `001`

## **5. Vorteile und Herausforderungen von Column Stores**
### **Vorteile**
- **Schnellere Abfragen**, da nur relevante Spalten geladen werden.
- **Höhere Kompressionsraten** reduzieren Speicherbedarf und I/O-Last.
- **Bessere Skalierbarkeit** für große Data Warehouses.

### **Herausforderungen**
- **Tupelrekonstruktion ist teuer**: Müssen mehrere Spalten zusammengeführt werden, kann dies die Abfragegeschwindigkeit reduzieren.
- **Schlechte Performance bei OLTP-Workloads**: Inserts und Updates sind ineffizient, da alle betroffenen Spalten aktualisiert werden müssen.

## **6. Hybride Ansätze und moderne Entwicklungen**
Einige moderne Systeme kombinieren row- und column-basierte Speicherstrategien:

### **Hybride Datenbanken (z. B. SAP HANA)**
- Kombination aus **Row Store und Column Store**.
- **Hauptspeicherbasierte Verarbeitung** für schnelle Analysen.
- **Redundante Speicherung** für OLTP- und OLAP-Abfragen.

### **Late Materialization**
- Verzögert die Rekonstruktion von Tupeln, um so lange wie möglich auf komprimierten Spalten zu arbeiten.
- Erhöht die Performance bei Abfragen mit Aggregationen.

## **7. Fazit**
Spaltenorientierte Datenbanken bieten enorme Vorteile für analytische Workloads und sind ein wichtiger Bestandteil moderner Data-Warehouse-Architekturen. Sie reduzieren Speicheranforderungen, verbessern die Abfragegeschwindigkeit und ermöglichen effiziente OLAP-Analysen. Allerdings sind sie nicht optimal für transaktionale Workloads und erfordern optimierte Techniken zur Tupelrekonstruktion.

