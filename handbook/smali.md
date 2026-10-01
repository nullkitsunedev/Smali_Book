# Smali & Android Reversing Handbook

## Who This Is For

This handbook is for:

- people learning Smali for authorized app analysis & reverse engineering apk
- malware researchers who want to read APK behavior more confidently

This handbook is not a guide for piracy, unauthorized patching, or bypassing protections on apps you do not own or have permission to assess.

## How To Use This Handbook

Read this in order:

1. First Contact
2. Smali vs Java
3. Class Headers, Fields, and Methods
4. Registers
5. Type Descriptors
6. Arrays
7. Core Instructions
8. Logic and Branching
9. Field Access and Method Calls
10. APK Structure and Workflow
11. Obfuscation and Malware Analysis Notes

Best study method:

1. read one section
2. open one real safe APK
3. find the same pattern
4. explain it in plain English

## 1. First Contact

### Concept

When you open Smali for the first time, it looks worse than Java because it is closer to the machine and less friendly to humans.

Plain-English version:
You are no longer reading nice source code with variable names like `password` or `userInput`. You are reading lower-level instructions where values move through numbered buckets called registers.

Your first goal is not:

- "understand everything"

Your first goal is:

- "stay calm and understand one line at a time"

Five things to remember:

- `.method` starts a method
- `.end method` ends it
- `p` registers hold method parameters; in non-static methods, `p0` is usually `this`
- `v0`, `v1`, `v2` are working registers
- labels like `:cond_0` or `:goto_1` are jump targets

### Java vs. Smali

#### Java
```java
public boolean isReady() {
    return true;
}
```

#### Smali
```smali
.method public isReady()Z
    .registers 2

    const/4 v0, 0x1
    return v0
.end method
```

Plain-English translation:
"This method always returns true."

### The Security Angle

If you panic at the first ugly method, you lose time. Malware analysis and Android RE reward calm pattern recognition much more than raw memorization.

### ASCII Visualization
```text
  .method public isReady()Z
          |
          v
    const/4 v0, 0x1
          |
          v
      return v0
          |
          v
        true
```

### Practice Lab

Tell me what this 3-line snippet does:

```smali
.method public ok()Z
const/4 v0, 0x0
return v0
```

## 2. Smali vs Java

### Concept

Android apps are usually written in Java or Kotlin, but what actually gets packaged into the APK is lower-level bytecode stored in `.dex` files.

Smali is the human-readable form of that Dalvik bytecode.

Important correction:

- Java source code is not directly "turned into Smali" during app development
- rather, Java or Kotlin is compiled into bytecode, then into DEX
- Smali is what we use when we disassemble DEX back into readable text

So the clean mental model is:

```text
Java/Kotlin source
       |
       v
compiled classes
       |
       v
DEX bytecode
       |
       v
Smali when disassembled
```

Important beginner note:
This `main()` example is only for understanding syntax. Real Android apps usually start from framework lifecycle entry points like `Application`, `Activity`, `Service`, or `BroadcastReceiver`, not a normal JVM-style `main()`.

### Java vs. Smali

#### Java
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

#### Smali
```smali
.class public LHelloWorld;
.super Ljava/lang/Object;

.method public static main([Ljava/lang/String;)V
    .registers 2

    sget-object v0, Ljava/lang/System;->out:Ljava/io/PrintStream;
    const-string v1, "Hello World!"
    invoke-virtual {v0, v1}, Ljava/io/PrintStream;->println(Ljava/lang/String;)V
    return-void
.end method
```

Line-by-line:

- `.class public LHelloWorld;` declares the class
- `.super Ljava/lang/Object;` says it extends `Object`
- `.method public static main([Ljava/lang/String;)V` declares the method
- `.registers 2` gives the method two registers
- `sget-object` loads `System.out`
- `const-string` loads `"Hello World!"`
- `invoke-virtual` calls `println`
- `return-void` ends the method

### The Security Angle

Decompiled Java is useful for speed, but Smali is closer to the truth. If JADX looks confusing or wrong, Smali usually reveals the exact control flow and register usage.

### ASCII Visualization
```text
  sget-object v0  --->  System.out
  const-string v1 --->  "Hello World!"
       v0 + v1    --->  println(...)
```

### Practice Lab

