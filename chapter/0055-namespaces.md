# Namensräume


Ein **Namensraum** ist eine Domäne, in der Namen von Typen **eindeutig** sein müssen.


Typen werden normalerweise innerhalb von hierarchischen Namensräumen organisiert. Das erleichtert es uns, Typen zu finden, die wir verwenden wollen.

```csharp
System.Security.Cryptography.RSA rsa = System.Security.Cryptography.RSA.Create();
```


Das Schlüsselwort `namespace` definiert einen **Namensraum**

```csharp
namespace Outer.Middle.Inner {
    class Class1 {}
    class Class2 {} 
}
```

Die Punkte im Namensraum stehen für eine Hierarchie verschachtelter Namensräume.


## Dateibezogene Namensräume

Mit C# 10 werden dateibezogene Namensräume eingeführt.

Dateibezogene Namensräume verringern überflüssigen Code und eliminieren eine unnötige Einrückungsebene.

```csharp
namespace MyNamespace; // Gilt für alles Folgende 

class Class1 {} // innerhalb von MyNamespace
class Class2 {} // innerhalb von MyNamespace
```


## Wiederholte Namensräume

Namensraumdeklaration können wiederholt werden, solange die Typnamen im Namensraum nicht für Konflikte sorgen:

```csharp
namespace Outer.Middle.Inner { class Class1 {} } 
namespace Outer.Middle.Inner { class Class2 {} }
```


## Die using-Direktive

Die using Direktive ermöglicht es uns, Typen aus einem Namensraum zu verwenden, ohne den Namensraum angeben zu müssen.

```csharp
using Outer.Middle.Inner;

class Test // Test ist im globalen Namensraum 
{
    static void Main() {
        Class1 c; // vollständig qualifizierter Name nicht erforderlich ...
    } 
}
```


## global using-Direktive

Mit C# 10 kann einer using-Direktive das Schlüsselwort global vorangestellt werden, damit die Direktive für alle Dateien im Projekt wirksam wird:

```csharp
global using System;
global using System.Collection.Generic;
```

Damit können die gebräuchlichsten Importe zentralisiert werden.


## using static

Die `using static`-Direktive importiert einen **Typ** anstelle eines **Namensraums**. 

Alle statischen Member dieses Typs können dann genutzt werden, ohne sie über den Typnamen qualifizieren zu müssen.

```csharp
using static System.Console; 
WriteLine ("Hallo");
```


## Aliase für Typen und Namensräume

Das Importieren eines **Namensraums** kann zu Kollisionen bei Typnamen führen.

Anstatt den gesamten Namensraum zu importieren, können nur die benötigten Typen importiert und jedem davon einen Aliasnamen gegeben werden.

```csharp
using PropertyInfo2 = System.Reflection.PropertyInfo; 

class Program { PropertyInfo2 p; }
```