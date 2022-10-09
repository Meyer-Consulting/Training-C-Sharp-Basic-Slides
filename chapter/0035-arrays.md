# Arrays

Ein Array repräsentiert eine feste Zahl von Elementen eines bestimmten Typs.

Die Elemente in einem Array werden immer in einem zusammenhängenden Speicherabschnitt gespeichert, wodurch man sehr schnell auf sie zugreifen kann.


## Erstellen von Arrays

Ein Array wird durch eckige Klammern nach dem Typ des Elements definiert.

```csharp
char[] vowels = new char[5];

char[] vowels = new char[] {'a','e','i','o','u'};
```


Eckige Klammern indexieren das Array auch, wodurch man auf ein bestimmtes Element über die Position zugreifen kann:

```csharp
vowels[0] = 'a'; 
vowels[1] = 'e'; 
vowels[2] = 'i'; 
vowels[3] = 'o'; 
vowels[4] = 'u';

Console.WriteLine(vowels[1]); // e
```


## Array - Methoden

Alle Arrays erben von der Klasse `System.Array`.

Ein Array hat folgende wichtige Methoden:
* `Length` - gibt die Anzahl der Elemente im Array zurück
* `Rank` - gibt die Anzahl der Dimensionen des Arrays zurück


**Statische Methoden:**
* `Copy` - kopiert ein Array in ein anderes Array
* `Sort` - sortiert ein Array
* `IndexOf`, `LastIndexOf`, `Find` durchsuchen ein Array


## Indizes

Indizes erlauben es, auf Elemente eines Arrays zuzugreifen. Mit `^1` kann auf das letzte Element zugegriffen werden.

```csharp
char[] vowels = new char[] {'a','e','i','o','u'};

Index first = 0;

char firstElement = vowels [first]; // 'a'
char lastElement = vowels [^1]; // 'u'
```


## Bereiche

Mit Bereichen kann ein Array mit dem Operator `..` in mehrere Teile aufgeteilt werden.

```csharp
char[] vowels = new char[] {'a','e','i','o','u'};

char[] firstTwo = vowels [..2]; // 'a', 'e' 
char[] lastThree = vowels [2..]; // 'i', 'o', 'u' 
char[] middleOne = vowels [2..3]; // 'i'
```


## Mehrdimensionale Arrays

Mehrdimensionale Arrays gibt es in zwei Varianten: **rechteckig** und **ungleichförmig**


**Rechteckige Arrays** werden deklariert, indem die einzelnen Dimensionen durch Kommata getrennt werden.

```csharp
int[,] matrix = new int [3, 3];

int[,] matrix = new int[,] {
    {0,1,2}, {3,4,5}, {6,7,8}
};
```


**Ungleichförmige Arrays** werden mit aufeinanderfolgenden eckigen Klammern deklariert, mit denen jede Dimension repräsentiert wird.
```csharp
int[][] matrix = new int[3][];

int[][] matrix = new int[][] {
    new int[] {0,1,2}, new int[] {3,4,5}, new int[] {6,7,8,9}
};
```