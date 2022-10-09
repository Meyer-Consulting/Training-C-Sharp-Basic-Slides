# XML-Dokumentation


Ein **Dokumentationskommentar** ist ein Stück eingebettetes XML, das einen Typ oder ein Member dokumentiert.

Ein Dokumentati- onskommentar steht direkt vor einer Deklaration des Typs oder Members und beginnt mit drei Slashes (`///`).

```CSharp
/// <summary>Beendet eine laufende Abfrage.</summary>
public void Cancel() { ... }
```


Kommentare über mehrere Zeilen geschrieben werden:

```csharp
/// <summary>
/// Beendet eine laufende Abfrage. 
/// </summary>
public void Cancel() { ... }

// oder so

/**
<summary> Beendet eine laufende Abfrage. </summary>
*/
public void Cancel() { ... }
```


### Standard-XML-Dokumentations-Tags

Hier ein paar der Standard-XML Tags, die Visual Studio und Dokumentationsgeneratoren erkennen.

```<summary>```: Definiert den Tooltipp, den IntelliSense für den Typ oder das Member anzeigen soll, meist ein einzelner Satz oder eine Phrase.

```<remarks>```: Zusätzlicher Text, der den Typ oder das Member beschreibt.

```<param name="Name">```: Erläutert einen Parameter einer Methode.

```<returns>```: Erläutert den Rückgabewert einer Methode.

```<exception cref="Typ">```: Führt eine Exception auf, die eine Methode eventuell wirft.