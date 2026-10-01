# Smali & Reversing Handbook

This handbook is split into eleven chapter files:

1. [Ch0_First_Contact.md](./Ch0_First_Contact.md)
2. [Ch1_The_Register_System.md](./Ch1_The_Register_System.md)
3. [Ch2_Method_Signatures_And_Type_Descriptors.md](./Ch2_Method_Signatures_And_Type_Descriptors.md)
4. [Ch3_Logic_And_Branching.md](./Ch3_Logic_And_Branching.md)
5. [Ch4_Field_Manipulation_And_Method_Invocations.md](./Ch4_Field_Manipulation_And_Method_Invocations.md)
6. [Ch5_Bypassing_Security.md](./Ch5_Bypassing_Security.md)
7. [Ch6_APK_Structure_And_Tools.md](./Ch6_APK_Structure_And_Tools.md)
8. [Ch7_Common_Smali_Instructions.md](./Ch7_Common_Smali_Instructions.md)
9. [Ch8_Android_Components.md](./Ch8_Android_Components.md)
10. [Ch9_Obfuscation_And_String_Hiding.md](./Ch9_Obfuscation_And_String_Hiding.md)
11. [Ch10_Malware_Analysis_Workflow.md](./Ch10_Malware_Analysis_Workflow.md)

Scope note:
This material is for authorized Android app analysis, malware research, and defensive reverse engineering in lab or permitted environments.

## Start Here If You Are Totally New

Read in this order:

1. Ch0
2. Ch1
3. Ch2
4. Ch3
5. Ch4
6. Ch5
7. Ch6
8. Ch7
9. Ch8
10. Ch9
11. Ch10

Three rules that will help immediately:

- `p` registers hold method parameters; in non-static methods, `p0` is usually `this`
- `v0`, `v1`, `v2` are temporary work buckets
- `0` often means `false`, and `1` often means `true`

How to read one Smali method:

1. Read the method signature first
2. Mark what each `p` register probably means
3. Track which `v` register gets new data
4. Watch for `if-*` and `goto` because they control the decision
5. Watch for `invoke-*` because that is where real work happens

Quick legend:

- `const/4` = put a small number into a register
- `move-result` = save the return value from a method call
- `return` = give back an integer or boolean
- `return-object` = give back an object like `String`
- `iget` = read from an object field
- `iput` = write to an object field

Useful free reference:

- Official Smali Wiki: https://github.com/JesusFreke/smali/wiki
