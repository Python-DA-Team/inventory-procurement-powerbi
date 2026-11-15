# Modellnachweis aus der veröffentlichten PBIX-Datei

Git-Blob der geprüften PBIX: `c2e660d77f5d21313a54c0eb747ac641d8e9e62f`. Stand der Dokumentation: 15.11.2026.

Die folgenden DAX- und Power-Query-Ausdrücke wurden aus dem eingebetteten Modell der veröffentlichten PBIX gelesen. Nur absolute lokale Excel-Pfade sind durch einen Platzhalter ersetzt. Die Ausdrücke sind ein lesbarer Modellnachweis, kein eigenständig ausführbares PBIP-Projekt.

## DAX-Kennzahlen

### Kennzahlen / Bestellmenge (Stk.)

```dax
SUM(PurchaseOrders[OrderedQty])
```

### Kennzahlen / Wareneingang (Stk.)

```dax
CALCULATE(
    SUM(Receipts[ReceivedQty]),
    USERELATIONSHIP(Calendar[Date], Receipts[ReceiptDate]),
    CROSSFILTER(PurchaseOrders[OrderID], Receipts[OrderID], NONE)
)
```

### Kennzahlen / Bestellwert (UAH)

```dax
SUMX(
    PurchaseOrders,
    PurchaseOrders[OrderedQty] * PurchaseOrders[UnitPrice]
)
```

### Kennzahlen / Lieferquote (%)

```dax
DIVIDE(
    SUM(Receipts[ReceivedQty]),
    [Bestellmenge (Stk.)],
    0
)
```

### Kennzahlen / Bestellanzahl

```dax
DISTINCTCOUNT(PurchaseOrders[OrderID])
```

### Kennzahlen / Verspätete Lieferungen

```dax
COUNTROWS(
    FILTER(
        Receipts,
        Receipts[ReceiptDate] > RELATED(PurchaseOrders[ExpectedDate])
    )
)
```

### Kennzahlen / Pünktliche Lieferungen (%)

```dax
DIVIDE(
    [Bestellanzahl] - [Verspätete Lieferungen],
    [Bestellanzahl],
    0
)
```

### Kennzahlen / Ø Verspätung (Tage)

```dax
AVERAGEX(
    FILTER(
        Receipts,
        Receipts[ReceiptDate] > RELATED(PurchaseOrders[ExpectedDate])
    ),
    Receipts[ReceiptDate] - RELATED(PurchaseOrders[ExpectedDate])
)
```

### Kennzahlen / Warenausgang (Stk.)

```dax
SUM(Issues[IssuedQty])
```

### Kennzahlen / Lagerbestand (Stk.)

```dax
CALCULATE(
    SUM(StockSnapshots[ClosingQty]),
    LASTNONBLANK(
        Calendar[Date],
        CALCULATE(COUNTROWS(StockSnapshots))
    )
)
```

### Kennzahlen / Letzter Bestandsstichtag

```dax
MAX(StockSnapshots[SnapshotDate])
```

### Kennzahlen / Artikelanzahl

```dax
DISTINCTCOUNT(Items[SKU])
```

### Kennzahlen / Ø Einkaufspreis (UAH)

```dax
DIVIDE(
    [Bestellwert (UAH)],
    [Bestellmenge (Stk.)],
    0
)
```

### Kennzahlen / Nachbestellbedarf

```dax
IF(
    [Lagerbestand (Stk.)] <= MAX(Items[ReorderPoint]),
    1,
    0
)
```

### Kennzahlen / Nachbestellpositionen

```dax
SUMX(
    CROSSJOIN(
        VALUES(Items[SKU]),
        VALUES(Warehouses[WarehouseID])
    ),
    [Nachbestellbedarf]
)
```

### Kennzahlen / Kritischer Bestand

```dax
IF(
    [Lagerbestand (Stk.)] <= MAX(Items[MinStock]),
    1,
    0
)
```

### Kennzahlen / Kritische Positionen

```dax
SUMX(
    CROSSJOIN(
        VALUES(Items[SKU]),
        VALUES(Warehouses[WarehouseID])
    ),
    [Kritischer Bestand]
)
```

### Kennzahlen / Bestandsstatus

