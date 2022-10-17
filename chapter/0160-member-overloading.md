# Überladen von Operatoren


Operatoren können überladen werden, um eine natürlichere Syntax für eigene Typen anzubieten.

Die symbolischen Operatoren, die sich überladen lassen, sind:

    + - * / ++ -- ! ~ % & | ^ == != < << >> >

Die zusammengesetzten Zuweisungsoperatoren werden automatisch überschrieben, wenn die nicht zusammengesetzten Operatoren überschrieben werden.


### Operatorfunktion

Ein Operator wird überladen, indem man eine **Operatorfunktion** deklariert. 

Eine Operatorfunktion muss:

- `static` sein
- min. einer der Operanden muss dem Typ entsprechen.


```csharp
public struct Note {
    int value;

    public Note(int semitonesFromA) => value = semitonesFromA;

    public static Note operator + (Note x, int semitones)
    {
        return new Note(x.value + semitones);
    } 
}
```

Durch dieses Überladen kann nun ein `int` zu einer Note addiert werden:

```csharp
Note B = new Note(2); 
Note CSharp = B + 2;
```

Da `+` überschrieben wurde, kann nun auch `+=` genutzt werden:

```csharp
CSharp += 2;
```


### Überladen von Gleichheits- und Vergleichsoperatoren

Gleichheits- und Vergleichsoperatoren werden häufig überschrieben, wenn man **Structs** erstellt.

Es gibt spezielle Regeln und Pflichten beim Überladen dieser Operatoren:

**Paare**: 
Der C#-Compiler erwartet, dass bei Operatoren, die logische Paare bilden, beide definiert sind. (z.B. `==` und `!=`)

**Equals und GetHashCode**:
Wenn `==` und `!=` überschrieben werden, muss auch `Equals` und `GetHashCode` überschrieben werden.

**IComparable und IComparable<T>**:
Wenn `<` und `>` überschrieben werden, muss auch `IComparable` und `IComparable<T>` implementiert werden.


```csharp
public struct Note {
    int value;

    public static bool operator == (Note n1, Note n2) => n1.value == n2.value;
    public static bool operator != (Note n1, Note n2) => !(n1.value == n2.value);

    public override bool Equals (object otherNote) {
        if (!(otherNote is Note)) return false;
            return this == (Note)otherNote; 
    }
    public override int GetHashCode() => value.GetHashCode();
}
```


## 🏋️‍♀️ Übung

<a href="https://github.com/roeb/Training-C-Sharp/160-member-overloading/" target="_blank">Dynamic Binding und Operatoren überladen</a>