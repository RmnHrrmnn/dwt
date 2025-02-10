# Zusammenfassung der Kapitel: Data Warehouse Technologien

## **Kapitel 1: Einführung**
Data Warehousing ist eine zentrale Technologie zur Unterstützung der Entscheidungsfindung in Unternehmen. 
Ein Data Warehouse (DWH) ermöglicht die Analyse großer Datenmengen aus verschiedenen Quellen.

### **Motivation**
Unternehmen speichern zunehmend große Datenmengen, die für operative Prozesse und strategische Entscheidungen genutzt werden. Data Warehouses dienen dazu, diese Daten effizient zu speichern und für analytische Zwecke bereitzustellen. 

Durch die zentrale Speicherung und Analyse von Daten aus unterschiedlichen operativen Systemen können Unternehmen Muster erkennen, zukünftige Entwicklungen prognostizieren und fundierte Entscheidungen treffen. Data Warehousing ist besonders in Bereichen wie Finanzwesen, Gesundheitswesen, Logistik und Marketing von hoher Bedeutung.

### **Grundbegriffe & Herausforderungen**
- **OLTP (Online Transaction Processing)**: Systeme zur schnellen Verarbeitung operativer Daten, z. B. Buchungssysteme.
- **OLAP (Online Analytical Processing)**: Systeme zur komplexen Analyse großer Datenmengen.
- **ETL (Extract, Transform, Load)**: Prozesse zur Extraktion, Transformation und Speicherung von Daten in einem Data Warehouse.
- **Data Lake**: Speicherung strukturierter und unstrukturierter Daten in Rohform zur späteren Analyse.
- **Big Data**: Verarbeitung sehr großer, vielfältiger und sich schnell ändernder Datenmengen.
- **Data Governance**: Regeln und Richtlinien zur Sicherstellung von Datenqualität und -sicherheit.
- **Herausforderungen**: Integration heterogener Datenquellen, hohe Datenmengen, Sicherstellung der Aktualität, komplexe Modellierung.

### **Zentrale vs. verteilte Architektur**
- **Zentrale Datenbank**: Ein einziges Data Warehouse speichert alle relevanten Daten.
- **Verteilte Data Warehouses**: Daten werden auf mehrere Systeme verteilt, um Last zu reduzieren und Skalierbarkeit zu erhöhen.

---

## **Kapitel 2: Architektur**
Ein Data Warehouse folgt einer bestimmten Architektur, um Daten effizient zu speichern und verarbeiten.

### **Anforderungen**
- **Skalierbarkeit**: Das System muss wachsende Datenmengen verarbeiten können.
- **Performance**: Schnelle Abfragezeiten durch optimierte Speicher- und Indexierungsstrategien.
- **Datenqualität**: Sicherstellung konsistenter und fehlerfreier Daten.
- **Datensicherheit**: Schutz der gespeicherten Informationen vor unbefugtem Zugriff.
- **Datenintegration**: Vereinheitlichung heterogener Datenquellen.
- **Unabhängigkeit von operativen Systemen**: Vermeidung von Störungen durch parallele Nutzung.
- **12 OLAP-Regeln nach Codd**: Definition eines OLAP-Systems (Multidimensionalität, Skalierbarkeit, Benutzerfreundlichkeit, schnelle Antwortzeiten).
- **FASMI-Prinzip (Fast Analysis of Shared Multidimensional Information)**: Ein leistungsfähiges BI-System muss schnell, flexibel und skalierbar sein.

### **Referenzarchitektur**
Typische Architekturkomponenten eines DWH:
- **Datenquellen**: ERP-Systeme, CRM-Systeme, externe Datenquellen.
- **ETL-Schicht**: Bereinigung, Transformation und Laden der Daten.
- **Data Warehouse**: Speicherung der bereinigten Daten.
- **Data Marts**: Spezialisierte Datenbanken für bestimmte Geschäftsbereiche.
- **Metadaten-Repository**: Verwaltung von Datenherkunft und Strukturinformationen.
- **Analytische Schicht**: Werkzeuge für Berichte, Dashboards und Data Mining.
- **Datenmodellierung**: Nutzung von Star-, Snowflake- oder Galaxy-Schemata.

---

## **Kapitel 3: Datenmodell**
Data Warehouses nutzen spezielle Datenmodelle zur effizienten Speicherung und Analyse von Daten.

### **Multidimensionale Modelle**
- **Star-Schema**: Eine zentrale Faktentabelle mit mehreren Dimensionstabellen. Vorteile: einfache Struktur, schnelle Abfragen, jedoch Redundanzen in Dimensionstabellen.
- **Snowflake-Schema**: Erweiterung des Star-Schemas durch Normalisierung, weniger Redundanz, jedoch komplexere Joins.
- **Galaxy-Schema (Fact Constellation)**: Kombination mehrerer Faktentabellen mit gemeinsamen Dimensionen.

### **Hierarchien & Aggregation**
- **Hierarchien** wie Jahr → Quartal → Monat → Tag verbessern die Drill-Down-Analyse.
- **Einfache vs. parallele Hierarchien**: Mehrere Gruppierungsstrukturen für eine Dimension.
- **Aggregationsstrategien**: ROLLUP, CUBE, GROUPING SETS für differenzierte Gruppierungen.

### **Praktische Umsetzung & Optimierung**
- **Denormalisierung** für bessere Leseperformance.
- **Normalisierung** zur Vermeidung von Redundanzen.
- **Materialisierte Sichten** zur Voraggregation oft genutzter Daten.
- **Relationale Umsetzung**: Spezielle Tabellenstrukturen für multidimensionale Modelle.

---

## **Kapitel 4: Anfragen**
Effiziente Abfragen sind entscheidend für die Nutzung eines Data Warehouses.

### **SQL für Data Warehouses**
- **Star-Join-Abfragen**: Effiziente Verknüpfung von Faktentabellen mit Dimensionstabellen.
- **GROUPING SETS, CUBE, ROLLUP**: Aggregationen auf mehreren Ebenen.
- **Pivoting & Unpivoting**: Transformation von Zeilen in Spalten und umgekehrt.
- **Analytische Funktionen**: `RANK()`, `DENSE_RANK()`, `LAG()`, `LEAD()` zur Reihenfolgen-Analyse.

### **Optimierungsstrategien**
- **Partitionierung** zur Reduzierung von Suchräumen.
- **Indexierung (B-Bäume, Bitmap-Indexe)** für schnellere Abfragen.
- **Materialisierte Sichten** zur Speicherung voraggregierter Werte.
- **Parallelisierung & Caching** für optimierte Anfragen.
- **Query Rewriting & Kostenbasierte Optimierung** zur verbesserten Performance.

---

## **Kapitel 5: ETL-Prozesse**
ETL-Prozesse (Extract, Transform, Load) sind für den Datenfluss in ein Data Warehouse verantwortlich. Sie stellen sicher, dass Daten aus operativen Systemen effizient erfasst, transformiert und in das Data Warehouse geladen werden.

### **Extraktion**
Die Extraktion ist der erste Schritt des ETL-Prozesses und bezieht sich auf das Abrufen von Daten aus verschiedenen Quellsystemen.
- **Vollständige Extraktion**: Alle Daten werden aus den Quellsystemen entnommen. Dies ist speicherintensiv, aber stellt sicher, dass keine Daten verloren gehen.
- **Inkrementelle Extraktion**: Nur neue oder geänderte Daten werden extrahiert, was die Performance verbessert.
- **Echtzeit-Extraktion**: Daten werden kontinuierlich erfasst (Change Data Capture, CDC), um stets aktuelle Informationen bereitzustellen.
- **Logbasierte Extraktion**: Nutzung von DB-Transaktionslogs zur Erfassung von Änderungen.
- **Trigger-basierte Extraktion**: Datenänderungen werden direkt durch Datenbank-Trigger erfasst.

### **Transformation**
Hier werden die extrahierten Daten bereinigt, harmonisiert und für die Analyse vorbereitet.
- **Datenbereinigung**: Identifikation und Korrektur von fehlerhaften oder unvollständigen Daten.
- **Formatierung**: Umwandlung von Datentypen und Vereinheitlichung von Formaten.
- **Datenanreicherung**: Ergänzung von Zusatzinformationen aus anderen Quellen.
- **Validierung**: Sicherstellung der Datenintegrität, z. B. durch Konsistenzprüfungen.
- **Aggregation**: Zusammenfassung von Daten auf höheren Granularitätsstufen.
- **Duplikatsbereinigung**: Eliminierung redundanter Datensätze.
- **Schlüsselgenerierung**: Erzeugung neuer Primärschlüssel für Datenintegrität.

