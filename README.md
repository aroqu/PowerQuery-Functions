# PowerQuery-Functions
This loads query code stored externaly into Power Query.

1. Copy the query below and paste it into the Advanced Editor of a blank query.
2. If this is your first time connecting to this data source you will need to specify how to connect. Click on 'Edit Credentials' and select 'Anonymous' then click on 'Connect'.
3. Rename this new query to 'PQF'.

```
let
    //Change the location of your Power Query Function file below
    Source = "https://raw.githubusercontent.com/aroqu/PowerQuery-Functions/main/PQF.pq",
    webData = Web.Contents(Source),
    textfile = Text.FromBinary(webData),
    functionText = List.Skip(Text.Split(textfile, "///")),
    functionNames = List.Transform(functionText, each Text.Trim(Text.BeforeDelimiter(_, "#(cr)#(lf)")) ),
    functionEvaluate = List.Transform(functionText, each Expression.Evaluate(Text.Insert(_,0,"// "), #shared)),
    Custom1 = Record.FromList(functionEvaluate, functionNames)
in
    Custom1
```

To add more queries to the PQF.pq file separate each query with 3 forward slashes '///' followed by the name of the query. Then paste the code of your query below that line.
