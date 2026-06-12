Typelists

\---------



What You will learn ?



* What a typelist is and how it works
* How to create a typelist
* How to extend an existing typelist
* How to create a typekey field
* How to create a typekey field with a typelist filter



A Typelist is a pre defined list of allowed values for a field.



used to constraint / limit what a user can enter



In the UI it appears as a dropdown list



Example :

we need a Gender field in form it has 3 values only either male ,female, transgender.....



The user cannot type anything else — they can only pick from this list.

That list = the typelist.



Typelist File Locations

Project View

│

├── ...\\ Metadata \\ Typelist \\       ← READ-ONLY

│       Most platform \& base app typelists

│       Some marked as final (cannot be extended)

│

└── ...\\ Extensions \\ Typelist \\     ← EDITABLE (YOUR files)

&#x20;       Custom typelists you create

&#x20;       Typelist extensions you create

Location		Files		Editable?

\\Metadata\\Typelist\\	TTI, TIX	❌ Read-only

\\Extensions\\Typelist\\	TTI, TTX	✅ Editable



Three types of typelists:



1.Internal Typelists : cannot be modified or exntended only for reading.

2.external Typelists : for base application and can be modified or extended and add our own typecodes

3.Custome Typelists : entirely created by user both typelists and typecodes



Naming Convention for Custom Typecodes (Slide 14)



⚠️ Important Rule:

When extending base application typelists, all new typecodes you add must have codes ending in \_Ext





📌 Example:

Extending the VendorType base typelist:

xml<!-- WRONG — no \\\\\\\_Ext suffix -->

<typecode code="Plumber" name="Plumber"/>



<!-- CORRECT — ends with \\\\\\\_Ext -->

<typecode code="Plumber\\\\\\\_Ext" name="Plumber"/>





Create a Typelist

Steps to Create (Slide 16)

Step 1: Create the typelist file

Step 2: Define typecodes

Step 3: Optionally localize typecode descriptions

Step 4: Optionally regenerate the dictionary

Step 5: Deploy the typelist



1.Step 1 — Create the Typelist File (Slide 17)

In Project View:

Navigate to .../config/Extensions/Typelists



Right-click → New → Typelist



Naming convention: end with \_Ext



Examples of valid custom typelist names:



InteractionReason\_Ext

BuildingType\_Ext

DoctorSpecialty\_Ext

ContactCategory\_Ext



Step 2 — Define Typecodes





example



Step 1



<!-- DoctorSpecialty\\\\\\\_Ext.tti -->

<typelist name="DoctorSpecialty\_Ext"

&#x20;         desc="Specialization areas for doctors">



&#x20;   <typecode code="GeneralMedicine\_Ext"

&#x20;             name="General Medicine"

&#x20;             desc="General medical practice"

&#x20;             priority="1"

&#x20;             retired="false"/>



&#x20;   <typecode code="Cardiology\_Ext"

&#x20;             name="Cardiology"

&#x20;             desc="Heart and blood vessel specialist"

&#x20;             priority="2"

&#x20;             retired="false"/>



&#x20;   <typecode code="Orthopedics\_Ext"

&#x20;             name="Orthopedics"

&#x20;             desc="Bone and joint specialist"

&#x20;             priority="3"

&#x20;             retired="false"/>



&#x20;   <typecode code="Neurology\_Ext"

&#x20;             name="Neurology"

&#x20;             desc="Brain and nervous system specialist"

&#x20;             priority="4"

&#x20;             retired="false"/>



&#x20;   <typecode code="Pediatrics\_Ext"

&#x20;             name="Pediatrics"

&#x20;             desc="Children specialist"

&#x20;             priority="5"

&#x20;             retired="false"/>



&#x20;   <typecode code="OldSpecialty\_Ext"

&#x20;             name="Old Specialty"

&#x20;             desc="No longer used"

&#x20;             priority="99"

&#x20;             retired="true"/>



</typelist>





step 2 what gets created in db



step 3 add typekey field on entity



<!-- Doctor\\\\\\\_Ext.eti -->

<entity name="Doctor\\\\\\\_Ext">



&#x20;   <!-- Doctor's name -->

&#x20;   <column name="DoctorName\_Ext"

&#x20;           type="varchar"

&#x20;           nullok="false">

&#x20;       <columnParam name="size" value="100"/>

&#x20;   </column>



&#x20;   <!-- Doctor's registration number -->

&#x20;   <column name="RegNumber\_Ext"

&#x20;           type="varchar"

&#x20;           nullok="true">

&#x20;       <columnParam name="size" value="50"/>

&#x20;   </column>



&#x20;   <!-- Typekey field — links to DoctorSpecialty\\\\\\\_Ext typelist -->

&#x20;   <typekey name="Specialty\_Ext"

&#x20;            typelist="DoctorSpecialty\_Ext"

&#x20;            nullok="true"/>



</entity>



step 4 entity in db



step 5 what user see in UI



Doctor Form

──────────────────────────────────────

Doctor Name :  \[ Dr. Arjun Kumar      ]



Reg Number  :  \[ REG-001              ]



Specialty   :  \[ Cardiology      ▼   ]

&#x20;                General Medicine      ← priority 1 (top)

&#x20;                Cardiology            ← priority 2

&#x20;                Orthopedics           ← priority 3

&#x20;                Neurology             ← priority 4

&#x20;                Pediatrics            ← priority 5

&#x20;                (OldSpecialty hidden) ← retired=true

──────────────────────────────────────



step 6 Extend the Typelist Later



<!-- DoctorSpecialty\\\\\\\_Ext.ttx -->

<extension typelist="DoctorSpecialty\\\\\\\_Ext">



&#x20;   <typecode code="Dermatology\_Ext"

&#x20;             name="Dermatology"

&#x20;             desc="Skin specialist"

&#x20;             priority="6"

&#x20;             retired="false"/>



&#x20;   <typecode code="Psychiatry\_Ext"

&#x20;             name="Psychiatry"

&#x20;             desc="Mental health specialist"

&#x20;             priority="7"

&#x20;             retired="false"/>



</extension>