```dax
IF(
    ISINSCOPE(Items[SKU]) && ISINSCOPE(Warehouses[City]),
    SWITCH(
        TRUE(),
        [Kritischer Bestand] = 1, "Kritisch",
        [Nachbestellbedarf] = 1, "Nachbestellung nötig",
        "Normal"
    )
)
```

### Kennzahlen / Einkaufswert YTD

```dax
TOTALYTD(
    [Bestellwert (UAH)],
    Calendar[Date]
)
```

### Kennzahlen / Einkaufswert Vorjahr

```dax
CALCULATE(
    [Bestellwert (UAH)],
    SAMEPERIODLASTYEAR(Calendar[Date])
)
```

### Kennzahlen / Einkaufswert Vormonat

```dax
CALCULATE(
    [Bestellwert (UAH)],
    DATEADD(Calendar[Date], -1, MONTH)
)
```

### Kennzahlen / Veränderung zum Vormonat (%)

```dax
IF(
    ISINSCOPE(Calendar[YearMonth]),
    DIVIDE(
        [Bestellwert (UAH)] - [Einkaufswert Vormonat],
        [Einkaufswert Vormonat]
    )
)
```

## Berechnete Tabellen

### DateTableTemplate_8bb3aefe-6bd0-44b3-b438-e8d3c5772efd

```dax
Calendar(Date(2015,1,1), Date(2015,1,1))
```

### LocalDateTable_25037d80-3278-413e-9d78-65f3dad3bd40

```dax
Calendar(Date(Year(MIN('PurchaseOrders'[ExpectedDate])), 1, 1), Date(Year(MAX('PurchaseOrders'[ExpectedDate])), 12, 31))
```

### Calendar

```dax
CALENDAR(DATE(2025, 1, 1), DATE(2026, 12, 31))
```

### Kennzahlen

```dax
ROW("Temp", 1)
```

## Power Query

### Issues

```powerquery
let
    Джерело = Excel.Workbook(File.Contents("<lokaler Datenordner>\03_Inventory_Procurement.xlsx"), null, true),
    tbl_03_Issues_Table = Джерело{[Item="tbl_03_Issues",Kind="Table"]}[Data],
    #"Змінений тип" = Table.TransformColumnTypes(tbl_03_Issues_Table,{{"IssueID", type text}, {"IssueDate", type date}, {"SKU", type text}, {"WarehouseID", type text}, {"IssueType", type text}, {"IssuedQty", Int64.Type}})
in
    Table.TransformColumns(#"Змінений тип", {{"IssueType", each if _ = "Продаж" then "Verkauf" else if _ = "Списання" then "Abschreibung" else if _ = "Переміщення" then "Umlagerung" else _, type text}})
```

### Items

```powerquery
let
    Джерело = Excel.Workbook(File.Contents("<lokaler Datenordner>\03_Inventory_Procurement.xlsx"), null, true),
    tbl_03_Items_Table = Джерело{[Item="tbl_03_Items",Kind="Table"]}[Data],
    #"Змінений тип" = Table.TransformColumnTypes(tbl_03_Items_Table,{{"SKU", type text}, {"ItemName", type text}, {"Category", type text}, {"MinStock", Int64.Type}, {"ReorderPoint", Int64.Type}})
in
    Table.TransformColumns(#"Змінений тип", {{"Category", each if _ = "Аксесуари" then "Zubehör" else if _ = "Електроніка" then "Elektronik" else if _ = "Офіс" then "Bürobedarf" else if _ = "Побутові товари" then "Haushaltswaren" else _, type text}, {"ItemName", each Text.Replace(_, "Номенклатура", "Artikel"), type text}})
```

### PurchaseOrders

```powerquery
let
    Джерело = Excel.Workbook(File.Contents("<lokaler Datenordner>\03_Inventory_Procurement.xlsx"), null, true),
    tbl_03_PurchaseOrders_Table = Джерело{[Item="tbl_03_PurchaseOrders",Kind="Table"]}[Data],
    #"Змінений тип" = Table.TransformColumnTypes(tbl_03_PurchaseOrders_Table,{{"OrderID", type text}, {"OrderDate", type date}, {"ExpectedDate", type date}, {"SKU", type text}, {"SupplierID", type text}, {"WarehouseID", type text}, {"OrderedQty", Int64.Type}, {"UnitPrice", type number}})
in
    #"Змінений тип"
```

