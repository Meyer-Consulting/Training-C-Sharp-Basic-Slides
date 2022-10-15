# Vererbung


Eine Klasse kann von einer Klasse erben, damit die ursprüngliche Klasse erweitert oder angepasst wird.

Durch das Erben von einer Klasse können Sie die Funktionalität in dieser Klasse nutzen, statt alles von Grund auf wieder selbst aufbauen zu müssen.

Eine Klasse kann nur von **einer** anderen Klasse erben.


## Beispiel

Die Subklassen `Stock` und `House` erben das Feld Name von der Basisklasse `Asset`.

Subklassen werden auch als abgeleitete Klassen bezeichnet.

```csharp
public class Asset { public string Name; }

public class Stock : Asset {
    public long SharesOwned; 
}

public class House : Asset {
    public decimal Mortgage; 
}
```


# 👨‍🏫 Demo

Vererbung in der Praxis


## Polymorphie

Polymorphie bedeutet **Vielgestaltigkeit**

Basisklassen mit virutellen Methoden, können von abgeleiteten Klassen überschrieben werden. 

Referenzen sind **polymorph**.

Das bedeutet, dass eine Variable des Typs **x** auf Instanzen von Typen verweisen kann, die Subklassen von **x** sind.

```csharp
public static void Display (Asset asset) {
    System.Console.WriteLine(asset.Name); 
}
```


## Casting und Referenzumwandlungen

Eine Objektreferenz kann:

- implizit zu einer Basisklassenreferenz nach oben gecastet werden (**Upcast**) 
- explizit zu einer Subklassenreferenz nach unten gecastet werden
(**Downcast**).


### Upcasting

Eine **Upcast**-Operation erzeugt aus einer Subklassenreferenz eine Basisklassenreferenz:

```csharp
Stock msft = new Stock();
Asset a = msft;
```

Nach dem Upcast verweist Variable `a` immer noch auf **dasselbe** Stock-Objekt wie die Variable `msft`


### Downcasting

Eine **Downcast**-Operation erzeugt eine Subklassenreferenz aus einer Basisklassenreferenz:

```csharp
Stock msft = new Stock();
Asset a = msft; // Upcast

Stock s = (Stock)a; // Downcast

Console.WriteLine (s.SharesOwned); // <Kein Fehler>
Console.WriteLine (s == a);  // True
Console.WriteLine (s == msft); // True
```

Ein Downcast benötigt einen **expliziten** Cast.


### Der as-Operator

Der Operator `as` führt einen **Downcast** durch, der `null` als Ergebnis hat, wenn der Downcast fehlschlägt.

```csharp
Asset a = new Asset();
Stock s = a as Stock; // s ist null; keine Ausnahme
```


### Der is-Operator

Der `is`-Operator prüft, ob eine Referenzumwandlung erfolgreich wäre.

```csharp
if (a is Stock) 
    Console.Write(((Stock)a).SharesOwned);
```

Seit C# 7 kann im Rahmen des `is`-Operators eine Variable eingeführt werden:

```csharp
if (a is Stock s)
    Console.WriteLine(s.SharesOwned);
```


## Virtuelle Funktions-Member

Eine Funktion, die als `virtual` gekennzeichnet ist, kann von Subklassen **überschrieben** werden, die eine speziellere Implementierung bereitstellen wollen.

**Methoden**, **Eigenschaften** und **Events** können als `virtual` deklariert werden:

```csharp
public class Asset {
    public string Name;
    public virtual decimal Liability => 0; 
} 
```

Eine Subklasse überschreibt eine virtuelle Methode, indem sie den Modifikator `override` angibt:

```csharp
public class House : Asset {
    public decimal Mortgage;
    public override decimal Liability => Mortgage; 
}
```


## Abstrakte Klassen und abstrakte Member

Eine Klasse, die als `abstrakt` deklariert wurde, kann man **niemals** instanziieren.

Es können nur ihre konkreten Subklassen instanziiert werden.

```csharp
public abstract class Asset {
    public abstract decimal NetValue { get; } 
}
```

Abstrakte Klassen können **abstrakte Member** deklarieren, die sich wie virtuelle Member verhalten, aber keine Standardimplementierung bereitstellen.


## Funktionen und Klassen versiegeln

Ein überschriebenes Funktions-Member kann seine Implementierung mithilfe des Schlüsselworts `sealed` **versiegeln**, um zu verhindern, dass es durch weitere Subklassen überschrieben wird.

```csharp
public class House : Asset {
    public decimal Mortgage;
    public sealed override decimal Liability { get { ... } }
}
```

Der Modifikator `sealed` kann auch selbst auf die Klasse angewandt werden, um das Erstellen von abgeleiteten Klassen zu verhindern.


## Das Schlüsselwort `base`

Das Schlüsselwort `base` entspricht dem Schlüsselwort `this`.

Es dient dem Zugriff auf ein überschriebenen Funktions-Member aus der Subklasse.

```csharp
public class House : Asset {
    public decimal Mortgage;
    public override decimal Liability => base.Liability + Mortgage; 
}
```


## Konstruktoren und Vererbung

Eine Subklasse **muss** ihre eigenen Konstruktoren definieren.

```csharp
public class Baseclass {
    public int X;
    public Baseclass () { }
    public Baseclass (int x) { this.X = x; }
}

public class Subclass : Baseclass { 
    public Subclass (int x) : base (x) { ... }
}
```

Das Schlüsselwort `base` funktioniert wie das Schlüsselwort `this`, nur dass es einen Konstruktor in der Basisklasse aufruft.


### Reihenfolge von Konstruktor- und Feldinitialisierung

Wenn ein Objekt instanziiert wird, geschieht die Initialisierung in dieser Reihenfolge:

1. Von der Subklasse zur Basisklasse
    - Felder werden initialisiert
    - Argumente für den Basisklassenkonstruktur werden ausgewertet

2. Von der Basisklasse zur Subklasse
    - Konstruktorcode wird ausgeführt


## Der Typ object

`object` (System.Object) ist die Ausgangsbasisklasse für alle Typen.

Jeder Typ kann per Upcast in ein object umgewandelt werden.


### Boxing und Unboxing

Boxing ist das Casten einer Instanz eines Werttyps auf eine Instanz eines Referenztyps.

Der Referenztyp kann entweder von der Klasse `object` oder ein Interface sein.

```csharp
int x = 9;
object obj = x; // int verpacken

int y = (int)obj; // int auspacken
```

Unboxing erfordert einen **expliziten** Cast.


### GetType-Methode und der typeof-Operator

Alle Typen in C# werden zur Laufzeit durch eine Instanz von `System.Type` repräsentiert.

Ein Objekt vom Typ `System.Type` kann man auf zweierlei Weise erhalten:

- Aufruf von `GetType` für die **Instanz**
- Einsatz des Operators `typeof` für einen **Typnamen**.

`GetType` wird zur Laufzeit ausgewertet, während `typeof` statisch beim Kompilieren ermittelt wird.

```csharp
int x = 3;

Console.Write (x.GetType().Name);
Console.Write (typeof(int).Name);
Console.Write (x.GetType().FullName);
Console.Write (x.GetType() == typeof(int));
```