# Exercise Answers

## Answer 1

```smali
invoke-static {}, Lcom/demo/Check;->ok()Z
move-result v0
if-eqz v0, :bad
```

Meaning:
Run a check, store the boolean result in `v0`, and jump to `:bad` if the result is false.

## Answer 2

```smali
const-string v0, "com.demo.Hidden"
invoke-static {v0}, Ljava/lang/Class;->forName(Ljava/lang/String;)Ljava/lang/Class;
move-result-object v0
```

Meaning:
The app is resolving a class dynamically through reflection.

## Answer 3

```smali
const-string v0, "demo"
invoke-static {v0}, Ljava/lang/System;->loadLibrary(Ljava/lang/String;)V
```

Meaning:
The app loads a native library named `demo`, so important logic may live in a `.so` file.

## Answer 4

```smali
.catch Ljava/lang/Exception; {:try_start_0 .. :try_end_0} :catch_0
```

Meaning:
A broad exception handler exists, which may hide failures or suppress visible errors.

## Answer 5

```smali
packed-switch p1, :pswitch_data_0
```

Meaning:
The method routes execution to different branches based on the value in `p1`.

## Answer 6

```smali
invoke-static {p0, v0}, Lkotlin/jvm/internal/Intrinsics;->checkNotNullParameter(Ljava/lang/Object;Ljava/lang/String;)V
```

Meaning:
This is often just Kotlin-generated null-check logic, not necessarily custom app behavior.
