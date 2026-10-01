# Ch7: Common Smali Instructions

## Concept
To read Smali comfortably, you need to recognize common instruction families quickly.

Plain-English version:
You do not need all instructions at once. You just need the common ones enough times that your brain stops treating them as strange.

High-value instructions:

- `const/4` = store a small number
- `const-string` = store a text string
- `move` = copy a value
- `move-object` = copy an object reference
- `new-instance` = create an object
- `return`, `return-object`, `return-void` = finish the method
- `check-cast` = treat an object as a specific class
- `packed-switch` = multi-branch switch logic

## Java vs. Smali

### Java
```java
public String hi() {
    String s = "hello";
    return s;
}
```

### Smali
```smali
.method public hi()Ljava/lang/String;
    .registers 3

    const-string v0, "hello"
    move-object v1, v0
    return-object v1
.end method
```

What happened:

- `const-string` loads `"hello"`
- `move-object` copies the object reference
- `return-object` returns the string

In plain English:
"Store the text `hello` and return it."

## The 'Security Angle'
This is where static indicators often show up.

Examples:

- `const-string` may reveal URLs, API keys, commands, package names
- `new-instance` may reveal suspicious helper objects
- `check-cast` may show framework or reflection behavior
- `packed-switch` may hide feature toggles or command routing

Malware analysis often starts with searching for `const-string`.

## ASCII Visualization
```text
  const-string v0, "hello"
          |
          v
      v0 = "hello"
          |
  move-object v1, v0
          |
          v
      v1 = "hello"
          |
          v
    return-object v1
```

## Practice Lab
Tell me what this 3-line snippet does:

```smali
const-string v0, "admin"
move-object v1, v0
return-object v1
```

Hint:
No computation happens here. It just stores and returns text.