Tell me what this 3-line snippet does:

```smali
const-string v0, "test"
invoke-static {v0}, Ljava/lang/String;->valueOf(Ljava/lang/Object;)Ljava/lang/String;
move-result-object v1
```

## 3. Class Headers, Fields, and Methods

### Concept

The top of a Smali file tells you what the class is, what it extends, and sometimes what it implements or where it came from.

Common directives:

- `.class` = class name and access
- `.super` = parent class
- `.source` = original source filename
- `.implements` = interface implemented by the class
- `.field` = class field
- `.method` = method definition

### Java vs. Smali

#### Java
```java
class ExampleClass implements Runnable {
    private String msg = "hello";

    public void run() {
        System.out.println(msg);
    }
}
```

#### Smali
```smali
.class public Lcom/example/ExampleClass;
.super Ljava/lang/Object;
.implements Ljava/lang/Runnable;
.source "ExampleClass.java"

.field private msg:Ljava/lang/String;

.method public run()V
    .registers 3

    iget-object v0, p0, Lcom/example/ExampleClass;->msg:Ljava/lang/String;
    sget-object v1, Ljava/lang/System;->out:Ljava/io/PrintStream;
    invoke-virtual {v1, v0}, Ljava/io/PrintStream;->println(Ljava/lang/String;)V
    return-void
.end method
```

### The Security Angle

Fields often store the most interesting state:

- API keys
- flags
- cached tokens
- booleans that control anti-analysis behavior

Method and field definitions are the map of the class. Before reading the body, look at that map.

### ASCII Visualization
```text
  Class Header
    |
    +-- .class
    +-- .super
    +-- .implements
    +-- .field
    +-- .method
```

### Practice Lab

Tell me what this line means:

```smali
.field private secret:Ljava/lang/String;
```

## 4. Registers

### Concept

Registers are the core idea in Smali.

Plain-English version:
Instead of named local variables, Smali uses numbered buckets.

Two main kinds:

- `v0`, `v1`, `v2` = local registers
- `p0`, `p1`, `p2` = parameter registers

In non-static methods:

- `p0` is usually `this`

In static methods:

- there is no `this`, so the first real input starts at `p0`

Register-count note:
`.registers` is the total register count for the whole method. In non-static methods, that total includes the implicit `this` reference as `p0`.

Important detail:

- `long` and `double` are 64-bit values, so they use two registers

### Java vs. Smali

#### Java
```java
public int addOne(int x) {
    int y = x + 1;
    return y;
}
```

#### Smali
```smali
.method public addOne(I)I
    .registers 4

    const/4 v0, 0x1
    add-int v1, p1, v0
    return v1
.end method
```

Meaning:

- `p1` holds `x`
- `v0` holds `1`
- `v1` holds `x + 1`

### The Security Angle

Reverse engineering is often register tracing:

1. where did this value come from?
2. where was it copied?
3. what method used it?
4. what branch depended on it?

That skill is how you follow suspicious strings, booleans, tokens, and file paths.

### ASCII Visualization
```text
  [ REGISTERS ]
  +----------------------+
  | p0 : [ this ]        |
  +----------------------+
  | p1 : [ x ]           |
  +----------------------+
  | v0 : [ 1 ]           |
  +----------------------+
  | v1 : [ x + 1 ]       |
  +----------------------+
```

### Practice Lab

Tell me what this does:

```smali
const/4 v0, 0x1
add-int v1, p1, v0
return v1
```

## 5. Type Descriptors

### Concept

Descriptors are the compressed type language of Smali.

Common ones:

- `V` = void
- `Z` = boolean
- `B` = byte
- `S` = short
- `C` = char
- `I` = int
- `F` = float
- `J` = long
- `D` = double
- `L...;` = object type
- `[` = array type

Examples:

- `Ljava/lang/String;` = `String`
- `[I` = `int[]`
- `[Ljava/lang/String;` = `String[]`

### Java vs. Smali

#### Java
```java
public boolean check(String user, int level) {
    return level > 5;
}
```

#### Smali
```smali
.method public check(Ljava/lang/String;I)Z
    .registers 5

    const/4 v0, 0x5
    if-le p2, v0, :fail
    const/4 v1, 0x1
    return v1

    :fail
    const/4 v1, 0x0
    return v1
.end method
```

