# try-Anweisungen und Exceptions


Eine `try`-Anweisung definiert einen Codeblock, der einer Fehlerehandlung untersteht oder nach dem auf jeden Fall aufgeräumt werden soll.

Auf den `try`-Block müssen einer oder mehrere `catch`-Blöcke, ein abschließender `finally`-Block oder beides folgen. 

Ein `catch`-Block hat Zugriff auf ein Exception-Objekt, das Informationen über den Fehler enthält.

Ein `finally`-Block sorgt für mehr Determinismus im Programm, indem er immer ausgeführt wird.


### Aufbau

Eine `try`-Anweisung sieht so aus:

```csharp
try {
    ... // Exception wird eventuell beim Ausführen dieses Blocks geworfen
}
catch (ExceptionA ex) {
    ... // kümmere dich um Exception vom Typ ExceptionA 
}
catch (ExceptionB ex) {
    ... // kümmere dich um Exception vom Typ ExceptionB 
}
finally {
    ... // Code zum Aufräumen 
}
```


### Beispiel

Da `y` 0 ist, löst die Runtime eine `DivideByZeroException` aus und bricht damit die Ausführung des Codes ab.

```csharp
try {
    int x = 3, y = 0;
    Console.WriteLine(x / y);
}
catch(DivideByZeroException) {
    Console.Write("y darf nicht null sein. ");
}
```


## Die catch-Klausel

Eine `catch`-Klausel legt fest, welche Typen von Exceptions gefangen werden sollen.

Diese müssen entweder vom Typ `System.Exception` oder einer Subklasse davon sein.


### Exception-Filter

In einer `catch`-Klausel kann ein Exception-Filter definiert werden, indem eine `when`-Klausel hinzugefügt wird:

```csharp
catch (WebException ex)
    when (ex.Status == WebExceptionStatus.Timeout)
{
...
}
```


## Der finally-Block

Ein `finally`-Block wird immer ausgeführt – egal, ob eine Exception geworfen wurde oder nicht und ob der `try`-Block bis zum Ende lief oder nicht.

```csharp
static void ReadFile() {
    StreamReader reader = null; 
    try
    {
        reader = File.OpenText("file.txt");
        if (reader.EndOfStream) 
            return; 
        Console.WriteLine(reader.ReadToEnd());
    } finally {
        if (reader != null) reader.Dispose(); 
    }
}
```


### Die using-Anweisung

Viele Klassen kapseln nicht verwaltete (unmanaged) Ressourcen, die nach der Verwendung wieder freigegeben werden müssen.

Diese Klassen implementieren `System.IDisposable`, das eine einzige parameterlose Methode namens `Dispose` definiert, um diese Ressourcen aufzuräumen.

Die Anweisung `using` stellt eine elegante Syntax für das Aufrufen der `Dispose`-Methode eines `IDisposable`-Objekts in einem `finally`-Block bereit.


### Beispiel

```csharp
using (StreamReader reader = File.OpenText ("file.txt")) {
    ... 
}

// ist das gleiche wie:

{
    StreamReader reader = File.OpenText ("file.txt"); 
    try
    {
        ... 
    }
    finally {
        if (reader != null) 
            ((IDisposable)reader).Dispose(); 
    }
}
```


### using-Deklarationen

Lässt man die geschweiften Klammern und den Anweisungsblock nach einer using-Anweisung weg, wird diese zu einer **using-Deklaration** (seit C# 8)

```csharp
if (File.Exists ("file.txt"))
{
    using var reader = File.OpenText("file.txt"); 
    Console.WriteLine(reader.ReadLine());
    ...
}
```

Die Ressource wird dann verworfen, wenn die Ausführung den umschließenden Anweisungsblock verlässt.


## Werfen von Exceptions

Exceptions können entweder von der Laufzeitumgebung oder aus dem Anwendungscode heraus geworfen werden.

```csharp
static void Display (string name) {
    if (name == null)
        throw new ArgumentNullException (nameof (name));
    
    Console.WriteLine (name); 
}
```


### Ausdrücke werfen

Seit C# 7 kann `throw` als Ausdruck in **<span translate="no">&nbsp;Expression&nbsp;</span>-bodied Funktionen** auftauchen:

```csharp
public string Foo() => throw new NotImplementedException();
```


### Eine Exception weiterwerfen

Exceptions können gefangen und weitergeworfen werden:

```csharp
try { ... }
catch (Exception ex) {
    // Fehler protokollieren
    ...
    throw; // gleiche Exception erneut werfen
}
```

Ein weiteres verbreitetes Szenario ist das Auslösen eines spezifischeren oder aussagekräftigeren Exception-Typs:

```csharp
try {
    ... // ein Geburtsdatum aus einem XML-Element parsen 
}
catch (FormatException ex) {
    throw new XmlException("Falsches Geburtsdatum", ex); 
}
```


#### Wichtige Eigenschaften von System.Exception

**StackTrace**: Ein String, der alle Methoden repräsentiert, die aufgerufen sind – vom Ursprung der Exception bis zum `catch`-Block.

**Message**: Ein String mit einer Beschreibung des Fehlers.

**InnerException**: Eine Referenz auf die Exception, die die aktuelle Exception ausgelöst hat.


### `lock` Anweisung

Die `lock`-Anweisung ruft die Sperre für ein bestimmtes Objekt ab, führt einen Anweisungsblock aus und hebt die Sperre anschließend auf. 

Während eine Sperre aufrechterhalten wird, kann der Thread, der die Sperre aufrechterhält, die Sperre abrufen und aufheben.

```csharp
private readonly object valueLock = new object();
private int value;

public void IncreaseValue(int newValue) {
    lock (valueLock) {
        value += newValue;
    }
}
```


## 🏋️‍♀️ Übung

<a href="https://github.com/roeb/Training-C-Sharp/115-try-catch-exceptions/" target="_blank">Lambdas und Fehlerbehandlung</a>