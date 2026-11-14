# 📦 Supply Chain Analytics

<p align="center"><strong>Power BI Portfolio · Supply Chain Analytics</strong></p>
<p align="center"><em>Lagerbestände · Einkaufsanalyse · Lieferantenleistung · Bestandsrisiken</em></p>
<p align="center">
  <img alt="Power BI Desktop" src="https://img.shields.io/badge/Power_BI-Desktop-F2C811?style=flat-square">
  <img alt="Datenaufbereitung mit Power Query" src="https://img.shields.io/badge/Power_Query-M-217346?style=flat-square">
  <img alt="Kennzahlen mit DAX" src="https://img.shields.io/badge/Kennzahlen-DAX-174A70?style=flat-square">
  <img alt="Excel als Datenquelle" src="https://img.shields.io/badge/Datenquelle-Excel-217346?style=flat-square">
</p>

Ein interaktiver Power-BI-Bericht zur Analyse von Beschaffung, Warenbeständen und Lieferzuverlässigkeit. Das Lern- und Portfolioprojekt verbindet Excel-Daten, Power Query, ein relationales Datenmodell und DAX-Kennzahlen zu vier Berichtsseiten.

**[📥 Deutschen Power-BI-Bericht öffnen](Supply_Chain_Analytics_DE.pbix)** · **[📄 Analytischen Bericht lesen](Supply_Chain_Analytics_Analytischer_Bericht_DE.pdf)**

---

## 📋 Schnellnavigation

