Creating New Entities

\---------------------



What You Will Learn



* What a custom entity is and how to create one
* How to add elements (fields, foreign keys, arrays) to an entity
* Understand related data model design elements



Custom Entities

\---------------



There are three types of entities.



1.Platform Entity - Guidewire creates - Common for all applications - ETI

2.Application Entity - Guidewire creates - specific to one application only - ETI

3.Custom Entity - User -  Your own extensions - ETI or EIX



Example :

User → Platform entity (exists in all apps)

Claim → Application entity (only in ClaimCenter)

Building\_Ext → Custom entity (created by user)



Entity File Types:

* ETI file — defines a brand new entity (new table in database)
* EIX file — extends an existing entity (adds fields to existing table)



Custom Entity + Gosu Class

When you create a custom entity, Guidewire automatically generates a Gosu class for it.



Example — Building\_Ext custom entity:

You create entity Building\_Ext with fields:



NumberOfEmployees

InspectionDate

HasParking



Guidewire automatically:



Creates database table abx\_building

Creates a Gosu class Building\_Ext

Creates instances like aBuilding\_Ext in memory at runtime



Flow diagram



Building\_Ext (entity definition)

&#x20;       ↓

abx\_building (database table)

&#x20;       ↓  read from database

aBuilding\_Ext (Gosu class instance in memory)

&#x20;       ↓  save to database

abx\_building (updated)



Creating and Defining an Entity

\-------------------------------



Steps to Create a Custom Entity

There are 4 steps:

Step 1: Create the entity file

Step 2: Add elements and specify attributes

Step 3: Optionally regenerate the dictionary

Step 4: Deploy the custom entity



Step 1 :



In Guidewire Studio → Project View:

Navigate to:

...\\ config \\ Extensions \\ Entity



Right-click → New → Entity

Naming Rules:



Maximum 25 characters

Use CamelCase format

Always end with \_Ext (marks it as a custom entity)

The database table name automatically removes \_Ext



Optional Entity Attributes

\-------------------------

When creating the entity, you can set these optional attributes:

1\. Extendable



For internal Guidewire use only

You don't need to set this for custom entities



2\. Exportable



Default = false

If set to true → allows the entity to be serialized (converted to data format) for SOAP API and RPC-E web services

Useful when you want to expose this entity to external systems



3\. Final



Default = true

If true → this entity cannot be a supertype (no subtypes can be created from it)

If false → other entities CAN inherit from this entity (create subtypes)





📌 Example:

xml<entity name="Building\_Ext"

&#x20;       exportable="true"

&#x20;       final="false">

This entity can be exported via web services AND can have subtypes





Step 2 — Add Elements and Attributes

You add elements (fields) to your entity using either:



Toolbar → select from dropdown → click + to add more

Context menu → right-click → Add new → select option



4 Common Elements You Can Add

\-----------------------------



1.<column /> — Data Field

Defines a simple data field with a specific data type.

Supported data types:



bit — True/False (boolean)

datetime — Date and time

integer — Whole number

varchar — Text (variable length)





Example:

<column name="NumberOfEmployees" type="integer" nullok="true"/>

<column name="InspectionDate"    type="datetime" nullok="true"/>

<column name="HasParking"        type="bit"      nullok="true"/>

<column name="BuildingName"      type="varchar"  nullok="true">

&#x20;   <columnParam name="size" value="60"/>

</column>



NumberOfEmployees → stores a number like 150

InspectionDate → stores a date like 2024-03-15

HasParking → stores true or false

BuildingName → stores text up to 60 characters



Important Rule: Do NOT end custom field names with \_Ext



2\. <foreignkey /> — Foreign Key Field

Defines a link/pointer to another entity (one-to-one relationship).



Example — Linking Building to a Contact:

<foreignkey name="PrimaryContact"

&#x20;           columnName="PrimaryContactID"

&#x20;           fkentity="ABContact"

&#x20;           nullok="true"/>



name → field name used in Gosu code

columnName → actual column name in database (always field name + ID)

fkentity → which entity this points to (ABContact)



In database table abx\_building:

ID	BuildingName	PrimaryContactID

1	Chennai HQ	42 (→ points to ABContact row 42)

2	Mumbai Office	87 (→ points to ABContact row 87)



Naming Convention:



Field name: PrimaryContact

Column name: PrimaryContactID (field name + ID)

For extensions: field name ends with \_Ext, column name ends with \_ExtID



3\. <array /> — Array Field

Defines a collection of related objects (one-to-many relationship).



Example — Building has many Inspections:

<array name="Inspections"

&#x20;      arrayentity="Inspection\_Ext"/>



This means: one Building\_Ext can have many Inspection\_Ext records.



Important rule: If entity A has an array of entity B, then entity B MUST have a foreign key pointing back to entity A.



