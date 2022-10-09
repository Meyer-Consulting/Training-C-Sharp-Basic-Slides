# Enums


Ein **Enum** ist ein spezieller Werttyp, der es ermöglicht, eine Gruppe von benannten numerischen Konstanten zu definieren.

```csharp
public enum BorderSide { Left, Right, Top, Bottom }
```

Ein Enum kann wie folgt verwendet werden:

```csharp
BorderSide topSide = BorderSide.Top;
bool isTop = (topSide == BorderSide.Top);
```


Jeder Enum-Member hat einen zugrunde liegenden ganzzahligen Wert.

Standardmäßig haben die zugrunde liegenden Werte den Typ `int`, und den Enum-Membern werden die Konstanten 0, 1, 2 ... zugewiesen.

Der Compiler ermöglicht nur ein paar der Enum-Member explizit mit einer Zahl zu versehen.

```csharp
public enum BorderSide : byte { 
    Left=1, 
    Right, 
    Top=10, 
    Bottom 
}
```


## Enum-Konvertierungen

Eine `enum`-Instanz kann mit einem expliziten Cast auf ihren zugrunde liegenden ganzzahligen Wert und wieder zurück umgewandelt werden:

```csharp
int i = (int) BorderSide.Left; 
BorderSide side = (BorderSide)i; 
bool leftOrRight = (int)side < 2;
```


## Flag-Enums

Enum-Member können kombiniert werden. 

Um Mehrdeutigkeiten zu vermeiden, müssen den Membern eines kombinierbaren `enum` explizit Werte zugewiesen werden.

Das Attribut `Flags` sollte auf kombinierbare Enum-Typen angewendet werden, damit `ToString()` die Namen anstelle der Zahlen ausgibt.

```csharp
[Flags]
public enum BorderSides
{ None=0, Left=1, Right=2, Top=4, Bottom=8 }
```


Verwendung von kombinierten Enums mit Flags:

```csharp
BorderSides leftRight = BorderSides.Left | BorderSides.Right;

if ((leftRight & BorderSides.Left) != 0) 
    Console.WriteLine("Enthält Left"); // Enthält Left

string formatted = leftRight.ToString(); // "Left, Right"

BorderSides s = BorderSides.Left;
s |= BorderSides.Right;
Console.WriteLine(s == leftRight); // True
```


Praktisch ist es, in eine Enum-Deklaration auch Kombinations-Member direkt aufzunehmen:

```csharp
[Flags] 
public enum BorderSides {
    None=0,
    Left=1, 
    Right=2, 
    Top=4, 
    Bottom=8, 
    LeftRight = Left | Right, 
    TopBottom = Top | Bottom,
    All = LeftRight | TopBottom
}
```


## Enum-Operatoren

Die folgenden Operatoren können auf Enums angewendet werden:

    = == != < > <= >= + - ^ & | ~ += -= ++ - sizeof

Die bitweise arbeitenden, arithmetischen und Vergleichsoperatoren liefern das Ergebnis einer Verarbeitung der zugrunde liegenden ganzzahligen Werte zurück.