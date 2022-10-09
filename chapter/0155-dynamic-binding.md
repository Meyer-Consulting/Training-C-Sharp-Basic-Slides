# Die dynamische Bindung


Die **Dynamische Bindung** schiebt die Bindung – den Vorgang der Auflösung von Typen, Membern und Operatoren – **von der Kompilierzeit bis zur Laufzeit hinaus**.

Das ist praktisch wenn man zur Komplierungszeit weiß das eine bestimmte Funktion/Member existiert, während der Compiler das nicht weiß.

Dies kann passieren wenn man z.B. mit COM arbeitet oder Reflection nutzt.


### Syntax

Ein dynamischer Typ wird mit dem Schlüsselwort `dynamic` deklariert:

```csharp
dynamic d = GetSomeObject(); 
d.Quack();
```


### Statische Bindung vs. dynamische Bindung

Wenn der Compiler den folgenden Ausdruck kompilieren soll, muss er die Implementierung einer Methode namens `Quack` finden:

```csharp
d.Quack();
```

Nehmen wir an, der statische Typ von `d` ist `Duck`:

```csharp
Duck d = ... 
d.Quack();
```

Der Compiler schaut ob `Duck` eine Implementierung für `Quack` hat. Wenn ja, wird der Aufruf kompiliert. Wenn nicht, wird ein Fehler geworfen. Das ist eine **statische Bindung**. 


Ändern wir nun den statischen Typ von `d` in `object`:

```csharp
object d = ... 
d.Quack();
```

Der Aufruf von `Quack` für zu einem Kompilierungsfehler, da der in d gespeicherte Wert zwar eine Methode namens Quack besitzen kann, der Compiler das aber nicht wissen kann.

Wenn der Typ nun auf `dynamic` geändert wird, sagen wir dem Compiler das er uns "vertrauen" kann und die Funktion `Quack` zur Laufzeit existiert.

```csharp
dynamic d = ... 
d.Quack();
```


### Benutzerdefinierte Bindung

Benutzerdefinierte Bindung erfolgt, wenn ein dynamisches Objekt `IDynamicMetaObjectProvider` implementiert.

```csharp
public class Duck : DynamicObject // in System.Dynamic 
{
    public override bool TryInvokeMember(InvokeMemberBinder binder, object[] args, out object result)
    {
        Console.WriteLine(binder.Name + " wurde gerufen"); 
        result = null;
        return true;
    }
}

dynamic d = new Duck();
d.Quack(); 
d.Waddle(); 
```

Die Klasse `Duck` hat eigentlich keine `Quack`-Methode. Stattdessen nutzt sie benutzerdefinierte Bindung, um alle Methodenaufrufe abzufangen und zu interpretieren.


### Sprachbindung

Sprachbindung findet statt, wenn ein dynamisches Objekt IDynamicMetaObjectProvider **nicht** implementiert. 

Sprachbindung ist praktisch, wenn man mit schlecht entworfenen Typen arbeiten muss oder eine Möglichkeit sucht, die Grenzen des .NET-Typsystems auszuhebeln.

```csharp
dynamic Mean (dynamic x, dynamic y) => (x+y) / 2;

int x = 3, y = 4; 
Console.WriteLine(Mean(x, y));
```

**Der Vorteil:** Der Code muss nicht mehr für jeden numerischen Typ einzeln geschrieben werden.

**Der Nachteil:** Typsicherheit geht verloren und man riskiert Laufzeifehler anstelle von Kompilierungsfehlern.


### RuntimeBinderException

Wenn ein Member **nicht** gebunden werden kann, wird eine `RuntimeBinderException` ausgelöst.

```csharp
dynamic d = 5;
d.Hello(); // löst RuntimeBinderException aus
```


### Dynamische Konvertierungen

Der Typ dynamic bietet **implizite Konvertierungen** in alle anderen Typen und zurück.

```csharp
int i = 7;
dynamic d = i;
long l = d; // Okay - implizite Konvertierung klappt 
short j = d; // RuntimeBinderException
```


### var vs. dynamic

Die Typen `var` und `dynamic` weisen eine oberflächliche Ähnlichkeit auf, aber die Unterschiede gehen tief:

**var**: Der Compiler bestimmt den Typ der Variable.
**dynamic**: Der Compiler weiß nicht den Typ der Variable. Die Laufzeit bestimmt den Typ der Variable.

```csharp
dynamic x = "Hallo"; 
var y = "Hallo"; 
int i = x; // Laufzeifehler
int j = y; // Kompilierungsfehler
```