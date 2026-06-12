Insurance Suit Fundamentals

\---------------------------



What You will learn

* Describe how Guidewire products are structured (architecture)
* Know the main tools used to configure Guidewire
* Understand how the Guidewire platform relates to its applications
* Explain what "TrainingApp" is
* Start and stop a Guidewire application
* Understand what Guidewire Studio does



What is Guidewire?



Guidewire is a software platform used by insurance companies around the world. It helps them run their core operations. It has 3 main products called Insurance suit.



1.Policy Center -> Create, change, renew, and cancel insurance policies

2.Claim Center -> Bill customers and pay commissions to agents

3.Billing Center -> Handle claims and pay customers when something bad happens (accident, loss, etc.)





Guidewire Architecture

\-----------------------

guide wire application has 3 layers:



1.Application tier(Brain) : it deals with business logic - the rules that make the app works. It runs on server like WebSphere , WebLogic ... etc

2.Data tier(storage) : it deals with database operations where the data is stored and supports the oracle....

3.Presentation tier(User end) : it deals with user interface - when you see in browser and works on Chrome 28+.



External Systems are also connect to Guidewire 

&#x09;Policy Admin systems, Address Books, Authentication systems, Check Printing, Document Storage



Gosu Language

\--------------



it is Guidewire's own programming Language. it is used to write the business logic that runs guidewire applications inside application tier.





Guidewire Configuration Technology

\----------------------------------



Guidewire uses the 4 types of technology - one for each tier and other for external connections



1. Database - entities - define the data
2. Application server - Gosu - write business logic
3. User Interface - PCF's - Build the interfaces
4. Integration - Plugins, API, Web Services - Connect to outside



How data flows

\--------------



Database Table -> Entity(Data model) -> Gosu class(logic) -> PCF(screen shown to user)



Entities define what data to store(like form)

Gosu classes uses that data to apply rules

Pcfs display the data on screen



When user changes something it goes back through the chain and updates in database



Mainly Integration deals to connect the Guidewire system to outside system



The Guidewire Platform

\-----------------------



one Platform , Many Apps



The Guidewire Platform is common base that all 3 products are built on



the platform provides 4 same technologies to all apps



each app has its own specific features

for example:

* Claim Center: Financials, Fraud Detection, Claim Maturity
* Policy Center: Product Model, Policy Validation, Jobflow
* Billing Center: Billing Plans, Payment Plans, Charge Patterns



Two Types of Configuration are there

1.Fundamental Configuration -> it is a base configuration and same for all apps

2.App Configuration -> it might be change based on client requirement . each app has its own configuration



Training App

\------------

Training App is just a fake/mock application built for only training purposes. It helps you learn all 4 areas of configuration in a simple, clean environment.



It acts like a Contact Management Application(Stores and manages contacts)



Its small and simple - not a real world complexity



It includes examples of all 4 areas : Data model , UI , Gosu , Integration



Training App Data Model

\------------------------



The main entity is ABContact (Address Book Contact) . Related to it : 

* Address, Service Evaluation, History Entry, Flag Entry, Contact Note, Bank Account



Subtype Hierarchy 

\-----------------



ABContact has subtypes -> there are different type of contacts like person , company .it inherits all from ABContact



TrainingApp Application Logic

\-----------------------------

Written in Gosu. Uses:



Widget attributes, static methods, enhancements, business rules, script parameters



Running Guidewire Applications

\-------------------------------



Directory Structure (Slide 32)

All Guidewire apps have the same folder layout:

admin, bin, idea, modules, studio, webapps

Starting the App (Slides 33–34)



Key files: build.xml (defines commands) and gwXX.bat (launches commands)

To start: Open command window in bin folder → type gwXX dev-start

When ready, console shows: \*\*\*\* \[ApplicationName] ready \*\*\*\*



Accessing the App in Browser (Slide 35)

URL format: http://hostName:port/appCode

App - PortApp - Code

TrainingApp - 8880 - ab

BillingCenter - 8580 - bc

ClaimCenter - 8080 - cc

PolicyCenter - 8180 - pc



Guidewire Studio

\----------------

It's an IDE (Integrated Development Environment) — basically a code editor made for Guidewire development.



Supports: Gosu, XML, Java (Java has some limitations)



Features: Refactoring, smart code completion, debugging, version control (Git, Subversion)



Guide DataModel

\----------------



What You Will Learn



* Understand what a Guidewire data model contains
* Find information about an application's data model
* Use dot notation to reference data



What is Data Model?

\--------------------



The data model is the complete set of data objects in guide wire applications and how they relate to each other.



&#x09;		Entity 				 Data Object

What it is	The blueprint/definition	The actual thing created from that blueprint

Where it lives	In the data model (design time)	In the application memory (runtime)

Think of it as	A class				An instance of that class

Example		ABContact entity definition	anABContact — one specific contact in memory

