# Attribute


**Attribute** sind ein Erweiterungsmechanismus, um Codeelementen (Assemblies, Typen, Mem- bern, Rückgabewerten und Parametern) **eigene Informationen hinzuzufügen**.


### Attributklassen

Ein Attribut wird durch eine Klasse definiert, die von der abstrakten Klasse `System.Attribute` erbt.

```csharp
public sealed class ObsoleteAttribute : Attribute {...}
```

Um einem Codeelement ein Attribut zuzuweisen, muss der **Attributname** in eckigen Klammern angegeben werden.

```csharp
[ObsoleteAttribute]
public class Foo {...}

// oder

[Obsolete]
public class Foo {...}
```


### Benannte und positionelle Attributparameter

Attribute können Parameter haben.

Attributparameter lassen sich immer einer von zwei Kategorien zuordnen: **positioniert** und **benannt**.

Das Attribut `XmlElementAttribute` bietet z.B. verschiedene Parameter an. 

```CSharp

[XmlElement("Customer", Namespace="http://oreilly.com")]
public class CustomerEntity { ... }
```


### Attributziele

Das Ziel eines Attributs ist das **Codeelement**, vor dem es steht.

Ein Attribute kann aber auch einem **Assembly** zugewiesen werden.

```csharp
[assembly:AssemblyVersionAttribute("4.3.2.1")]
```


### Angeben mehrerer Attribute

Es können **mehrere Attribute** für ein einzelnes Codeelement angegeben werden.

```csharp
[Serializable, Obsolete, Conditional("DEBUG")] 
public class Bar {...}

// oder

[Serializable] [Obsolete] [Conditional("DEBUG")] 
public class Bar {...}
```


### Schreiben eigener Attribute

Eigene Attribute können erstellt werden, in dem man von `System.Attribute` ableitet.

```csharp
[AttributeUsage (AttributeTargets.Method)] 
public sealed class TestAttribute : Attribute {
    public int Repetitions; 
    public string FailureMessage;

    public TestAttribute() : this(1) { } 
    public TestAttribute (int repetitions) => Repetitions = repetitions; 
}
```

Verwendung erfolgt dann wie folgt:

```csharp

class Foo {
    [Test]
    public void Method1() { ... }

    [Test(20)]
    public void Method2() { ... }
    
    [Test(20, FailureMessage="Debugging Time!")]
    public void Method3() { ... } 
}
```