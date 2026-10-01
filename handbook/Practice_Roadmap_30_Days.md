# 30-Day APK Reversing Practice Roadmap

## Goal
This roadmap is designed to take you from:

- "I am completely new to Smali"

to:

- "I can open an APK, inspect the code, follow logic, and explain what the app is doing at a beginner practical level"

This is not magic. The key is consistency.

## Rules For This Roadmap

- Study every day, even if only for 30 to 45 minutes
- Do not try to memorize everything at once
- Repeat the same ideas on real APKs
- Write notes in simple English after every session
- If you get stuck, slow down and trace one register at a time

## What You Need

- this handbook
- `jadx`
- `apktool`
- `adb`
- a safe Android emulator or test device
- legal, safe APKs you are allowed to inspect

## Weekly Structure

- Week 1 = learning how Smali looks
- Week 2 = learning how app logic works
- Week 3 = learning how APK structure and Android components fit together
- Week 4 = learning how to analyze more realistic apps with a repeatable workflow

## Day 1

Read:

- `README.md`
- `Ch0_First_Contact.md`

Do:

- Learn what `.method`, `p0`, `v0`, and `return` mean
- Write down the difference between `p` registers and `v` registers

Success check:

- You can explain what `()Z` means

## Day 2

Read:

- `Ch1_The_Register_System.md`

Do:

- Trace 5 tiny Smali snippets by hand
- For each line, write what is inside `v0`, `v1`, and `p1`

Success check:

- You can explain "register = bucket"

## Day 3

Read:

- `Ch2_Method_Signatures_And_Type_Descriptors.md`

Do:

- Decode 15 method signatures by hand
- Practice reading `I`, `Z`, `V`, `L...;`, and `[`

Success check:

- You can read a signature without panicking

## Day 4

Read:

- `Ch3_Logic_And_Branching.md`

Do:

- Draw branch diagrams for 5 small `if-eqz` examples
- Practice saying what happens when the branch is taken and not taken

Success check:

- You can explain an `if-eqz` line in plain English

## Day 5

Read:

- `Ch4_Field_Manipulation_And_Method_Invocations.md`

Do:

- Find examples of `iget`, `invoke-*`, and `move-result`
- Trace how one value moves across 4 to 6 lines

Success check:

- You can explain "read field -> call method -> store result"

## Day 6

Read:

- `Ch5_Bypassing_Security.md`

Do:

- Practice identifying where a boolean check happens
- Look for `if-eqz`, `if-nez`, and boolean returns

Success check:

- You can identify which line enforces a yes/no decision

## Day 7

Review day.

Do:

- Re-read your notes from Days 1 to 6
- Pick 10 tiny snippets and explain them without looking at the handbook

Success check:

- You understand at least 60 percent of what you wrote earlier

## Day 8

Read:

- `Ch6_APK_Structure_And_Tools.md`

Do:

- Learn what lives in `AndroidManifest.xml`, `classes.dex`, `res/`, `assets/`, and `lib/`
- Open one safe APK in `jadx`

Success check:

- You know where to look first in an APK

## Day 9

Read:

- `Ch7_Common_Smali_Instructions.md`

Do:

- Collect examples of `const-string`, `move`, `return-object`, `new-instance`
- Write one-line meanings for each instruction

Success check:

- Common instructions stop looking random

## Day 10

Read:

- `Ch8_Android_Components.md`

Do:

- Identify one `Activity`, one `Service`, and one `BroadcastReceiver` in a safe APK

Success check:

- You can answer "what kind of component is this?"

## Day 11

Read:

- `Ch9_Obfuscation_And_String_Hiding.md`

Do:

- Search for suspicious `const-string` values
- Find one helper method that appears many times

Success check:

- You can recognize simple string-hiding patterns

## Day 12

Read:

- `Ch10_Malware_Analysis_Workflow.md`

Do:

- Apply the workflow to one safe APK
- Write a mini report with:
  - permissions
  - key components
  - suspicious strings
  - one interesting method

Success check:

- You can produce a small structured analysis note

## Day 13

Practice day.

Do:

- Open a second safe APK
- Repeat the same workflow faster

