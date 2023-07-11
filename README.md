# DestatisData-to-PowerBI

Destatis Table-Code `61241-0006`

Power Query Formula
```
let
    Source = Json.Document(Web.Contents("https://www-genesis.destatis.de/genesisWS/rest/2020/data/table?username=DE717OZ104&password=Steinwerder*2023&name=61241-0006&area=all&compress=false&transpose=false&startyear=2022&endyear=2022&timeslices=&regionalvariable=&regionalkey=&classifyingvariable1=&classifyingkey1=&classifyingvariable2=&classifyingkey2=&classifyingvariable3=&classifyingkey3=&job=false&stand=&language=de")),
    #"Converted to Table" = Table.FromRecords({Source}),
    #"Expanded Object" = Table.ExpandRecordColumn(#"Converted to Table", "Object", {"Content", "Structure"}, {"Object.Content", "Object.Structure"}),
    Content = #"Expanded Object"{0}[Object.Content],
    #"Split Text" = Text.Split(Content, "#(lf)"),
    #"Converted to Table1" = Table.FromList(#"Split Text", Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Entfernte oberste Zeilen" = Table.Skip(#"Converted to Table1",7),
    #"Höher gestufte Header" = Table.PromoteHeaders(#"Entfernte oberste Zeilen", [PromoteAllScalars=true]),
    #"Geänderter Typ" = Table.TransformColumnTypes(#"Höher gestufte Header", {{";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;Dezember", type text}}),
    #"Spalte nach Trennzeichen teilen" = Table.SplitColumn(#"Geänderter Typ", ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;Dezember", Splitter.SplitTextByDelimiter(";", QuoteStyle.Csv), {
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;Deze", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.1", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.2", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.3", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.4", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.5", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.6", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.7", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.8", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.9", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.10", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.11", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.12", 
        ";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.13"}),
    #"Geänderter Typ1" = Table.TransformColumnTypes(#"Spalte nach Trennzeichen teilen",{
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;Deze", type text}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.1", type text}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.2", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.3", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.4", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.5", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.6", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.7", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.8", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.9", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.10", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.11", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.12", type number}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.13", type number}
        }),
    #"Umbenannte Spalten" = Table.RenameColumns(#"Geänderter Typ1",{
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;Deze", "Gewerbliche Produkte"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.1", "Produktnamen"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.2", "Januar"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.3", "Februar"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.4", "März"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.5", "April"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.6", "Mai"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.7", "Juni"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.8", "Juli"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;De.9", "August"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.10", "September"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.11", "Oktober"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.12", "November"}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.13", "Dezember"}
        }),
    #"Entpivotierte Spalten" = Table.UnpivotOtherColumns(#"Umbenannte Spalten", {"Gewerbliche Produkte", "Produktnamen"}, "Attribut", "Wert"),
    #"Umbenannte Spalten1" = Table.RenameColumns(#"Entpivotierte Spalten",{{"Attribut", "Monate"}}),
    #"Geänderter Typ2" = Table.TransformColumnTypes(#"Umbenannte Spalten1",{{"Wert", type number}}),
    #"Entfernte Fehler" = Table.RemoveRowsWithErrors(#"Geänderter Typ2", {"Wert"}),
    #"Umbenannte Spalten2" = Table.RenameColumns(#"Entfernte Fehler",{{"Gewerbliche Produkte", "Gewerbliche Produktnummer"}})
in
    #"Umbenannte Spalten2"
```

## Umgeschlagene Güter (Binnenschifffahrt): Deutschland, Jahre, Ausgewählte Binnenhäfen, Güterverzeichnis (Abteilungen)
Destatis Table-Code `46321-0016`

```
let
    Source = Json.Document(Web.Contents("https://www-genesis.destatis.de/genesisWS/rest/2020/data/table?username=DE717OZ104&password=Steinwerder*2023&name=46321-0016&area=all&compress=false&transpose=false&startyear=2011&endyear=2022&timeslices=&regionalvariable=&regionalkey=&classifyingvariable1=&classifyingkey1=&classifyingvariable2=&classifyingkey2=&classifyingvariable3=&classifyingkey3=&job=false&stand=&language=de")),
    #"Converted to Table" = Table.FromRecords({Source}),
    #"Expanded Object" = Table.ExpandRecordColumn(#"Converted to Table", "Object", {"Content", "Structure"}, {"Object.Content", "Object.Structure"}),
    Content = #"Expanded Object"{0}[Object.Content],
    #"Split Text" = Text.Split(Content, "#(lf)"),
    #"Converted to Table1" = Table.FromList(#"Split Text", Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Entfernte oberste Zeilen" = Table.Skip(#"Converted to Table1",6),
    #"Höher gestufte Header" = Table.PromoteHeaders(#"Entfernte oberste Zeilen", [PromoteAllScalars=true]),
    #"Geänderter Typ" = Table.TransformColumnTypes(#"Höher gestufte Header", {{";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022", type text}}),
    #"Spalte nach Trennzeichen teilen" = Table.SplitColumn(#"Geänderter Typ", ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022", Splitter.SplitTextByDelimiter(";", QuoteStyle.Csv), {
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.01", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.02", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.03", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.04", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.05", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.06", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.07", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.08", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.09", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.10", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.11", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.12", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.13", 
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.14",
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.15",
        ";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.16"
        }),
    #"Geänderter Typ1" = Table.TransformColumnTypes(#"Spalte nach Trennzeichen teilen",{
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.01", type text}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.02", type text}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.03", type text}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.04", type text}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.05", type number}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.06", type number}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.07", type number}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.08", type number}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.09", type number}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.10", type number}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.11", type number}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.12", type number}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.13", type number}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.14", type number},
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.15", type number},
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.16", type number}
        }),
    #"Umbenannte Spalten" = Table.RenameColumns(#"Geänderter Typ1",{
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.01", "Güterverzeichnis"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.02", "Produktnamen"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.03", "Hafen_ID"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.04", "Ort"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.05", "2011"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.06", "2012"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.07", "2013"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.08", "2014"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.09", "2015"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.10", "2016"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.11", "2017"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.12", "2018"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.13", "2019"}, 
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.14", "2020"},
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.15", "2021"},  
        {";;;;2011;2012;2013;2014;2015;2016;2017;2018;2019;2020;2021;2022.16", "2022"}
        }),
    #"Entpivotierte Spalten" = Table.UnpivotOtherColumns(#"Umbenannte Spalten", {"Güterverzeichnis", "Produktnamen"}, "Attribut", "Wert"),
    #"Umbenannte Spalten1" = Table.RenameColumns(#"Entpivotierte Spalten",{{"Attribut", "Monate"}}),
    #"Geänderter Typ2" = Table.TransformColumnTypes(#"Umbenannte Spalten1",{{"Wert", type number}}),
    #"Entfernte Fehler" = Table.RemoveRowsWithErrors(#"Geänderter Typ2", {"Wert"}),
    #"Umbenannte Spalten2" = Table.RenameColumns(#"Entfernte Fehler",{{"Gewerbliche Produkte", "Gewerbliche Produktnummer"}})
in
    #"Umbenannte Spalten2"
```
