# Generics


C# verfügt über zwei separate Mechanismen, um Code zu schreiben, der mit verschiedenen Typen verwendbar ist: **Vererbung** und **Generics**.

Während bei der Vererbung die Wiederverwendbarkeit durch einen Basistyp ausgedrückt wird, geschieht das bei Generics durch ein **Template**, das Typen als **Platzhalter** enthält.

Vorteil gegenüber Vererbung: Weniger **Casting / Boxing** und mehr **Typsicherheit**.


## Generische Typen

Ein **generischer** Typ deklariert **Typparameter**.

So genannte Platzhaltertypen, die vom Anwender des generischen Typs gefüllt werden, indem er die **Typargumente** bereitstellt.


Definition einer generischen Klasse:

```csharp
public class Stack<T> {
    int position;
    T[] data = new T[100];
    public void Push (T obj) => data[position++] = obj; 
    public T Pop() => data[--position];
}
```

Nutzung der generischen Klasse:

```csharp
var stack = new Stack<int>(); 

stack.Push(5);
stack.Push(10);
int x = stack.Pop(); 
int y = stack.Pop(); 
```


`Stack<int>` gibt für den Typparameter `T` das Typargument `int` vor, wodurch implizit ein Typ erstellt wird.

```csharp
public class Stack<int> {
    int position;
    int[] data = new int[100];

    public void Push (int obj) => data[position++] = obj;
    public int Pop() => data[--position]; 
}
```


## Generische Methoden

Eine **generische Methode** deklariert Typparameter innerhalb ihrer Signatur.

```csharp
static void Swap<T> (ref T a, ref T b) {
    T temp = a; a = b; b = temp; 
}
```

Verwendung:

```csharp
int x = 5, y = 10;

Swap (ref x, ref y);
```

Es ist nicht notwendig, Typargumente an eine generische Methode zu übergeben, da der Compiler den Typ implizit ermitteln kann. 

Führt es jedoch zu einer Mehrdeutigkeit, sollte der Typ explizit angegeben werden.

```csharp
Swap<int> (ref x, ref y);
```


## Deklarieren generischer Parameter

Typparameter können bei der Deklaration von Klassen, Structs, Interfaces, Delegates und Methoden eingeführt werden.

```csharp
class Dictionary<TKey, TValue> {...}

// Nutzung
var myDict = new Dictionary<int,string>();
```


## Der generische Wert default

Das Schlüsselwort default kann genutzt werden, um den Standardwert für einen angegebenen generischen Typparameter zu erhalten.

```csharp
static void Zap<T> (T[] array) {
    for (int i = 0; i < array.Length; i++) 
        array[i] = default(T);
}
```

Seit C# 7.1 kann auch das Typargument bei `default` weggelassen werden.


## Generische Constraints

Standardmäßig kann ein Typparameter durch jeden beliebigen Typ ersetzt werden.

**Constraints** können auf einen Typparameter angewandt werden, um spezifischere Typargumente zu verlangen.

Es gibt acht Arten von Constraints:

```csharp
where T : Basisklasse // Basisklassen-Constraint
where T : Interface // Interface-Constraint
where T : class // Referenztyp-Constraint
where T : class? // (Nullbare Referenztypen)
where T : struct // Werttyp-Constraint
where T : unmanaged // ungemanagter Constraint
where T : new() //parameterloser Konstruktor Constraint
where U : T // Typ-Constraint
where T : notnull // nicht nullbare Werttypen / Referenztypen
```


Im folgenden Beispiel verlangt `GenericClass<T,U>`, dass `T` von `SomeClass` abgeleitet ist und `Interface1` implementiert, und verlangt außerdem, dass `U` einen parameterlosen Konstruktor anbietet:

```csharp
class SomeClass {} 
interface Interface1 {}

class GenericClass<T,U> where T : SomeClass, Interface1 
                        where U : new()
{ ... }
```

Constraints können überall dort angewendet werden, wo generische Parameter definiert sind, sowohl in Methoden als auch in Typdefinitionen.


## 🏋️‍♀️ Übung

<a href="https://github.com/roeb/Training-C-Sharp/090-generics/" target="_blank">Mit Interfaces, Enaums und Generics flexibler entwickeln</a>