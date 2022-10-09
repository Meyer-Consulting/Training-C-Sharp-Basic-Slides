# Events


Ein Event ist eine **Benachrichtigung**, die **von einem Objekt gesendet** wird, um das Auftreten einer **Aktion zu signalisieren**.


## Publisher & Subscriper

Die Klasse die ein Event **ausgelöst** hat, wird als **Publisher** bezeichnet.

Die Klasse die ein Event **empfängt** wird als **Subscriper** bezeichnet.

Es kann mehrere Subscriper geben, die auf ein Event reagieren können.

Ein Event ist ein gekapseltes **Delegat**.


## Deklarieren eines Events

```csharp
public delegate void Notify();
                    
public class ProcessBusinessLogic
{
    public event Notify ProcessCompleted;
}
```

`Notify`fungiert als **Delegat** und wird als **Event-Handler** bezeichnet.

`ProcessCompleted` ist der **Event-Name** eines Events vom Typ `Notify`.


## Auslösen eines Events

```csharp
public delegate void Notify();
                    
public class ProcessBusinessLogic
{
    public event Notify ProcessCompleted;

    public void StartProcess()
    {
        Console.WriteLine("Process Started!");
        // do your things here ...
        OnProcessCompleted();
    }

    protected virtual void OnProcessCompleted()
    {
        //if ProcessCompleted is not null then call delegate
        ProcessCompleted?.Invoke(); 
    }
}
```

Wenn `StartProcess` aufgerufen wird, wird am Ende `OnProcessCompleted` aufgerufen. 

`OnProcessCompleted` ruft den Event-Handler auf, wenn dieser nicht `null` ist.


## Verarbeiten eines Events

```csharp
public static void Main()
{
    ProcessBusinessLogic bl = new ProcessBusinessLogic();
    bl.ProcessCompleted += bl_ProcessCompleted;
    bl.StartProcess();
}

public static void bl_ProcessCompleted()
{
    Console.WriteLine("Process Completed!");
}
```

Es wird eine neue Instanz von `ProcessBusinessLogic` erstellt und der Event-Handler `bl_ProcessCompleted` wird dem Event `ProcessCompleted` hinzugefügt.

`bl_ProcessCompleted` enthält die Logic um das Event zu verarbeiten.