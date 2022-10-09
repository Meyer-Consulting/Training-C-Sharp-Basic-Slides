# Patterns


Mit dem `is`-Operator kann man die prüfen ob eine Referenzumwandlung möglich ist.

```csharp
if (obj is string s) 
    Console.WriteLine(s.Length);
```

Hier kommt ein **Pattern** zum Einsatz.


### var-Pattern

Das `var`-Pattern ist eine Abwandlung des Typmusters, bei dem der Typnamen durch das Schlüsselwort `var` ersetzt wird.

Hier liegt der Zweck vor allem darin, die folgende Variable wiederzuverwenden

```csharp
bool IsJanetOrJohn(string name) => 
    name.ToUpper() is var upper && (upper == "JANET" || upper == "JOHN");

// das entspricht:

bool IsJanetOrJohn(string name) {
    string upper = name.ToUpper();
    return upper == "JANET" || upper == "JOHN"; 
}
```


### Konstanten-Pattern

Mit dem **Konstantenmuster** kann direkt mit einer Konstanten vergliechen werden, was bei der Arbeit mit dem Typ `object` nützlich ist:

```csharp
void Foo (object obj) {
    if (obj is 3) ... 
}

// das entspricht:

obj is int && (int)obj == 3
```


### Relationale Pattern

Seit C# 9 können die Operatoren <, >, <= und => in Mustern eingesetzt werden:

```csharp
if (x is > 100) 
    Console.Write("x ist größer als 100");
```

Richtig nützlich wird dies in einem `switch`:

```csharp
string GetWeightCategory (decimal bmi) => bmi switch {
    < 18.5m => "untergewichtig", 
    < 25m => "normal",
    < 30m => "übergewichtig",
    _ => "adipös"
};
```


### Logische Pattern

Seit C# 9 können die Schlüsselwörter `and`, `or` und `not` verwendet werden, um Muster zu kombinieren:

```csharp
bool IsJanetOrJohn(string name) => name.ToUpper() is "JANET" or "JOHN";
bool Between1And9(int n) => n is >= 1 and <= 9;
```


### Eigenschaftsmuster

Ein **Eigenschaftsmuster** findet (seit C# 8) einen oder mehrere Eigenschaftswerte eines Objekts:

```csharp
if (obj is string { length:4 }) ...

// das entspricht:

if (obj is string s && s.Length == 4) ...
```