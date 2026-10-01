# Case Studies

These are safe learning case studies. They focus on analysis thinking, not unauthorized tampering.

## Case Study 1: Constant Boolean Return

```smali
.method public isReady()Z
    .registers 2
    const/4 v0, 0x1
    return v0
.end method
```

What to learn:

- method signature
- boolean return
- constant true

Analyst note:
This method is not a real check. It always returns true.

## Case Study 2: UI Input To String

```smali
invoke-virtual {v0}, Landroid/widget/EditText;->getText()Landroid/text/Editable;
move-result-object v0
invoke-virtual {v0}, Ljava/lang/Object;->toString()Ljava/lang/String;
move-result-object v0
```

What to learn:

- UI input is being read
- editable text becomes a string

Analyst note:
This usually appears before validation, comparison, or request submission.

## Case Study 3: Decode Helper

```smali
const-string v0, "6874747073"
invoke-static {v0}, Lcom/demo/Util;->decode(Ljava/lang/String;)Ljava/lang/String;
move-result-object v1
```

What to learn:

- encoded constant
- helper method likely hides real text

Analyst note:
Check the helper method before reading the rest of the class.

## Case Study 4: Branch Enforced By Boolean

```smali
invoke-static {}, Lcom/demo/Checks;->ok()Z
move-result v0
if-eqz v0, :blocked
```

What to learn:

- method returns boolean
- branch depends on that return

Analyst note:
This is a standard trust-gate pattern.

## Case Study 5: Array Data

```smali
new-array v0, v1, [I
fill-array-data v0, :array_0
```

What to learn:

- fixed embedded data
- maybe lookup or transformation table

Analyst note:
When values look strange, inspect how the array is used later.
