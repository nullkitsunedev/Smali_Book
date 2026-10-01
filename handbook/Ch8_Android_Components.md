# Ch8: Android Components

## Concept
Android apps are built from components. If you do not know the component type, it is harder to understand why code runs.

Main component types:

- `Activity` = UI screen
- `Service` = background work
- `BroadcastReceiver` = reacts to system or app events
- `ContentProvider` = shares or exposes data

Plain-English version:
Before asking "What does this method do?", first ask "What kind of Android component am I inside?"

## Java vs. Smali

### Java
```java
public void onReceive(Context c, Intent i) {
    String action = i.getAction();
}
```

### Smali
```smali
.method public onReceive(Landroid/content/Context;Landroid/content/Intent;)V
    .registers 4

    invoke-virtual {p2}, Landroid/content/Intent;->getAction()Ljava/lang/String;
    move-result-object v0
    return-void
.end method
```

What happened:

- `p1` is the `Context`
- `p2` is the `Intent`
- the method asks the intent for its action string

In plain English:
"When the receiver gets an event, read the event action."

## The 'Security Angle'
Components often reveal execution triggers.

Examples:

- a receiver may trigger on boot
- a service may keep malware running in the background
- an activity may hide phishing UI
- a content provider may leak data

In triage, component type helps answer:

- When does this code run?
- Does the user need to click anything?
- Can the system trigger it automatically?

## ASCII Visualization
```text
  Android Event
       |
       v
  BroadcastReceiver.onReceive(...)
       |
       +--> p1 = Context
       +--> p2 = Intent
                |
                v
          getAction() -> v0
```

## Practice Lab
Tell me what this 3-line snippet does:

```smali
invoke-virtual {p2}, Landroid/content/Intent;->getAction()Ljava/lang/String;
move-result-object v0
return-void
```

Hint:
The code reads something from the incoming `Intent`, but does not return it.
