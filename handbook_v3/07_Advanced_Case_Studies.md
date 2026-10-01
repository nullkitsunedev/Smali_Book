# Advanced Case Studies

These are safe pattern-analysis case studies.

## Case Study 1: Broad Catch Hides Failure

```smali
:try_start_0
invoke-static {}, Lcom/demo/Net;->send()V
:try_end_0
goto :done

.catch Ljava/lang/Exception; {:try_start_0 .. :try_end_0} :catch_0
```

Takeaway:
If everything is wrapped in a broad catch, errors may be deliberately hidden.

## Case Study 2: Kotlin Noise Before Real Logic

```smali
const-string v0, "arg"
invoke-static {p0, v0}, Lkotlin/jvm/internal/Intrinsics;->checkNotNullParameter(Ljava/lang/Object;Ljava/lang/String;)V
```

Takeaway:
Do not over-focus on compiler-generated null checks.

## Case Study 3: Reflection Hides Target

```smali
const-string v0, "com.demo.Hidden"
invoke-static {v0}, Ljava/lang/Class;->forName(Ljava/lang/String;)Ljava/lang/Class;
```

Takeaway:
The real target may not appear as a direct method call anywhere else.

## Case Study 4: Native Boundary

```smali
invoke-virtual {p0}, Lcom/demo/App;->getSecret()Ljava/lang/String;
move-result-object v0
```

Takeaway:
If `getSecret()` is native, the important behavior is outside Smali.

## Case Study 5: Loader Pattern

```smali
new-instance v0, Ldalvik/system/DexClassLoader;
invoke-direct {v0, v1, v2, v3, v4}, Ldalvik/system/DexClassLoader;-><init>(Ljava/lang/String;Ljava/lang/String;Ljava/lang/String;Ljava/lang/ClassLoader;)V
```

Takeaway:
A second-stage code path may exist.
