# Interfaces


Ein **Interface** ähnelt einer Klasse, stellt aber für ihre Member nur eine Spezifikation und keine Implementierung bereit!

Interface-Member sind implizit `public` und dürfen keine Zugriffsmodifikatoren deklarieren.

Implementiert man ein Interface, muss man eine `public` Implementierung für alle seine Member bereitstellen.


Ein Interface hat folgende Besonderheiten:

- Interface-Member sind alle **implizit abstrakt**. Im Gegensatz dazu kann eine Klasse sowohl abstrakte als auch konkrete Member mit Implementierungen bereitstellen.
- Eine Klasse kann mehrere Interfaces implementieren. Im Gegensatz dazu kann sie nur von einer einzigen Klasse erben. Structs können Interfaces implementieren, aber nicht von einer Klasse erben.


## Beispiel

```csharp
public interface IEnumerator {
    bool MoveNext();
    object Current { get; } 
    void Reset();
}

internal class Countdown : IEnumerator {
    int count = 6;
    public bool MoveNext() => count-- > 0 ; 
    public object Current => count;
    public void Reset() => count = 6;
}
```


Ein Objekt kann jederzeit implizit auf sein Interface gecastet werden, das es implementiert.

```csharp
IEnumerator e = new Countdown(); 
while (e.MoveNext())
    Console.Write(e.Current + " ");
```


## Erweitern eines Interface

Interfaces können von anderen Interfaces abgeleitet werden:

```csharp
public interface IUndoable { void Undo(); } 
public interface IRedoable : IUndoable { void Redo(); }
```


## Explizite Implementierung eines Interface

Das Implementieren mehrerer Interfaces kann manchmal zu einem Konflikt zwischen den Member-Signaturen führen.

Solche Konflikte können durch die explizite Implementierung eines Interface gelöst werden:

```csharp
interface I1 { void Foo(); } 
interface I2 { int Foo(); }

public class Widget : I1, I2 {
    public void Foo()
    {
        Console.Write("Widget-Implementierung von I1.Foo"); 
    }

    int I2.Foo()
    {
        Console.Write("Widget-Implementierung von I2.Foo");
        return 42; 
    }
}
```

So erfolgt der Aufruf:

```csharp
Widget w = new Widget();
w.Foo(); 
((I1)w).Foo(); 
((I2)w).Foo();
```


## Default-Interface-Member

Ab C# 8 können Sie Interface-Member mit einer Default-Implementierung versehen und damit eine eigene Implementierung optional machen:

```csharp
interface ILogger {
    void Log (string text) => Console.WriteLine(text); 
}
```

Default-Implementierungen sind immer explizit – definiert also eine Klasse, die ILogger implementiert, keine Log-Methode, kann diese nur über das Interface aufgerufen werden:

```csharp
class Logger : ILogger { }
...
((ILogger)new Logger()).Log("message");
```