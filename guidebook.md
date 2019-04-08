# ServiceNow apps localization

## Goal

The goal of this lab is to describe the internationalization and localization
framework of the ServiceNow platform, demonstrate  how this applies to development
and localization of an app distributed on the ServiceNow platform and also to
discuss some of the best practices, gotchas, challenges and nuances of
delivering and deploying global apps.  

### This Workshop is divided into the following sections:
#### Section 1: Introduction to ServiceNow i18n features
1. Log into your instance and verify that you have a language plugin installed
1. Create a sample application in Studio based on a doctor's waiting room analogy
1. Populate the waiting room application with field types representative of ServiceNow internationalization features
1. Show i18n plugin structure and how to change locale

#### Section 2: Export and translate the UI features
1. Locate your app's UI features in the localization tables
1. Export and 'translate' the user-interface features
1. Import and transform the translated fields back into your application
1. Switch locale and create a new record

#### Section 3: Translate a UI Page
1. Create a simple new UI page
1. Change locale and view the new UI page
1. Wrap the user-facing messages in appropriate calls
1. Create the corresponding sys_ui_message entry
1. Create a translated version of the key
1. Switch locale and view the UI page

#### Section 4: Translate a Portal Widget
1. Navigate to Widget Editor and view the Hello World 1 widget
1. Switch to another locale and review the widget outputs
1. Clone Hello World 1 and review the hard-coded features
1. Update HTML Template to externalize the string and include a hard-coded string
1. Update the Client Script
1. Update the JSON demo Data
1. Quiz
1. Create the necessary keys
1. Discuss some Best Practices

#### Section 5: TranslationLoader Script Include
1. Navigate to the script include
2. Show the TraslationLoader Script
3. Run the script in the background
4. Show the generated outputs in the tables
5. Show how to export the generated outputs and re-import



#### Section 1: Introduction to ServiceNow i18n features
1. Navigate to the unique instance URL provided to you.

1. Log into your instance and verify that you have a language plugin installed
 ![](./images/systemsettingschooselocale.png)

1. If you have a language plugin installed, you will already have the Internationalization plugin enabled (com.glide.i18n). For the purposes of the workshop, we will use the I18N: Internationalization Translation helper, so lets check if it is enabled.
1.   Go to Plugins in the Filter navigator, and search for plugins.  In the Plugin search window, search for 'i18n'.
1. Make sure you have your chosen language (in our case French), as well as the i18n plugins referenced here:
![](./images/i18nPlugins.png)

1. Now that we know we have the relevant plugins, we're going to create our App in Studio.

1. From Studio, [Create Custom Application]
![](./images/CreateCustomApplication.png)

1. Add a single "Waiting Room" table.
![](./images/AddWaitingRoomTable.png)

1. Click on the "Waiting Room" table to edit it
![](./images/EditWaitingRoomTable.png)

1. Add the fields as listed in the screenshot below, selecting the appropriate field type.
![](./images/AddFieldsWaitingRoomTable.png)

1. Edit the [Appointment] field, click on the [Choices] tab, and add the two values as show in the screenshot.
![](./images/AddWaitingTableChoices.png)

1. Refresh the app and find the 'Waiting Room' App menu and select it.
![](./images/SelectWaitingRoomApp.png)

1. Click "New" to create a new entry       
![](./images/CreateNewEntry.png)

1. Now let's swap language and see how our app looks.  
 ![](./images/systemsettingschooselocale.png)

1. As you can see, 'system-defined' or OOTB text is translated, but none of the fields in our new app are translated.
![](./images/SystemDefaultTranslations.png)

#### Section 2: Export and translate the UI features
1. From the Navigator, type [System Localization] and from the sub-menu, select [Field Labels]
![](./images/ExportFieldLabels.png)

1. Filter on the new [Waiting Room] table.
![](./images/FilterWaitingRoomTable.png)

1. Export the filtered list.  Right-Click above the table, select [Export] and [Excel (.xls)].  Select a location and save the document.
![](./images/ExportFilteredList.png)

1. We're going to perform a quick 'fake' translation of the exported values, so open the spreadsheet in your favorite editing application, update the language field so that it matches the two letter ServiceNow language code.  Edit the relevant fields (in the case of Field Labels, you need to edit the: Language, Label, Plural, Help and Hint columns as appropriate.  Remember to save the document!
![](./images/FakeTranslate.png)  
1. Some quick options to 'fake translate' your values using Excel formulas:

    Option 1: Quick and dirty: 'This will pre-pend a value like `it -` before each of your values
     `=concat("it -", D2)`<p>
     Option 2: A little more complex vowel substitution, convenient for visual testing.  Yields results like: `Knówlédgé Vérsíóns` `=SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(SUBSTITUTE(A2,"a","á"),"e","é"),"i","í"),"o","ó"),"u","ú"),"A","Á"),"E","É"),"I","Í"),"O","Ó"),"U","Ú")`
   Option 3: Just edit each string and make a few simple changes!
If using an Excel formula, remember to paste the resulting value 'as- a-value'.

1. Now we're going to import our new translations back into our Instance.  From the [System Import Sets] menu, select the [Load Data] sub-menu.  Make sure you select the correct [Import Set Table].
![](./images/ImportSet.png)

