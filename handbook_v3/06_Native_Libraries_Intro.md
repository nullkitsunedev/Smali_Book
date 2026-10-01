# Native Libraries Intro

## Concept

Some Android apps move logic into native `.so` files.

Folders:

- `lib/armeabi-v7a/`
- `lib/arm64-v8a/`
- `lib/x86_64/`

Plain-English version:
If the Java or Smali side looks too thin, the important logic may have been pushed into native code.

Common Smali clues:

- `System.loadLibrary(...)`
- `native` methods

## Java vs. Smali

### Java
```java
static {
    System.loadLibrary("demo");
}

public native String getSecret();
```

### Smali
```smali
const-string v0, "demo"
invoke-static {v0}, Ljava/lang/System;->loadLibrary(Ljava/lang/String;)V

.method public native getSecret()Ljava/lang/String;
.end method
```

## The Security Angle

Native code often holds:

- crypto routines
- anti-debugging
- string decryption
- emulator checks
- payload logic

If Smali calls a `native` method, that method body is not in Smali.

## ASCII Visualization
```text
  Smali side
     |
     v
 System.loadLibrary("demo")
     |
     v
 native .so loaded
     |
     v
 native method does the real work
```

## Practice Lab

Tell me what this implies:

```smali
.method public native getSecret()Ljava/lang/String;
.end method
```
