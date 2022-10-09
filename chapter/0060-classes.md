# Klassen


Die Klasse ist die am häufigsten verwendete Form eines Referenztyps.

```csharp
class Foo {
}
```


## Felder

Ein Feld ist eine Variable, die ein Member einer Klasse oder eines Struct ist:

```csharp
class Octopus {
    string name;
    public int age = 10;
}
```


**Felder** können die Eigenschaft `readonly` haben, um zu verhindern, dass sie verändert werden können.

Die Initialisierung eines Felds ist optional. Es hat dann seinen **Standardwert** (0, null, false)


Es können mehrer Felder des gleichen Typs kommasepariert deklariert werden:

```csharp
static readonly int legs = 8, eyes = 2;
```


## Konstanten

Eine **Konstante** ist ein Feld, dessen Wert sich nicht ändern kann.

Eine Konstante kann einen der eingebauten numerischen Typen, `bool`, `char` und `string` oder einen `enum`-Typ nutzen.

```csharp
public class Test {
    public const string Message = "Hello World";
}
```


## Methoden

Eine **Methode** führt eine Aktivität als Abfolge von Anweisungen aus. Sie kann **Eingabedaten** vom aufrufenden Code bekommen, indem **Parameter** spezifiziert werden, und **Ausgabedaten** an den Aufrufenden zurückgeben, indem ein **Rückgabetyp** angegeben wird.

Wenn eine **Methode** den Rückgabetyp `void` hat, gibt sie keinen Wert zurück.

Die **Signatur** einer Methode muss innerhalb des Typs eindeutig sein.


### <span translate="no">&nbsp;Expression&nbsp;</span>-bodied Methoden

Eine Methode, die aus einem einzelnen Ausdruck besteht:

```csharp
int Foo (int x) { return x * 2; }
```

kann kompakter als <span translate="no">&nbsp;Expression&nbsp;</span>-bodied Methode geschrieben werden:

```csharp
int Foo (int x) => x * 2;
```


### Lokale Methoden

Eine Methode kann innerhalb einer anderen Methode definiert werden:

```csharp
void WriteCubes() {
    Console.WriteLine(Cube (3));
    int Cube (int value) => value * value * value; 
}
```


## Instanzkonstruktoren

Konstruktoren führen Initialisierungscode für eine Klasse oder ein Struct aus. 

```csharp
Panda p = new Panda ("Petey"); 

public class Panda
{
    string name;

    public Panda (string n) {
        name = n;
    }
}
```


**Klassen** und **Structs** können Konstruktoren überladen. Eine Überladung ruft über das Schlüsselwort `this` eine andere auf:

```csharp
public class Wine {
    public Wine (decimal price) {...} 
    
    public Wine (decimal price, int year): this (price) {...}
}
```

💡 **Wenn ein Konstruktor einen anderen aufruft, wird der aufgerufene
Konstruktor zuerst ausgeführt!** 💡


### Implizite parameterlose Konstruktoren

Für Klassen generiert der C#-Compiler automatisch einen parameterlosen Konstruktor, wenn  keinerlei eigene Konstruktoren definiert sind.


## Dekonstruktoren

Ein Destruktor wird aufgerufen, wenn ein Objekt von der Garbage Collection entfernt wird.

ine Dekonstruktormethode muss den Namen Decon struct tragen und einen oder mehrere out-Parameter besitzen:

```csharp
class Rectangle {
    public readonly float Width, Height;
    
    public Rectangle (float width, float height) {
        Width = width; Height = height; 
    }

    public void Deconstruct (out float width, out float height) { 
        width = Width; 
        height = Height; 
    }
}
```


Der **Destruktor** wird wie folgt aufgerufen:

```csharp
var rect = new Rectangle (3, 4);
rect.Deconstruct (out var width, out var height);
```

oder ein einer kürzeren Form:

```csharp
var rect = new Rectangle (3, 4);
(float width, float height) = rect;
```

Das **Destruktoren** eine implizite Konvertierung erlauben können wir auch folgendes schreiben:

```csharp
(var width, var height) = rect;
// oder
var (width, height) = rect;
```


## Objektinitialisierer

Mit Hilfe der **Objektinitialisierer** können Felder und Eigenschaften eines Objekts direkt beim Erstellen initialisiert werden:

