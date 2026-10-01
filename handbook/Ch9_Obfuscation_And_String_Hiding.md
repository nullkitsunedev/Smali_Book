# Ch9: Obfuscation and String Hiding

## Concept
Real-world APKs often try to make analysis annoying.

Plain-English version:
Obfuscation usually does not make code magical. It mostly makes code ugly, shorter, noisier, or harder to search.

Common patterns:

- class names like `a`, `b`, `c`
- method names like `a()`, `b()`
- encrypted or split strings
- wrapper methods that hide real behavior
- reflection instead of direct calls

## Java vs. Smali

### Java
```java
public String getUrl() {
    return decode("6874747073");
}
```

### Smali
```smali
.method public getUrl()Ljava/lang/String;
    .registers 3

    const-string v0, "6874747073"
    invoke-static {v0}, Lcom/demo/Util;->decode(Ljava/lang/String;)Ljava/lang/String;
    move-result-object v1
    return-object v1
.end method
```

What happened:

- a suspicious string is loaded
- helper method `decode()` is called
- decoded result is returned

In plain English:
"This method hides real text behind a decode function."

## The 'Security Angle'
Obfuscation matters because many dangerous values are not visible in plain text.

Common analyst questions:

- Is this string encoded?
- Is this method name meaningless because of obfuscation?
- Is there a helper method used everywhere for decryption?
- Is reflection hiding the real API call?

One of the best tactics is to find repeated helper methods and understand them first.

## ASCII Visualization
```text
  const-string v0, "6874747073"
            |
            v
      invoke-static decode(v0)
            |
            v
      move-result-object v1
            |
            v
       real string appears
```

## Practice Lab
Tell me what this 3-line snippet suggests:

```smali
const-string v0, "ZmFrZQ=="
invoke-static {v0}, Lcom/x/Util;->decode(Ljava/lang/String;)Ljava/lang/String;
move-result-object v1
```

Hint:
Do not worry about the exact encoding yet. Focus on the pattern.
