# Variablen und Parameter


Eine **Variable** repräsentiert einen Speicherbereich, der einen veränderbaren Wert enthält. 

Eine Variable kann eine **lokale Variable**, ein **Parameter** (value, ref, out oder in), ein **Feld** (Instanz oder statisch) oder ein **Array**-Element sein.


## Der Stack und der Heap

Der Stack und der Heap sind die Orte, an denen Variablen abgelegt werden.
<br/><br/>
Der **Stack** ist der Speicher, in dem **lokale Variablen** und **Parameter** gespeichert werden.
<br/><br/>
Der **Heap** ist der Speicher, in dem **Objekte**, **statische Felder** und **Konstanten** liegen.


## Vorgabewerte

Alle Typinstanzen haben einen Vorgabewert.

Der Vorgabewert für jeden Typ, kann mit dem Schlüsselwort default angefordert werden.

```csharp
Console.WriteLine(default(decimal)); // 0 
decimal d = default;
```


## Parameter

Eine Methode kann eine Reihe von **Parametern** haben.

Mit den **Modifikatoren** `ref`, `in` und `out` kann beeinflusst werden, wie die Parameter übergeben werden.

```csharp
Foo(8); // 8 ist ein Argument 
static void Foo (int p) {...} // p ist ein Parameter
```


## Der Modifikator ref

Um eine **Referenz** zu übergeben (by Reference), stellt C# den Modifikator `ref` bereit.

```csharp
int x = 8;
Foo (ref x); // Foo direkt mit x arbeiten lassen 

Console.WriteLine (x); // x ist jetzt 9

static void Foo (ref int p) {
    p = p + 1; // p um 1 erhöhen
    Console.WriteLine (p); // p am Bildschirm ausgeben 
}
```


## Der Modifikator out

Ein `out`-Argument ist wie ein `ref`-Argument, jedoch mit folgenden Ausnahmen:

* Es muss nicht zugewiesen sein, bevor es der Funktion übergeben wird.
* Es muss zugewiesen sein, bevor die Funktion verlassen wird. 

Der Modifikator `out` wird meist genutzt, um mehrere Rückgabewerte in einer Methode zu haben.

```csharp
int.TryParse("123", out int x); 
Console.WriteLine (x);
```


## Der Modifikator in 

Seit C# 7.2 kann einem Parameter der Modifikator `in` vorangestellt werden, um zu verhindern, dass er in der Methode verändert wird.


## Der Modifikator params

Der Modifikator `params` kann für den letzten Parameter einer Methode angegeben werden.

Mit dem Modifikator `params` kann eine Methode eine beliebige Anzahl von Parametern haben.

```csharp
int Sum(params int[] ints) {
    int sum = 0;
    for (int i = 0; i < ints.Length; i++) sum += ints[i]; 
    return sum;
}
```

Diese Methode kann wie folgt aufgerufen werden:

```csharp
Console.WriteLine(Sum(1, 2, 3, 4)); // 10
```


## Optionale Parameter

**Methoden**, **Konstruktoren** und **Indexer** können optionale Parameter deklarieren.

Ein Parameter ist optional, wenn er in seiner Deklaration einen Vorgabewert definiert:

```csharp
void Foo(int x = 23) { Console.WriteLine (x); }
```

Optionale Parameter können ausgespart werden, wenn die Methode aufgerufen wird:

```csharp
Foo(); // 23
Foo(42); // 42
```


## Benannte Argumente 

Statt über die Position können Sie Argumente auch über den Namen identifizieren:

```csharp
void Foo (int x, int y) {
    Console.WriteLine (x + ", " + y); 
}

Foo (x:1, y:2); // 1, 2
```


## var

Wenn eine Variable in einem Schritt deklariert und initialisiert wird, kann der Typ weggelassen werden.

```csharp
var x = "Hallo";
var y = new System.Text.StringBuilder(); 
var z = (float)Math.PI;
```

Entspricht dem folgenden Code:

```csharp
string x = "Hallo"; 
System.Text.StringBuilder y = new System.Text.StringBuilder(); 
float z = (float)Math.PI;
```


## Mit dem Ziel typisierte new-Ausdrücke

Eine Möglichkeit, Wiederholungen im Code zu vermeiden, ist der Einsatz von mit dem Ziel typisierten new-Ausdrücken

```csharp
StringBuilder sb1 = new(); 
StringBuilder sb2 = new ("Test");

// entspricht

StringBuilder sb1 = new StringBuilder(); 
StringBuilder sb2 = new StringBuilder ("Test");
```


## Ausdrücke und Operatoren

Ein **Ausdruck** entspricht prinzipiell einem Wert.

Ausdrücke können mithilfe von Operatoren umgewandelt und kombiniert werden.

Ein **Operator** erwartet einen oder mehrere Operanden als Eingabe, um einen neuen **Ausdruck** auszugeben.

```csharp
12 * 30 // * ist ein Operator; 12 und 30 sind Operanden.
```


## Zuweisungsausdrücke

Ein Zuweisungsausdruck nutzt den Operator `=, um einer Variablen das Ergebnis eines anderen Ausdrucks zuzuweisen.

Ein Zuweisungsausdruck ist kein `void`-Ausdruck.

```csharp
x = x * 5
```


Die **zusammengesetzten Zuweisungsoperatoren** sind syntaktische Abkürzungen, die eine Zuweisung mit einem anderen Operator kombinieren:

```csharp
x *= 2 // entspricht x = x * 2 
x -= 1 // entspricht x = x - 1
```


## Priorität und Assoziativität von Operatoren

Wenn ein **Ausdruck** mehrere Operatoren enthält, bestimmen **Priorität** und **Assoziativität** die Auswertungsreihenfolge.

Operatoren mit einer höheren Priorität werden vor Operatoren mit einer niedrigeren Priorität ausgeführt. 


### Priorität

Der Ausdruck `1 + 2 * 3` wird als `1 + (2 * 3)` ausgewertet, weil `*` eine höhere Priorität hat als `+`.


### Linksassoziative Operatoren

Binäre Operatoren sind linksassoziativ.

Sie werden von links nach rechts ausgewertet.

Beispielsweise wird der Ausdruck `8/4/2` aufgrund der Linksassozia- tivität als `(8/4)/2` ausgewertet.


### Rechtsassoziative Operatoren

Die Zuweisungs- und Lambda-Operatoren, der Null-Verbindungsoperator sind rechtsassoziativ. 

Sie werden von rechts nach links ausgewertet.

Die Rechtsassoziativität ermöglicht die Kompilation von Mehrfachzuweisungen wie x=y=3: Das funktioniert, weil erst y 3 zugewiesen wird und dann das Ergebnis dieses Ausdrucks (3) x.