# Ch2: Method Signatures & Type Descriptors (L, [, I, Z)

## Concept
Smali uses compact type descriptors instead of full Java type names.

Plain-English version:
Type descriptors are just a compressed way to say "what goes in" and "what comes out" of a method.

The most common ones:

- `I` = `int`
- `Z` = `boolean`
- `V` = `void`
- `Ljava/lang/String;` = `String`
- `Lcom/example/MainActivity;` = custom class
- `[I` = `int[]`
- `[Ljava/lang/String;` = `String[]`

Easy memory trick:

- `L ... ;` means "some object/class"
- `[` means "array of"
- single letters like `I` and `Z` are primitive types

Method signatures follow this pattern:

```text
methodName(parameterTypes)returnType
```

Example:

```text
isAllowed(Ljava/lang/String;I)Z
```

Meaning:

- method name: `isAllowed`
- parameters: `String`, `int`
- return type: `boolean`

In plain English:
"This method takes a String and an int, then returns true or false."

This is one of the most important reading skills in reversing. If you cannot read descriptors quickly, APK analysis feels slow and noisy.

## Java vs. Smali

### Java
```java
public boolean isAdmin(String user, int level) {
    return level > 5;
}
```

### Smali
```smali
.method public isAdmin(Ljava/lang/String;I)Z
    .registers 5

    const/4 v0, 0x5
    if-le p2, v0, :not_admin

    const/4 v1, 0x1
    return v1

    :not_admin
    const/4 v1, 0x0
    return v1
.end method
```

How to read the signature:

- `Ljava/lang/String;` = first parameter
- `I` = second parameter
- `Z` = return value

Register mapping here:

- `p0` = `this`
- `p1` = `user`
- `p2` = `level`

Beginner shortcut:
Read signatures from left to right:

1. method name
2. stuff inside `()`
3. final type after `)`

## The 'Security Angle'
Descriptors help you identify risky code paths fast.

Examples:

- `([B)Ljava/lang/String;` often means raw bytes are being decoded
- `(Ljava/lang/String;)Z` often appears in root checks, string filters, or permission checks
- `([Ljava/lang/String;)V` may indicate a command array being passed toward execution logic
- `()Ljava/security/Signature;` may lead to signing or verification behavior

In malware research, signatures let you scan thousands of methods and quickly guess intent without reading every line.

You are building a triage instinct:

- "This method takes bytes and returns a string"
- "This one takes a path and returns boolean"
- "This one takes context and returns package signature info"

## ASCII Visualization
```text
  Signature:
    isAdmin(Ljava/lang/String;I)Z

  Breakdown:
    isAdmin ( Ljava/lang/String;   I ) Z
               |                   |   |
               |                   |   +--> returns boolean
               |                   +------> int parameter
               +--------------------------> String parameter

  Register View:
    p0 = this
    p1 = user
    p2 = level
```

One more fast example:

```text
  doCheck([Ljava/lang/String;)V

  Read it as:
    method name = doCheck
    input = String[]
    return = void
```

## Practice Lab
Tell me what this 3-line snippet suggests about the method:

```smali
.method public check(Ljava/lang/String;[I)Z
    .registers 4
.end method
```

Hint:
Focus only on the signature. Ignore `.registers 4` for now.
