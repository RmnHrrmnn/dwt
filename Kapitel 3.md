# **3. Multidimensionale Datenmodelle in Data Warehouses**

## **1. Einführung in multidimensionale Datenmodelle**
Multidimensionale Datenmodelle dienen der effizienten Organisation und Analyse großer Datenmengen. Sie ermöglichen die Betrachtung betriebswirtschaftlicher Kennzahlen aus verschiedenen Perspektiven durch die Modellierung als **Datenwürfel (Cubes)** mit **Dimensionen** und **Fakten**.

### **Grundlegende Begriffe**
- **Fakten:** Messbare Werte (z. B. Umsatz, Verkaufszahlen, Kosten).
- **Dimensionen:** Perspektiven zur Analyse der Fakten (z. B. Zeit, Produkt, Standort).
- **Hierarchien:** Strukturen innerhalb einer Dimension (z. B. Jahr → Quartal → Monat → Tag).
- **Kennzahlen:** Aggregationen oder Berechnungen über Fakten (z. B. Durchschnittsumsatz pro Filiale).

## **2. Datenmodellierung und Schema-Varianten**
Die Gestaltung des Datenmodells hat maßgeblichen Einfluss auf die Performance und Flexibilität der Analyseprozesse.

### **Star-Schema**
- **Struktur:** Eine zentrale Faktentabelle, die über Fremdschlüssel mit mehreren Dimensionstabellen verbunden ist.
- **Vorteile:**
  - Einfache Struktur und intuitive Abfrageformulierung.
  - Effiziente Abfrageperformance innerhalb einer Dimension (wenige Joins).
- **Nachteile:**
  - Redundanzen in Dimensionstabellen führen zu erhöhtem Speicherbedarf.

### **Snowflake-Schema**
- **Struktur:** Erweiterung des Star-Schemas durch Normalisierung der Dimensionstabellen.
- **Vorteile:**
  - Reduzierte Redundanz und geringerer Speicherbedarf.
- **Nachteile:**
  - Erhöhte Anzahl an Joins, was die Abfrageperformance negativ beeinflussen kann.

### **Galaxy-Schema (Fact Constellation)**
- **Struktur:** Mehrere Faktentabellen, die mit einer oder mehreren gemeinsamen Dimensionen verbunden sind.
- **Vorteile:**
  - Flexibilität für komplexe analytische Szenarien.
  - Möglichkeit der Modellierung mehrerer Geschäftsprozesse.
- **Nachteile:**
  - Höhere Komplexität bei der Abfrageverarbeitung.

## **3. Hierarchien und Aggregationen in Dimensionen**
Hierarchien in Dimensionstabellen erleichtern die Navigation innerhalb der Daten und ermöglichen verschiedene Aggregationsebenen.

### **Arten von Hierarchien:**
- **Einfache Hierarchien:** Jede untergeordnete Ebene ist einer übergeordneten Ebene eindeutig zugeordnet (z. B. Jahr → Quartal → Monat → Tag).
- **Parallele Hierarchien:** Mehrere unabhängige Klassifikationen innerhalb einer Dimension (z. B. Standort kann sowohl nach Region als auch nach Vertriebsleiter strukturiert sein).

### **Aggregationstechniken:**
- **ROLLUP:** Aggregation entlang einer Hierarchie (z. B. Umsätze pro Monat → Quartal → Jahr).
- **CUBE:** Berechnung aller möglichen Aggregationskombinationen für mehrere Dimensionen.
- **GROUPING SETS:** Kombination von Aggregationsszenarien innerhalb einer Abfrage.

## **4. Relationale Umsetzung des multidimensionalen Modells**
Zur Speicherung von multidimensionalen Datenmodellen gibt es verschiedene Ansätze:

- **ROLAP (Relational OLAP):** Umsetzung des Modells auf relationalen Datenbanken mit Star- oder Snowflake-Schema.
- **MOLAP (Multidimensional OLAP):** Direkte Speicherung in spezialisierten multidimensionalen Datenbanken.
- **HOLAP (Hybrid OLAP):** Kombination aus ROLAP und MOLAP (Detaildaten relational, Aggregationen multidimensional gespeichert).

## **5. Optimierung von Datenmodellen für Performance**
Effiziente Modellierung verbessert die Abfragegeschwindigkeit und reduziert Speicherbedarf.

- **Denormalisierung:** Reduktion von Joins zur Performance-Optimierung.
- **Materialisierte Sichten:** Speicherung häufig genutzter Aggregationen zur Beschleunigung von Abfragen.
- **Indexierung:** Nutzung von Bitmap-Indizes für Dimensionstabellen zur effizienten Filterung.
- **Partitionierung:** Horizontale oder vertikale Aufteilung von Tabellen zur Reduzierung der Abfragezeiten.

## **6. Fazit**
Multidimensionale Datenmodelle sind essenziell für analytische Workflows in Data Warehouses. Die Wahl zwischen Star-, Snowflake- oder Galaxy-Schema beeinflusst die Performance und Wartbarkeit des Systems erheblich. Durch geschickte Optimierungstechniken lassen sich Skalierbarkeit und Effizienz eines Data Warehouses erheblich verbessern.

