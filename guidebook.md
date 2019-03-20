# ServiceNow apps localization

## Goal

The goal of this lab is to describe the internationalization and localization 
framework of the ServiceNow platform, discuss how this applies to development 
and localization of an app distributed on the ServiceNow platform and also to 
discuss some of the best practises, gotchas, challenges and nuances of 
delivering and deploying global apps.  

```
Demonstration instance:
=======================
Need to validate the following:
1. Availability of instance with i18n plugins pre-installed
1. Language plugin installed (got to pick a language)
1. Attendees will have access to sample app with some sample code pre-included
```
  


### Agenda
#### Section 1:
==========
1. Introduce and create a simple application that includes some of the 
features that support the localization process on our platform.
1. Have you create a user interface element that demonstrates the usage of 
each one of these features
1. Describe how to suitably externalize each one of these user interface features

#### Section 2:
==========
1. Export content for delivery to a translation team
1. Verify the returned deliverables from the translation team
1. Import the translated fields back into your application
1. Switch locale to view your application in its translated glory
1. Review your translated application and identify any outstanding issues
1. Troubleshoot our application to identify globalization issues

#### Section 3:
==========
1. Correct problems with our sys_choice elements
1. Identify issues and rectify problems with our sys_documentation and
sys_translated(txt) table items
1. Find hard-coded strings in your code
1. Use the appropriate method to externalize these hard-coded strings so 
that they are available for translation

#### Section 4:
==========
1. Use locale-aware time and date formats in our code that structure these
formats according to the user's preferred display settings

1. Differentiate between price and currency and learn when to use which feature

#### Section 5: (optional)
=====================
1. Translation of Knowledge articles `(can only talk about Madrid functionality)`

1. Demonstrate the usage of the TranslationLoader script


#### Getting Started - Log on to Your Training Instance 
====================================================

1. Navigate to the unique instance URL provided to you. 

1. Log on with the provided credentials. 

1. Retrieve our source application


#### Change your locale
===================

1. Check that you have the necessary i18n plugins installed

1. Explore the options available for changing your user locale

1. Change your locale and navigate to our custom app screens

1. Observe the difference between the platform translations and those 
originating from our app


#### Create new user interface elements
==================================
1. Create a new language record `(may be out of scope)`

1. Create a new choice record

1. Create a new translated_field item

1. Create a new translated_text field

1. Create a new translated_HTML field

1. Create a new client script message


#### Export your content for translation
=============================================
1. Export the contents of the translation tables

1. Understand the requirements of the export types (Plurals, Hints, etc.)


#### Import your translated content
=============================================
1. Retrieve the set of translations we have created for you

1. Import the set of retrieved translations
https://docs.servicenow.com/bundle/london-platform-administration/page/administer/localization/task/t_ImportATranslationFromExcel.html

1. Review the import information

1. Run the transforms

1. Switch user locale and view your now-translated app

1. Identify problems with the now-translated application
```
(UI elements that do not use a translated field type)
(Hard-coded strings introduced in user script)
(User script elements that do not have an associated record in sys_ui_message)
```

#### Fix globalization issues
=========================

1. Switch to using a translated field type

1. Wrap user script element in getMessage tag

1. Create table entry for item in sys_ui_message

1. Change user script to display current time in user locale
```
(need a preexisting field that outputs the current time using setValue() )
Update the field to display the current time using setdisplayValue()
```

1. Demonstrate difference between price and currency
```
(use pre-existing code and highlight the difference between:
price: as the cost of an item
Currency: transactional spend
Reference currency
Session currency
```

#### Bonus topics:
=============
1. Export a knowledge article

1. Import a knowledge article

1. Talk about the TranslationLoader script 
`(won't have time to actively use)`
