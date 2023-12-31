# DestatisData-to-PowerBI

## Getting Data from Federal Statistical Office of Germany (Statistisches Bundesamt, Destatis)

### Erzeugerpreisindex gewerblicher Produkte: Deutschland, Monate, Güterverzeichnis (GP2009 2-6-Steller Hierarchie)
Example from https://www.yodabi.com/destatis-genesis-and-power-bi/

Destatis Table-Code `61241-0006`

Power Query Formula
```
let
    Source = Json.Document(Web.Contents("https://www-genesis.destatis.de/genesisWS/rest/2020/data/table?username=<USERNAME>&password=<PASSWORD>&name=61241-0006&area=all&compress=false&transpose=false&startyear=2022&endyear=2022&timeslices=&regionalvariable=&regionalkey=&classifyingvariable1=&classifyingkey1=&classifyingvariable2=&classifyingkey2=&classifyingvariable3=&classifyingkey3=&job=false&stand=&language=de")),
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
    Source = Json.Document(Web.Contents("https://www-genesis.destatis.de/genesisWS/rest/2020/data/table?username=<USERNAME>&password=<PASSWORD>&name=46321-0016&area=all&compress=false&transpose=false&startyear=2011&endyear=2022&timeslices=&regionalvariable=&regionalkey=&classifyingvariable1=&classifyingkey1=&classifyingvariable2=&classifyingkey2=&classifyingvariable3=&classifyingkey3=&job=false&stand=&language=de")),
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
    #"Entpivotierte Spalten" = Table.UnpivotOtherColumns(#"Umbenannte Spalten", {"Güterverzeichnis", "Produktnamen", "Hafen_ID", "Ort"}, "Jahr", "Wert"),
    #"Umbenannte Spalten1" = Table.RenameColumns(#"Entpivotierte Spalten",{{"Jahr", "Jahr"}}),
    #"Geänderter Typ2" = Table.TransformColumnTypes(#"Umbenannte Spalten1",{{"Wert", type number}}),
    #"Entfernte Fehler" = Table.RemoveRowsWithErrors(#"Geänderter Typ2", {"Wert"}),
    #"Umbenannte Spalten2" = Table.RenameColumns(#"Entfernte Fehler",{{"Produktnamen", "Produktbeschreibung"}})
in
    #"Umbenannte Spalten2"
```

## Aus- und Einfuhr (kalender- und saisonbereinigt) (Außenhandel): Deutschland, Monate, Länder und Ländergruppen
Destatis Table-Code: `51000-0021`

