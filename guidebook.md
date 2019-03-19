# ServiceNow apps localization

## Goal

The goal of this lab is to describe the internationalization and localization 
framework of the ServiceNow platform, discuss how this applies to development 
and localization of an app distributed on the ServiceNow platform and also to 
discuss some of the best practises, gotchas, challenges and nuances of 
delivering and deploying global apps.  

During this workshop, we intend to:
Part 1:
1. Introduce and create a simple application that includes some of the 
features that support the localization process on our platform.
1. Have you create a user interface element that demonstrates the usage of 
each one of these features
1. Describe how to suitably externalize each one of these user interface features

Part 2:
1. Export content for delivery to a translation team
1. Verify the returned deliverables from the translation team
1. Import the translated fields back into your application
1. Switch locale to view your application in its translated glory
1. Review your translated application and identify any outstanding issues
1. Troubleshoot our application to identify globlaization issues

Part 3:
1. Correct problems with our sys_choice elements
1. Identify issues and rectify problems with our sys_documentation and
sys_translated(txt) table items
1. Find hard-coded strings in your code
1. Use the appropriate method to externalize these hard-coded strings so 
that they are available for translation

Part 4:
1. Use locale-aware time and date formats in our code that structure these
formats according to the user's preferred display settings
1. Differentiate between price and currency and learn when to use which feature


Getting Started - Log on to Your Training Instance 
====================================================

1. Navigate to the unique instance URL provided to you. 

1. Log on with the provided credentials. 

1. Retrieve our source application


Change your locale
===================

