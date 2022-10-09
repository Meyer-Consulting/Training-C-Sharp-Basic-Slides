# Delegates


Ein Delegate verbindet einen **Aufrufer** einer Methode zur Laufzeit mit seiner **Zielmethode**.

Es gibt zwei Ausprägungen bei einem Delegate: Typ und Instanz.


### Typ

Ein Delegate-Typ definiert ein Protokoll, an das sich Aufrufender und Ziel halten und das aus einer Liste von Parametertypen und einem Rückgabetyp besteht.

Die Deklaration eines Delegate-Typs beginnt mit dem Schlüsselwort `delegate`, sieht aber ansonsten wie eine abstrakte Methodendeklaration aus:

```csharp
delegate int Transformer (int x);
```


### Instanz

Eine Delegate-Instanz ist ein Objekt, das sich auf eine oder mehrere Zielmethoden bezieht, die diesem Protokoll entsprechen.

Eine Delegate-Instanz funktioniert als Delegierter für den Aufrufenden: Der Aufrufende wendet sich an das Delegate, und das Delegate ruft die Zielmethode auf.

```csharp
int Square (int x) => x * x;

Transformer t = new Transformer(Square);
int result = t.Invoke(3); 
Console.Write(result);
```


TODO!!!!