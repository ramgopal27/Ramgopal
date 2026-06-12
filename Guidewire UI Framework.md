Guidewire UI Framework

\----------------------



In most web application , developers write HTML, CSS , JS directly. Guidewire works differently. Instead of writing HTML, you configure PCF files - XML based configuration files that describe what the UI looks and how it behaves.

Guidewire reads these files and render them as HTML automatically.



Think of it like this: instead of drawing every brick of a wall yourself, you hand Guidewire a blueprint (PCF), and it builds the wall for you.



Two Fundamental Categories :



Every PCF element belongs to one of two categories:



widget : anything the user can see on screen. A text field, a table, a button, a whole form panel — all widgets.



Location : anywhere the user can navigate to. When you click a link and a new page loads, you navigated to a Location. Locations don't contain visual content themselves; they reference Screens, which do



Real-world example: In Claim Center, when you click on a claim number and the Claim Summary page opens — you navigated to a Location (the Claim Summary page location). What you see on that page — the fields, the panels, the buttons — are all Widgets.



Atomic Widgets

\--------------



An atomic widget is the smallest, non-divisible building block of the Guidewire user interface.Each atomic widget does exactly one thing: display one data value or trigger one action.



It does exactly one of two things:



* Displays one piece of data (like showing a person's name or date of birth)
* Executes one action (like clicking a Save button)



they are three types of atomic widgets:



1.Input widget : An Input widget displays and edits one field of a single record

2.Cell widget : A Cell widget displays one field per row in a List View (a table showing many records)

3.Button widget : A Button widget executes one action when clicked. It lives in a Toolbar (the bar of action buttons at the top of panels)



Container widgets

\-----------------

A container widget is a collection of atomic widgets and/or other container widgets, organised into a logical group.



You can't show a useful form using only atomic widgets scattered randomly. You need a container to arrange them properly — with layout, structure, labels, and sections.



Locations

\---------



A Location is like a street address. It tells the application "where to go." It does NOT decorate the house — it just identifies it.



In Guidewire, every time you click something and a new page or window opens — you navigated to a Location. The Location itself holds no buttons, no fields, no tables. It simply points to a Screen, and that Screen holds all the visual content.





Location = the destination address (e.g., "123 Main Street")

Screen = the building at that address (the actual visual content)

Panels + Widgets = the furniture inside the building (Detail Views, List Views, Input fields)



7 types of Locations :



1.Page : A Page is the simplest location — it holds exactly one Screen and displays it in the main content area.



2.Location group : A Location Group is a collection of related Pages grouped together under one shared info bar, actions menu, and sidebar. In the UI, it appears as tabs.



3.Wizard : A Wizard is an ordered sequence of steps for completing a complex business process. It has Back and Next buttons built in.

Simple analogy

A Wizard is like a step-by-step form to apply for a passport:



Step 1: Fill in personal details

Step 2: Upload documents

Step 3: Confirm appointment

Step 4: Review and submit



4.popup : A Popup is a small overlay window that appears on top of your current page. When you close it, you automatically return to exactly where you were before.



5.worksheet : A Worksheet contains one Screen but renders it in the workspace frame — a secondary panel area on the right side of the screen — instead of the main content area.



6.Forward : A Forward runs business logic first and then automatically redirects to one of several other locations — the user never sees a Forward directly.



7.Exit Point : An Exit Point takes the user completely out of the Guidewire application and sends them to an external URL or another application.





PCF files

\---------



A PCF file is an XML file that defines either a Location or a Container Widget. It may also define child containers inside it, reference other PCF files as embedded child containers, and include atomic widgets.



PCF File Naming Conventions

The filename always ends with a suffix that tells you what type of container it defines:

Suffix		Type			Example

DV		Detail View Panel	ABContactSummaryDV

LV		List View Panel		ABContactHistoryLV

InputSet	Input Set		GlobalAddressInputSet

CV		Card View Panel		ABContactCV

LDV		List Detail Panel	ABContactAddressesLDV

Screen		Screen			ABContactSummaryScreen

LG		Location		GroupABContactLG

Wizard		Wizard			NewClaimWizard



The Power of PCF Reusability

One of the most important benefits of PCF files is reusability. If you create a container widget as its own PCF file, any other PCF file can reference and embed it.



Debug Tools

\-----------

shortcuts in image downloaded



The PCF Editor

\--------------

The PCF Editor is the visual tool inside Guidewire Studio for viewing and editing PCF files



Deploying PCF Changes

\---------------------

PCF files are read by the server at startup. There are two ways to deploy changes:

Full server restart — always works, reads all PCF files fresh from disk.

Hot reload without restart — press ALT+SHIFT+L (requires debug tools enabled) or go to Internal Tools → Reload → Reload PCF Files in the running application. This reloads all PCFs instantly without restarting.



Developer workflow in practice:



Open PCF in Studio using ALT+SHIFT+E on the screen you want to change

Make your changes in the PCF Editor

Press Build → Recompile to check for errors

Switch to the browser and press ALT+SHIFT+L to reload

Refresh the page — your changes appear immediately





Introduction to Locations

\-------------------------

A Location is like a street address in Guidewire. It tells the application where to go when you click something. The Location itself has no buttons or fields — it just points to a Screen, and the Screen has the actual content the user sees.



The key rule: Location = destination. Screen = content at that destination.



How Screens Connect Locations to Content

\----------------------------------------

A Location doesn't hold panels or widgets directly. It references a Screen, and the Screen holds all the actual content — Detail View Panels, List View Panels, Input Sets, Buttons, etc.

Think of it like this:



You navigate to an address (Location)

You arrive at a building (Screen)

Inside the building is the furniture (Panels + Widgets)



How to Make a Widget Navigate — The 3 Navigation Methods

\---------------------------------------------------------

Most atomic widgets have an action property. When you set this property to a Gosu navigation statement, clicking that widget takes the user to a location.

The syntax is always:

LocationName.method(objectToPass)

There are exactly 3 methods:

.go() — Opens the location in the same main frame. Replaces what was there.



Use for: Pages, Location Groups, Wizards, Forwards

Example: ABContactLG.go(anABContact)



.push() — Opens the location but remembers where you came from so you can return.



Use for: Popups, Exit Points

Example: UserSearchPopup.push()



.goInWorkspace() — Opens the location in the side workspace frame.



Use for: Worksheets only

Example: ContactNoteWorksheet.goInWorkspace(anNote)



Step by Step - enabling navigation on a widget

\----------------------------------------------



Step 1 — Open the destination Location's PCF: Press ALT+SHIFT+E on the target screen in the browser, or use CTRL+N in Studio to search for the PCF file by name.

Step 2 — Find the Entry Point: In the PCF Editor, click the location name at the top. Go to the "Entry Points" tab. You'll see the entry point name and the object(s) it needs. Example: ABContactLG(anABContact) — it needs one Contact object passed to it.

Step 3 — Set the widget's action property: On your source widget (the one the user will click), set the action property using the entry point syntax with the chosen method. Example: ABContactLG.go(anABContact).

Step 4 — Deploy: ALT+SHIFT+L to reload PCFs without restarting.



Atomic Widget

\-------------

In simple words:

An atomic widget is the smallest piece of UI — you can't break it down further. Like a single LEGO brick. It does exactly one thing: either shows one piece of data, or performs one action.



Creating an Atomic Widget — Step by Step

Step 1 — Pick the right widget from the Toolbox.

The generic Input widget is the smartest choice — it auto-detects the field's data type and renders the right control:

Field Type		What Input renders

Text / Varchar		Plain text box

Date / DateTime		Calendar date picker

TypeKey (picklist)	Dropdown menu

Boolean (yes/no)	Radio buttons (Yes / No)



🔍 Example: One Input widget on a Contact form. If bound to DateOfBirth → shows a calendar picker. If bound to Gender (TypeKey) → shows a dropdown. Same widget, different display.



Step 2 — Drag it onto the PCF canvas. A light green line shows exactly where it will land. A dark green line shows other valid positions.

Step 3 — Set required properties (\* means required):



id\* — a unique name within this PCF file. Like a variable name. No two widgets can share the same id.

value\* — which data field this widget shows (for Input and Cell widgets). Uses dot notation.



Step 4 — Set the label using a display key (explained in Topic 3).

Step 5 — Set optional properties (explained in Topic 4).

Step 6 — Deploy using ALT+SHIFT+L.



Binding Widgets to Data

\-----------------------

In simple words: Binding means telling a widget "this is the specific data field you show." You do this using dot notation — object name, dot, field name.

Every container (Detail View Panel or List View Panel) declares a root object — the record it works with. For example: anABContact of type ABContact. Every widget inside that container can then access fields on that object.



Display Keys — Labels Without Hardcoding

\----------------------------------------

The problem with hardcoded labels: If you write label = "First Name" directly in the PCF, the whole app is stuck in English. A French user sees "First Name" instead of "Nom".

The solution — Display Keys: A display key is a named reference to text stored in a separate display.properties file. The PCF stores only the key name. The actual text lives in locale-specific files.



How to create a display key in Studio:



Type the key name in the label property field

Click "Create Display Key"

Enter the text value for your locale (e.g. English: "First Name")

Refresh the PCF



Handy shortcuts: CTRL+HOVER over any display key in the Properties Window to preview its text. CTRL+CLICK to jump directly to the display.properties file.



Input Set

\---------

An Input Set is a named group of Input widgets that lives inside a Detail View Panel. It solves two different but related problems: grouping widgets under one shared condition, and reusing a group of fields across multiple pages.

Think of it like a form section with a label and rules. Instead of adding 5 address fields one by one to every form, you create one "Address" group and drop it anywhere you need it.

Where it fits in the hierarchy:

Screen → Card View Panel → Detail View Panel → Input Set → Input Widgets



Two Use Cases for Input Sets

\----------------------------



Use Case 1 — Shared Logic (Inline Input Set)

You want 4 financial fields to appear only when the contact is an ABCompanyVendor. Without an Input Set, you'd set visible = (anABContact typeis ABCompanyVendor) on each of the 4 fields separately. If the condition ever changes, you'd need to update all 4 widgets.

With an Input Set, you wrap all 4 fields inside one InputSet widget and set visible once on the set. All child fields inherit it automatically.



Use Case 2 — Reusable Input Set (PCF File)

Address fields (Street, City, State, Zip, Country) are needed on the Contact Summary page AND the Contact Addresses page. You create GlobalAddressInputSet.pcf once, then both pages reference it using an InputSetRef widget.

If you ever need to add a "Country Code" field to all addresses, you add it once in GlobalAddressInputSet.pcf and it appears on both pages automatically.



InputSet Widget vs InputSetRef Widget

\-------------------------------------

This is where beginners often get confused. There are two different things with similar names:

InputSet widget — an inline container defined directly inside a Detail View Panel. Not a PCF file. Used for grouping widgets under shared logic (visibility/editability conditions).

InputSetRef widget — a reference widget that points to an external InputSet PCF file. Used for reusability. Its def property uses the syntax: GlobalAddressInputSet(anABContact).



Creating a Shared Logic Input Set — Step by Step

\------------------------------------------------

The goal: Make 3 financial fields visible only when the contact is of type ABCompanyVendor.

Step 1 — Add an InputSet widget inside an Input Column inside the Detail View Panel. (Green line shows where it will land.)

Step 2 — Specify the shared logic on the Input Set itself:

visible = (anABContact typeis ABCompanyVendor)

This uses the Gosu typeis operator which checks whether an object is a certain type.

Step 3 — Add atomic widgets inside the Input Set. These child widgets inherit the visibility condition from the Input Set parent.

Step 4 — Deploy with ALT+SHIFT+L.



What happens in the UI: When the Contact record is an ABCompanyVendor, the Input Set (and all its child fields) become visible. When it's any other type, the entire group disappears. You only had to write the condition once.





Creating a Reusable Input Set — Step by Step

\--------------------------------------------

The goal: Create address fields once and use them on two different pages.

Step 1 — Create a new PCF file (type: Input Set). In Project View → right-click PCF folder → New PCF File → select "Input Set" → type name (Studio appends InputSet). Creates e.g. GlobalAddressInputSet.pcf.

Step 2 — Declare the root object on the Required Variables tab:

anABContact : ABContact

This tells the Input Set what object type it works with.

Step 3 — Add Input widgets inside the Input Set. Bind each one: anABContact.PrimaryAddress.AddressLine1, anABContact.PrimaryAddress.City, etc.

Step 4 — Reference it from the parent Detail View Panel by adding an InputSetRef widget:

def = GlobalAddressInputSet(anABContact)

This passes the Contact object to the Input Set so its widgets have data to show.

Step 5 — Deploy with ALT+SHIFT+L.



Partial Page Update — The Performance Benefit

\---------------------------------------------

The problem: On a complex form, a group of fields should appear or disappear based on user input. If Guidewire had to refresh the entire HTML page every time, it would be slow and clunky.

The solution — Partial Page Update: When widgets are grouped inside an Input Set, Guidewire can refresh only that Input Set's HTML section — not the whole page.



