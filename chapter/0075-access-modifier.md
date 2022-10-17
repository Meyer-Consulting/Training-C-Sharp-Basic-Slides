# Zugriffsmodifikatoren


Um die Kapselung zu unterstützen, kann ein Typ oder Typ-Member seine **Sichtbarkeit** gegenüber anderen Typen und Assemblies einschränken, indem er seiner Deklaration einen von fünf **Zugriffsmodifikatoren** hinzufügt:


### public

Vollständig sichtbar. Der implizite Wert für Member eines Enum oder eines Interface.


### internal

Sichtbar nur innerhalb der Assembly, in der man sich befindet, und in befreundeten Assemblies. Standardwert für nicht verschachtelte Typen.


### private

Sichtbar nur für den umschließenden Typ. Standardwert für Member einer Klasse oder eines Struct.


### protected

Sichtbar nur für enthaltende Typen oder Subklassen.


### protected internal

Die Vereinigung von `protected` und `internal`.

Auf den Typ oder Member kann von beliebigem Code in derselben Assembly oder von jeder abgeleiteten Klasse in einer anderen Assembly zugegriffen werden.


### private protected

Die Schnittmenge von `protected` und `internal`.

Auf den Typ oder Member kann nur durch Code in derselben Klasse oder Struktur oder in einer abgeleiteten Klasse aus derselben Assembly, aber nicht aus einer anderen Assembly zugegriffen werden.


## Beispiele


Im folgenden Beispiel ist Class2 auch außerhalb seiner Assembly erreichbar, Class1 nicht:

```csharp
class Class1 {} 
public class Class2 {}
```


ClassB stellt das Feld x anderen Typen in der Assembly bereit, ClassA nicht:

```csharp
class ClassA { int x; } 
class ClassB { internal int x; }
```


## Friend Assemblies

`internal`-Member können für andere befreundete Assemblies sichtbar gemacht werden, indem  das Attribut `System.Runtime.CompilerServices.InternalsVisibleTo` gesetzt wird.

```csharp
[assembly: InternalsVisibleTo("Friend")]
```


## 🏋️‍♀️ Übung

<a href="https://github.com/roeb/Training-C-Sharp/075-access-modifier/" target="_blank">Zugriffsmodifikatoren, Structs & Vererbung</a>