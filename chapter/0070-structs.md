# Structs


Ein `struct` ähnelt einer Klasse, es gibt aber auch entscheidende Unterschiede:

- Ein `struct` ist ein Werttyp, während eine Klasse ein Referenztyp ist.
- Ein `struct` unterstützt keine Vererbung.

Ein Struct kann alle Member nutzen, die eine Klasse haben kann, mit Ausnahme von virtuellen oder geschützten (`protected`) Membern.


### Wann nutzt man einen Struct

Ein `struct` ist passend, wenn die Werttyp-Semantik gewünscht wird. Beispiele sind:

Da ein `struct` ein Werttyp ist, muss nicht bei jeder Instanziierung ein Objekt auf dem Heap erstellt werden.

Wird ein `struct` in einem Array genutzt, so wird es im Heap landen.


### Semantik beim Erzeugen eines Struct

Anders als bei Klassen muss in einem `struct` jedes Feld explizit im Konstruktor zugewiesen werden.

Neben den erstellten Konstruktoren, hat jedes `struct` einen parameterlosen Konstruktor der die Felder auf den Standardwert setzt.

```csharp
struct Point { int x, y; }

Point p = new Point(); // p.x und p.y sind 0
```

```csharp
struct Point {
    int x = 1; int y;
    public Point() => y = 1; 
}

Point p1 = new Point(); 
Point p2 = default;
```


### Structs mit readonly

Der Modifikator `readonly` kann auf ein `struct` angewandt werden, um sicherzustellen das alle Felder `readonly` sind.

```csharp
readonly struct Point {
    public readonly int X, Y; // X und Y müssen readonly sein 
}
```