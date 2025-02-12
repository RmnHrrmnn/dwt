# **6. Speicherstrukturen in Data Warehouses**

## **1. Einführung in Speicherstrukturen**
Die Wahl der Speicherstruktur beeinflusst maßgeblich die **Performance, Skalierbarkeit und Effizienz** eines Data Warehouses. Dabei gibt es zwei wesentliche Speicherungsansätze:
- **Relationale Speicherung (ROLAP - Relational Online Analytical Processing)**
- **Multidimensionale Speicherung (MOLAP - Multidimensional Online Analytical Processing)**

### **Vergleich: Relationale vs. Multidimensionale Speicherung**
| Eigenschaft | Relationale Speicherung (ROLAP) | Multidimensionale Speicherung (MOLAP) |
|------------|--------------------------------|--------------------------------|
| Speicherform | Tabellenbasiert (Star-/Snowflake-Schema) | OLAP-Würfel |
| Abfrageperformance | Langsamer aufgrund vieler Joins | Schnellere Aggregationen |
| Skalierbarkeit | Sehr hoch (bewältigt große Datenmengen) | Eingeschränkter, aber optimiert für analytische Abfragen |
| Speicherplatzbedarf | Höher durch Redundanzen | Effizient durch komprimierte Strukturen |

## **2. Partitionierung in Data Warehouses**
Partitionierung ist eine Technik zur Aufteilung großer Datenmengen in kleinere, effizient verwaltbare Teile. Sie reduziert Abfragezeiten und erleichtert die Verwaltung.

### **Horizontale Partitionierung**
Aufteilung der Daten in Zeilenblöcke, meist nach **Zeit**, **Region** oder **Produktkategorie**.
- **Range-Partitionierung**: Daten werden anhand eines Wertebereichs aufgeteilt (z. B. Jahreszahlen: `2020`, `2021`).
- **Hash-Partitionierung**: Ein Hash-Algorithmus bestimmt, zu welcher Partition ein Tupel gehört.

**Beispiel:**
```sql
CREATE TABLE Verkauf (
    ArtikelID INT,
    Umsatz DECIMAL(10,2),
    Jahr INT
) PARTITION BY RANGE (Jahr) (
    PARTITION p1 VALUES LESS THAN (2020),
    PARTITION p2 VALUES LESS THAN (2025)
);
```

### **Vertikale Partitionierung**
Aufteilung von **Tabellenspalten** in mehrere Tabellen, um selten genutzte Attribute auszulagern.
- Vorteil: Reduktion der Speicheranforderungen für häufig genutzte Tabellen.
- Nachteil: Teure **Join-Operationen** zur Wiederherstellung der Daten.

**Beispiel:** Aufteilung einer `Kunden`-Tabelle in:
- `Kunden_Basis` (enthält ID, Name, Adresse)
- `Kunden_Details` (enthält Geburtsdatum, Bonität, Beruf)

## **3. Spezielle Speicherstrategien und Indexierung**

### **Mini-Dimensionen**
- Große Dimensionstabellen (z. B. `Kunden`) können durch Abspalten seltener genutzter Attribute in kleinere Tabellen (`Mini-Dimensionen`) effizienter verwaltet werden.
- Dadurch wird der **Speicherbedarf reduziert** und Abfragen laufen schneller.

### **Append-Mode-Tabellen**
- Optimierte Tabellen für **INSERT-Operationen**, in denen Datensätze am Tabellenende angehängt werden.
- Werden häufig in Data Warehouses für **Log-Daten** oder **Verkaufsdaten** genutzt.

**Beispiel in DB2:**
```sql
ALTER TABLE Bestellung
APPEND ON;
```

### **Bereichsgeclusterte Tabellen (Range-Clustered Tables - RCT)**
- Optimierte Speichertechnik für **Sequenzdaten** (z. B. Bestellungen nach Datum sortiert).
- Ermöglicht einen **schnelleren Zugriff**, da Datensätze mit ähnlichen Werten auf **benachbarten Speicherplätzen** gespeichert werden.

## **4. Multidimensionale Speicherstrukturen**
Neben der relationalen Speicherung existieren alternative Konzepte für effiziente **OLAP-Abfragen**:
- **Cube Forests** und **Hierarchically Cube Forests**: Mehrstufige OLAP-Würfel.
- **Condensed Cube**: Komprimierte Cube-Strukturen für schnelle Aggregationen.
- **Quotient Cube**: Optimierter Cube für große Datenmengen mit weniger Speicherverbrauch.

### **Multidimensionale Clustered Tables (MDC)**
- Speichert Daten **mehrdimensional geclustert**, um **OLAP-Abfragen zu beschleunigen**.
- Indizierung erfolgt über **Block-Indexe**, die effiziente Bereichsabfragen ermöglichen.

**Beispiel in DB2:**
```sql
CREATE TABLE Verkauf (
    Umsatz NUMBER, Jahr INT, Stadt VARCHAR(20)
) ORGANIZE BY DIMENSIONS (Stadt, Jahr);
```

## **5. Fazit**
Die Wahl der richtigen Speicherstrategie beeinflusst maßgeblich die **Performance und Effizienz** eines Data Warehouses. Während relationale Speicherstrukturen flexibler sind, bieten multidimensionale Speicherstrategien enorme Vorteile für analytische Abfragen. Durch **Partitionierung, Indexierung und spezialisierte Speicherformate** können große Datenmengen effizient organisiert und analysiert werden.

