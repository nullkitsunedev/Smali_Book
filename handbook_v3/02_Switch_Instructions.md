# Switch Instructions

## Concept

When an app chooses between many branches, Smali often uses switch instructions instead of many `if-*` lines.

Main types:

- `packed-switch` = dense, consecutive values
- `sparse-switch` = scattered values

Plain-English version:
This is Smali's way of saying, "jump to a different place depending on the value."

## Java vs. Smali

### Java
```java
switch (mode) {
    case 1: return "one";
    case 2: return "two";
    default: return "other";
}
```

### Smali
```smali
.method public test(I)Ljava/lang/String;
    .registers 3

    packed-switch p1, :pswitch_data_0

    const-string v0, "other"
    return-object v0

    :pswitch_1
    const-string v0, "one"
    return-object v0

    :pswitch_2
    const-string v0, "two"
    return-object v0

    :pswitch_data_0
    .packed-switch 0x1
        :pswitch_1
        :pswitch_2
    .end packed-switch
.end method
```

## The Security Angle

Switches often appear in:

- command dispatchers
- message handlers
- menu action routing
- malware command IDs

If you see a switch, you may have found a central router.

## ASCII Visualization
```text
      p1 = mode
         |
         v
   packed-switch p1
     |      |      |
     |      |      +--> case 2
     |      +---------> case 1
     +----------------> default
```

## Practice Lab

Tell me what this suggests:

```smali
packed-switch p1, :pswitch_data_0
const-string v0, "other"
return-object v0
```
