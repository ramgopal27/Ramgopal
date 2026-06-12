Gosu Concepts

\-------------



Gosu Programming

\-----------------



Gosu is the programming language used inside Guidewire applications. Just like how a calculator runs on logic circuits, Guidewire's business logic runs on Gosu. You use it to make the application smart — deciding who gets assigned a claim, calculating someone's age, hiding fields based on conditions, and running complex business rules.



Example : If Guidewire is a smart vending machine, Gosu is the software running inside it — deciding what to show, what to calculate, and what to do when you press a button.



Key facts:

Similar to Java — if you know Java, Gosu will feel familiar

Has both procedural (step-by-step instructions) and object-oriented (objects + properties) features

Case-sensitive (from v8 onwards) — ABContact is not the same as abcontact



Where Gosu is Used in Guidewire

\----------------------------------

Gosu appears in five places across Guidewire applications:



1.Entity Enhancement

2.Business Rules

3.Gosu Classes

4.PCF UI Behaviour

5.workflows



The **Gosu Scratchpad** is like a mini-editor inside Guidewire Studio where you can write, test, and debug Gosu code live. Think of it as a calculator's test mode — you type code, hit Run, and see results immediately.

Open it with: Main Menu → Tools → Gosu Scratchpad or ALT+SHIFT+S



Three parts of the Scratchpad:



Code Editor — where you write Gosu code

Run Window — shows output when you run code without a server (for basic logic)

Debug Window — shows output when running in the Debug process (needed for database queries)



Gosu Statements — Writing Code

\------------------------------

Variables

gosuvar counter1 : int = 10

var counter2 = 0       // type inferred automatically

var name : String = "John"



No semicolons needed — Gosu detects statement endings automatically. You can use ; but Guidewire recommends against it.

Comments

gosu// This is a single-line comment   (CTRL+/)

/\* This is a

&#x20;  multi-line comment \*/           (CTRL+SHIFT+/)

If-Else

gosuvar status = "Open"

if (status == "Open") {

&#x20; print("Claim is open")

} else if (status == "Closed") {

&#x20; print("Claim is closed")

} else {

&#x20; print("Unknown status")

}

Ternary Operator — Short If-Else

gosuvar result = (score > 50) ? "Pass" : "Fail"



A **Gosu Query** is how you search the Guidewire database for records. Instead of writing SQL, you write Gosu code that Guidewire translates to SQL internally. You tell it what entity to search, and what conditions to filter by.



A **Gosu Class** is a custom, reusable code file that groups related methods together. While an Enhancement adds methods TO an existing entity, a Gosu Class is a completely new, standalone file for logic that doesn't naturally belong to any single entity.

