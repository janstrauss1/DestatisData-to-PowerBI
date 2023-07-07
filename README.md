# DestatisData-to-PowerBI

Destatis Table-Code `61241-0006`

Power Query Formula
```
let
    Source = Json.Document(Web.Contents("https://www-genesis.destatis.de/genesisWS/rest/2020/data/table?username=DE717OZ104&password=Steinwerder*2023&name=61241-0006&area=all&compress=false&transpose=false&startyear=&endyear=&timeslices=&regionalvariable=&regionalkey=&classifyingvariable1=&classifyingkey1=&classifyingvariable2=&classifyingkey2=&classifyingvariable3=&classifyingkey3=&job=false&stand=&language=de")),
    #"Converted to Table" = Table.FromRecords({Source}),
    #"Expanded Object" = Table.ExpandRecordColumn(#"Converted to Table", "Object", {"Content", "Structure"}, {"Object.Content", "Object.Structure"}),
    Content = #"Expanded Object"{0}[Object.Content],
    #"Split Text" = Text.Split(Content, "#(lf)"),
    #"Converted to Table1" = Table.FromList(#"Split Text", Splitter.SplitByNothing(), null, null, ExtraValues.Error),
    #"Entfernte oberste Zeilen" = Table.Skip(#"Converted to Table1",7),
    #"Höher gestufte Header" = Table.PromoteHeaders(#"Entfernte oberste Zeilen", [PromoteAllScalars=true]),
    #"Geänderter Typ" = Table.TransformColumnTypes(#"Höher gestufte Header", {
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;Dezember", type text}
        }),
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
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.11", type text}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.12", type text}, 
        {";;Januar;Februar;März;April;Mai;Juni;Juli;August;September;Oktober;November;D.13", type text}
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

Destatis Table-Code ´46321-0016´
