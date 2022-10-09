# Anonyme Typen


Ein anonymer Typ ist eine einfache **Klasse**, die bei **Bedarf erzeugt** wird, um ein paar Werte zu speichern.


Um einen anonymen Typ zu erzeugen, verwendet man das Schlüsselwort `new`, gefolgt von einem Objektinitialisierer, in dem die Eigenschaften und Werte angegeben werden, die der Typ enthalten wird.

```csharp
var dude = new { Name = "Bob", Age = 1 };
```


Der **Eigenschaftsname** eines anonymen Typs kann aus einem Ausdruck ermittelt werden, der selbst ein Bezeichner ist:

```csharp
int Age = 1;
var dude = new { Name = "Bob", Age };

// äquivalent zu

var dude = new { Name = "Bob", Age = Age };
```


Anonyme Typen sind **immutabel**, daher können Instanzen nach ihrem Erstellen **nicht** mehr verändert werden.

Anonyme Typen werden vor allem beim Schreiben von **LINQ-Abfragen** genutzt.