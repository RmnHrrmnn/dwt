# **9. Materialisierte Sichten in Data Warehouses**

## **1. Einführung in materialisierte Sichten**
Materialisierte Sichten (engl. *materialized views*) sind gespeicherte Abfrageergebnisse, die für wiederkehrende Anfragen genutzt werden. Sie bieten eine **Leistungssteigerung** durch die Reduktion von teuren Berechnungen in komplexen Abfragen.

### **Motivation für den Einsatz materialisierter Sichten**
- **Beschleunigung von Abfragen:** Speicherung berechneter Aggregationen vermeidet redundante Berechnungen.
- **Entlastung der Basisrelationen:** Durch die Nutzung materialisierter Sichten werden die Originaltabellen weniger belastet.
- **Einfache Wiederverwendung:** Automatische Nutzung in Abfragen durch **Query Rewrite-Techniken**.

### **Anwendungsfälle**
- OLAP-Analysen mit **aggregierten Daten** (z. B. Summe der Verkäufe pro Region).
- Vorberechnete **Statistiken** und Berichte.
- **Data Marts** zur Bereitstellung fachspezifischer Daten für Abteilungen.

## **2. Auswahl von materialisierten Sichten**
Die Auswahl der zu materialisierenden Sichten basiert auf einer **Kosten-Nutzen-Analyse**:

### **Kriterien für die Auswahl**
- **Speicherbedarf:** Zusätzlicher Speicherverbrauch für materialisierte Daten.
- **Aktualisierungsaufwand:** Bei Änderungen der Basisrelationen müssen die Sichten aktualisiert werden.
- **Antwortzeitverbesserung:** Einsparung von Berechnungszeit durch voraggregierte Werte.

## **3. Anfrageverarbeitung mit materialisierten Sichten**

### **Automatische Nutzung durch Query Rewrite**
Datenbanksysteme erkennen automatisch, wenn eine Anfrage von einer materialisierten Sicht beantwortet werden kann. Dies erfolgt durch das **Umschreiben der Anfrage (Query Rewriting)**.

**Beispiel:** Eine materialisierte Sicht `M` speichert die aggregierten Verkaufszahlen:
```sql
CREATE MATERIALIZED VIEW M AS
SELECT ProdGruppe, Region, SUM(Verkäufe) FROM Verkauf GROUP BY ProdGruppe, Region;
```

Wenn eine neue Anfrage die Verkaufszahlen pro Produktgruppe und Region benötigt:
```sql
SELECT ProdGruppe, Region, SUM(Verkäufe) FROM Verkauf GROUP BY ProdGruppe, Region;
```
Erkennt das System, dass `M` bereits die benötigten Ergebnisse enthält, und ersetzt die Originalabfrage durch:
```sql
SELECT * FROM M;
```

## **4. Aktualisierung materialisierter Sichten**
Eine **aktuelle Datenhaltung** ist entscheidend für die Konsistenz zwischen den materialisierten Sichten und den zugrunde liegenden Tabellen.

### **Methoden zur Aktualisierung**
- **Komplette Aktualisierung (`REFRESH COMPLETE`)**
  - Die Sicht wird bei jeder Aktualisierung **vollständig neu berechnet**.
  - Hoher Rechenaufwand, aber konsistente Ergebnisse.

- **Inkrementelle Aktualisierung (`REFRESH FAST`)**
  - Nur geänderte oder neue Daten werden aktualisiert.
  - Benötigt **View Logs**, die Änderungen in der Basisrelation protokollieren.

- **On-Demand (`REFRESH ON DEMAND`)**
  - Aktualisierung erfolgt nur auf **explizite Anforderung**, z. B. durch ein Skript.

- **On-Commit (`REFRESH ON COMMIT`)**
  - Automatische Aktualisierung bei jeder Änderung der Basisrelation.
  - Belastet das System bei häufigen Änderungen.

**Beispiel für eine materialisierte Sicht mit inkrementeller Aktualisierung:**
```sql
CREATE MATERIALIZED VIEW LOG ON Verkauf WITH ROWID;
CREATE MATERIALIZED VIEW M REFRESH FAST AS SELECT ProdGruppe, SUM(Verkäufe) FROM Verkauf GROUP BY ProdGruppe;
```

## **5. Unterstützung in kommerziellen Datenbanksystemen**
Materialisierte Sichten werden von den meisten Datenbanksystemen unterstützt, z. B.:
- **Oracle**: `CREATE MATERIALIZED VIEW` mit `BUILD IMMEDIATE | BUILD DEFERRED` und `REFRESH COMPLETE | REFRESH FAST`.
- **PostgreSQL**: Unterstützung für materialisierte Sichten mit explizitem `REFRESH MATERIALIZED VIEW`.
- **SQL Server**: Nutzung von **Indexed Views** für eine ähnliche Funktionalität.

## **6. Fazit**
Materialisierte Sichten sind ein leistungsstarkes Werkzeug zur Optimierung von Anfragen in Data Warehouses. Durch die gezielte Auswahl, Nutzung von Query Rewrite-Techniken und effiziente Aktualisierungsstrategien lassen sich erhebliche Performancegewinne erzielen.

