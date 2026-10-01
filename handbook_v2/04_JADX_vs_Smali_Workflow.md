# JADX vs Smali Workflow

## Rule

Use JADX for speed.
Use Smali for certainty.

## When JADX Is Better

- getting the big picture
- finding class names quickly
- reading long methods faster
- understanding Android component flow

## When Smali Is Better

- exact branch logic
- exact register flow
- exact method signatures
- exact object construction
- exact control flow when decompiled Java looks weird

## Practical Workflow

1. Open APK in JADX
2. Read manifest and components
3. Search strings, URLs, package names, permissions
4. Identify suspicious methods
5. Switch to Smali for:
   - branch checks
   - trust decisions
   - return values
   - helper method truth

## Example

JADX may show:

```java
if (check()) {
    run();
}
```

Smali may reveal:

```smali
invoke-static {}, Lcom/demo/Checks;->check()Z
move-result v0
if-eqz v0, :skip
invoke-static {}, Lcom/demo/Main;->run()V
```

That tells you exactly where the boolean is stored and how the branch is enforced.

## Beginner Mistake To Avoid

Do not trust decompiled Java blindly.
If something matters, confirm it in Smali.
