# Lambda-Ausdrücke


Ein **Lambda-Ausdruck** ist eine Methode ohne Namen, die anstelle einer Delegate-Instanz geschrieben wird.

Im folgenden Beispiel ist `x => x * x` ein Lambda-Ausdruck:

```csharp
delegate int Transformer(int i);

Transformer sqr = x => x * x;
Console.WriteLine(sqr(3)); // 9 
```

Ein Lambda-Ausdruck hat die folgende Form:

```csharp
(Parameter) => Ausdruck-oder-Anweisungsblock
```


Der Code eines **Lambda-Ausdrucks** kann statt eines Ausdrucks auch einen Anweisungsblock enthalten:

```csharp
x => { return x * x; };
```


**Lambda-Ausdrücke** werden am häufigsten mit den Func- und Action-Delegates eingesetzt 

```csharp
Func<int,int> sqr = x => x * x;
```

In der Regel kann der Compiler den Typ der Lambda-Parameter aus dem Kontext erschließen. Wenn das nicht der Fall ist, können die Typen der Parameter explizit angeben werden:

```csharp
Func<int,int> sqr = (int x) => x * x;
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


Nacharbeiten!