# ADO.NET - Datenbankzuguriff


ADO.NET stellt Zugriff auf Datenquellen wie SQL Server bereit, die durch OLE DB und ODBC verfügbar gemacht werden.

ADO.NET trennt den Datenzugriff von der Datenbearbeitung in einzelne Komponenten, die separat oder zusammen verwendet werden können. 

ADO.NET stellt die direkteste Methode des Datenzugriffs innerhalb von .NET Framework bereit. 


### Komponenten von ADO.NET

Komponenten sind für Datenmanipulation und schnelleren Datenzugriff ausgelegt. 

`Connection`, `Command`, `DataReader`, `DataAdapter` und `DataSet`  sind die Komponenten von ADO.NET, die zum Ausführen von Datenbankoperationen verwendet werden.


### Connection

Definiert das Kernverhalten der Datenbankverbindungen und stellt eine Basisklasse für datenbankspezifische Verbindungen bereit

Mögliche Connections sind: `SQLConnection`, `OracleConnection`, `OleDbConnection`, `OdbcConnection`

```csharp
using (SqlConnection connection = new SqlConnection(connectionString))  
{  
    connection.Open();  
    // Do work here.  
}  
```


### ConnectionString

Ein Connectionstring beeinhalten die Informationen, die für die Verbindung mit einer Datenquelle erforderlich sind. 

Er unterscheidet sich je nach DB Anbieter. 

```
Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;
```


### Command

Die ADO.NET `SqlCommand`-Klasse in C# wird verwendet, um die SQL-Anweisung für die SQL Server-Datenbank zu speichern und auszuführen. 

```csharp
using (SqlConnection connection = new SqlConnection(ConString))
{
    SqlCommand cm = new SqlCommand("DELETE FROM student WHERE ID=3", connection);

    connection.Open();
    var rowsAffected = cm.ExecuteNonQuery();
}
```


### DataReader

Die ADO.NET `SqlDataReader`-Klasse in C# wird verwendet, um Daten auf die effizienteste Weise aus der SQL Server-Datenbank zu lesen. 

Es liest Daten nur in Vorwärtsrichtung. 

Es gibt keine Möglichkeit, zurückzugehen und den vorherigen Datensatz zu lesen.


```csharp
using (SqlConnection connection = new SqlConnection(ConString))
{
    SqlCommand cm = new SqlCommand("select * from student", connection);

    connection.Open();

    SqlDataReader sdr = cm.ExecuteReader();
    while (sdr.Read())
    {
        Console.WriteLine(sdr["Name"] + ",  " + sdr["Email"] + ",  " + sdr["Mobile"]);
    }
}
```


### Data-Adapter

Der ADO.NET `SqlDataAdapter` in C# fungiert als Brücke zwischen einem `DataSet` und einer Datenquelle (SQL Server-Datenbank), um Daten abzurufen.

Der `SqlDataAdapter` ist eine Klasse, die eine Reihe von SQL-Befehlen und eine Datenbankverbindung darstellt. 

Es kann verwendet werden, um das DataSet zu füllen und die Datenquelle zu aktualisieren.


```csharp
using (SqlConnection connection = new SqlConnection(ConString))
{
    SqlDataAdapter da = new SqlDataAdapter("select * from student", connection);
    
    DataTable dt = new DataTable();
    da.Fill(dt);
    Console.WriteLine("Using Data Table");
    foreach (DataRow row in dt.Rows)
    {
        Console.WriteLine(row["Name"] +",  " + row["Email"] + ",  " + row["Mobile"]);
    }
    Console.WriteLine("---------------");

    DataSet ds = new DataSet();
    da.Fill(ds, "student");                   
    Console.WriteLine("Using Data Set");
    foreach (DataRow row in ds.Tables["student"].Rows)
    {
        Console.WriteLine(row["Name"] + ",  " + row["Email"] + ",  " + row["Mobile"]);
    }                     
}
```


### Datatable

Die DataTable in C# ähnelt den Tabellen in SQL.

`DataTable`stellt auch die relationalen Daten in Tabellenform dar, dh. Zeilen und Spalten, und diese Daten im Speicher gespeichert werden


### DataSet

Das DataSet-Objekt in ADO.NET ist nicht anbieterspezifisch.

Die abgerufenen Daten können dann in einem `DataSet` gespeichert werden und unabhängig von der Datenbank arbeiten.

Das `DataSet` enthält eine Sammlung von einem oder mehreren `DataTable`-Objekten.


```csharp
DataTable table1 = new DataTable();
DataTable table2 = new DataTable();

DataColumn dc11 = new DataColumn("ID", typeof(Int32));
DataColumn dc12 = new DataColumn("Name", typeof(string));
DataColumn dc13 = new DataColumn("City", typeof(string));

table1.Columns.Add(dc11);
table1.Columns.Add(dc12);
table1.Columns.Add(dc13);

table1.Rows.Add(111,"Amit Kumar", "Jhansi");
table1.Rows.Add(222, "Rajesh Tripathi", "Delhi");
table1.Rows.Add(333, "Vineet Saini", "Patna");
table1.Rows.Add(444, "Deepak Dwij", "Noida");

DataSet dset = new DataSet();
dset.Tables.Add(table1);
```


### Error Handling

Das Fehlerbehandlungsmodell von ADO.NET basiert auf .NET `Exception`

Funktionen die aufgrund eines Fehlers fehlschlagen können, müssen in einen **try/ catch/ finally** -Block eingeschlossen werden.

```csharp
try {
    using (SqlConnection connection = new SqlConnection(ConString))
   connection.Open( );

   <span translate="no">&nbsp;statement&nbsp;</span> = connection.CreateCommand( );
   <span translate="no">&nbsp;statement&nbsp;</span>.CommandText = SQL;

   resultSet = <span translate="no">&nbsp;statement&nbsp;</span>.ExecuteReader( );

} catch(SqlException e ) {
   // Notify user that an error occurred
} finally {
    // Former close connection or open readers
}
```