# Ch3: Logic & Branching (if-eqz, goto, and patching)

## Concept
Most app decisions in Smali are simple jumps.

Plain-English version:
An `if-*` instruction is just a fork in the road. If the condition matches, execution jumps to a label. If it does not match, execution keeps moving downward.

The common pattern:

1. Put a value in a register
2. Test the register
3. Jump to a label if the condition matches

Key instructions:

- `if-eqz v0, :label` = jump if `v0 == 0`
- `if-nez v0, :label` = jump if `v0 != 0`
- `goto :label` = unconditional jump
- labels like `:fail` or `:ok` are jump targets

For booleans:

- `0` usually means false
- `1` usually means true

This is why so many reverse-engineering patches are tiny. One changed branch can completely alter behavior.

Beginner shortcut:
When you see a label like `:fail`, draw two arrows:

- jump taken
- jump not taken

That makes the logic much easier to follow.

## Java vs. Smali

### Java
```java
public boolean canLogin(boolean valid) {
    if (!valid) {
        return false;
    }
    return true;
}
```

### Smali
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

What happened:

- `p1` contains `valid`
- `if-eqz p1, :fail` means "if valid is false, jump to fail"
- otherwise return `1`

In plain English:
"If login is not valid, return false. Otherwise return true."

## The 'Security Angle'
This is the core of many defensive research tasks:

- locating a root check
- understanding anti-emulator logic
- tracing a premium/license gate
- verifying whether malware enables a payload only under certain conditions

In authorized lab analysis, researchers often inspect branch points to answer:

- What condition triggers the malicious payload?
- What disables app functionality?
- What causes a sample to exit on rooted devices or emulators?

Important safety boundary:
For real targets, only modify or patch software you own or are explicitly authorized to assess.

## ASCII Visualization
```text
      [ START ]
          |
      p1 = valid
          |
   if-eqz p1, :fail
      /          \
     /            \
 [p1 != 0]      [p1 == 0]
     |              |
 const/4 v0,1   :fail
 return v0       const/4 v0,0
                 return v0
```

Another common view:

```text
  Test Register:
    v0 = 0  -> false path
    v0 = 1  -> true path

  Branch:
    if-eqz v0, :blocked
```

Foundational branch picture:

```text
      [ START: checkLicense() ]
                 |
        [ Is user Premium? ]
        [ Register v0 = ?  ]
                 |
        /--------+--------\
       |                   |
  [ v0 is 0 ]        [ v0 is 1 ]
  (False/Fail)       (True/Pass)
       |                   |
 [ if-eqz v0, :fail ]      |
       |                   |
       V                   V
  [:fail Label]      [:success Label]
  (Close App)        (Grant Access)
```

## Practice Lab
Tell me what this 3-line snippet does:

```smali
if-eqz p1, :no
const/4 v0, 0x1
return v0
```

Hint:
Ask yourself, "What happens only when `p1` is not zero?"
