# Ch5: Bypassing Security (Root checks and Signature verification)

## Concept
In authorized reverse-engineering work, "bypassing security" usually means understanding how a protection works so you can test app behavior, validate hardening, or analyze a sample in a controlled lab.

Plain-English version:
At the Smali level, many protections are just yes/no decisions. The app computes an answer, stores it in a register, and then branches based on that answer.

Two common patterns:

1. Root detection
2. Signature or integrity verification

These usually reduce to the same low-level pattern:

- gather a value
- compare it
- branch on success or failure

Examples of what analysts often see:

- checking for `su` binaries
- checking build tags like `test-keys`
- comparing package signatures
- setting a boolean such as `isTrusted`

The research value is in identifying:

- where the decision is made
- which register holds the result
- which branch enforces the decision

Beginner shortcut:
Do not start by trying to understand the whole protection. Start smaller:

1. find the method that returns `Z`
2. find the register that gets the result
3. find the `if-*` line that uses that register

## Java vs. Smali

### Java
```java
public boolean isTrusted(boolean sigOk) {
    if (!sigOk) {
        return false;
    }
    return true;
}
```

### Smali
```smali
.method public isTrusted(Z)Z
    .registers 3

    if-eqz p1, :bad

    const/4 v0, 0x1
    return v0

    :bad
    const/4 v0, 0x0
    return v0
.end method
```

This is intentionally simple because most real-world protections still collapse into branch logic like this.

In plain English:
"If the signature is not okay, return false. Otherwise return true."

## The 'Security Angle'
For malware analysis and appsec research, this is useful in safe, authorized scenarios:

- confirming whether a protection is only cosmetic
- testing how a sample behaves once anti-analysis checks are neutralized in a lab
- identifying weak client-side trust decisions
- proving that a signature check can be tampered with if it is enforced only on-device

Defensive lesson:
Any trust decision enforced only in client-side code can often be discovered and altered by a determined reverser. Strong security needs server-side verification, hardware-backed trust where appropriate, and integrity controls beyond a single boolean branch.

I'm deliberately keeping this chapter focused on recognition and defensive reasoning, not operational misuse.

## ASCII Visualization
```text
      [ START: verify() ]
               |
        p1 = sigOk / rootClean
               |
      if-eqz p1, :blocked
          /            \
         /              \
    [ p1 = 1 ]       [ p1 = 0 ]
      trusted          blocked
         |                |
      return 1         return 0
```

Another field-style pattern:

```text
  invoke-* checkSomething() --> move-result v0
                                |
                                v
                         if-eqz v0, :fail
```

How to read that in one sentence:
"Run the check, store the answer in `v0`, and jump to fail if the answer is false."

## Practice Lab
Tell me what this 3-line snippet does:

```smali
invoke-static {}, Lcom/demo/Checks;->isRooted()Z
move-result v0
if-eqz v0, :safe
```

Hint:
`isRooted()` returns a boolean. Then the branch checks whether that result is zero or non-zero.