### Receipts

```powerquery
let
    Джерело = Excel.Workbook(File.Contents("<lokaler Datenordner>\03_Inventory_Procurement.xlsx"), null, true),
    tbl_03_Receipts_Table = Джерело{[Item="tbl_03_Receipts",Kind="Table"]}[Data],
    #"Змінений тип" = Table.TransformColumnTypes(tbl_03_Receipts_Table,{{"ReceiptID", type text}, {"OrderID", type text}, {"ReceiptDate", type date}, {"ReceivedQty", type number}})
in
    #"Змінений тип"
```

### StockSnapshots

```powerquery
let
    Джерело = Excel.Workbook(File.Contents("<lokaler Datenordner>\03_Inventory_Procurement.xlsx"), null, true),
    tbl_03_StockSnapshots_Table = Джерело{[Item="tbl_03_StockSnapshots",Kind="Table"]}[Data],
    #"Змінений тип" = Table.TransformColumnTypes(tbl_03_StockSnapshots_Table,{{"SnapshotDate", type date}, {"SKU", type text}, {"WarehouseID", type text}, {"ClosingQty", Int64.Type}, {"MinStock", Int64.Type}, {"ReorderPoint", Int64.Type}})
in
    #"Змінений тип"
```

### Suppliers

```powerquery
let
    Джерело = Excel.Workbook(File.Contents("<lokaler Datenordner>\03_Inventory_Procurement.xlsx"), null, true),
    tbl_03_Suppliers_Table = Джерело{[Item="tbl_03_Suppliers",Kind="Table"]}[Data],
    #"Змінений тип" = Table.TransformColumnTypes(tbl_03_Suppliers_Table,{{"SupplierID", type text}, {"SupplierName", type text}, {"Country", type text}})
in
    Table.TransformColumns(#"Змінений тип", {{"SupplierName", each Text.Replace(_, "Постачальник", "Lieferant"), type text}, {"Country", each if _ = "Україна" then "Ukraine" else if _ = "Німеччина" then "Deutschland" else if _ = "Польща" then "Polen" else _, type text}})
```

### Warehouses

```powerquery
let
    Джерело = Excel.Workbook(File.Contents("<lokaler Datenordner>\03_Inventory_Procurement.xlsx"), null, true),
    tbl_03_Warehouses_Table = Джерело{[Item="tbl_03_Warehouses",Kind="Table"]}[Data],
    #"Змінений тип" = Table.TransformColumnTypes(tbl_03_Warehouses_Table,{{"WarehouseID", type text}, {"City", type text}})
in
    Table.TransformColumns(#"Змінений тип", {{"City", each if _ = "Київ" then "Kyjiw" else if _ = "Львів" then "Lwiw" else if _ = "Дніпро" then "Dnipro" else _, type text}})
```

## Beziehungen

| Von | Nach | Aktiv | Kardinalität | Filterrichtung |
| --- | --- | --- | --- | --- |
| PurchaseOrders.ExpectedDate | LocalDateTable_25037d80-3278-413e-9d78-65f3dad3bd40.Date | Ja | n:1 | Einfach |
| Issues.SKU | Items.SKU | Ja | n:1 | Einfach |
| PurchaseOrders.SKU | Items.SKU | Ja | n:1 | Einfach |
| StockSnapshots.SKU | Items.SKU | Ja | n:1 | Einfach |
| Issues.WarehouseID | Warehouses.WarehouseID | Ja | n:1 | Einfach |
| PurchaseOrders.WarehouseID | Warehouses.WarehouseID | Ja | n:1 | Einfach |
| StockSnapshots.WarehouseID | Warehouses.WarehouseID | Ja | n:1 | Einfach |
| Receipts.OrderID | PurchaseOrders.OrderID | Ja | 1:1 | Beide |
| PurchaseOrders.SupplierID | Suppliers.SupplierID | Ja | n:1 | Einfach |
| PurchaseOrders.OrderDate | Calendar.Date | Ja | n:1 | Einfach |
| Issues.IssueDate | Calendar.Date | Ja | n:1 | Einfach |
| StockSnapshots.SnapshotDate | Calendar.Date | Ja | n:1 | Einfach |
| Receipts.ReceiptDate | Calendar.Date | Nein | n:1 | Einfach |
