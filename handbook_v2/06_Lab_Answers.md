# Lab Answers

These are short answer keys for the practice-lab style snippets.

## Answer 1

```smali
const/4 v0, 0x1
add-int v1, p1, v0
return v1
```

Meaning:
It adds 1 to the input in `p1` and returns the result.

## Answer 2

```smali
.method public check(Ljava/lang/String;[I)Z
```

Meaning:
This method takes a `String` and an `int[]`, then returns a boolean.

## Answer 3

```smali
if-eqz p1, :no
const/4 v0, 0x1
return v0
```

Meaning:
If `p1` is zero, execution jumps to `:no`. Otherwise it returns true.

## Answer 4

```smali
iget-object v0, p0, Lcom/demo/App;->url:Ljava/lang/String;
invoke-static {v0}, Lcom/demo/Net;->fetch(Ljava/lang/String;)V
return-void
```

Meaning:
It reads the `url` field from the current object and passes it into `fetch()`.

## Answer 5

```smali
invoke-static {}, Lcom/demo/Checks;->isRooted()Z
move-result v0
if-eqz v0, :safe
```

Meaning:
It runs a root check, stores the boolean result in `v0`, and jumps to `:safe` if the result is false.

## Answer 6

```smali
const-string v0, "admin"
move-object v1, v0
return-object v1
```

Meaning:
It stores the string `"admin"` and returns it.

## Answer 7

```smali
invoke-virtual {p0}, Landroid/content/Context;->getPackageName()Ljava/lang/String;
move-result-object v0
return-object v0
```

Meaning:
It returns the current app package name.

## Answer 8

```smali
const-string v0, "ZmFrZQ=="
invoke-static {v0}, Lcom/x/Util;->decode(Ljava/lang/String;)Ljava/lang/String;
move-result-object v1
```

Meaning:
It loads an encoded-looking string and sends it into a decode helper, so the real value is probably hidden until runtime.
