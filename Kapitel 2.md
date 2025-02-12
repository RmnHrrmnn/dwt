# 2. Architektur eines Data Warehouses

## **1. Anforderungen an die Architektur**
Ein Data Warehouse muss verschiedene Anforderungen erfüllen, um effiziente Analysen und eine stabile Datenverarbeitung zu gewährleisten:
- **Unabhängigkeit zwischen Datenquellen und Analysesystemen**: Vermeidung von Beeinträchtigungen der operativen Systeme durch analytische Abfragen.
- **Dauerhafte Datenbereitstellung**: Speicherung integrierter und abgeleiteter Daten für langfristige Analysen.
- **Mehrfachverwendbarkeit der Daten**: Die gespeicherten Daten müssen flexibel für unterschiedliche analytische Zwecke genutzt werden können.
- **Erweiterbarkeit**: Integration neuer Datenquellen und Erweiterung der Funktionalitäten.
- **Automatisierung von Prozessen**: Minimierung manueller Eingriffe zur Datenverarbeitung.
- **Eindeutige Definition von Datenstrukturen und Berechtigungen**: Sicherstellung der Konsistenz und Sicherheit.
- **Optimierung der Abfrageperformance**: Skalierbare und performante Architektur zur Verarbeitung großer Datenmengen.

## **2. Referenzarchitektur eines Data Warehouses**
Ein Data Warehouse besteht aus mehreren Ebenen, die spezifische Aufgaben übernehmen:

### **Datenquellen**
- Interne und externe Datenbanken (ERP, CRM, Web-Services, IoT-Daten).
- Datenformate: strukturierte (SQL-Datenbanken), semi-strukturierte (XML, JSON) und unstrukturierte Daten (Textdokumente, Bilder).

### **Datenbeschaffung und ETL-Prozesse**
- **Extraktion**: Abruf der Daten aus heterogenen Quellen.
- **Transformation**: Bereinigung, Harmonisierung und Aggregation der Daten.
- **Laden (Load)**: Speicherung der verarbeiteten Daten im Data Warehouse.
- **Datenbereinigungsbereich**: Temporäre Speicherung zur Integration und Korrektur von Daten.

### **Zentrale Datenbank (Data Warehouse)**
- Speicherung der bereinigten und integrierten Daten.
- Nutzung relationaler oder multidimensionaler Datenbanken.
- Grundlage für analytische Abfragen und Data Marts.

### **Datenwürfel (OLAP-Speicherung)**
- Speicherung aggregierter Daten in mehrdimensionalen Strukturen für schnelle Analysen.
- Optimierung durch Pre-Aggregation und Indexierung.

### **Metadaten-Repository**
- Verwaltung von Informationen über Datenherkunft, Strukturen, Prozesse und Berechtigungen.

### **Analyse- und Reporting-Schicht**
- **OLAP-Tools**: Unterstützung für multidimensionale Abfragen.
- **BI-Tools**: Dashboards und Visualisierungen.
- **Data Mining**: Identifikation von Mustern in den gespeicherten Daten.

## **3. OLAP und die 12 Regeln nach Codd**
Online Analytical Processing (OLAP) beschreibt eine Methode zur Analyse multidimensionaler Datenstrukturen. Die 12 OLAP-Regeln nach Codd definieren die Anforderungen an OLAP-Systeme:
1. **Multidimensionalität**: Unterstützung mehrdimensionaler Datenanalyse.
2. **Transparenz**: Integration in verschiedene Datenquellen ohne Anpassungen.
3. **Zugriffsmöglichkeit**: Bereitstellung einer intuitiven Benutzeroberfläche.
4. **Performanz**: Hohe Geschwindigkeit bei Aggregationen und Drill-Downs.
5. **Skalierbarkeit**: Anpassbarkeit an steigende Datenmengen.
6. **Generische Dimensionalität**: Flexible Struktur für verschiedene Analyseebenen.
7. **Dynamische Handhabung dünnbesetzter Strukturen**: Effiziente Verwaltung unvollständiger Datenbereiche.
8. **Mehrbenutzerbetrieb**: Gleichzeitiger Zugriff durch mehrere Nutzer.
9. **Uneingeschränkte Operationen**: Unterstützung komplexer Abfragen.
10. **Intuitive Benutzeroberfläche**: Einfache Bedienbarkeit.
11. **Flexibles Reporting**: Anpassbare Berichtsfunktionen.
12. **Beliebig viele Dimensionen und Aggregationsebenen**: Unterstützung für detaillierte Datenanalysen.

## **4. FASMI-Kriterien für OLAP-Systeme**
Das FASMI-Modell (Fast Analysis of Shared Multidimensional Information) ergänzt die 12 OLAP-Regeln und stellt zusätzliche Anforderungen:
- **Fast (Schnelligkeit)**: Abfragen sollen innerhalb weniger Sekunden Ergebnisse liefern.
- **Analysis (Analysefähigkeit)**: Unterstützung komplexer analytischer Operationen.
- **Shared (Mehrbenutzerfähigkeit)**: Simultane Nutzung durch mehrere User mit unterschiedlichen Zugriffsrechten.
- **Multidimensional (Mehrdimensionalität)**: Darstellung und Analyse in mehreren Dimensionen.
- **Information (Datenumfang)**: Verarbeitung großer und heterogener Datenmengen.

## **5. Phasen des Data Warehousing**
Data Warehousing erfolgt in mehreren Phasen, um eine kontinuierliche Datenintegration und Analyse zu gewährleisten:
1. **Datenextraktion**: Regelmäßige Übernahme aus operativen Systemen.
2. **Datenbereinigung und Transformation**: Korrektur, Harmonisierung und Aggregation.
3. **Datenintegration**: Zusammenführung aus verschiedenen Quellen.
4. **Datenladung**: Speicherung in der zentralen Datenbank.
5. **Datenanalyse**: Nutzung von OLAP- und BI-Tools zur Gewinnung von Erkenntnissen.

## **6. Fazit**
Die Architektur eines Data Warehouses ist komplex und erfordert eine durchdachte Strukturierung, um effiziente Analysen zu ermöglichen. Durch standardisierte Architekturen, optimierte ETL-Prozesse und OLAP-Technologien lassen sich große Datenmengen effizient verwalten und für strategische Entscheidungen nutzen.