1. Once our import has completed successfully, we need to perform a transformation.  Select [Run Transform] from the resulting 'Success' window.
![](./images/RunTransform.png)

1. Make sure you select the correct Import Set if you are working with multiple import sets and select [Transform].
![](./images/PerformTransform.png)

1. After completing the import and transformation, view the app and create another record.  Fields are now translated.
![](./images/PostTranslationNewRecord.png)

1. Repeat the process for other field types, specifically:
<p>choices
<p>translated text
<p>Translated Name / Fields


#### Section 3: Translate a UI Page
The process for handling dynamic/messages within code is a little different, so this next section is going to focus specifically on this area.  To do this we're going to create a simple UI page.

1. Create a simple new UI page.  Return to your [Waiting Room] app in [Studio] and from the [Create Application File], select [Forms and UI] and [UI Page] and click on [Create].
![](./images/CreateUIPage.png)

1. Insert some simple HTML with plain text messages.  If you like, just copy and paste the following text in between the opening/closing HTML tags:<p>
`<body bgcolor="${background}" text="#FFFFFF">`<p>
`<h1>Welcome to the Las Vegas Waiting room.</h1>`<p>
`<h2>Please sign-in and take a seat</h2>`<p>
`</body>`<p>
Your HTML section should look something like this:
![](./images/WelcomeLasVegas.png)

1. Change locale and click on [Try It] or click on the Endpoint Links
![](./images/UIPageTryIt.png)
1. Not unexpectedly, nothing is translated.  We need to wrap the translatable user-facing text in `getMessage` calls and we also need to create and translate the associated key/values in the sys_ui_message table.
1. Firstly, update the text in the HTML Panel of your UI Page, to wrap the text in 'getMessage' wrappers, or just copy the following and replace the existing code in the HTML panel:<p>
`<h1>${gs.getMessage("Welcome to the Las Vegas Waiting room")}</h1>`<p>
`<h2>${gs.getMessage("Please sign-in and take a seat")}</h2>`<p>

1. Your HTML section should now look like this:
![](./images/UIPagwWrappedingetMessage.png)

1. Now we need to create the English key and value in the sys_ui_message table.

** Best Practice:  Create the English (source language) key and value pair in sys_ui_message, to support easier exporting, translation and maintenance of mutli-lingual apps and solutions **

8. Return to your Instance, and navigate to [System Localization] and select the [Messages] sub-menu.  Click on [New] to create a new key/value pair.  Insert the text you wrapped in the `getMessage` call into the key field and the Message field.  
![](./images/UIPageCreateenKey.png)

1. Repeat this step and create a key/message pair for the second string in your UI Page, specifically: `Please sign-in and take a seat`.<p>
** Best Practice: Because the key/value pair is 'keyed' on the source string, if you change the text in your source code, you must make the corresponding change the key, otherwise you will have disassociated the text in the `getMessage` call from the key/value pair.**

1. Now we can either follow the process outlined in **[Section 2: Export and translate the UI features]**, but in the interest of expediency, we're going to simply create the corresponding key/value pairs for our target language directly in the sys_ui_message table.

1. Create a new key/value pair for our UI strings once again, however, this time select your target language from the [Language] drop-down and instead of inserting the same English message, insert something that will be immediately recognizable, for instance:
`Wélcómé tó thé Lás Végás Wáítíng róóm`.  Remember, the key should remain exactly as it appears in the `getMessage` call.

1. Your translated record should look like the following:
![](./images/sys_ui_translatedWelcome.png)

1. Repeat the process again for your second UI message, again, keeping the key the same, selecting your preferred language, and updating the value, with something like: `Pléásé sígn-ín ánd táké á séát`

1. Return to your UI Page in the Studio.  In the main ServiceNow Instance Window change your locale, and then either hit the [Try It] button in Studio, or click on the [Endpoint] URL.  Hopefully, you're now presented with a 'fake translated' UI Page!
![](./images/UIPageWelcomePseudo.png)

** Best Practices: ** <p>
 If your UI page is not showing up translated, check the following items:
* Make sure that the string wrapped in the `getMessage` call is the same as the string used in the key in the sys_ui_message table.
* If you are exporting your app entries from sys_ui_message to translate them, make sure you have created English keys for all of your User-Interface strings, otherwise they will not be part of the export.  
* Also make sure that all user-facing strings are wrapped in the appropriate `getMessage`/externalization calls.


#### Section 4: Translate a Portal Widget
Following on from the translation of the custom UI page, let's take a look at the changes involved in translating a Service Portal Widget.  
1. Go to [Service Portal Configuration], [Widget Editor], Select the [Hello World 1] widget.
1. Preview the widget – Switch your locale to French ([Profile] [Language] [Refresh])
![](./images/PortalWidgetUntranslated.png)

1. As you can see, the Widget name has been translated (from the 'out-of-the-box' language plugin) all of our widget text remains in English.  

1. Let's switch back to English.  How many hard-coded strings can you count?
![](./images/WidgetHardCoded.png)