| **Projekt kennenlernen** | **Bericht verwenden** | **Methodik verstehen** |
| :-- | :-- | :-- |
| [📌 Überblick](#project-overview) | [📊 Berichtsseiten](#report-pages) | [📐 Kennzahlen und DAX](#technical-notes) |
| [🎯 Zentrale Ergebnisse](#key-results) | [▶️ Erste Schritte](#getting-started) | [🗃️ Datenmodell](#data-model) |
| [🛠 Technologien](#technology-stack) | [📁 Projektdateien](#project-structure) | [⚠️ Einordnung](#known-limitations) |

---

<a id="project-overview"></a>

## 📌 Überblick

Der Bericht beantwortet vier praktische Fragen:

- **Bestände:** Welche Artikel liegen an welchen Standorten, und wie verteilen sich die Mengen nach Kategorie?
- **Einkauf:** Wie entwickeln sich Bestellwerte und Bestellmengen im Zeitverlauf?
- **Lieferanten:** Welche Lieferanten liefern zuverlässig, und wie stark unterscheiden sich ihre Bestellvolumina?
- **Risiken:** Welche Kombinationen aus Artikel und Lager benötigen eine Nachbestellung oder haben einen kritischen Bestand?

> [!NOTE]
> **Projektumfang:** 4 Berichtsseiten, 22 DAX-Kennzahlen, 30 Artikel, 10 Lieferanten und 3 Lagerstandorte. Die deutsche PBIX-Datei enthält die importierten Daten und kann ohne erneuten Excel-Import betrachtet werden.

### Gezeigte Kompetenzen

| **Bereich** | **Umsetzung im Projekt** |
| --- | --- |
| Datenaufbereitung | Excel-Import, Datentypen und Übersetzung ausgewählter Kategorien mit Power Query |
| Datenmodellierung | Verknüpfung von Stamm- und Bewegungsdaten sowie Einbindung einer Kalendertabelle |
| DAX | Filterkontext, Bestandsstichtage, Lieferquoten, Schwellenwerte und Zeitvergleiche |
| Visualisierung | KPI-Karten, Balken- und Liniendiagramme, Tabellen, Filter und bedingte Formatierung |
| Fachliche Interpretation | Unterscheidung von Mengenverfügbarkeit, Termintreue und lokalem Bestandsrisiko |

---

<a id="key-results"></a>

## 🎯 Zentrale Ergebnisse

Die folgenden Werte gelten ohne zusätzliche Berichtsfilter. Bestandswerte beziehen sich auf den letzten vorhandenen Bestandsstichtag, den **01.09.2026**. Bestellkennzahlen berücksichtigen den gesamten verfügbaren Bestellzeitraum vom **01.01.2025 bis 02.10.2026**.

| **Kennzahl** | **Wert** | **Bedeutung** |
| --- | ---: | --- |
| Bestellwert | **13.443.206,00 UAH** | Summe aus Bestellmenge × Einzelpreis |
| Bestellanzahl | **240** | Eindeutige Bestellungen |
| Bestellmenge | **21.430 Stk.** | Insgesamt bestellte Einheiten |
| Eingegangene Menge | **20.547 Stk.** | Insgesamt erfasste Wareneingänge |
| Lieferquote | **95,88 %** | Eingegangene Menge im Verhältnis zur bestellten Menge |
| Pünktliche Lieferungen | **26,67 %** | Anteil gemäß der im Modell verwendeten Bestell- und Lieferlogik |
| Verspätete Lieferungen | **176** | Wareneingänge nach dem erwarteten Lieferdatum |
| Ø Verspätung | **7,3 Tage** | Durchschnitt ausschließlich der verspäteten Lieferungen |
| Lagerbestand | **8.086 Stk.** | Bestand am letzten verfügbaren Bestandsstichtag |
| Nachbestellpositionen | **25** | Artikel-Lager-Kombinationen am oder unter dem Meldebestand |
| Kritische Positionen | **8** | Artikel-Lager-Kombinationen am oder unter dem Mindestbestand |

**Interpretation:** Die Lieferquote von 95,88 % beschreibt die Mengenversorgung. Die deutlich niedrigere Pünktlichkeitsquote von 26,67 % zeigt zugleich ein Problem bei der Termineinhaltung. Eine hohe gelieferte Menge bedeutet daher noch keine zuverlässige Lieferzeit.

Die Bestandsanalyse macht außerdem Risiken auf Ebene einzelner Lager sichtbar: Trotz eines Gesamtbestands von 8.086 Stück benötigen 25 Positionen eine Nachbestellung. Die 8 kritischen Positionen sind darin enthalten und sollten zuerst geprüft werden.

---

<a id="report-pages"></a>

## 📊 Berichtsseiten

| **Seite** | **Inhalt** | **Interaktion** |
| --- | --- | --- |
| **Lagerbestandsübersicht** | Bestände je Artikel, Standort und Kategorie; Anzahl der Nachbestell- und kritischen Positionen | Auswahl eines Lagerstandorts |
| **Einkaufsanalyse** | Bestellwert, Bestellmenge, Lieferquote, Kategorien und monatliche Entwicklung mit YTD-, Vorjahres- und Vormonatsvergleich | Auswahl des Jahres |
| **Lieferantenleistung** | Bestellanzahl, verspätete Lieferungen, Pünktlichkeit und durchschnittliche Verspätung; Lieferantenvergleich | Auswahl von Lieferant und Jahr |
| **Bestandsrisiken** | Artikel-Lager-Tabelle mit Meldebestand, Mindestbestand und farbigem Bestandsstatus | Lagerauswahl und Schaltfläche zum Zurücksetzen der Filter |

> [!NOTE]
> Die vorhandenen Screenshots zeigen einen früheren Stand der deutschen Berichtsversion. In der aktuellen PBIX-Datei wurden lange Beschriftungen gekürzt und automatische Zahleneinheiten entfernt. Einzelne Systemtexte in Filtern und Summenzeilen folgen weiterhin der Sprache von Power BI Desktop.

### Lagerbestandsübersicht

![Lagerbestandsübersicht mit KPI-Karten und Bestandsverteilung](<png/Изображение ChatGPT 14 нояб. 2026 г., 14_16_02.png>)

<details>
<summary>📊 Weitere Berichtsseiten anzeigen</summary>

### Einkaufsanalyse

![Einkaufsanalyse mit Bestellwert, Lieferquote und monatlicher Entwicklung](<png/Изображение ChatGPT 14 нояб. 2026 г., 14_16_23.png>)

### Lieferantenleistung

![Lieferantenvergleich nach Einkaufswert und Pünktlichkeit](<png/Изображение ChatGPT 14 нояб. 2026 г., 14_16_29.png>)

### Bestandsrisiken

![Bestandsrisiken mit farblich markierten Artikel-Lager-Positionen](<png/Изображение ChatGPT 14 нояб. 2026 г., 14_16_33.png>)

</details>

---

<a id="technology-stack"></a>

## 🛠 Technologien

| **Aufgabe** | **Technologie** | **Verwendung** |
| --- | --- | --- |
| Bericht und Datenmodell | Power BI Desktop | Interaktive Analyse und lokale PBIX-Datei |
| Datenaufbereitung | Power Query / M | Excel-Import, Datentypen und deutsche Textwerte |
| Kennzahlen | DAX | Aggregationen, Filterlogik und Zeitintelligenz |
| Datenquelle | Microsoft Excel | Arbeitsmappe mit Stamm- und Bewegungsdaten |
| Dokumentation | Markdown, PNG und PDF | Projektbeschreibung und Vorschau ohne Power BI |

---

<a id="data-model"></a>

## 🗃️ Datenmodell

Die Excel-Arbeitsmappe enthält sieben Datentabellen und ein ergänzendes Datenwörterbuch. Im Power-BI-Modell kommen unter anderem die Kalendertabelle `Calendar` und die Kennzahlentabelle `Kennzahlen` hinzu.

| **Quelltabelle** | **Zeilen** | **Inhalt** |
| --- | ---: | --- |
| `Items` | 30 | Artikel, Kategorien, Melde- und Mindestbestände |
| `Suppliers` | 10 | Lieferanten und Länder |
| `Warehouses` | 3 | Lagerstandorte |
| `PurchaseOrders` | 240 | Bestellungen, Mengen, Preise und erwartete Lieferdaten |
| `Receipts` | 240 | Wareneingänge mit tatsächlichen Lieferdaten |
| `Issues` | 700 | Warenausgänge |
| `StockSnapshots` | 1.890 | Bestandsaufnahmen je Artikel, Lager und Stichtag |
| `DataDictionary` | 4 | Ergänzende Erläuterungen zur Datenquelle |

```text
Excel-Arbeitsmappe
       ↓
Power Query: Import, Datentypen und Textübersetzung
       ↓
Datenmodell: Stammdaten, Bewegungsdaten und Kalender
       ↓
DAX: Bestände, Einkauf, Lieferleistung und Bestandsrisiken
       ↓
Vier interaktive Berichtsseiten
```

Artikel und Lager sind über `SKU` beziehungsweise `WarehouseID` mit den zugehörigen Bewegungsdaten verbunden. Lieferanten werden über `SupplierID` den Bestellungen zugeordnet. Zwischen `PurchaseOrders` und `Receipts` besteht im vorliegenden Modell eine bidirektionale 1:1-Beziehung über `OrderID`.

`Calendar` umfasst den Zeitraum **01.01.2025 bis 31.12.2026**. Aktive Datumsbeziehungen bestehen zu Bestellungen, Warenausgängen und Bestandsaufnahmen. Die Beziehung zum Wareneingangsdatum ist inaktiv und wird in der Kennzahl `Wareneingang (Stk.)` gezielt mit `USERELATIONSHIP` verwendet.

---

<a id="technical-notes"></a>

## 📐 Kennzahlen und DAX

### Definitionen

| **Kennzahl / Regel** | **Berechnungslogik** |
| --- | --- |
| Bestellwert | Summe von `OrderedQty × UnitPrice` |
| Lieferquote | Summe von `ReceivedQty` ÷ Bestellmenge im aktuellen Filterkontext |
| Verspätete Lieferung | `ReceiptDate > ExpectedDate` |
| Pünktlichkeit | `(Bestellanzahl − verspätete Lieferungen) ÷ Bestellanzahl` |
| Durchschnittliche Verspätung | Mittelwert von `ReceiptDate − ExpectedDate`, nur für verspätete Lieferungen |
| Lagerbestand | Summe von `ClosingQty` am letzten Datum mit Bestandsdaten innerhalb des aktuellen Filterkontexts |
| Nachbestellbedarf | Lagerbestand ≤ Meldebestand (`ReorderPoint`) |
| Kritischer Bestand | Lagerbestand ≤ Mindestbestand (`MinStock`) |
| Bestandsstatus | Zuerst „Kritisch“, danach „Nachbestellung nötig“, sonst „Normal“ |
| Einkaufswert YTD | Bestellwert vom Jahresbeginn bis zum betrachteten Datum |
| Vorjahreswert | Bestellwert im entsprechenden Zeitraum des Vorjahres |
| Veränderung zum Vormonat | `(aktueller Bestellwert − Vormonatswert) ÷ Vormonatswert` |

Nachbestell- und kritische Positionen werden über die Kombinationen aus **Artikel und Lager** gezählt. Sie sind keine Anzahl unterschiedlicher Artikel. Die Statusanzeige ist für einzelne Artikel-Lager-Zeilen vorgesehen.

<details>
<summary>🧮 DAX-Beispiele aus dem Bericht</summary>

```dax
Bestellwert (UAH) =
SUMX(
    PurchaseOrders,
    PurchaseOrders[OrderedQty] * PurchaseOrders[UnitPrice]
)
```

```dax
Lagerbestand (Stk.) =
CALCULATE(
    SUM(StockSnapshots[ClosingQty]),
    LASTNONBLANK(
        Calendar[Date],
        CALCULATE(COUNTROWS(StockSnapshots))
    )
)
```

```dax
Kritische Positionen =
SUMX(
    CROSSJOIN(
        VALUES(Items[SKU]),
        VALUES(Warehouses[WarehouseID])
    ),
    [Kritischer Bestand]
)
```

</details>

### Prüfung

Die Gesamtwerte für Bestellungen, Mengen, Bestellwert, Liefertermine und Bestandsrisiken wurden mit der Excel-Quelle abgeglichen. Alle vier Berichtsseiten wurden auf Darstellung und Beschriftung geprüft. Die gespeicherte deutsche PBIX-Datei wurde erneut geöffnet; die eingebetteten Daten wurden dabei erfolgreich geladen.

---

<a id="project-structure"></a>

## 📁 Projektdateien

Die wichtigsten Dateien im Projektordner:

```text
Курсовой проект/
├── README.md
├── Supply_Chain_Analytics_DE.pbix
├── Supply_Chain_Analytics.pbix
├── 03_Inventory_Procurement.xlsx
├── Supply_Chain_Analytics_Analytischer_Bericht_DE.pdf
└── png/
    └── Vier PNG-Vorschauen der Berichtsseiten
```

| **Datei / Ordner** | **Zweck** |
| --- | --- |
| [Supply_Chain_Analytics_DE.pbix](Supply_Chain_Analytics_DE.pbix) | Deutsche Berichtsversion mit eingebetteten Daten |
| [Supply_Chain_Analytics.pbix](Supply_Chain_Analytics.pbix) | Unveränderte ursprüngliche Berichtsversion |
| [03_Inventory_Procurement.xlsx](03_Inventory_Procurement.xlsx) | Datenquelle für eine erneute Aktualisierung |
| [Analytischer Bericht auf Deutsch](Supply_Chain_Analytics_Analytischer_Bericht_DE.pdf) | Begleitende Auswertung als PDF |
| [png/](png/) | Vorhandene Vorschauen der vier Berichtsseiten |

---

<a id="getting-started"></a>

## ▶️ Erste Schritte

### Bericht ansehen

1. `Supply_Chain_Analytics_DE.pbix` herunterladen.
2. Die Datei in Power BI Desktop unter Windows öffnen.
3. Zwischen den vier Berichtsseiten wechseln und die verfügbaren Filter verwenden.

Die gespeicherten Daten sind bereits im Bericht enthalten. Für die erste Betrachtung ist keine Aktualisierung der Excel-Quelle erforderlich. Ohne Power BI können die eingebundenen PNG-Vorschauen und der PDF-Bericht genutzt werden.

### Daten aktualisieren

1. Die Arbeitsmappe `03_Inventory_Procurement.xlsx` lokal bereitstellen.
2. Im Power-Query-Editor die Excel-Quellpfade der importierten Tabellen auf den eigenen Speicherort anpassen.
3. Änderungen übernehmen und den Bericht aktualisieren.
4. Prüfen, ob Kennzahlen und Bestandsstichtag den erwarteten Daten entsprechen.

> [!TIP]
> Die Quelldatei ist derzeit über einen absoluten lokalen Pfad eingebunden. Beim Umzug auf einen anderen Computer muss dieser Pfad angepasst werden. Für die Bilddarstellung auf GitHub muss der Ordner `png` zusammen mit dem README im gleichen relativen Pfad vorhanden sein.

---

<a id="known-limitations"></a>

## ⚠️ Einordnung und Grenzen

- **Lernprojekt:** Die Ergebnisse beziehen sich auf den bereitgestellten Übungsdatensatz. Eine gemessene betriebliche Einsparung oder Verbesserung wird damit nicht nachgewiesen.
- **Unterschiedliche Zeitstände:** Bestellungen reichen bis zum 02.10.2026, Wareneingänge bis zum 18.10.2026 und Bestandsaufnahmen bis zum 01.09.2026. Der Lagerbestand ist deshalb keine Echtzeitbestandszahl.
- **Lieferlogik:** Die Pünktlichkeitskennzahl setzt die vorliegende Zuordnung von einer Bestellung zu einem Wareneingang voraus. Für Teillieferungen, mehrere Eingänge pro Bestellung oder offene Bestellungen müsste die Logik angepasst werden.
- **Währung:** Alle Geldbeträge bleiben in ukrainischen Hrywnja (`UAH`). Die deutsche Übersetzung beinhaltet keine Umrechnung in Euro.
- **Zeitvergleiche:** YTD- und Vorjahreswerte hängen vom gewählten Zeitraum ab. Fehlende Vergleichszeiträume können leere Werte ergeben; YTD-Werte sollten nicht über Monatszeilen aufsummiert werden.
- **Sprache:** Berichtsüberschriften, Kategorien und Statuswerte wurden ins Deutsche übertragen. Automatische Filter- und Summenbeschriftungen können weiterhin der eingestellten Anwendungssprache folgen.
- **Zugriff:** Ein im Bericht vorhandener Power-BI-Service-Link ist kein Nachweis eines öffentlich zugänglichen Dashboards. Für die lokale Betrachtung ist die PBIX-Datei vorgesehen.
