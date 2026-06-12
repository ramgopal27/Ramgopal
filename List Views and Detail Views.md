List Views and Detail Views

\---------------------------



Detail View Panel

\-----------------



A Detail View Panel is a form that shows everything about one record. When you open a Contact and see their name, phone number, address, status — that entire form is a Detail View Panel. It focuses on one object at a time.



A Detail View Panel allows the user to view, and sometimes create or edit the record — depending on how it is configured.



Root Object — The Record the Panel is About

\-------------------------------------------

Every Detail View Panel has a root object — the one data record the panel works with. It's declared on the "Required Variables" tab in Studio. Without a root object, the panel has no data to show.



Example: anABContact : ABContact — this says "I work with a Contact record, and I'll call it anABContact inside this PCF."



Every Input widget inside the panel accesses fields on this root object using dot notation: anABContact.FirstName, anABContact.DateOfBirth, etc.

Important: If a parent container references this Detail View Panel, it must pass the root object to it. The Panel cannot load data on its own — it always needs to be handed a record.



Structure of a Detail View Panel

\--------------------------------

A Detail View Panel needs at minimum:



At least one Input Column — the vertical column that organises fields side by side. Two columns = left side and right side layout.

Input widgets inside the columns — one widget per field.



It can optionally also contain:



Input Sets — reusable groups of fields (like an Address section)

Embedded List View Panels — a table of related records inside the form

Layout widgets — dividers, section headers, labels to make the form readable



Reusable vs Inline — When to Use Which

\--------------------------------------

Reusable PCF file — create the Detail View Panel as its own PCF file (name ends in DV). Multiple parent containers can reference it. Change it once → all references update.

Inline widget — define the Detail View Panel directly inside a Screen or Card View Panel. Cannot be referenced by anything else. Use only if this panel appears in one place only.



Creating a Detail View Panel — Step by Step

\-------------------------------------------

Step 1 — Create the PCF file:

In Studio Project View → right-click PCF folder → New → PCF File → enter name → select "Detail View" → Studio auto-appends DV. Creates e.g. ABContactSummaryDV.pcf.

Step 2 — Declare the root object:

On the Required Variables tab, define: anABContact : ABContact. This is the record the panel will display.

Step 3 — Set editable and visible (optional):

editable — if true, child widgets can be edited. If blank, inherits from parent.

visible — if false, entire panel is hidden. If blank, inherits (defaults to true).

Step 4 — Add Input Columns:

Drag at least one Input Column onto the canvas. Two columns gives a clean side-by-side layout.

Step 5 — Add Input widgets inside columns:

Drag Input widgets from the Toolbox. Set id, value (dot notation field), and label (display key) on each.

Step 6 — Deploy:

ALT+SHIFT+L to reload PCFs without restarting.



Referencing a Detail View Panel using PanelRef

\------------------------------------------------

Once you have a Detail View Panel as a PCF file, a parent container (Screen, Card View Panel) embeds it using a PanelRef widget. PanelRef is like a placeholder that says "put this panel here."

The key property is def — it specifies which panel to load and what object to pass:

def = ABContactSummaryDV(anABContact)

ABContactSummaryDV = the panel PCF file name

anABContact = the contact record to pass as the root object

PanelRef can also optionally wrap the panel with a Title bar and Toolbar — things the Detail View Panel file itself doesn't define.

Steps to reference:



Add a PanelRef widget to the parent container

Set the def property: ABContactSummaryDV(anABContact)

Deploy with ALT+SHIFT+L



Default behaviour reminder: A freshly created Detail View Panel is read-only. Users can see but not change data. The next PPT (CONF080) explains how to add Edit buttons.



Editable Detail Views

\---------------------

Read-Only vs Edit Mode

In simple words:

By default, a Detail View Panel shows data but doesn't let you change it — it's like a printed form. To let users edit, the page needs to switch to edit mode. Guidewire uses a built-in Edit Buttons widget for this — it provides three buttons: Edit, Update, and Cancel.



A Detail View Panel is like a glass display case in a museum. In read-only mode, you can look but not touch. Clicking Edit is like getting the key — now you can open the case. Update locks it back with your changes. Cancel locks it back as it was.



The Editability Hierarchy — Why Everything Must Be Editable

\-----------------------------------------------------------

Editability flows from top to bottom. For a single Input widget to be editable on screen, every container above it in the hierarchy must also be editable. If even one level says "not editable," everything below it is locked.



Imagine a set of nested boxes — a big box (Screen) containing a medium box (Card View Panel) containing a small box (Detail View Panel) containing the item (Input widget). To reach the item, you must be able to open every box. If the medium box is locked, it doesn't matter if the small box and the item are unlocked — you still can't get to them.



List View Panels

\----------------



A List View Panel is a table that shows many records at once. Where a Detail View Panel goes deep on one record, a List View Panel goes wide across many records — showing a few key columns for each.



Real example: The list of all claims on the ClaimCenter home page — showing Claim Number, Status, Date, and Policy Number in columns. Each row is one claim. That table is a List View Panel called ABContactHistoryLV.



A **RowIterator** is like a photocopier template. You design one row template (with cells). The RowIterator takes your array of objects and makes one copy of that template for each object. 10 history entries → 10 rows automatically.



Creating a List View Panel — Step by Step

\-----------------------------------------

Step 1 — Create PCF file:

New → PCF File → select "List View" → Studio appends LV → creates e.g. ABContactHistoryLV.pcf.

Step 2 — Declare root object:

anABContact : ABContact

Step 3 — Set editable/visible as needed.

Step 4 — Add RowIterator:

Set value = anABContact.HistoryEntries, elementName = currentObj, editable = false.

Step 5 — Add Row inside the RowIterator. Usually no properties needed.

Step 6 — Add Cell widgets inside the Row. For each column set id, label (display key for column header), value = currentObj.FieldName. Use specialized cells like TypeKeyCell for typekey fields.

Step 7 — Deploy with ALT+SHIFT+L.



In simple words:

An **editable List View Panel** lets users do three things directly in the table:



Modify existing rows — click a cell and type a new value

Add new rows — click an Add button to create a new record in the table

Remove rows — select rows with checkboxes and click Remove



The **IteratorButtons** widget gives you Add and Remove buttons for a list. When placed in a Toolbar associated with the List View Panel, it lets users add new rows and delete existing ones using checkboxes.



This is stricter than Detail Views. For a **single Cell to be editable on screen**, all **four levels in the chain must be editable**:





