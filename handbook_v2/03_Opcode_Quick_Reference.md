# Opcode Quick Reference

This is not every opcode. It is the set you will see constantly as a beginner.

## Constants

- `const/4` = small integer constant
- `const/16` = larger small integer
- `const` = full integer constant
- `const-string` = string constant
- `const-wide` = long or double constant

## Moves

- `move` = copy primitive value
- `move-object` = copy object reference
- `move-result` = store primitive return
- `move-result-object` = store object return
- `move-exception` = store caught exception

## Returns

- `return`
- `return-object`
- `return-void`

## Object Creation

- `new-instance` = create object
- `invoke-direct {v0}, Lx;-><init>()V` = call constructor

## Method Invocation

- `invoke-virtual`
- `invoke-static`
- `invoke-direct`
- `invoke-super`
- `invoke-interface`

## Field Access

- `iget`
- `iget-object`
- `iput`
- `iput-object`
- `sget`
- `sget-object`
- `sput`
- `sput-object`

## Comparisons and Branches

- `if-eqz`
- `if-nez`
- `if-eq`
- `if-ne`
- `if-lt`
- `if-gt`
- `if-le`
- `if-ge`
- `goto`

## Arithmetic

- `add-int`
- `sub-int`
- `mul-int`
- `div-int`
- `rem-int`
- `add-int/lit8`

## Arrays

- `new-array`
- `fill-array-data`
- `aget`
- `aget-object`
- `aput`
- `aput-object`
- `array-length`

## Type and Casting

- `check-cast`
- `instance-of`

## Switch

- `packed-switch`
- `sparse-switch`

## Exceptions

- `:try_start`
- `:try_end`
- `.catch`
- `move-exception`

## How To Read An Opcode Fast

Ask:

1. is it loading a value?
2. moving a value?
3. calling a method?
4. branching?
5. returning?