### The Security Angle

Reading signatures fast lets you triage code faster:

- `(Ljava/lang/String;)Z` often means a yes/no string check
- `([B)Ljava/lang/String;` often means byte decoding
- `()Ljava/security/Signature;` may be related to signing logic

### ASCII Visualization
```text
  check(Ljava/lang/String;I)Z

  check ( String , int ) boolean
```

### Practice Lab

What does this signature mean?

```smali
.method public verify([Ljava/lang/String;)V
```

## 6. Arrays

### Concept

Arrays hold a fixed number of values of the same type.

Array descriptors:

- `[I` = `int[]`
- `[C` = `char[]`
- `[Ljava/lang/String;` = `String[]`

Common array instructions:

- `new-array` = create an array
- `fill-array-data` = load fixed values
- `aget` = read an array element
- `aput` = write an array element
- `array-length` = get length

### Java vs. Smali

#### Java
```java
int[] myArray = new int[]{1, 2, 3};
int x = myArray[1];
```

#### Smali
```smali
const/4 v0, 0x3
new-array v1, v0, [I
fill-array-data v1, :array_data
const/4 v2, 0x1
aget v3, v1, v2

:array_data
.array-data 4
    0x1
    0x2
    0x3
.end array-data
```

### The Security Angle

Arrays show up in:

- string splitting
- byte decoding
- cryptographic material
- command lists
- obfuscated data blobs

### ASCII Visualization
```text
  v1 ---> [ 1 ][ 2 ][ 3 ]
             0    1    2

  aget v3, v1, v2
       |
       +--> read array[v2]
```

### Practice Lab

Tell me what this suggests:

```smali
new-array v0, v1, [I
fill-array-data v0, :array_0
aget v2, v0, v1
```

## 7. Core Instructions

### Concept

You do not need every Smali instruction at once. You need the common ones enough times that they stop looking scary.

High-value instructions:

- `const/4`
- `const/16`
- `const-string`
- `move`
- `move-object`
- `move-result`
- `move-result-object`
- `new-instance`
- `invoke-virtual`
- `invoke-static`
- `invoke-direct`
- `return`
- `return-object`
- `return-void`

### Java vs. Smali

#### Java
```java
String s = "hello";
return s;
```

#### Smali
```smali
const-string v0, "hello"
move-object v1, v0
return-object v1
```

### The Security Angle

Static strings, object creation, and helper calls reveal a lot:

- URLs
- package names
- hidden commands
- decryption helpers
- logging behavior

### ASCII Visualization
```text
  const-string v0, "hello"
          |
          v
      v0 = "hello"
          |
          v
  return-object v0
```

### Practice Lab

Tell me what this does:

```smali
const-string v0, "admin"
return-object v0
```

## 8. Logic and Branching

### Concept

Most app decisions are just jumps.

Common branch instructions:

- `if-eqz` = jump if register is zero
- `if-nez` = jump if register is not zero
- `if-eq` = compare two registers
- `goto` = unconditional jump

For booleans:

- `0` usually means false
- `1` usually means true

### Java vs. Smali

#### Java
```java
public boolean canLogin(boolean valid) {
    if (!valid) {
        return false;
    }
    return true;
}
```

#### Smali
```smali
.method public canLogin(Z)Z
    .registers 3

    if-eqz p1, :fail
    const/4 v0, 0x1
    return v0

    :fail
    const/4 v0, 0x0
    return v0
.end method
```

### The Security Angle

This is how many important checks appear in practice:

- root checks
- emulator checks
- trust booleans
- feature gates
- environment checks

For authorized research, the key skill is recognizing where the decision is enforced.

### ASCII Visualization
```text
      [ START ]
          |
      p1 = valid
          |
   if-eqz p1, :fail
      /          \
     /            \
 [true path]   [false path]
```

### Practice Lab

Tell me what this does:

```smali
if-eqz p1, :no
const/4 v0, 0x1
return v0
```

## 9. Field Access and Method Calls

### Concept

This is where a lot of real behavior becomes visible.

Field access:

- `iget` = read instance field
- `iput` = write instance field
- `sget` = read static field
- `sput` = write static field

Method invocation:

- `invoke-virtual`
- `invoke-static`
- `invoke-direct`
- `invoke-interface`

