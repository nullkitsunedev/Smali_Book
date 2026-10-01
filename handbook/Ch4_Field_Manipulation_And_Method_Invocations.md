# Ch4: Field Manipulation (iget/iput) and Method Invocations

## Concept
Fields are object state. Methods are behavior.

Plain-English version:
Fields are values stored inside an object. Method calls are actions. A lot of Smali reading is just:

1. read a value from an object
2. call a method with that value
3. store the answer

In Smali:

- `iget` reads an instance field from an object
- `iput` writes an instance field into an object
- `sget` reads a static field
- `sput` writes a static field

Method calls use `invoke-*` instructions:

- `invoke-virtual`
- `invoke-static`
- `invoke-direct`
- `invoke-interface`

If a method returns a value, the next instruction usually captures it:

- `move-result v0`
- `move-result-object v0`

This `invoke -> move-result` pair is everywhere in Android reversing.

Beginner shortcut:
Whenever you see `invoke-*`, immediately look at the next line. If the method returns something, the next line often tells you where that answer is stored.

## Java vs. Smali

### Java
```java
public String getTokenLength() {
    String token = this.token;
    return String.valueOf(token.length());
}
```

### Smali
```smali
.field private token:Ljava/lang/String;

.method public getTokenLength()Ljava/lang/String;
    .registers 4

    iget-object v0, p0, Lcom/example/MainActivity;->token:Ljava/lang/String;
    invoke-virtual {v0}, Ljava/lang/String;->length()I
    move-result v1

    invoke-static {v1}, Ljava/lang/String;->valueOf(I)Ljava/lang/String;
    move-result-object v2
    return-object v2
.end method
```

Reading flow:

- pull `token` from object into `v0`
- call `length()`
- store returned `int` in `v1`
- call `String.valueOf(int)`
- store returned string in `v2`

In plain English:
"Read `this.token`, get its length, convert that number to text, and return it."

## The 'Security Angle'
This chapter is where real app behavior becomes visible.

What researchers look for:

- `iget-object` pulling configuration fields, API keys, flags
- `invoke-static` calling crypto or decoding helpers
- `invoke-virtual` on networking, file, or package-manager objects
- `iput-boolean` setting a `verified` or `initialized` flag

In malware analysis, many important stories are told by field + invoke chains:

- read encrypted blob from field
- decrypt it with helper method
- pass result into network or dynamic loader method

If you learn to read these chains, APK behavior becomes much less mysterious.

## ASCII Visualization
```text
  Object State:
    p0 ---> [ MainActivity instance ]
               |
               +--> field token = "abc123"

  Read:
    iget-object v0, p0, ...->token:Ljava/lang/String;

  Call Chain:
    v0 -----------------> length()
                           |
                           v
                         v1 = 6
                           |
                           v
                  String.valueOf(v1)
                           |
                           v
                         v2 = "6"
                           |
                           v
                       return-object
```

Simple field picture:

```text
  p0 = this
   |
   v
  [ App Object ]
      |
      +--> token = "abc123"
      +--> url   = "https://..."

  iget-object reads from the object into a register
  iput-object writes from a register back into the object
```

## Practice Lab
Tell me what this 3-line snippet does:

```smali
iget-object v0, p0, Lcom/demo/App;->url:Ljava/lang/String;
invoke-static {v0}, Lcom/demo/Net;->fetch(Ljava/lang/String;)V
return-void
```

Hint:
First line reads a field. Second line uses that field as input to a method call.
