Further Study

\-------------



A **Script Parameter** is a global variable for the entire Guidewire application. Think of it as a setting knob — a developer sets it up once in code, but an Administrator can change its value at any time through the UI, without touching code or redeploying.



Real World Analogy:

Think of the brightness setting on your TV. The engineer who built the TV set a default brightness of 50. You (the user) can increase or decrease it anytime using the remote — without opening the TV or changing any hardware. The brightness setting = Script Parameter. The remote = Admin UI.



Two people involved:

Person		Role

Developer	Creates the parameter in XML with a default value

Administrator	Changes the value anytime via Admin UI — no code needed



Two Files Behind Script Parameters

\----------------------------------



1.ScriptParameters.xml - Developer defines parameters here Sets default values App ONLY uses params listed here

2.ScriptParameters.eti - Entity definition for DB storage When admin changes value → creates a record in database DB value overrides XML default



Creating a Script Parameter — 4 Steps

\-------------------------------------



Step 1 — Open ScriptParameters.xml:

Studio → Project View → …\\config\\resources\\ScriptParameters.xml → open in XML editor.

Step 2 — Add the parameter in XML:

xml<ScriptParameterPack

&#x20;   ParamName="MaximumViewedContacts"

&#x20;   ParamType="integer">

&#x20; <ParamValue>5</ParamValue>

</ScriptParameterPack>

Three things you must define:



ParamName — the name used to reference it in Gosu code

ParamType — the data type (integer, String, boolean, decimal)

<ParamValue> — the default value when no DB value exists



Step 3 — Deploy (restart the server):

Script parameters are read at server startup — a restart is required.

Step 4 — Reference in Gosu code:

gosustatic property get maximumViewedContacts() : int {

&#x20; var value : int = 5            // safe default fallback

&#x20; if (ScriptParameters.MaximumViewedContacts > 0) {

&#x20;   value = ScriptParameters.MaximumViewedContacts

&#x20; }

&#x20; return value

}

Syntax to access: ScriptParameters.YourParameterName



Why the safety check > 0? Defensive programming. If someone accidentally sets the value to 0 or a negative number, the code falls back to a safe default of 5 rather than breaking the UI.



Admin Updating a Script Parameter — No Code Needed

\--------------------------------------------------

Steps for the Admin:



Log in as Administrator

Go to Administration → Utilities → Script Parameters

Click the parameter name in the list

Click Edit → change the Value field → click Update



The change is immediate and global — every logged-in user sees the new value right away. No server restart, no code deployment needed.

Real world comparison: Like a manager changing the office Wi-Fi password from their phone — everyone gets the new password immediately, no IT team needed.



A **Dynamic Dropdown** is a dropdown list whose options come from live data in the database at the moment the user opens the form — not from a fixed pre-defined list.



Real World Analogy:

Think of ordering food at a restaurant. A static menu (printed) shows the same items every day — that's a typecode dropdown. A daily specials board changes based on what ingredients are available today — that's a dynamic dropdown. The options depend on real, current data.





Range Widgets — The Building Block of Dynamic Dropdowns

\-----------------------------------------------------

Dynamic dropdowns are created using Range widgets — not regular Input or TypeKey Input widgets.

Two range widgets:



Range Input — used inside Detail Views (single record forms)

Range Cell — used inside List Views (table columns)



Two essential properties:

valueRange — the list/array of objects to show as dropdown options

value — stores which option the user selected



A **Dependent Dropdown** is a parent-child pair of dropdowns where choosing a value in the parent automatically filters what appears in the child dropdown.



Real World Analogy — Country and State:

When you book a flight online, you pick a Country first. The moment you select "India", the State dropdown automatically shows only Indian states. Switch to "USA" → only US states appear. The State dropdown DEPENDS on the Country dropdown. That's a dependent dropdown.



TypeKey Fields and TypeFilters — The Foundation

\-------------------------------------------------

TypeKey Field = the dropdown box itself — the field on your form where a user picks a value.

