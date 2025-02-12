# 1. Einführung in Data Warehousing

## **1. Motivation und Anwendungsfälle**
Ein Data Warehouse (DWH) ist eine zentrale Datenbank, die große Mengen an strukturierten Daten speichert und für Analysen zur Verfügung stellt. Es dient der Entscheidungsunterstützung und wird in Unternehmen, Wissenschaft und öffentlichen Einrichtungen genutzt.

### **Typische Anwendungsfälle:**
- **Unternehmenssteuerung:** Manager nutzen Data Warehouses zur Analyse von Verkaufszahlen, Kosten und Kundenverhalten.
- **Finanzanalyse:** Banken und Versicherungen analysieren Risiken und Markttrends.
- **Supply Chain Management:** Optimierung von Lagerbeständen und Lieferketten.
- **Wissenschaftliche Forschung:** Umwelt- und Klimadaten werden aggregiert und analysiert.
- **Technische Anwendungen:** Nutzung in geografischen Informationssystemen oder für medizinische Studien.

## **2. Herausforderungen bei der Nutzung von Datenbanken für Analysen**
Operative Datenbanksysteme sind nicht optimal für analytische Anfragen geeignet. Herausforderungen sind unter anderem:
- **Datenvolumen:** Effiziente Speicherung und Verarbeitung großer Datenmengen.
- **Datenmodellierung:** Zeitbezug und multidimensionale Abfragen müssen unterstützt werden.
- **Heterogene Datenquellen:** Integration verschiedener Systeme wie CRM, ERP oder externe Datenquellen.
- **Langsame Abfragezeiten:** Operative Systeme sind auf schnelle Transaktionen ausgelegt, nicht auf komplexe Abfragen.

## **3. Lösungsansätze: Zentrale vs. Verteilte Datenbanken**
Es gibt zwei zentrale Strategien für Data-Warehouse-Systeme:
- **Zentrale Datenbank:** Alle Daten werden in einer einzigen, konsolidierten Datenbank gespeichert. Vorteil: Einheitlichkeit und einfacher Zugriff. Nachteil: Hohe Last auf einer einzelnen Datenbank.
- **Verteilte Datenbanken:** Daten sind auf verschiedene Systeme aufgeteilt und durch ein einheitliches Schema zugänglich. Vorteil: Lastenverteilung und bessere Skalierbarkeit. Nachteil: Komplexe Abfrageverarbeitung.

## **4. Komponenten eines Data-Warehouse-Systems**
Ein Data Warehouse besteht aus mehreren Komponenten, die zusammenarbeiten, um Daten zu erfassen, zu speichern und zu analysieren:
- **ETL-Prozesse (Extract, Transform, Load):** Daten werden aus verschiedenen Quellen extrahiert, bereinigt, transformiert und ins Data Warehouse geladen.
- **Datenbankspeicher:** Speichert Daten entweder relational (ROLAP) oder multidimensional (MOLAP).
- **OLAP-Server:** Ermöglicht schnelle, multidimensionale Analysen der Daten.
- **Data Marts:** Teilmengen des Data Warehouses, die speziell auf bestimmte Anwendungsbereiche zugeschnitten sind.
- **Reporting & Analyse-Tools:** Bereitstellung von Abfrage- und Visualisierungsmöglichkeiten für Endnutzer.

## **5. Beispiele für Data-Warehouse-Nutzung**
- **Handelsunternehmen:** Wal-Mart nutzt ein Data Warehouse zur Analyse von Verkaufszahlen und Lagerbeständen.
- **Wissenschaftliche Forschung:** Das Earth Observing System speichert täglich mehrere Terabyte an meteorologischen Daten.
- **Gesundheitswesen:** Data Warehouses helfen bei der Analyse von Patientenakten und klinischen Studien.

## **6. Fazit**
Data Warehousing ist ein essenzieller Bestandteil moderner Unternehmens- und Forschungsinfrastrukturen. Durch die strukturierte Speicherung und Analyse großer Datenmengen ermöglicht es eine verbesserte Entscheidungsfindung, Optimierung von Geschäftsprozessen und tiefgehende Analysen in verschiedenen Anwendungsbereichen.

