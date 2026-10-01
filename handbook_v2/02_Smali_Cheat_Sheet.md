# Smali Cheat Sheet

## Fast Rules

- `p` = parameter register
- `v` = local register
- `p0` is usually `this` in non-static methods
- `0` often means `false`
- `1` often means `true`

## Common Directives

- `.class`
- `.super`
- `.field`
- `.method`
- `.registers`
- `.locals`
- `.end method`

## Common Types

- `V` = void
- `Z` = boolean
- `B` = byte
- `S` = short
- `C` = char
- `I` = int
- `F` = float
- `J` = long
- `D` = double
- `Ljava/lang/String;` = String
- `[I` = int[]
- `[Ljava/lang/String;` = String[]

## Common Instructions

- `const/4 v0, 0x1` = put small number in register
- `const-string v0, "abc"` = put string in register
- `move v1, v0` = copy value
- `move-object v1, v0` = copy object reference
- `move-result v0` = store return value
- `move-result-object v0` = store returned object
- `return v0` = return primitive
- `return-object v0` = return object
- `return-void` = return nothing

## Method Calls

- `invoke-virtual`
- `invoke-static`
- `invoke-direct`
- `invoke-interface`

Pattern:

```smali
invoke-static {v0}, Lcom/demo/Util;->decode(Ljava/lang/String;)Ljava/lang/String;
move-result-object v1
```

## Fields

- `iget-object` = read object field
- `iput-object` = write object field
- `sget-object` = read static object field
- `sput-object` = write static object field

## Branching

- `if-eqz v0, :fail`
- `if-nez v0, :ok`
- `if-eq v0, v1, :same`
- `goto :label`

## Arrays

- `new-array`
- `fill-array-data`
- `aget`
- `aget-object`
- `aput`
- `array-length`

## Debug Mindset

When lost:

1. read the signature
2. map `p` registers
3. track `v` registers
4. find `invoke-*`
5. find `if-*`