```
let
    Source = Json.Document(Web.Contents("https://www-genesis.destatis.de/genesisWS/rest/2020/data/table?username=DE717OZ104&password=Destatis*2023&name=51000-0021&area=all&compress=false&transpose=false&startyear=2008&endyear=2023&timeslices=&regionalvariable=&regionalkey=&classifyingvariable1=&classifyingkey1=&classifyingvariable2=&classifyingkey2=&classifyingvariable3=&classifyingkey3=&job=false&stand=&language=de")),
    #"Converted to Table" = Table.FromRecords({Source}),
    #"Expanded Object" = Table.ExpandRecordColumn(#"Converted to Table", "Object", {"Content", "Structure"}, {"Object.Content", "Object.Structure"}),
    Content = #"Expanded Object"{0}[Object.Content],
    #"Split Text" = Text.Split(Content, "#(lf)"),
    #"Converted to Table1" = Table.FromList(#"Split Text", Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Entfernte oberste Zeilen" = Table.Skip(#"Converted to Table1",5),
    #"Höher gestufte Header" = Table.PromoteHeaders(#"Entfernte oberste Zeilen", [PromoteAllScalars=true]),
    #"Geänderter Typ" = Table.TransformColumnTypes(#"Höher gestufte Header", {{";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember", type text}}),
    #"Spalte nach Trennzeichen teilen" = Table.SplitColumn(#"Geänderter Typ", ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember", Splitter.SplitTextByDelimiter(";", QuoteStyle.Csv), {
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.01", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.02", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.03", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.04", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.05", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.06", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.07", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.08", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.09", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.10", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.11", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.12", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.13", 
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.14",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.15",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.16",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.17",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.18",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.19",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.20",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.21",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.22",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.23",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.24",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.25",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.26",
        ";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.27"
        }),
    #"Geänderter Typ1" = Table.TransformColumnTypes(#"Spalte nach Trennzeichen teilen",{
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.01", type text}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.02", type text}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.03", type text}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.04", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.05", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.06", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.07", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.08", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.09", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.10", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.11", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.12", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.13", type number}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.14", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.15", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.16", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.17", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.18", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.19", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.20", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.21", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.22", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.23", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.24", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.25", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.26", type number},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.27", type number}
        }),
    #"Umbenannte Spalten" = Table.RenameColumns(#"Geänderter Typ1",{
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.01", "Jahr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.02", "Code"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.03", "Land"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.04", "Januar_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.05", "Januar_Einfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.06", "Februar_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.07", "Februar_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.08", "März_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.09", "März_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.10", "April_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.11", "April_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.12", "Mai_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.13", "Mai_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.14", "Juni_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.15", "Juni_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.16", "Juli_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.17", "Juli_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.18", "August_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.19", "August_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.20", "September_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.21", "September_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.22", "Oktober_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.23", "Oktober_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.24", "November_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.25", "November_Einfuhr"},
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.26", "Dezember_Ausfuhr"}, 
        {";;;Januar;Januar;Februar;Februar;März;März;April;April;Mai;Mai;Juni;Juni;Juli;Juli;August;August;September;September;Oktober;Oktober;November;November;Dezember;Dezember.27", "Dezember_Einfuhr"}         
        }),
    #"Removed Top Rows" = Table.Skip(#"Umbenannte Spalten",2),
    #"Removed Bottom Rows" = Table.RemoveLastN(#"Removed Top Rows",4),
    #"Entpivotierte Spalten" = Table.UnpivotOtherColumns(#"Removed Bottom Rows", {"Jahr", "Code", "Land"}, "Attribut", "Wert"),
    #"Split Column by Delimiter" = Table.SplitColumn(#"Entpivotierte Spalten", "Attribut", Splitter.SplitTextByDelimiter("_", QuoteStyle.Csv), {"Monat", "Attribut"}),
    #"Removed Errors" = Table.RemoveRowsWithErrors(#"Split Column by Delimiter", {"Wert"}),
    #"Added Custom" = Table.AddColumn(#"Removed Errors", "Ursprungsland", each "Deutschland"),
    #"Reordered Columns" = Table.ReorderColumns(#"Added Custom",{"Jahr", "Ursprungsland", "Code", "Land", "Monat", "Attribut", "Wert"}),
    #"Date Column" = Table.AddColumn(#"Reordered Columns", "Datum", each Date.FromText(Text.Combine({Text.From([Jahr], "de-DE"), [Monat]}, " ")), type date),
    #"Reordered Columns1" = Table.ReorderColumns(#"Date Column",{"Jahr", "Ursprungsland", "Code", "Land", "Monat", "Datum", "Attribut", "Wert"})
in
    #"Reordered Columns1"
```
## Find GeoCode of countries for visual mapping of transport flows
+ https://stackoverflow.com/a/59484986
+ https://www.dalesandro.net/mapping-continents-and-countries-in-power-bi/

```
let
    Source = Xml.Tables(Web.Contents("http://dev.virtualearth.net/REST/v1/Locations/)?o=xml&key=AltjrZWvNOGAYC0VVFZashw0z7wfUvJxT0UzIxIx5NC0n_sGqQneRNsRSy9reNvo")),
    #"Changed Type" = Table.TransformColumnTypes(Source,{{"Copyright", type text}, {"BrandLogoUri", type text}, {"StatusCode", Int64.Type}, {"StatusDescription", type text}, {"AuthenticationResultCode", type text}, {"TraceId", type text}}),
    ResourceSets = #"Changed Type"{0}[ResourceSets],
    ResourceSet = ResourceSets{0}[ResourceSet],
    #"Changed Type1" = Table.TransformColumnTypes(ResourceSet,{{"EstimatedTotal", Int64.Type}}),
    Resources = #"Changed Type1"{0}[Resources],
    #"Expanded Location" = Table.ExpandTableColumn(Resources, "Location", {"Name", "Point", "BoundingBox", "EntityType", "Address", "Confidence", "MatchCode", "GeocodePoint"}, {"Location.Name", "Location.Point", "Location.BoundingBox", "Location.EntityType", "Location.Address", "Location.Confidence", "Location.MatchCode", "Location.GeocodePoint"}),
    #"Location Point" = #"Expanded Location"{0}[Location.Point],
    #"Changed Type2" = Table.TransformColumnTypes(#"Location Point",{{"Latitude", type number}, {"Longitude", type number}})
in
    #"Changed Type2"
```