And then usually:

- `move-result`
- `move-result-object`

### Java vs. Smali

#### Java
```java
public String getTokenLength() {
    String token = this.token;
    return String.valueOf(token.length());
}
```

#### Smali
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

### The Security Angle

This pattern often reveals:

- config reads
- string decoding
- API request building
- environment flag updates
- trust values being stored

### ASCII Visualization
```text
  p0 ---> object
           |
           +--> field token
                    |
                    v
               iget-object v0
                    |
                    v
               invoke length()
                    |
                    v
              move-result v1
```

### Practice Lab

Tell me what this does:

```smali
iget-object v0, p0, Lcom/demo/App;->url:Ljava/lang/String;
invoke-static {v0}, Lcom/demo/Net;->fetch(Ljava/lang/String;)V
return-void
```

## 10. APK Structure and Workflow

### Concept

Before reading Smali, know where things live inside an APK.

Important files and folders:

- `AndroidManifest.xml`
- `classes.dex`
- `classes2.dex`, `classes3.dex`, and so on
- `res/`
- `assets/`
- `lib/`

Useful tools:

- `jadx`
- `apktool`
- `adb`
- `apksigner`

Golden rule:

- use JADX for fast orientation
- use Smali for exact truth

### Java vs. Smali

#### Java
```java
public String getPkg() {
    return getPackageName();
}
```

#### Smali
```smali
invoke-virtual {p0}, Landroid/content/Context;->getPackageName()Ljava/lang/String;
move-result-object v0
return-object v0
```

### The Security Angle

Workflow matters more than random curiosity.

A simple beginner workflow:

1. inspect the manifest
2. inspect permissions
3. identify main components
4. search for strings
5. find network, file, crypto, and package-manager code
6. trace suspicious methods in Smali

### ASCII Visualization
```text
  Manifest
     |
     v
  Components
     |
     v
  Strings
     |
     v
  Methods
     |
     v
  Branches + Data Flow
```

### Practice Lab

Tell me what this method returns:

```smali
invoke-virtual {p0}, Landroid/content/Context;->getPackageName()Ljava/lang/String;
move-result-object v0
return-object v0
```

## 11. Obfuscation and Malware Analysis Notes

### Concept

Real APKs are often uglier than training examples.

Common obstacles:

- renamed classes like `a`, `b`, `c`
- helper wrappers
- encoded strings
- reflection
- multiple DEX files
- native libraries

### Java vs. Smali

#### Java
```java
public String getUrl() {
    return decode("6874747073");
}
```

#### Smali
```smali
const-string v0, "6874747073"
invoke-static {v0}, Lcom/demo/Util;->decode(Ljava/lang/String;)Ljava/lang/String;
move-result-object v1
return-object v1
```

### The Security Angle

In malware analysis, your job is usually not to perfectly decompile everything.

Your job is to answer practical questions:

- what triggers execution?
- what data is collected?
- where does the data go?
- what checks block analysis?
- what strings or helpers hide intent?

### ASCII Visualization
```text
  encoded string
       |
       v
    decode(...)
       |
       v
   readable output
```

### Practice Lab

Tell me what this pattern suggests:

```smali
const-string v0, "ZmFrZQ=="
invoke-static {v0}, Lcom/x/Util;->decode(Ljava/lang/String;)Ljava/lang/String;
move-result-object v1
```

## What I Changed From The Source Notes

I kept the strongest useful parts:

- Smali vs Java grounding
- class headers
- registers
- descriptors
- arrays
- core instructions
- method and field behavior
- workflow thinking

I deliberately removed or reframed:

- cracking and piracy-oriented guidance
- unauthorized bypass instructions
- modding language that encourages misuse
- tool-specific patching steps for third-party apps

That makes this version safer, cleaner, and much more useful for long-term defensive learning.

## Best References

- Smali Wiki: https://github.com/JesusFreke/smali/wiki
- Android Dalvik bytecode docs: https://source.android.com/docs/core/runtime/dalvik-bytecode
- JADX: https://github.com/skylot/jadx
- Apktool: https://apktool.org/

## Final Advice

Do not aim for:

- "I understand every line instantly"

Aim for:

- "I can explain what this method is trying to do"

That is the real beginner-to-intermediate reversing skill.
