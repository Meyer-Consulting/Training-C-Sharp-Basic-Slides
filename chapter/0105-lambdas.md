# Lambdas


Es wird unterschieden zwischen **Ausdruckslambdas** und **Anweisungslambdas**.


## Ausdruckslambdas

Ein Lambdaausdruck mit einem **Ausdruck** auf der **rechten Seite** des `=>`-Operators wird als Ausdruckslambda bezeichnet.

Ein **Lambda-Ausdruck** ist eine Methode ohne Namen, die anstelle einer Delegate-Instanz geschrieben wird.

Ein Lambda-Ausdruck hat die folgende Form:

```csharp
(Parameter) => Ausdruck-oder-Anweisungsblock
```


Im folgenden Beispiel ist `x => x * x` ein Lambda-Ausdruck:

```csharp
delegate int PerformCalculation(int x, int y);

PerformCalculation multiplication = x => x * x;
Console.WriteLine(multiplication(3, 3)); // 9 
```


## Anweisungslambdas

Ein Anweisungslambda ähnelt einem **Ausdruckslambda**, allerdings sind die Anweisungen in Klammern eingeschlossen.

Der Code eines **Lambda-Ausdrucks** kann statt eines Ausdrucks auch einen Anweisungsblock enthalten:

```csharp
x => { return x * x; };
```


**Lambda-Ausdrücke** werden am häufigsten mit den Func- und Action-Delegates eingesetzt 

```csharp
Func<int,int> multiplication = x => x * x;
```

In der Regel kann der Compiler den Typ der Lambda-Parameter aus dem Kontext erschließen. Wenn das nicht der Fall ist, können die Typen der Parameter explizit angeben werden:

```csharp
Func<int,int> multiplication = (int x) => x * x;
```


Dies ist ein Beispiel für einen Ausdruck, der keine Argumente übernimmt:

```csharp
Func<string> greeter = () => "Hallo Welt";
```

Mit C# 10 erlaubt der Compiler ein implizites Typisieren mit Lambda-Ausdrücken:

```csharp
var greeter = () => "Hallo Welt";
```


### Äußere Variablen übernehmen

Ein **Lambda-Ausdruck** kann beliebige Variablen referenzieren, die dort erreichbar sind, wo der Lambda-Ausdruck definiert wird.

Diese werden als äußere Variablen bezeichnet und können lokale Variablen, Parameter und Felder enthalten:

```csharp
int factor = 2;
Func<int, int> multiplier = n => n * factor; 
Console.WriteLine(multiplier (3));
```


## Statische Lambdas

Lambdas können auch statisch sein. Das bedeutet, dass sie nicht auf eine Instanz einer Klasse oder eines Structs verweisen.

```csharp
Func<int, int> multiplier = static n => n * 2;
```

Würde die statische Lambda versuchen auf eine lokale Variable zuzugreifen, kommt es zu einem Kompilierungsfehler.


## Lambda-Ausdrücke vs. lokale Methoden

Funktionalität vom Lambda Ausdrücken und lokalen Methoden unterscheiden sich in folgenden Punkten:

- Lokale Methoden können überladen werden, Lambda-Ausdrücke nicht.
- Lokale Methoden können rekursiv sein, Lambda-Ausdrücke nicht.
- Lambda Ausdrücken erlauben mit Delegate den Zugriff auf Funktionen in einer höheren Ebene.