# Tupel


**Tupel** bieten wie anonyme Typen (seit C# 7) eine einfache Möglichkeit, einen Satz von Variablen abzulegen.

Haupteinsatzgebiet von Tupeln ist die sichere Rückgabe mehrerer Werte aus einer Methode, ohne `out`-Parameter nutzen zu müssen.

```csharp
var bob = ("Bob", 23); 

Console.WriteLine (bob.Item1); // Bob 
Console.WriteLine (bob.Item2); // 23
```


Anders als bei **anonymen Typen** ist `var` bei Tupels optional.

```csharp
(string,int) bob = ("Bob", 23);
```

Tupel können als **Rückgabewert** einer Methode verwendet werden.

```csharp
(string,int) GetPerson() => ("Bob", 23);

(string,int) person = GetPerson(); 
Console.WriteLine(person.Item1); // Bob 
Console.WriteLine(person.Item2); // 23
```


### Tupel-Elemente benennen

Tupel Literate können mit Namen versehen werden:

```csharp
var tuple = (Name:"Bob", Age:23); 

Console.WriteLine(tuple.Name); // Bob 
Console.WriteLine(tuple.Age); // 23
```


### Records


Ein Datensatz oder **Record** ist (seit C# 9) eine bestimmte Form einer **Klasse** oder eines **Struct**, die dazu gedacht ist, gut mit immutablen Daten zusammenzuarbeiten. 

Datensätze sind auch hilfreich, um Typen zu erstellen, die lediglich Daten kombinieren oder vorhalten.


**Records** sind standardmäßig eine Klasse, können aber auch ein Struct sein.

```csharp
record Point { } // Point ist eine Klasse

record struct Point { } // Point ist ein Struct
```

Ein einfacher **Record** kann aus Eigenschaften und einem Konstruktor bestehen.

```csharp
record Point {
    public Point (double x, double y) => (X, Y) = (x, y);

    public double X { get; init; }
    public double Y { get; init; } 
}
```


### Parameterlisten

Eine Datensatzdefinition kann auch eine Parameterliste enthalten:

```csharp
record Point (double X, double Y) {
    ... 
}
```

Wird eine Parameterliste angegeben, werden die Eigenschaften automatisch erstellt und initialisiert.

```csharp
public Point (double X, double Y) {
    this.X = X; this.Y = Y; 
}
```


### Nicht destruktive Mutation

**Records** sind standardmäßig **immutable**. Das heißt, dass die Eigenschaften nach der Erstellung nicht mehr verändert werden können.

Sie können jedoch kopiert und verändert werden. Hier hilft das Schlüsselwort `with`.

```csharp
record Point (double X, double Y);

Point p1 = new Point (3, 3);
Point p2 = p1 with { Y = 4 };

Console.WriteLine(p2);
```


### Datensätze und Gleichheit

Wie **Structs**, **anonyme Typen** und **Tupel** bieten auch Datensätze ohne Zusatzaufwand eine strukturelle Gleichheit - zwei Datensätze sind also gleich, wenn ihre Felder gleich sind.

```csharp
record Point (double X, double Y);

var p1 = new Point (1, 2);
var p2 = new Point (1, 2);

Console.WriteLine(p1.Equals(p2)); // Wahr
Console.WriteLine(p1 == p2); // Wahr
```


Um eine eigene Gleichheitslogik zu implementieren, fügt man die `Equals` Methode hinzu.

Dabei ist es wichtig, das diese `virtual` und `public` ist.

```csharp
record Point (double X, double Y) {
    public virtual bool Equals (Point other) => 
        other != null && X == other.X && Y == other.Y;
}
```


## 🏋️‍♀️ Übung

<a href="https://github.com/roeb/Training-C-Sharp/140-tupel-records/" target="_blank">Extension Methods, Anonyme Typen, Tupel und Records</a>