### **Laden (Load)**
Nach der Transformation werden die Daten ins Data Warehouse überführt.
- **Batch-Loading**: Daten werden in vordefinierten Zeitintervallen geladen.
- **Streaming-Loading**: Kontinuierliches Laden neuer Daten.
- **Delta-Loading**: Nur geänderte Datensätze werden aktualisiert.
- **Initial-Load vs. Incremental-Load**: Erster Ladevorgang umfasst alle Daten, während spätere Ladungen nur noch Änderungen übernehmen.
- **Bulk Loading**: Nutzung von optimierten Ladeprozessen zur Steigerung der Effizienz.
- **Partitioned Loading**: Laden von Daten in Partitionen zur Verbesserung der Performance.

---

## **Kapitel 6: Speicherstrukturen**
Effiziente Speicherstrukturen sind essenziell für die Performance von Data Warehouses.

### **Relationale Speicherung (ROLAP - Relational Online Analytical Processing)**
- Speicherung in relationalen Datenbanken mit normalisierten Tabellen.
- Nutzung von SQL für Abfragen und Analysen.
- Vorteil: Skalierbarkeit und Flexibilität durch relationale DBMS.
- Unterstützung durch Indexierung, Joins und Partitionierung für bessere Performance.

### **Multidimensionale Speicherung (MOLAP - Multidimensional Online Analytical Processing)**
- Speicherung der Daten in mehrdimensionalen Würfeln.
- Optimiert für OLAP-Abfragen, bietet schnelle Aggregationsmöglichkeiten.
- Nachteil: Erhöhter Speicherbedarf durch voraggregierte Daten.
- Vorteil: Schnellere analytische Abfragen durch vordefinierte Aggregate.

### **Hybrid-Speicherung (HOLAP - Hybrid Online Analytical Processing)**
- Kombination aus ROLAP und MOLAP.
- Häufig genutzte Aggregationen werden materialisiert gespeichert (MOLAP), während detaillierte Daten relational gespeichert bleiben (ROLAP).
- Vorteil: Flexible Balance zwischen Speicherplatz und Abfragegeschwindigkeit.

### **Partitionierung**
- **Horizontale Partitionierung**: Aufteilung der Daten nach Zeilen, z. B. nach Datum oder Regionen.
- **Vertikale Partitionierung**: Trennung von oft und selten verwendeten Attributen.
- **Hybrid-Partitionierung**: Kombination aus horizontaler und vertikaler Partitionierung.
- **Dynamische Partitionierung**: Automatische Anpassung basierend auf Zugriffsmustern.
- **Range-Partitionierung**: Segmentierung nach bestimmten Wertebereichen.
- **List-Partitionierung**: Einteilung basierend auf bestimmten Listenwerten.
- **Hash-Partitionierung**: Verteilung von Daten über mehrere Partitionen mittels Hashing.

---

## **Kapitel 7: Indexstrukturen**
Indexe verbessern die Performance von Abfragen durch schnelleren Zugriff auf Daten.

### **B-Bäume & B+-Bäume**
- Selbstbalancierende Baumstrukturen für schnelle Suchen, Einfügungen und Löschungen.
- B+-Bäume speichern Daten nur in Blattknoten, wodurch Bereichssuchen effizienter sind.
- Vorteile: Effiziente Sortierung und Unterstützung für Bereichsabfragen.
- Nachteil: Speicherintensive Struktur.

### **Bitmap-Indexe**
- Geeignet für Attribute mit niedriger Kardinalität (z. B. Geschlecht oder Status).
- Speicherung als Bitvektoren, die schnelle Abfragen ermöglichen.
- Besonders effizient für analytische Anfragen mit vielen `WHERE`-Klauseln.
- Vorteil: Speicherplatzsparend für wenige unterschiedliche Werte.
- Nachteil: Nicht optimal für häufig aktualisierte Daten.

