# Exception Handling

## Concept

Smali represents exception handling with labels and catch directives rather than nice high-level `try/catch` blocks.

Common pieces:

- `:try_start_*`
- `:try_end_*`
- `.catch`
- `move-exception`

Plain-English version:
The code marks a risky region, tells the runtime what exception type to catch, and jumps into a handler block if something fails.

## Java vs. Smali

### Java
```java
try {
    risky();
} catch (Exception e) {
    return;
}
```

### Smali
```smali
.method public test()V
    .registers 2

    :try_start_0
    invoke-static {}, Lcom/demo/App;->risky()V
    :try_end_0
    goto :done

    .catch Ljava/lang/Exception; {:try_start_0 .. :try_end_0} :catch_0

    :catch_0
    move-exception v0
    return-void

    :done
    return-void
.end method
```

## The Security Angle

Exception handlers can hide:

- failed environment checks
- network fallback behavior
- silent error suppression
- anti-analysis tricks that swallow crashes

In malware analysis, a broad catch block can hide important failures and keep the app moving quietly.

## ASCII Visualization
```text
  :try_start_0
       |
       v
    risky()
       |
   success? ------ yes ------> :done
       |
      no
       |
       v
   :catch_0
       |
       v
 move-exception v0
       |
       v
   return-void
```

## Practice Lab

Tell me what this does:

```smali
.catch Ljava/lang/Exception; {:try_start_0 .. :try_end_0} :catch_0
:catch_0
move-exception v0
```
