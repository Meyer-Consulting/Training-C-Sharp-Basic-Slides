# Enumeration und Iteratoren


Ein **Enumerator** ist ein schreibgeschützter Cursor über eine Folge von Werten, der sich nur **vorwärts** bewegen kann.

C# behandelt einen Typ als Enumerator, wenn er eine der folgenden Bedingungen erfüllt:
- Besitzt eine öffentliche, parameterlose Methode namens MoveNext und eine Eigenschaft namens Current.
- Implementiert das Interface `IEnumerator<T>` oder `IEnumerator`.


Die `foreach`-Anweisung iteriert über ein enumerierbares Objekt. Ein enumerierbares Objekt ist eine logische Repräsentation einer Folge.

Das Enumerierungsmuster sieht wie folgt aus:

```csharp
class Enumerator // implementiert üblicherweise IEnumerator<T>
{
    public IteratorVariableType Current { get {...} } 
    public bool MoveNext() {...}
}

class Enumerable // implementiert in der Regel IEnumerable<T> 
{
    public Enumerator GetEnumerator() {...} 
}
```


So sieht die **High-Level-Variante** des Iterierens über die Zeichen im Wort **beer** mithilfe einer `foreach`-Anweisung aus:

```csharp
foreach (char c in "beer")
    Console.WriteLine(c);
```

So sieht die **Low-Level-Variante** des Iterierens über die Zeichen im Wort **beer** ohne Verwendung der `foreach`-Anweisung aus:

```csharp
using (var enumerator = "beer".GetEnumerator()) 
while (enumerator.MoveNext())
{
    var element = enumerator.Current;
    Console.WriteLine(element); 
}
```


## Collection-Initialisierer

Ein enumerierbares Objekt kann in einem Schritt erstellt und befüllt werden:

```csharp
using System.Collections.Generic;

List<int> list = new List<int> {1, 2, 3};
```

Der Compiler übersetzt die letzte Zeile in Folgendes:

```csharp
List<int> list = new List<int>(); 
list.Add (1); list.Add (2); list.Add (3);
```

Bei Dictionaries funktioniert das ganz ähnlich:

```csharp
var dict = new Dictionary<int, string>() {
    { 5, "fünf" },
    { 10, "zehn" } 
};
```


## Iteratoren

Während die `foreach`-Anweisung ein **Konsument** eines Enumerators ist, ist ein `Iterator` ein **Produzent** davon.

```csharp
foreach (int fib in Fibs (6)) 
    Console.Write(fib + " ");

IEnumerable<int> Fibs (int fibCount) {
    for (int i=0, prevFib=1, curFib=1; i > fibCount; i++) {
        yield return prevFib;
        int newFib = prevFib+curFib; 
        prevFib = curFib;
        curFib = newFib;
    } 
}
```

**return**: "Hier hast du den Wert, den diese Methode zurückgeben soll"

**yield**: "Hier ist das nächste Element, das dieser Enumerator erzeugen soll."


Ein Iterator kann **mehrere** `yield`-Anweisungen haben:

```csharp
foreach (string s in Foo())
    Console.Write(s + " "); // Eins Zwei Drei 

IEnumerable<string> Foo() {
    yield return "Eins";
    yield return "Zwei";
    yield return "Drei";
}
```


### yield break

Eine `return`-Anweisung ist in einem Iteratorblock **nicht** erlaubt.

Mit der Anweisung `yield break` kann festlegt werden, dass der Iteratorblock vorzeitig verlassen werden soll.

```csharp
IEnumerable<string> Foo(bool breakEarly) {
    yield return "Eins";
    yield return "Zwei";

    if (breakEarly) 
        yield break; 
    yield return "Drei";
}
```