So Inspection\_Ext must have:

<foreignkey name="Building"

&#x20;           columnName="BuildingID"

&#x20;           fkentity="Building\_Ext"

&#x20;           nullok="false"/>



Naming Convention:



Use plural form of the name

For extensions: name ends with \_Ext



4.<typekey /> — Typekey Field

Defines a field that is linked to a typelist (dropdown list of values).



Example — Building Type:

<typekey name="BuildingType"

&#x20;        typelist="BuildingType"

&#x20;        nullok="true"/>

This links to a BuildingType typelist that might contain:



Office

Warehouse

Retail

Manufacturing



Entity Editor: Attribute Pane (Slide 13)

When you click on any element in the editor, you see its attributes in a panel:

AttributeStyle	Meaning

Bold		Required — must be filled in

Black		Editable — optional but you can set it

Grayed out	Non-editable — read-only



Special attribute — nullok:



No default value set automatically

Always set it to true in most cases (means the field can be empty/null)

Set to false only when the field is mandatory



Add Subelements

Some elements need subelements to provide extra configuration.

Most important: <columnParam /> — required for varchar type to specify the text size.



Example — varchar field needs size:

xml<!-- WRONG — missing size, will cause error -->

<column name="Summary" type="varchar" nullok="true"/>



<!-- CORRECT — size specified via columnParam -->

<column name="Summary" type="varchar" nullok="true">

&#x20;   <columnParam name="size" value="60"/>

</column>

Without <columnParam name="size">, the system throws an error like:

ERROR: Column "Summary" doesn't have column parameter "size"

&#x20;      required for "varchar" data type.



Entity Names

\------------

What is an Entity Name?



It's how the entity's name is displayed in the UI to the user — a human-readable label.



Where it's defined:

\\configuration\\config\\Entity Names\\

Physical folder: displaynames



How to access in Gosu code:

gosu

&#x09;anABContact.DisplayName

→ Returns the display name of that contact object



Rules:



Entity names are defined using Gosu code

Many base entities already have entity names defined

For every new custom entity that appears in the UI → always create a default entity name

Edit using the Entity Name Editor in Studio





Example:

Without entity name → UI might show: Building\_Ext#1234

With entity name defined → UI shows: "Chennai HQ Building" ← much more readable!



Array

\--------

A collection of pointers to multiple instances of another entity. Maintained by code at runtime (not directly stored).

Requires: A reverse foreign key on the child entity.



Rule: If Entity A has an array of Entity B → Entity B must have a foreign key pointing back to Entity A.





Example:

Each ABContact can have zero to many ContactNotes:



On ABContact (parent):



xml<array name="ContactNotes" arrayentity="ContactNote"/>



On ContactNote (child) — must have reverse foreign key:



xml<foreignkey name="Contact"

&#x20;           columnName="ContactID"

&#x20;           fkentity="ABContact"

&#x20;           nullok="false"/>



Database — contactnote table:



NoteID	Subject			ContactID

1	"Called Monday"		1 (→ ABContact row 1)

2	"Meeting Friday"	1 (→ ABContact row 1)

3	"Sent email"		2 (→ ABContact row 2)



So anABContact.ContactNotes returns rows 1 and 2 for Contact 1.



Naming Convention:



Use plural form: ContactNotes (not ContactNote)

For extensions: ContactNotes\_Ext



Delegates

\-----------

A delegate is a virtual/reusable entity — a bundle of fields and/or methods that can be shared across multiple unrelated entities.

Think of it like a reusable plugin you attach to entities.

Two ways to use delegates:



<implementsEntity /> — attach an existing delegate to your entity

<delegate /> — create a new delegate



Extending Entities

\-------------------



Base Application Entities



There are 3 layers of entities in Guidewire:



┌─────────────────────────────────────────────┐

│           Guidewire Platform                │

│   Platform Entities (common to ALL apps)    │

│   Examples: Activity, User                  │

│                                             │

│  ┌──────────┐ ┌──────────┐ ┌────────────┐  │

│  │ClaimCenter│ │PolicyCtr │ │BillingCtr  │  │

│  │  Claim   │ │  Policy  │ │  Producer  │  │

│  │(App ETI) │ │(App ETI) │ │ (App ETI)  │  │

│  └──────────┘ └──────────┘ └────────────┘  │

└─────────────────────────────────────────────┘

&#x20;       ↓ Customer adds on top

&#x20;  Claim.etx  Policy.etx  Producer.etx





File Type	Full Name			Who Creates It		Purpose

ETI		Entity				Guidewire		Defines a brand new entity

EIX		Internal Entity Extension	Guidewire		Guidewire's own internal extensions to its entities

ETX		Entity Extension		YOU (Customer)		YOUR extensions/additions to existing entities

