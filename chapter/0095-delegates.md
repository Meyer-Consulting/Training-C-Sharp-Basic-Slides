# Delegates


Ein Delegate verbindet einen **Aufrufer** einer Methode zur Laufzeit mit seiner **Zielmethode**.

Delegates sind keine Methoden sondern **Methodenverweise**

Delegaten werden verwendet, um Methoden als Argumente an anderen Methoden zu übergeben. 

Es gibt zwei Ausprägungen bei einem Delegate: Typ und Instanz.


### Typ

Ein Delegate-Typ definiert ein Protokoll, an das sich Aufrufender und Ziel halten und das aus einer Liste von Parametertypen und einem Rückgabetyp besteht.

Die Deklaration eines Delegate-Typs beginnt mit dem Schlüsselwort `delegate`, sieht aber ansonsten wie eine abstrakte Methodendeklaration aus:

```csharp
public delegate int PerformCalculation(int x, int y);
```


### Instanz

Eine Delegate-Instanz ist ein Objekt, das sich auf eine oder mehrere Zielmethoden bezieht, die diesem Protokoll entsprechen.

Eine Delegate-Instanz funktioniert als Delegierter für den Aufrufenden: Der Aufrufende wendet sich an das Delegate, und das Delegate ruft die Zielmethode auf.


```csharp
public delegate int PerformCalculation(int x, int y);

public static int PerformAddidtion(int x, int y)
{
    return x = x + y;
}

public static int PerformSubtraction(int x, int y)
{
    return x = x - y;
}

PerformCalculation handler = new PerformCalculation(PerformAddidtion);
int result = handler(3, 4);
```


Ein häufiger Einsatzzweck ist die Übergabe von eines Delegaten an eine Methode als Parameter.

```csharp
public static void MethodWithCallback(int param1, int param2, PerformCalculation callback)
{
    Console.WriteLine(callback(param1, param2));
}

MethodWithCallback(3, 4, PerformSubtraction);
```


## Multicasting Delegates

Eines der nützlichsten Merkmale von Delegates ist die Fähigkeit durch so genanntes **Multicasting** komplexe Ereignisketten zu bilden, die dann durch einen einzigen Aufruf abgearbeitet werden.

```csharp
PerformCalculation handler;
PerformCalculation additionHandler = new PerformCalculation(PerformAddidtion);
PerformCalculation subtractionHandler = new PerformCalculation(PerformSubtraction);
```

Zuerst werden **drei Verweisobjekte** erzeugt. 

Im Unterschied zum ersten Beispiel enthält `handler` aber noch keinen Verweis. `additionHandler` und `subtractionHandler` hingegen schon. 


```csharp
handler = additionHandler;
handler += subtractionHandler;
```

Hier bekommt `handler` den Verweis von `additionHandler`. Danach wird dann durch die Verwendung von `+=` eine zweite Methode angehängt.

```csharp
handler(3, 4);
```

Bei der Ausführung werden beide Implementierung der Reihe nach ausgeführt. Zuerst die Addidtion und danach die Subtraktion.


## Multicasting Delegates mir Referenzen

Im vorherigen Beispiel haben wir gesehen, das bei jedem verketteten Funktionsaufruf die Werte **3** und **4** genutzt wurden, obwohl die einzelnen Implementieren `x` das Ergebnis der Rechenoperation zugewiesen haben.

Mit einem `ref` Verweis, können wir dieses gewünschte Verhalten erreichen.


```csharp
public delegate int PerformCalculation(ref int x, ref int y);
public static int PerformAddidtion(ref int x, ref int y)
{
    return x = x + y;
}

public static int PerformSubtraction(ref int x, ref int y)
{
    return x = x - y;
}

PerformCalculation handler;
PerformCalculation additionHandler = new PerformCalculation(PerformAddidtion);
PerformCalculation subtractionHandler = new PerformCalculation(PerformSubtraction);

handler = additionHandler;
handler += subtractionHandler;

int x = 3;
int y = 4;

Console.WriteLine(handler(ref x, ref y));
```