TypeFilter = the filter applied to that dropdown — controls which values from the full list are actually shown to the user.



Real life analogy:

Imagine a food delivery app with a "Cuisine" filter.

The full list of cuisines = the Typelist (Indian, Chinese, Italian, Mexican, Thai, Greek, Japanese...)

The "Cuisine" dropdown on the form = the TypeKey Field (the actual field you interact with)

The "Show only Asian cuisines" filter = the TypeFilter (restricts what appears in that dropdown)



Three Filter Types — Static and Dynamic

\-----------------------------------------



Typefilter (Static)

What it does

It permanently cuts down a typelist to a fixed smaller list. The dropdown always shows the same values — it doesn't matter what the user does or what other fields are selected.



Category Filter (Dynamic)

What it does

When the user picks a value in the parent dropdown, the child dropdown automatically changes to show only the values linked to that parent choice. Different parent = different child options





Categorylist Filter (Catch-All)

What it does

This is a special version of the category filter. One particular child typecode is linked to ALL parent values — so it always shows in the Specialty dropdown no matter what Category is selected.



Validation Rules

\----------------

Validation rules are the quality control gate before data gets saved. They check whether the data is correct and complete. If something is wrong, they either completely block saving (error) or warn the user but let them save anyway (warning).



Two Types of Validation

\--------------------------



Type 1 — Prevent Bad / Invalid Data

What it means

This validation simply says: "This data is wrong. Fix it before saving." It has nothing to do with levels or maturity. It's just a basic data quality check.

Think of a flight booking form online.

You try to book a ticket and leave the "Date of Travel" field empty.

The form immediately says: "Date of Travel is required. You cannot proceed."

It doesn't matter how many other fields you filled — that one wrong/missing field blocks you.

That's Type 1 validation.



Type 2 — Object Maturity (Validation Levels)

What it means

This is completely different from Type 1. Here, the claim has a maturity level — a number that represents how "complete" and "ready" the claim is. The claim can only advance to a higher level if certain fields are filled in. This controls the claim's entire lifecycle.

Real World Analogy



Think of a student's academic progression:

Level 1 (Grade 1) — just enrolled, basic details needed

Level 2 (Grade 5) — must pass basic exams before moving up

Level 3 (Grade 10) — must pass board exams

Level 4 (Grade 12) — must pass final exams

You cannot jump from Grade 1 to Grade 12 — you must pass each level's requirements. If you're already in Grade 10 and somehow fail Grade 10 requirements, you cannot go back to Grade 9 either.

The claim is the student. The levels are the grades. Validation rules are the exams.









Validation Levels — The Maturity Ladder

\-----------------------------------------

A claim in ClaimCenter grows through maturity levels — like a student progressing from Grade 1 to Grade 12. Each level has requirements. The claim can only advance if those requirements are met.



Warnings vs Errors — The Critical Difference

\--------------------------------------------

&#x20;Real analogy:

Warning = Yellow traffic light. You CAN go, but be careful.

Error = Red traffic light + barrier. You CANNOT go. Stop completely.



The reject() Method — Writing Validation Rules

\----------------------------------------------

The reject() method is what you call inside a validation rule to raise a warning or error. It takes 4 arguments:

gosuobject.reject(errorLevel, errorMessage, warnLevel, warnMessage)



Argument		What it does

errorLevel		At which level does this become a blocking error?errorMessage		Message shown when it's an error

warnLevel		At which level is this just a warning?

warnMessage		Message shown when it's a warning



The rejectField() Method — Highlighting the Exact Problem Field

\----------------------------------------------------------------

reject() shows a message but doesn't highlight anything. rejectField() goes further — it puts a red highlight on the specific field that has the problem so the user can find it instantly.



The rejectSubField() Method — For Fields on Related Objects

\------------------------------------------------------------

When the problem field is on a related object (like a field on an Incident that belongs to a Claim, or a field on an array element), use rejectSubField()