1. Technically, the answer is 4. Strictly speaking the demo data within the JSON is not hard-coded, but if it is used as a default value within client facing user content, then it too fits the bill.
![](./images/WidgetHardCodedHighlighted.png)
** Best Practice:  If you want your user interface elements to switch dynamically based on a user's preferred locale, don't store values in a non-internationalized structure like demo-data, unless you want to manually manage the dynamic switching **

1.  Now that we know what the problems are, let's fix them.  OOTB widgets are read-only, and we're going to make some changes, so let's clone the widget and call the new widget a very original 'Hello World 4'.
![](./images/WidgetClone.png)

1. Firstly, let's externalize the text in HTML Template, by wrapping it in ${some text for translation}.
1. While we're art it, let's add another string, which we'll keep hard-coded, for comparison.  So your HTML Template should look like the following: <p>
`<div>`<p>
`  ${Please enter your name:}`<p>
`  <p>This is hard-coded text</p>` <p>
`...`<p>
`</div>`<p>
![](./images/WidgetHTMLTemplate.png)

1. Next we'll fix up the Client Script.  Add a new variable and assign it with the formerly hard-coded text, andf while we're at it, wrap it in an externalization call, like the following: `	c.welcomemessage = "${Hello to: }"` and substitute the previous body of text with our new variable:<p>
 `c.data.message = (c.data.sometext) ? c.welcomemessage	+ c.data.sometext + '!' : '';`
![](./images/WidgetClientScript.png)

1. Finally, just for good measure, we'll update the JSON demo data.  Change it to `		"sometext": "le monde"`.  It's still hard-coded (won't change dynamically based on user locale), but for the purposes of our workshop, at least it's not in English.

1. Let's click on [Preview] (the eye icon).  
** Quiz: ** Why has nothing changed?
 * Well, we haven't changed our locale to something other than English.
 * We also have not yet created the required keys in sys_ui_message.
 * We also haven't provided any translations.  So let's do that quickly now.

 1. Navigate to the Message table [sys_ui_message] and create the English and translated keys:
![](./images/WidgetTranslateKeys.png)

** Best Practice: ** Do we really need to create the English keys?  Aren't they available in the code?
 Better to create the English keys, so that if you later add a further language and perform an export you will be exporting all of the key/strings you need.

1. Return to [Widget Editor].  Switch your language, and open [Preview].  Fix bugs if necessary.  Happy days!
![](./images/WidgetHelloWorld.png)

** Best Practices: **
 * Client Script uses a concatenated string:   <Hello to: > + <monde/input text> + <!>.  Simple example, but concatenated strings may not be translatable in all locales (language syntax).
 * Messages with quotes or double-quotes may cause javascript errors in client script, so best to assign value in server script then assign using angular binding.

1. Based on that last Best Practice, let's take one last look at Portal Widget Localization, this time using server-side script to retrieve our localized value then assign it to the client side in an Angular binding.

1. Here we've cloned the Hello World -3 widget (which contains server-side script), and made the same changes internationalizaiton changes.  This time we've assigned the string to a variable in the server-script variable `data.welcomestring`.  Note the apostrophe in the source string.  We created the required keys in English and French, and you can see it works as expected.  
 ![](./images/WidgetHelloWorldServerSide.png)


#### Section 5: TranslationLoader Script Include
TranslationLoader is a script include that expedites the process of gathering translatable content from some (not all!) of the message tables, and streamlines the process of extracting and re-importing this content.  
** Dependancy: The TranslationLoader script include has a dependency on the `I18N: Internationalization Translation helper plugin` so please make sure you have this installed before trying to run the script.

** Best Practice: **
The TranslationLoader script takes approximately 3-4 hours to run on a clean z-booted instance, so don't attempt this unless you have time on your hands.  

For the purposes of demonstrating the script, we have edited it, so that it only consolidates and exports records that were created 'today'.

1. Navigate to the GIT Repository: [ServiceNowEvents GIT Repository](https://github.com/servicenowevents) and grab the updated TranslationLoader script.

1. Navigate to the [System UI] [Script Includes], find the TranslationLoader script and either update it with our edited script or create a new Script include.
![](./images/TranslationLoader.png)

1. Run the amended script in Global scope:
![](./images/TranslationLoaderExecuteScript.png)

1. Script will eventually return the consolidation/extraction results:
![](./images/TranslationLoaderExecuteScriptResult.png)

1. Navigate to each of the tables:
 * Translated Name / Field Translations
 * Message Translations
 * Field Labels Translations
 * Choice Translations
 * Translated Text Translations
Filter the required fields and export.
![](./images/TranslationLoaderExecuteScriptExport.png)

Note: the exported spreadsheet format differs from the standard table export, with column corresponding to the source, for each of the  target languages.
![](./images/TranslationLoaderExported.png)

6. Translate the values corresponding to your desired target locales and re-import.

![](./images/UnfinishedBuisiness.png)

1. At this point, we're coming to the conclusion of our workshop and we'd like to circle back and discuss some lessons learned from our process  of performing an internationalization review of the DevOps content.  We hope you manage to stick with us for the duration of the workshop and we would welcome your thoughts and feedback, but also your suggestions.
