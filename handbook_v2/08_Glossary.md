# Glossary

## APK

Android application package.

## DEX

Dalvik Executable format used by Android runtime.

## Smali

Human-readable assembly-like representation of Dalvik bytecode.

## Baksmali

Tool or process used to disassemble DEX into Smali.

## Register

Temporary storage bucket used by Smali instructions.

## Local Register

`v` register used inside a method.

## Parameter Register

`p` register used for method inputs.

## Descriptor

Compact Smali type notation like `I`, `Z`, or `Ljava/lang/String;`.

## Directive

A structural line like `.class`, `.method`, or `.field`.

## Field

A variable stored inside a class or object.

## Method Signature

The method name with parameter types and return type.

## Branch

A jump in control flow based on a condition.

## Label

A jump target such as `:cond_0` or `:goto_1`.

## invoke-virtual

Instruction used to call a normal instance method.

## invoke-static

Instruction used to call a static method.

## invoke-direct

Instruction commonly used for constructors or private methods.

## move-result

Stores a primitive return value from a method call.

## move-result-object

Stores an object return value from a method call.

## iget

Reads an instance field.

## iput

Writes an instance field.

## sget

Reads a static field.

## sput

Writes a static field.

## Obfuscation

Techniques that make code harder to read or search.

## Multi-dex

APK layout with more than one `classes*.dex` file.

## Reflection

Dynamic access to classes or methods at runtime.

## Manifest

`AndroidManifest.xml`, which defines components, permissions, and app metadata.
