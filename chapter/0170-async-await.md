# Asynchrone Funktionen


Die Schlüsselwörter `await` und `async` unterstützen die **asynchrone Programmierung**.

Ein Programmierstil, bei dem Funktionen, die lange laufen, einen Großteil ihrer Arbeit verrichten, nachdem sie die Ausführungskontrolle an den Aufrufer zurückgegeben haben.


Aufbau einer **synchronen Funktion**, die lange läuft:

```csharp
int ComplexCalculation() {
    double x = 2;

    for (int i = 1; i < 100000000; i++)
        x += Math.Sqrt (x) / i; return (int)x;
}

int result = ComplexCalculation(); 
// Ausführung blockiert, bis die Berechnung abgeschlossen ist
Console.WriteLine(result);
```


Die CLR definiert eine Klasse namens `Task<TResult>`, um das Konzept einer Operation zu **kapseln**, die irgendwann in der **Zukunft** abgeschlossen wird. 

Ein `Task<TResult>` kann mit `Task.Run` in einem separaten Thread ausgeführt werden:

```csharp
Task<int> ComplexCalculationAsync() => Task.Run(() => ComplexCalculation());
```

Diese Methode ist **asynchron**, weil sie die Ausführung unmittelbar an den Aufrufer zurückgibt, während sie **nebenläufig** ausgeführt wird.


Damit der Aufrufer mitbekommt, wann die Berechnung abgeschlossen ist, kann er auf das `Task<TResult>` warten, indem er 'GetAwaiter()' aufruft:

```csharp
Task<int> task = ComplexCalculationAsync(); 

var awaiter = task.GetAwaiter();
awaiter.OnCompleted (() => {
    int result = awaiter.GetResult();
    Console.WriteLine (result); 
});
```


### Die Schlüsselwörter await und async

Das Schlüsselwort `await` vereinfacht den Aufruf von asynchronen Methoden.

```csharp
int result = await ComplexCalculationAsync(); 
Console.WriteLine (result);
```

Das lässt sich nur kompilieren, wenn wir die umschließende Methode mit dem Modifikator `async` versehen:

```csharp
async void Test() {
    int result = await ComplexCalculationAsync();
    Console.WriteLine(result); 
}
```

Der Modifikator `async` sagt dem Compiler, dass er `await` als **Schlüsselwort** behandeln soll und nicht als Bezeichner (vor C# 5.0 gab es noch kein async/await).


### Lokale Zustände einfangen

Die wahre Macht von `await`-Ausdrücken liegt darin, dass sie praktisch **überall** im Code erscheinen können.

```CSharp
async void Test() {
    for (int i = 0; i < 10; i++) {
        int result = await ComplexCalculationAsync();
        Console.WriteLine(result); 
    }
}
```

Beim ersten Aufruf von `ComplexCalculationAsync` kehrt die Ausführung dank `await`-Ausdruck an den Aufrufer zurück. 

Wenn die Methode fertig ist, wird die Ausführung dort wieder aufgenommen, wo sie abgebrochen wurde, und die Werte der lokalen Variablen und Schleifenzähler bleiben dabei erhalten. 

Das erreicht der Compiler dadurch, dass er derartigen Code in eine **Zustandsmaschine** übersetzt, wie er es bei Iteratoren macht.


### Asynchrone Funktionen schreiben

Bei jeder asynchronen Funktion kann der Rückgabetyp `void` durch `Task` ersetzt werden, um die Mehtode selbst **asynchron** zu machen.

```CSharp
async Task Test() {
    for (int i = 0; i < 10; i++) {
        int result = await ComplexCalculationAsync();
        Console.WriteLine(result); 
    }
}
```


### Task<TResult> zurückliefern

Es kann `Task<TResult>` geliefert werden, wenn der Methodenrumpf `TResult` liefert:

```csharp
async Task<int> GetAnswerToLife() {
    await Task.Delay(5000);
    int answer = 21 * 2;
    return answer;
}
```


### Aufruf mehrerer asynchroner Funktionen

```CSharp
async Task Go() {
    await PrintAnswerToLife();
    Console.WriteLine ("Fertig"); 
}

async Task PrintAnswerToLife() {
    int answer = await GetAnswerToLife();
    Console.WriteLine(answer); 

}
async Task<int> GetAnswerToLife() {
    await Task.Delay(5000); 
    int answer = 21 * 2; 
    return answer;
}
```


### Parallelität

Der Aufruf einer asynchronen Methode ohne `await` gestattet die parallele Ausführung des nachfolgenden Codes.

```csharp
var task1 = PrintAnswerToLife(); 
var task2 = PrintAnswerToLife(); 

await task1; 
await task2;
```

Die Klasse `Task` bietet eine statische Methode namens `WhenAll`, mit der sich das gleiche Ergebnis etwas effizienter erreichen lässt.

```CSharp
await Task.WhenAll(PrintAnswerToLife(), PrintAnswerToLife());
```


### Asynchrone Lambda-Ausdrücke

Auch **namenlose** Mehtoden können asynchron geschrieben werden. Dazu muss ihnen das Schlüsselwort `async` vorangestellt werden.

```CSharp
myButton.Click += async (sender, args) => {
    await Task.Delay(1000);
    myButton.Content = "Fertig"; 
};
```

Implementierung ohne Lambda <span translate="no">&nbsp;Expression&nbsp;</span>:

```CSharp
myButton.Click += ButtonHandler;
...
async void ButtonHandler (object sender, EventArgs args) {
    await Task.Delay (1000);
    myButton.Content = "Done"; 
};
```

