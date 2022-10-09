# Erweiterungsmethoden


**Erweiterungsmethoden** ermöglichen es, bestehende Typen durch neue Methoden zu erweitern, **ohne** die Definition des Ursprungstyps verändern zu müssen.


Eine Erweiterungsmethode ist eine **statische Methode** einer **statischen Klasse**, bei der der Modifikator `this` dem ersten Parameter mitgegeben wird.

Der Typ des ersten Parameters ist dann der zu erweiternde Typ:

```csharp
public static class StringHelper {
    public static bool IsCapitalized(this string s) {
        if (string.IsNullOrEmpty(s)) 
            return false;
    
    return char.IsUpper(s[0]); }
}
```

Der Aufruf erfolgt dann wie folgt:

```csharp
Console.Write("Perth".IsCapitalized());
```


## Verketten von Erweiterungsmethoden

Erweiterungsmethoden können wie Instanzmethoden verkettet werden.

```csharp
public static class StringHelper {
    public static string Pluralize(this string s) {...}
    public static string Capitalize(this string s) {...} 
}

// Nutzung

string x = "sausage".Pluralize().Capitalize();

string y = StringHelper.Capitalize(StringHelper.Pluralize("sausage"));
```