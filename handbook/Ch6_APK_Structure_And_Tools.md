# Ch6: APK Structure and Tools

## Concept
An APK is just a packaged Android app. To reverse it, you need to know where things live.

Plain-English version:
If you do not know the layout of the APK, you waste time opening the wrong files.

Common parts:

- `AndroidManifest.xml` = app identity, components, permissions
- `classes.dex` = Dalvik bytecode
- `smali/` = disassembled code after using `apktool`
- `res/` = XML layouts, strings, images
- `lib/` = native `.so` libraries
- `assets/` = arbitrary bundled files

Core beginner tools:

- `apktool` = decode and rebuild APKs
- `jadx` = view Java-like decompilation
- `adb` = interact with device or emulator
- `apksigner` = sign rebuilt APKs

Golden rule:
Use `jadx` to get the big picture, and use Smali when you need exact truth.

## Java vs. Smali

### Java
```java
public String getPkg() {
    return getPackageName();
}
```

### Smali
```smali
.method public getPkg()Ljava/lang/String;
    .registers 2

    invoke-virtual {p0}, Landroid/content/Context;->getPackageName()Ljava/lang/String;
    move-result-object v0
    return-object v0
.end method
```

What this teaches:

- JADX may show this clearly in Java
- Smali shows the exact call and exact return handling

## The 'Security Angle'
This chapter matters because investigation usually starts from files, not instructions.

Examples:

- Manifest reveals suspicious permissions
- `lib/` may hide native anti-analysis logic
- `assets/` may contain encrypted payloads
- multiple `classes*.dex` files may suggest a large or modular sample

In malware triage, APK structure often tells you where to look first.

## ASCII Visualization
```text
  sample.apk
     |
     +-- AndroidManifest.xml   -> permissions, components
     +-- classes.dex           -> app bytecode
     +-- res/                  -> resources
     +-- assets/               -> bundled files
     +-- lib/                  -> native code

  apktool decode
     |
     +-- smali/               -> readable Smali files
```

## Practice Lab
Tell me what this 3-line snippet does:

```smali
invoke-virtual {p0}, Landroid/content/Context;->getPackageName()Ljava/lang/String;
move-result-object v0
return-object v0
```

Hint:
This method asks Android for something about the app itself.