In database	Defines the table structure	One row in that table



It consists of 4 main things:



Entities — the data objects

Entity Fields — the data inside those objects

Typelists — predefined dropdown/list values

Typekeys — fields that link to a typelist



What is an Entity?

\------------------

An entity is an abstract definition of a group of business objects.

Think of it like a template or blueprint for a type of data.

Examples: ABContact (Address Book Contact), User



Entities in the Database



Each entity usually has its own database table

Example: ABContact entity → stored in table ab\_abcontact

Exceptions:

* Subtype entities share a table with their parent
* Virtual/non-persistent entities don't save to the database at all



Supertype \& Subtype Entities

Think of it like parent and child:



Supertype = the parent entity (e.g., ABContact)

Subtype = the child entity that inherits all parent fields (e.g., ABAttorney inherits from ABContact)



In the database:



The parent table stores ALL data — both parent and child records

A special subtype column identifies which type each row is

Fields that don't apply to a specific subtype are stored as null



ABContact (Supertype / Parent)

&#x20;   │

&#x20;   ├── ABPerson    (Subtype 1) — Individual person

&#x20;   ├── ABCompany   (Subtype 2) — A company

&#x20;   └── ABAttorney  (Subtype 3) — An attorney





How It Looks in the Database

There is only ONE table — ab\_abcontact — that stores ALL records (parent + all subtypes).

The Table:

ID	subtype	FirstName	LastName	CompanyName BarLicenseNo	

1	1	John		Smith		NULL		NULL	

2	1	Priya		Kumar		NULL		NULL

3	2	NULL		NULL		Tata Motors	NULL

4	3	NULL		NULL		NULL		LIC-98765

5	2	NULL		NULL		Infosys Ltd	NULL



🔍 Reading the Table Row by Row:

Row 1 — subtype = 1 → ABPerson



FirstName = "John", LastName = "Smith" ✅

CompanyName = NULL ❌ (not a company, so not needed)

BarLicenseNo = NULL ❌ (not an attorney)



Row 2 — subtype = 1 → ABPerson



FirstName = "Priya", LastName = "Kumar" ✅

CompanyName = NULL ❌

BarLicenseNo = NULL ❌



Row 3 — subtype = 2 → ABCompany



CompanyName = "Tata Motors" ✅

FirstName = NULL ❌ (company has no first/last name)

LastName = NULL ❌

BarLicenseNo = NULL ❌



Row 4 — subtype = 3 → ABAttorney



BarLicenseNo = "LIC-98765" ✅

FirstName = NULL ❌

LastName = NULL ❌

CompanyName = NULL ❌



Row 5 — subtype = 2 → ABCompany



CompanyName = "Infosys Ltd" ✅

Everything else = NULL ❌





Why NULL?



Because all subtypes share the same table, but each subtype only uses its own relevant columns. The rest are simply left as NULL — they are irrelevant for that subtype.



Think of it like a government form that has fields for both Individual and Company — if you are a company, you leave the "Father's Name" field blank (NULL). If you are an individual, you leave the "Company Registration Number" blank (NULL).





The Subtype Column — How it Works

The subtype column is the key identifier. It tells the system what type each row is:

subtype value	Means

1		ABPerson

2		ABCompany

3		ABAttorney



This subtype column points to a typelist table that looks like this:

ab\_abcontact\_type (Typelist Table):

ID	Typecode

1	ABPerson

2	ABCompany

3	ABAttorney



So when the system reads subtype = 2 in the main table, it looks up this typelist table and knows → this is an ABCompany record.



Abstract Entity

\---------------



An abstract entity is like a template that can never be used directly — it only exists so that other entities can inherit from it.



If you try to create just an ABContact directly → the system won't allow it

You must always create one of its subtypes — ABPerson, ABCompany, or ABAttorney





ABSTRACT ENTITY

ABContact (you cannot create this directly)

&#x20;   │

&#x20;   │  Common fields shared by all:

&#x20;   │  Name, PublicID, CreateTime, AssignedUser...

&#x20;   │

&#x20;   ├── ABPerson (subtype = 1)

&#x20;   │       Own fields: FirstName, LastName, DateOfBirth, CellPhone

&#x20;   │       DB row: FirstName="John", LastName="Smith", CompanyName=NULL

&#x20;   │

&#x20;   ├── ABCompany (subtype = 2)

&#x20;   │       Own fields: CompanyName, NumEmployees

&#x20;   │       DB row: FirstName=NULL, LastName=NULL, CompanyName="Tata Motors"

&#x20;   │

&#x20;   └── ABAttorney (subtype = 3)

&#x20;           Own fields: BarLicenseNo, Specialization

&#x20;           DB row: FirstName=NULL, LastName=NULL, BarLicenseNo="LIC-98765"



Abstract entity -> A parent that can never be created directly — only inherited

Why abstract -> To define common fields ONCE and share them across all subtypes