### **Mehrdimensionale Indexe**
- Speziell für OLAP-Anfragen, um Datenwürfel effizienter zu durchsuchen.
- Beispiele: R-Bäume, kd-Bäume und Grid-Dateien.
- Unterstützen schnelle Suchen in hochdimensionalen Daten.
- Anwendung: Geodaten, räumliche Analysen, multidimensionale OLAP-Abfragen.

---

## **Kapitel 8: Anfrageverarbeitung**
Die Optimierung von Abfragen ist entscheidend für die Performance eines Data Warehouses.

### **Query-Optimierung**
- **Regelbasierte Optimierung**: Anwendung von heuristischen Regeln zur Verbesserung der Abfrageeffizienz.
- **Kostenbasierte Optimierung**: Der beste Ausführungsplan wird anhand von Statistiken und Kostenmodellen berechnet.
- **Umformung von Abfragen**: Reduktion komplexer Joins, Umstrukturierung von `GROUP BY`-Anfragen.
- **Predicate Pushdown**: Verlagerung von Filterbedingungen, um die Datenmenge frühzeitig zu reduzieren.
- **Join-Strategien**: Nested Loop, Hash-Join, Merge-Join für optimale Joins.

### **Star-Join Optimierung**
- Optimierte Verarbeitung von Joins im Star-Schema.
- Nutzung von Bitmap-Join-Indexen zur Vermeidung teurer Joins mit großen Faktentabellen.
- Einsatz von Hash- und Merge-Joins zur Verbesserung der Performance.
- Materialisierte Joins für wiederkehrende Abfragen.

### **Physische Optimierung**
- Einsatz von Indizes (B-Bäume, Bitmap-Indizes) zur Beschleunigung der Abfragen.
- Nutzung von Partitionierung zur Reduzierung des Datenumfangs bei Abfragen.
- **Materialisierte Sichten** für häufig genutzte Aggregationen.
- **Parallelverarbeitung** zur besseren Skalierung.
- **Automatisches Caching** zur Vermeidung redundanter Berechnungen.

---

## **Kapitel 9: Materialisierte Sichten**
Materialisierte Sichten speichern berechnete Ergebnisse zur Reduzierung von Berechnungszeiten und verbessern die Performance komplexer Analysen in Data Warehouses.

### **Definition & Motivation**
- Eine materialisierte Sicht speichert das Ergebnis einer Abfrage und ermöglicht schnellere Antwortzeiten für häufig genutzte Abfragen.
- Wird in der Datenbank persistiert, sodass sie nicht bei jeder Abfrage neu berechnet werden muss.
- Reduziert die Last auf der Datenbank durch Vorkalkulation und Speicherung von Aggregaten.

### **Vorteile materialisierter Sichten**
- **Performance-Steigerung**: Häufige und komplexe Abfragen werden durch bereits gespeicherte Ergebnisse beschleunigt.
- **Entlastung der Datenbank**: Durch weniger Berechnungen sinkt die Last auf dem System.
- **Optimierung analytischer Prozesse**: Schneller Zugriff auf aggregierte Daten.

### **Nachteile materialisierter Sichten**
- **Speicherbedarf**: Gespeicherte Ergebnisse benötigen zusätzlichen Speicherplatz.
- **Aktualisierungsaufwand**: Änderungen in Basisdaten erfordern Aktualisierung der Sichten.
- **Komplexität**: Verwaltung und Pflege kann anspruchsvoll sein, insbesondere bei Echtzeit-Daten.

### **Aktualisierungsstrategien**
- **Komplette Neuberechnung**: Die gesamte Sicht wird neu erstellt.
- **Inkrementelle Aktualisierung**: Nur geänderte Daten werden angepasst.
- **Automatische Aktualisierung**: Die Datenbank-Engine aktualisiert die Sicht in regelmäßigen Intervallen oder basierend auf Triggers.
- **Manuelle Aktualisierung**: Aktualisierung erfolgt durch geplante Jobs oder Skripte.

---

## **Kapitel 10: Business Intelligence (BI)**
Business Intelligence umfasst Werkzeuge, Technologien und Prozesse zur Analyse von Daten und Unterstützung der Entscheidungsfindung in Unternehmen.

