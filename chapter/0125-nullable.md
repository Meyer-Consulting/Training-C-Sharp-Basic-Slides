# Nullbare Werttypen


**Referenztypen** können einen nicht existierenden Wert durch eine **Nullreferenz** darstellen.

```csharp
string s = null; // Okay - Referenztyp 
int i = null; // Kompilierungsfehler - int kann nicht null sein
```

Um `null` (nicht 0) in einem **Werttyp** zu repräsentieren, muss ein spezielles Konstrukt `?` namens nullbarer Typ genutzt werden.

```csharp
int? i = null; // Okay - nullbarer Typ 
Console.WriteLine(i == null); // True
```


## Das Struct Nullable<T>

`T?` wird in `System.Nullable<T>` umgewandelt. `Nullable<T>` ist ein leichtgewichtiges, unveränderliches **Struct**, das nur zwei Felder für Value und HasValue besitzt.

```csharp
public struct Nullable<T> where T : struct {
    public T Value {get;}
    public bool HasValue {get;}
    public T GetValueOrDefault();
    public T GetValueOrDefault (T defaultValue); 
    ...
}
```

Die Umwandlung von `int?` wäre dann:

```csharp
int? i = null; 
Console.WriteLine(i == null);

// in:

Nullable<int> i = new Nullable<int>(); 
Console.WriteLine(!i.HasValue);
```


## Nullbare Konvertierungen

Die Konvertierung von `T` nach `T?` ist implizit, von `T?` nach `T` dagegen explizit:

```csharp
int? x = 5; // implizit 
int y = (int)x; // explizit
```


## Nullbare Typen und Null-Operatoren

Nullbare Typen können mit den Null-Operatoren `?.` und `??` genutzt werden.

```csharp
int? x = null;
int y = x ?? 5; // y ist 5

int? a = null, 
b = null, 
c = 123; 
Console.WriteLine(a ?? b ?? c); // 123
```


Nullbare Typen sind auch gut für den Einsatz mit dem **Null-Bedingungsoperator** zu gebrauchen:

```csharp
System.Text.StringBuilder sb = null; 
int? length = sb?.ToString().Length;
```

Das kann mit dem **Null-Verbindungsoperater** verknüpft werden.

```csharp
int length = sb?.ToString().Length ?? 0;
```


# Nullbare Referenztypen


Während **nullbare Typen** Werttypen den Wert null ermöglichen, tun **nullbare Referenztypen** (seit C# 8) das Gegenteil – sie statten Referenztypen mit Nicht-Nullbarkeit aus. 

Damit sollen `NullReferenceExceptions` vermieden werden.

Nullbare Referenztypen bieten eine Sicherheitsebene, die nur durch den Compiler in Form von Warnungen umgesetzt wird.

Um **nullbare Referenztypen** zu ermöglichen, muss in der `.csproj` Datei das **Nullable** Element aufgenommen werden.

```xml
<Nullable>enable</Nullable>
```


Nach dem Aktivieren macht der Compiler die Nicht-Nullbarkeit zum Standard:

```csharp
#nullable enable // aktiviert nullbare Referenztypen 

string s1 = null; // erzeugt eine Compilerwarnung!
string? s2 = null; // okay: s2 ist nullbarer Referenztyp
```

Der folgende Code sorgt ebenfalls für eine Warnung, weil `x` nicht initialisiert wurde:

```csharp
class Foo { string x; }
```


## 🏋️‍♀️ Übung

<a href="https://github.com/roeb/Training-C-Sharp/125-nullable/" target="_blank">Enumerations & Nullable verstehen</a>