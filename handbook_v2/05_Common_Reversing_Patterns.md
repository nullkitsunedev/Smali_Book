# Common Reversing Patterns

This section focuses on safe pattern recognition for analysis.

## Pattern 1: Load String -> Call Helper

```smali
const-string v0, "6874747073"
invoke-static {v0}, Lcom/demo/Util;->decode(Ljava/lang/String;)Ljava/lang/String;
move-result-object v1
```

What it usually means:

- hidden string
- decode helper
- likely obfuscation or configuration hiding

## Pattern 2: Method Call -> Boolean Branch

```smali
invoke-static {}, Lcom/demo/Checks;->isReady()Z
move-result v0
if-eqz v0, :fail
```

What it usually means:

- check result is stored
- false path jumps to fail
- likely trust or environment gate

## Pattern 3: Read Field -> Use In Network Call

```smali
iget-object v0, p0, Lcom/demo/App;->url:Ljava/lang/String;
invoke-static {v0}, Lcom/demo/Net;->fetch(Ljava/lang/String;)V
```

What it usually means:

- class stores endpoint or path
- endpoint is used immediately

## Pattern 4: Get Text -> toString -> Compare

```smali
invoke-virtual {v0}, Landroid/widget/EditText;->getText()Landroid/text/Editable;
move-result-object v0
invoke-virtual {v0}, Ljava/lang/Object;->toString()Ljava/lang/String;
move-result-object v0
```

What it usually means:

- input is read from the UI
- then converted to a plain string for checking or use

## Pattern 5: Array Data Block

```smali
new-array v0, v1, [I
fill-array-data v0, :array_0
```

What it usually means:

- fixed lookup values
- embedded data table
- sometimes obfuscated constants

## Pattern 6: Constructor Pair

```smali
new-instance v0, Ljava/lang/StringBuilder;
invoke-direct {v0}, Ljava/lang/StringBuilder;-><init>()V
```

What it usually means:

- object is created
- constructor immediately initializes it

## Pattern 7: Repeated Helper Method

If one helper method shows up everywhere, study that first.

Examples:

- `decode()`
- `checkEnv()`
- `buildRequest()`
- `getConfig()`

## Pattern 8: Manifest -> Component -> Method Flow

Workflow logic:

1. identify component
2. find its lifecycle method
3. inspect strings and helper calls
4. follow important branches
