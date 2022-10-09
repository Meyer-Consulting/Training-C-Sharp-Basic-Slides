# Anweisungen


**Funktionen** enthalten **Anweisungen**, die in der Reihenfolge abgearbeitet werden, in der sie im Quelltext erscheinen.

Ein **Anweisungsblock** ist eine Abfolge von **Anweisungen**


# Deklarationsanweisungen

Eine Variablendeklaration führt eine neue Variable ein und initiali- siert sie optional mit einem Ausdruck.

```csharp
bool rich = true, famous = false;

const double c = 2.99792458E08;
```


# Auswahlanweisungen

Auswahlanweisungen steuern den Fluss der Programmausführung mithilfe von Bedingungen.


## if Anweisung

Eine `if`-Anweisung führt eine Anweisung aus, wenn ein bool-Ausdruck wahr ist:

```csharp

if (5 < 2 * 3)
    Console.WriteLine ("true");
```

Die Anweisung kann auch ein Anweisungsblock sein:

```csharp
if (5 < 2 * 3) {
    Console.WriteLine ("true");
    Console.WriteLine ("...") 
}
```


## else Klausel

Eine `if`-Anweisung kann optional eine `else`-Klausel haben:

```csharp
if (2 + 2 == 5)
    Console.WriteLine ("Wird nicht ausgeführt");
else
    Console.WriteLine ("False");
```

In eine `else`-Klausel können weitere `if`-Anweisungen einbetten werden:

```csharp
if (2 + 2 == 5)
    Console.WriteLine ("Wird nicht ausgeführt");
else
    if (2 + 2 == 4)
        Console.WriteLine ("Wird ausgeführt");
```


## else if Klausel

Eine `if`-Anweisung kann optional eine `else if`-Klausel haben:

```csharp
if (age >= 40) 
    Console.WriteLine("Sie können Bundespräsident werden!");
else if (age >= 18)
    Console.WriteLine("Sie dürfen wählen!");
else if (age >= 16) 
    Console.WriteLine("Sie dürfen Bier trinken!");
else 
    Console.WriteLine("Sie müssen warten!");
``` 


## switch Anweisung

Eine `switch`-Anweisung führt eine Anweisung aus, wenn ein Ausdruck mit einem der `case`-Ausdrücken übereinstimmt:

```csharp
static void ShowCard (int cardNumber) {
    switch (cardNumber) {
        case 13:
            Console.WriteLine("König");
            break; 
        case 12:
            Console.WriteLine("Königin");
            break; 
        case 11:
            Console.WriteLine("Bauer");
            break;
        default:
            Console.WriteLine(cardNumber);
            break; 
    }
}
```


## switch Ausdrücke

Eine `switch`-Anweisung kann auch einen Ausdruck haben:

```csharp
string cardName = cardNumber switch {
    13 => "King", 
    12 => "Queen", 
    11 => "Jack",
    _ => "Pip card"
};
```


# 👨‍🏫 Demo

Das arbeiten mit `if`, `else` und `switch`


# Iterationsanweisungen

C# ermöglicht es, eine Abfolge von Anweisungen mit den Anweisungen `while`, `do-while`, `for` und `foreach` wiederholt auszuführen.


## while-Schleife

`while`-Schleifen wiederholen Code, solange ein `bool`-Ausdruck wahr ist. 

```csharp
    int i = 0; 
    while (i < 3) {
        Console.Write(i++); 
    }
```

`do-while`-Schleifen unterscheiden sich von `while`-Schleifen dadurch, dass sie den Ausdruck prüfen, nachdem der Anweisungsblock durchlaufen wurde.

```csharp
    int i = 0; 
    do {
        Console.Write(i++); 
    } while (i < 3);
```


## for Schleife

`for`-Schleifen sind wie `while`-Schleifen mit besonderen Klauseln für die Initialisierung und das Iterieren einer Schleifenvariablen.

```csharp
for (Initialisierung; Bedingung; Iteration) 
    Anweisung-oder-Anweisungsblock
```

**Initialisierungsklausel** wird vor dem Eintritt in die Schleife ausgeführt.

**Bedingungsklausel** ist ein bool-Ausdruck, der vor jedem Schleifendurchlauf ausgeführt wird.

**Iterationsklausel** wird nach jeder Ausführung des Schleifeninhalts ausgeführt.

```csharp
for (int i = 0; i < 3; i++) 
    Console.WriteLine(i);
```


## foreach Schleife

Die `foreach`-Schleife iteriert über jedes Element eines aufzählbaren Objekts.

Sowohl ein Array als auch ein String ist aufzählbar.

```csharp
foreach (char c in "Bier") 
    Console.WriteLine(c + " "); // B i e r
```


# Sprunganweisungen

Die C#-Sprunganweisungen sind `break`, `continue`, `goto`, `return` und `throw`.


## break

Die `break`-Anweisung beendet die Ausführung des Bodys einer Iterations- oder `switch`-Anweisung:

```csharp
int x = 0; 
while (true) {
    if (x++ > 5) 
        break; // Aus der Schleife ausbrechen. 
}

// Hier wird die Ausführung nach dem break fortgesetzt 
...
```


## continue

Die `continue`-Anweisung übergeht die restlichen Anweisungen einer Schleife und geht zur nächsten Iteration weiter.

```csharp
for (int i = 0; i < 10; i++) {
    if ((i % 2) == 0) 
        continue; 
    
    Console.Write (i + " "); // 1 3 5 7 9
}
```


## goto

Die `goto`-Anweisung übergibt die Ausführung an eine Marke in einem Anweisungsblock

```csharp
int i = 1;
startLoop:

if (i <= 5) {
    Console.Write(i + " "); 
    i++;
    goto startLoop;
}
```


## return

Die `return`-Anweisung beendet die Methode und muss einen Ausdruck liefern, dessen Typ dem Rückgabetyp der Methode entspricht, wenn die Methode nicht `void` ist:

```csharp
static decimal AsPercentage (decimal d) {
    decimal p = d * 100m;
    return p;
}
```


# 👨‍🏫 Demo

Iterationen und Sprunganweisungen verstehen