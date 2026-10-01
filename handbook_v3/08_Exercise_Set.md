# Exercise Set

## Exercise 1

What does this do?

```smali
invoke-static {}, Lcom/demo/Check;->ok()Z
move-result v0
if-eqz v0, :bad
```

## Exercise 2

What kind of pattern is this?

```smali
const-string v0, "com.demo.Hidden"
invoke-static {v0}, Ljava/lang/Class;->forName(Ljava/lang/String;)Ljava/lang/Class;
move-result-object v0
```

## Exercise 3

What does this suggest about the app?

```smali
const-string v0, "demo"
invoke-static {v0}, Ljava/lang/System;->loadLibrary(Ljava/lang/String;)V
```

## Exercise 4

Why might this matter?

```smali
.catch Ljava/lang/Exception; {:try_start_0 .. :try_end_0} :catch_0
```

## Exercise 5

What does this likely mean?

```smali
packed-switch p1, :pswitch_data_0
```

## Exercise 6

Why should you not overreact to this?

```smali
invoke-static {p0, v0}, Lkotlin/jvm/internal/Intrinsics;->checkNotNullParameter(Ljava/lang/Object;Ljava/lang/String;)V
```