Success check:

- You waste less time jumping around randomly

## Day 14

Review day.

Do:

- Revisit all chapters from Ch0 to Ch10
- Make a one-page personal cheat sheet

Success check:

- You now have your own fast reference

## Day 15

Focus:

- signatures + registers together

Do:

- Pick 10 real methods
- For each one, write:
  - method name
  - parameters
  - return type
  - what `p0`, `p1`, `p2` likely mean

Success check:

- You can combine Ch1 and Ch2 naturally

## Day 16

Focus:

- branching on real code

Do:

- Find 5 real `if-*` branches in an APK
- Draw the true path and false path

Success check:

- Real branch logic becomes readable

## Day 17

Focus:

- field access and calls

Do:

- Trace 3 invoke chains from start to finish
- Stop only when you understand the final action

Success check:

- You can follow data instead of just reading lines

## Day 18

Focus:

- strings and network hints

Do:

- Search for URLs, hostnames, API routes, package names, and file paths

Success check:

- You can quickly spot interesting static indicators

## Day 19

Focus:

- Android lifecycle

Do:

- Find `onCreate`, `onStartCommand`, or `onReceive`
- Explain why that lifecycle method matters

Success check:

- You start understanding when code runs

## Day 20

Focus:

- package manager and security-related logic

Do:

- Search for methods involving package info, signatures, root checks, or installer checks

Success check:

- You can identify likely trust or environment checks

## Day 21

Review day.

Do:

- Re-analyze your first APK from Day 8
- Compare your current notes with your old notes

Success check:

- You clearly see improvement

## Day 22

Focus:

- obfuscation under pressure

Do:

- Open an APK with uglier names
- Ignore names and focus on method behavior

Success check:

- You rely less on readable names

## Day 23

Focus:

- helper methods

Do:

- Find one utility method used everywhere
- Explain what it really does

Success check:

- You can reduce noise by understanding one key helper

## Day 24

Focus:

- component-driven reasoning

Do:

- Pick one component and answer:
  - how is it triggered?
  - what does it read?
  - what does it call?
  - what happens next?

Success check:

- You can tell a behavior story, not just read instructions

## Day 25

Focus:

- comparing JADX and Smali

Do:

- Open the same method in both views
- Note where Java-like output is helpful
- Note where Smali is more exact

Success check:

- You stop trusting only one view

## Day 26

Focus:

- building mini reports

Do:

- Write a short report on one APK:
  - app purpose
  - permissions
  - main components
  - interesting strings
  - suspicious methods
  - your best guess about behavior

Success check:

- You can communicate findings clearly

## Day 27

Focus:

- repetition

Do:

- Re-do your weakest chapter
- Re-trace 10 snippets from that topic

Success check:

- Your weakest area gets less scary

## Day 28

Focus:

- time-boxed analysis

Do:

- Give yourself 45 minutes on a fresh safe APK
- Try to answer:
  - what is this app?
  - what matters?
  - what should I inspect next?

Success check:

- You can work under a simple structure

## Day 29

Focus:

- teaching as learning

Do:

- Explain one Smali method out loud as if teaching another beginner

Success check:

- If you can teach it simply, you probably understand it

## Day 30

Final checkpoint.

Do:

- Pick one safe APK
- Analyze it from manifest to suspicious methods
- Write a clean one-page summary

Success check:

- You can now reverse engineer APKs at a practical beginner level

## What "Success" Looks Like After 30 Days

If you complete this honestly, you should be able to:

- read basic Smali without freezing
- decode common method signatures
- trace registers across short methods
- understand simple and medium branch logic
- inspect Android components
- spot suspicious strings and helper methods
- explain basic app behavior in plain English

## What You Still Will Need After 30 Days

You will still improve a lot with:

- more APK practice
- more obfuscated samples
- native library analysis
- reflection-heavy apps
- advanced malware behaviors
- rebuilding and signing APKs for lab work

That is normal. Thirty days builds a real base. It does not finish the whole journey.

## Best Mindset

Do not ask:

- "Do I understand every line?"

Ask:

- "Can I explain what this method is trying to do?"

That mindset is what makes reverse engineering manageable.
