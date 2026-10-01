# Core Handbook

This file is the polished single-file handbook version.

Start here, then use the rest of the folder as support material.

## First Contact

Smali is the human-readable form of Dalvik bytecode.

Plain-English version:
It is lower-level than Java, so it looks uglier, but it is also more exact.

Five things to remember:

- `.method` starts a method
- `.end method` ends it
- `p` registers are usually method inputs
- `v` registers are working buckets
- labels like `:cond_0` and `:goto_1` are jump targets

### Java vs. Smali

```java
public boolean ok() {
    return true;
}
```

```smali
.method public ok()Z
    .registers 2

    const/4 v0, 0x1
    return v0
.end method
```

### Security Angle

In malware analysis, you do not need to make the code pretty. You need to understand what it does.

## Smali vs Java

Real pipeline:

```text
Java/Kotlin
    |
    v
compiled code
    |
    v
DEX
    |
    v
Smali when disassembled
```

Use Java-like decompilation for orientation.
Use Smali for exact truth.

## Registers

Smali uses registers instead of named local variables.

- `v0`, `v1`, `v2` = local registers
- `p0`, `p1`, `p2` = parameter registers
- in non-static methods, `p0` is usually `this`
- `.registers` counts the total method registers, including parameter registers

```smali
const/4 v0, 0x1
add-int v1, p1, v0
return v1
```

Plain-English version:
"Take the input in `p1`, add 1, and return it."

## Type Descriptors

- `V` = void
- `Z` = boolean
- `I` = int
- `J` = long
- `D` = double
- `L...;` = object
- `[` = array

Example:

```text
check(Ljava/lang/String;I)Z
```

Meaning:

- method name = `check`
- parameters = `String`, `int`
- return type = `boolean`

## Fields and Methods

- `iget` = read instance field
- `iput` = write instance field
- `sget` = read static field
- `sput` = write static field
- `invoke-*` = call method
- `move-result*` = store return value

## Branching

- `if-eqz` = jump if zero
- `if-nez` = jump if not zero
- `goto` = unconditional jump

```smali
if-eqz p1, :fail
const/4 v0, 0x1
return v0
```

Meaning:
"If `p1` is false, jump to fail. Otherwise return true."

## Arrays

- `new-array`
- `fill-array-data`
- `aget`
- `aput`
- `array-length`

## APK Workflow

Start with:

1. `AndroidManifest.xml`
2. permissions
3. components
4. interesting strings
5. suspicious methods
6. exact Smali logic

## Obfuscation

Common signs:

- class names like `a`, `b`, `c`
- helper wrappers
- `decode()` methods
- reflection
- multi-dex layouts

## Rule For Beginners

Do not ask:

- "Do I understand every line?"

Ask:

- "Can I explain what this method is trying to do?"