```csharp
public class Bunny {
    public string Name;
    public bool LikesCarrots, LikesHumans;

    public Bunny () {}
    public Bunny (string n) { Name = n; } 
}

Bunny b1 = new Bunny {
    Name="Bo", 
    LikesCarrots = true, 
    LikesHumans = false
};
```


## Die Referenz `this`

Die Referenz `this` verweist auf die Instanz selbst.

```csharp
public class Panda {
    public Panda Mate;
    
    public void Marry (Panda partner) {
        Mate = partner;
        partner.Mate = this;       
    }
}
```

Sie sorgt zudem für das Auflösen möglicher Mehrdeutigkeiten zwischen lokalen Variablen und Parametern.

```csharp
public class Test {
    string name;
    
    public Test (string name) { 
        this.name = name; 
    } 
}
```


## Eigenschaften

Eigenschaften sehen aus wie Felder, sind aber Methoden, die aufgerufen werden, wenn auf sie zugegriffen wird.

```csharp
public class Stock {
    decimal currentPrice; 
    
    public decimal CurrentPrice {
        get { return currentPrice; }
        set { currentPrice = value; } 
    }
}
```

`get` und `set` sind Eigenschafts-Accessors. Der `set` Operator ist optional.


### <span translate="no">&nbsp;Expression&nbsp;</span>-bodied Eigenschaften

Wenn eine Eigenschaft schreibgeschützt ist, kann man sie auch kompakter als **<span translate="no">&nbsp;Expression&nbsp;</span>-bodied Eigenschaft** deklarieren.

```csharp
public decimal Worth => currentPrice * sharesOwned;
```

Seit C# 7 können auch `set`-Accessoren <span translate="no">&nbsp;Expression&nbsp;</span>-bodied sein:

```csharp
decimal currentPrice, sharesOwned;

public decimal Worth {
    get => currentPrice * sharesOwned;
    set => sharesOwned = value / currentPrice;
}
```


### Automatische Eigenschaften

Eine Deklaration einer **automatischen Eigenschaft** weist den Compiler an, **Getter** und **Setter** automatisch bereitzustellen.

```csharp
public class Stock {
    public decimal CurrentPrice { get; set; } 
}
```

Der **Setter** hat als `private` oder `protected` deklariert werden.


### Eigenschaftsinitialisierer

Eigenschaften können, wie schon Felder, mit einem Eigenschaftsinitialisierer ausgestattet werden:

```csharp
public decimal CurrentPrice { get; set; } = 123;
```


### Init-Only-Setter

Seit C# 9 können Eigenschafts-Accessor statt mit `set` auch mit `init` deklariert werden:

```csharp
public class Note {
    public int Pitch { get; init; } = 20;
    public int Duration { get; init; } = 100; 
}
```

Diese **Init-Only-Eigenschaften** verhalten sich wie schreibgeschützte Eigenschaften, nur dass sie auch über einen Objektinitialisierer gesetzt werden können:

```csharp
var note = new Note { Pitch = 50 };
```


## Statische Konstruktoren

Ein statischer Konstruktor wird einmal pro **Typ** statt einmal pro **Instanz** ausgeführt. 

Pro Typ kann es nur **einen** statischen Konstruktor geben.

```csharp
class Test {
    static int maxValue;

    static Test() { 
        maxValue = 10;
        Console.Write ("Typ initialisiert"); 
    } 
}
```

Statische Konstruktoren werden automatisch aufgerufen, und zwar unmittelbar bevor der Typ genutzt wird.


## Statische Klassen

Eine Klasse, die als static markiert ist:

* kann nicht instanziiert werden
* von ihr kann nicht abgeleitet werden
* muss vollständig aus statischen Membern bestehen


## Partielle Typen

Partielle Typen ermöglichen es, eine Typdefinition aufzuteilen (auch mehrere Dateien).

Beispiel: Automatisch generierter Code aus einem Designer und manuell geschreibener Code.

```csharp
// PaymentFormGen.cs - auto-generated 
partial class PaymentForm { ... }

// PaymentForm.cs - handgeschrieben 
partial class PaymentForm { ... }
```

Jeder Teil muss die Deklaration `partial` einschließen.