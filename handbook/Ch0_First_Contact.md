# Ch0: First Contact

## Concept
Before reading Smali, you need one mental reset:
you are not reading nice source code anymore. You are reading a lower-level representation of app behavior.

Plain-English version:
Smali is like reading the app one machine-step at a time, but still in text form. It looks ugly at first because names are shorter and the code is more mechanical.

Five things to remember:

- `.method` starts a method
- `.end method` ends it
- `p` registers are parameters
- `v` registers are local work buckets
- labels like `:ok` and `:fail` are jump targets

How to survive your first Smali file:

1. Ignore most class noise
2. Find the method you care about
3. Read the signature
4. Mark `p0`, `p1`, `p2`
5. Follow data movement one line at a time

## Java vs. Smali

### Java
```java
public boolean isReady() {
    return true;
}
```

### Smali
```smali
.method public isReady()Z
    .registers 2

    const/4 v0, 0x1
    return v0
.end method
```

What happened:

- method name is `isReady`
- it takes no parameters because `()`
- it returns `Z`, which means boolean
- `v0` gets `1`
- `return v0` returns `true`

In plain English:
"This method always returns true."

## The 'Security Angle'
Your first job in reversing is not "understand everything." Your first job is "do not panic when the code looks unfamiliar."

In malware or appsec work, first contact usually means:

- locating the right class
- locating the interesting method
- deciding which lines are real logic and which are boilerplate

That triage skill saves a lot of time.

## ASCII Visualization
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

## Practice Lab
Tell me what this 3-line snippet does:

```smali
.method public ok()Z
const/4 v0, 0x0
return v0
```

Hint:
`0` usually means `false`.
