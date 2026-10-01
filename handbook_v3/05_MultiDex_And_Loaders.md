# MultiDex and Loaders

## Concept

Large or complex apps often have multiple DEX files:

- `classes.dex`
- `classes2.dex`
- `classes3.dex`

Plain-English version:
If you cannot find logic in the first DEX, it may simply live elsewhere.

Some apps also load code dynamically through class loaders.

Common signs:

- `DexClassLoader`
- `PathClassLoader`
- code under `assets/` or downloaded at runtime

## Java vs. Smali

### Java
```java
DexClassLoader loader = new DexClassLoader(path, out, null, parent);
```

### Smali
```smali
new-instance v0, Ldalvik/system/DexClassLoader;
invoke-direct {v0, v1, v2, v3, v4}, Ldalvik/system/DexClassLoader;-><init>(Ljava/lang/String;Ljava/lang/String;Ljava/lang/String;Ljava/lang/ClassLoader;)V
```

## The Security Angle

Dynamic loading matters because:

- payloads may not be in the main code path
- behavior may only appear after a second-stage load
- malicious logic may live outside the obvious classes

## ASCII Visualization
```text
  APK
   |
   +-- classes.dex
   +-- classes2.dex
   +-- assets/
         |
         v
   DexClassLoader
         |
         v
   second-stage code
```

## Practice Lab

Tell me what this line suggests:

```smali
new-instance v0, Ldalvik/system/DexClassLoader;
```