### **Ziele von Business Intelligence**
- **Verbesserte Entscheidungsfindung**: Bereitstellung von relevanten, aktuellen und analysierbaren Daten.
- **Effizienzsteigerung**: Automatisierung von Reporting- und Analyseprozessen.
- **Erkennung von Trends**: Analyse historischer Daten zur Identifikation von Markttrends.
- **Optimierung von Geschäftsprozessen**: Datengetriebene Strategien zur besseren Unternehmenssteuerung.

### **BI-Komponenten**
- **Datenquellen**: ERP-Systeme, CRM-Systeme, externe Datenquellen.
- **ETL-Prozesse**: Extraktion, Transformation und Laden der Daten ins Data Warehouse.
- **Data Warehouse**: Zentrale Speicherung und Verwaltung der Daten.
- **OLAP-Systeme**: Ermöglichen interaktive Analysen durch multidimensionale Datenmodelle.
- **BI-Tools**: Dashboards, Berichte, Ad-hoc-Abfragen.
- **Data Mining**: Methoden zur Identifikation versteckter Muster in großen Datenmengen.
- **KPI-Scorecards**: Messung und Visualisierung von Leistungsindikatoren zur Bewertung von Unternehmensstrategien.

### **Anwendungsfälle von BI**
- **Customer Relationship Management (CRM)**: Analyse des Kundenverhaltens, Kundensegmentierung.
- **Supply Chain Management (SCM)**: Optimierung der Lieferkette durch Echtzeit-Analysen.
- **Finanzanalyse**: Überwachung finanzieller Kennzahlen und Betrugserkennung.
- **Marketinganalysen**: Auswertung von Kampagnenleistung, Zielgruppenanalyse.
- **Warenkorbanalyse**: Ermittlung von Kaufmustern zur Empfehlung von Produkten.

---

## **Kapitel 11: Spaltenorientierte Datenbanksysteme**
Spaltenorientierte Datenbanksysteme (Columnar Databases) sind speziell für analytische Workloads optimiert und bieten erhebliche Vorteile gegenüber zeilenorientierten Systemen.

### **Vergleich: Zeilen- vs. Spaltenorientierte Speicherung**
- **Zeilenorientiert** (Row-Based): Geeignet für OLTP-Systeme mit häufigen Insert- und Update-Operationen.
- **Spaltenorientiert** (Column-Based): Geeignet für OLAP-Systeme, da Abfragen nur die relevanten Spalten scannen müssen.

### **Vorteile der Spaltenorientierung**
- **Bessere Abfrageperformance**: Durch Spaltenkompression und reduzierte I/O-Zugriffe.
- **Effiziente Kompression**: Ähnliche Werte innerhalb einer Spalte lassen sich effizient komprimieren.
- **Optimiert für analytische Abfragen**: Aggregationen und Filterungen auf Spaltenebene erfolgen schneller.

### **Erweiterte Index-Techniken**
- **Standard-Bitmap-Index**: Optimiert für Daten mit wenigen Ausprägungen.
- **Mehrkomponenten-Bitmap-Index**: Reduziert Speicheraufwand und verbessert Bereichsabfragen.
- **Bereichs-kodierte Bitmap-Indexe**: Effiziente Speicherung für numerische Werte, die Bereichsoperationen erleichtern.

---

## **Kapitel 12: SQL-Erweiterungen für Data Warehouses**
SQL wurde für analytische Abfragen um spezielle Funktionen erweitert.

### **Erweiterte Aggregationsfunktionen**
- **GROUPING SETS, CUBE, ROLLUP**: Berechnung mehrdimensionaler Aggregationen.
- **Window Functions**: `RANK()`, `DENSE_RANK()`, `LAG()`, `LEAD()` für sequenzielle Berechnungen.
- **Pivoting & Unpivoting**: Transformation von Zeilen in Spalten und umgekehrt.

### **Performance-Optimierung**
- **Materialisierte Sichten**: Verbesserung der Abfragegeschwindigkeit durch gespeicherte Aggregationen.
- **Partitionierung**: Aufteilung großer Tabellen zur Optimierung von Zugriffsmustern.
- **Indexing-Techniken**: Einsatz von Bitmap- und B-Baum-Indizes für schnelle Abfragen.
- **Query Rewriting**: Automatische Optimierung durch Umformung von Abfragen.

---
