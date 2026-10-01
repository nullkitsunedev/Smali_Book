# Reflection Patterns

## Concept

Reflection lets code look up classes and methods by name at runtime.

Common classes:

- `Ljava/lang/Class;`
- `Ljava/lang/reflect/Method;`
- `Ljava/lang/reflect/Field;`

Common calls:

- `Class.forName(...)`
- `getMethod(...)`
- `invoke(...)`

Plain-English version:
Instead of directly calling a method, the app builds the call dynamically.

## Java vs. Smali

### Java
```java
Class<?> c = Class.forName("com.demo.Hidden");
Method m = c.getMethod("run");
m.invoke(null);
```

### Smali
```smali
const-string v0, "com.demo.Hidden"
invoke-static {v0}, Ljava/lang/Class;->forName(Ljava/lang/String;)Ljava/lang/Class;
move-result-object v0
const-string v1, "run"
invoke-virtual {v0, v1, v2}, Ljava/lang/Class;->getMethod(Ljava/lang/String;[Ljava/lang/Class;)Ljava/lang/reflect/Method;
move-result-object v1
invoke-virtual {v1, v3, v4}, Ljava/lang/reflect/Method;->invoke(Ljava/lang/Object;[Ljava/lang/Object;)Ljava/lang/Object;
```

## The Security Angle

Reflection can hide:

- sensitive method targets
- dynamic plugin behavior
- anti-analysis routines
- loader logic

If direct calls are missing, reflection may be the path.

## ASCII Visualization
```text
  class name string
        |
        v
   Class.forName(...)
        |
        v
    getMethod(...)
        |
        v
      invoke(...)
```

## Practice Lab

Tell me what this likely means:

```smali
const-string v0, "com.demo.Hidden"
invoke-static {v0}, Ljava/lang/Class;->forName(Ljava/lang/String;)Ljava/lang/Class;
move-result-object v0
```
