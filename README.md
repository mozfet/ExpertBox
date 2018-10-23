# ExpertBox
[ExpertBox.com](https://ExpertBox.com) is used by experts from different knowledge domains to create constructs of systematic conversations that converse unattended with consumers, for free or for a small fee. In other words everyone can build their own chatbot for free, and then charge people to talk to it.

Constructs are automated conversations and business process automation flows, created on [ExpertBox.com](https://ExpertBox.com) using the construct designer. Users can have paid or free or subscribe to sessions with constructs and access these session via the Construct Viewer.

## Video Examples
[Construct Example 1 - Pages and Controls](https://youtu.be/avgnqf6HHfo)

## Sessions
Once a construct is published by its handler, users can start creating sessions with that construct. Constructs can be published privately, publicly and within organisations.

To start a session with a construct the user must locate the construct on their stage, where it will appear if the construct was published to this user or their organisation or to the public. If the handler charges a fee for using the construct, the user will require a sufficient amount of credits to start the session.

In order to ensure that users always are using fresh knowledge, sessions expire automatically after a period of time determined by the handler. the expiration period is determined by the handler based on the nature of the knowledge, for example, day trading advice will expire before agricultural planning advice because day trading markets change fast and agricultural seasons changes slowly.

Once a session was started, the construct viewer will be shown for the construct in question. While a session has not expired, it can be found using the Sessions menu. Once expired, only reports of a session will be available, until archived or deleted by the user.

## Construct Viewer
The construct viewer looks a lot like a software installation wizard, and was inspired by that design. It has 3 main areas, the current page of the session on the left for presentation of information and collection of information from the user, the timeline on the right to show what pages have been completed and which pages are expected next (this changes all the time due to new information provided by the user), and the navigation at the bottom of the page for the user to go forward and backward between pages.

For constructs following the mini-app pattern instead of a dialogue patter, it may be desired to remove the the timeline; to do this, on the main menu click on the (construct) design menu item, then find and click on edit action of the construct card. To disable the timeline ensure the switch is set (to the left) to "Do not show the timeline". Submit, and from now on there will be no more timeline displayed for new sessions of this construct.

## Construct Designer
The construct designer includes an action button that, when touched or hovered over presents an icon menu (with tooltips) relevant to the current component that is selected in the model. The model is a serie component, the top level component of the construct, and the entry point to all other components in the construct.

### Action Button
The action button shows the actions available to the currently selected component. Not all actions apply to all components, here is the complete list of available component actions:

| Action          | Components                          |
|-----------------|-------------------------------------|
| Add Serie       | Serie, Page, Expression, Leaf, Loop |
| Add Loop        | Serie, Leaf, Loop                   |
| Add Branch      | Serie, Leaf, Loop                   |
| Add Leaf        | Branch                              |
| Add Expression  | Serie, Leaf, Loop                   |
| Add Page        | Serie, Leaf, Loop                   |
| Add Text        | Page                                |
| Add Question    | Page                                |
| Add Choice      | Page                                |
| Add Dices       | Page                                |
| Add CRUD        | Page                                |
| Update          | All                                 |
| Delete          | All                                 |
| Anchor          | Serie, Page                         |
| Link            | Page                                |

### Select Component
When hovering over a component, you will notice its shadow darken. To select that component, touch or click on it and the background colour of the component will darken. Once a component is selected, the the magic action button will show only actions relevant to the selected component.

### Move Component
To move a component to another component, click or touch and drag the component, while dragging the component it will fit itself into other components until dropped, after which it will stay at the last position.

### Anchor and Link
To link two components in order to make one component aware of the value of another component, select the component with the value that is being linked to, and use the purple anchor action button to create an anchor.

Once an anchor is created a notification will appear to show the currently anchored component. The anchor can be cancelled by swiping the notification to the right, which will remove the anchor notification. An anchor notification does not go away by itself, it will stay on the screen until either dismissed or until a component links to it.

After anchor is created and a different linkable component is selected, a link can be created to the anchor using the purple link action button. The anchor notification will go away, and a quick confirmation notification will be shown to confirm successful linking.

## Components
Each component has a specific logical function to perform, many of these functions are inspired by software development and adapted specifically for use in knowledge automation, business process automation and automated conversations.

### Values
Each component has a value at any given time. How the value for each component is calculated is different.

| Component         | Value                                                           |
|-------------------|-----------------------------------------------------------------|
| Serie, Page, Leaf | an object consisting of the values of contained components      |
| Branch            | the value of the active leaf of the branch                      |
| Loop              | an array of iteration objects each contain contained components |
| Controls          | the value of the information shared or collected with/from user |

### Containers
Container component can contain other components and thus can be deeply nested to facilitate deep and complex conversation flows.

| Container | Content                               | Function |
|-----------|---------------------------------------|----------|
| Serie     | Serie, Page, Loop, Branch, Expression | Sequence |
| Page      | Text, Question, Choice                | Node     |
| Branch    | Leaf                                  | Fork     |
| Leaf      | Serie, Page, Loop, Branch, Expression | Fork     |
| Loop      | Serie, Page, Loop, Branch, Expression | Repeat   |

### Elements
Elements are components that cannot contain other components. In general elements represent data elements than can be assigned a literal value, such as ```1``` or ```hello```, or an object such as ```{color: 'GREEN', width: 16}```.

#### Expressions
Expression components contain [JavaScript expressions](https://developer.mozilla.org/en/docs/Web/JavaScript/Guide/Expressions_and_Operators) that enable simple data evaluation, calculation and assignment, but also complex logic, algorithms, formulas and access to other data sources on the internet.

Expressions have special functions depending on which components they are added to, however all expressions can be linked to regardless of their special functions.

Expressions, when evaluated, that have a value that is a defined value or true is consider to be true. Expressions, when evaluated, that have a value that is undefined or empty or false is considered to be false.

In order to make constructs more manageable and easier to understand in the Construct Designer, it is advised to keep each expression as short and compact as possible and rather to break up large expressions into multiple linked expressions.

| Components | Expression Special Function                                 |
|------------|-------------------------------------------------------------|
| Serie      | n/a                                                         |
| Leaf       | first leaf of a branch with true expression is branch value |
| Loop       | stop loop iteration if expression is true                   |

Expressions have direct access to the value of linked values as same named local variables in script, and have access to a set of utility libraries to simplify and speed up the writing of scrips.

Simple Example - always returns true:
```
  return true;
```

Typical Example - return ture if a linked component named ```colour``` has the value ```GREEN```
```
  return colour === 'GREEN';
```

Advanced example - use the underscore library to group a linked list of colours by type
```
  var isPrimary = function(colour) {
    switch(colour) {
      case: 'RED';
      case: 'YELLOW';
      case: 'BLUE';
        return true;
      default: ;
        return false;
    }
  };
  var colourType = function(colour) {
    if(isPrimary(colour)) {
      return 'PRIMARY';
    }
    else {
      return 'SECONDARY';
    }
  };
  return api._.groupBy(colourList, function(colour){return colourType(colour)});
```

Execution options:
An Expression component can be processed to determine the next page, but also in
other contexts, such as predicting the contents of the timeline.

```
data.options.isFuture
```
Used to prevent processing while predicting the content of the timeline. E.g.
from the AI Example, to avoid racking up high charges on Algorithmia, while
still enabling future prediction to continue.
```
if (data.options.isFuture) {
  return ['fake'];
}
else {
  var result = api.algorithmia.htmlToStringList(targetUrl);
  return api._.filter(result,  function (str) {
    return !api._.isEmpty(str);
  })
}
```

##### Persistence API

tbd

##### Math API

tbd

##### Text API

###### Case
This part of the API is an facade (wrapping) for the wonderful set of text case conversion tools of [to-case](https://github.com/ianstormtaylor/to-case).

Case Conversion Example:
```
api.text.case.to.camel('what_the_heck')      // "whatTheHeck"
api.text.case.to.capital('what the heck')    // "What The Heck"
api.text.case.to.constant('whatTheHeck')     // "WHAT_THE_HECK"
api.text.case.to.dot('whatTheHeck')          // "what.the.heck"
api.text.case.to.inverse('whaT tHe HeCK')    // "WHAt ThE HeCK"
api.text.case.to.lower('whatTheHeck')        // "what the heck"
api.text.case.to.pascal('what.the.heck')     // "WhatTheHeck"
api.text.case.to.sentence('WHAT THE HECK.')  // "What the heck."
api.text.case.to.slug('whatTheHeck')         // "what-the-heck"
api.text.case.to.snake('whatTheHeck')        // "what_the_heck"
api.text.case.to.space('what.the.heck')      // "what the heck"
api.text.case.to.title('what the heck')      // "What the Heck"
api.text.case.to.upper('whatTheHeck')        // "WHAT THE HECK"
```

Case detection:
```
api.text.case.to('thisIsAString')      // "camel"
api.text.case.to('This Is A String')   // "capital"
api.text.case.to('THIS_IS_A_STRING')   // "constant"
api.text.case.to('this.is.a.string')   // "dot"
api.text.case.to('this is a string.')  // "lower"
api.text.case.to('ThisIsAString')      // "pascal"
api.text.case.to('This is a string.')  // "sentence"
api.text.case.to('this-is-a-string')   // "slug"
api.text.case.to('this_is_a_string')   // "snake"
api.text.case.to('this is a string')   // "space"
api.text.case.to('This Is a String')   // "title"
api.text.case.to('THIS IS A STRING')   // "upper"
```

###### Padding

Pads text to the left or right of other text up to a limit.

Right Pad Example:
```
api.text.pad.right("input", 5, "padblock")
```

Result: ```inputpadbl```

Left Pad Example:
```
api.text.pad.left("input", 7, " _")
```

Result: ``` _ _ _ input```

###### Convert To
Utility functions that convert text to literals, objects and arrays.

####### Number
Convert text to a number for use in calculations.

Example
```
api.text.to.number("5");
```

Result: ```5```

####### Paragraphs
Convert text to an array of texts where each index in the array contains a paragraph, in order, without leading or trailing whitespace, from the input text.

Example:
```
api.text.to.paragraphs("  This is paragraph 1.  \n  This is paragraph2.");
```

Result: ```["This is paragraph 1.", "This is paragraph 2."]```

###### Convert From
api.text.from
```
```

###### Currency
api.text.currency
```
```

##### Underscore JS
[UnderscoreJS](https://underscorejs.org) is an JavaScript utility library that simplifies working with collections of data and objects; the Swiss army knife of JavaScript.

Example where stoogeNames equals ```["moe", "larry", "curly"]```
```
var stooges = [{name: 'moe', age: 40}, {name: 'larry', age: 50}, {name: 'curly', age: 60}];
var stoogeNames = api._.pluck(stooges, 'name');
```

##### Algorithmia.com
Algorithmia.com provides a wide range of Artificial Intelligence algorithms via our easy to use API.

You can use your own Algorithmia.com account. All content in expression scripts are safely stored encrypted in our secure private cloud database server and never leave our servers except to the owner of the construct, so your key is safe.
```javascript
const result = api.algorithmia.algorithm(key, algorithm, input);
```

Or use a selection of built algorithms without needing an Algorithmia.com account.
```javascript
const arrayOfStrings = api.alorithmia.htmlToStringList(urlString);
const twoLetterLanguageCode = api.alorithmia.languageIdentification(text);
const yandexTranslation = api.alorithmia.yandexTranslate(inputLanguage, outputLanguage, text);
const googleTranslationEnglish = api.alorithmia.googleTranslate(text);
const summary = api.algorithmia.summarize(text);
const tagArray = api.algorithmia.autoTag(text);
const entityArray = namedEntityRecognition(text);
const
```

##### Textgears
[Textgears](textgears.com) provide and English grammar correcting service, it is intended to be used as human guided tool for use in text editors.

We wrapped the [Textgears NPM Library](https://www.npmjs.com/package/textgears) to instantly apply the best changes to a piece of text. Our wrap will synchronously fix English text according to the best guess of the Artificial Intelligence, whether that is desired is up to you...

```javascript
const fixedText = api.textgears.fix('I is an engeneer.. I wihh i was here');
```

Results in: ```I am an engineer.. I with I was here.```

Note that the best guess is not always the right guess. In this case ```I wihh i was here``` was fixed to ```I with I was here.```, where the intension was ```I wish I was there.```. Ideal usage is to automatically clean-up rough text with bad formatting.

Interesting observation: Missing fullstops are fixed if other fullstops are present in the text, but double fullstops are not fixed...

#### Controls
Controls are GUI (Graphical User Interface) components used to present or
collect information from the construct user in the viewer. Controls can only be
added to pages and all controls on  a page is shown to the user in the construct viewer at the same time.

| Control    | Purpose                                                       |
|------------|---------------------------------------------------------------|
| Text       | Present text information to user.                             |
| Question   | Ask the user an open question with a text answer.             |
| Choice     | Ask the user a closed question from a list of options.        |
| Dices      | Present random values using 3d animated gambling dice.        |
| CRUD       | Database collection with Create, Read, Update, Destroy.       |

| Control    | When linked to anchor value                              |
|------------|---------------------------------------------------------------|
| Text       | Show the value of the anchor as text, overriding the default. |
| Question   | Use anchor value object to override label and value.          |
| Choice     | Use anchor value object to override label, value, options.    |
| Dices      | Use anchor value object to override label and value.          |
| CRUD       | Cannot be linked to an anchored component.                    |

##### Text
It can receive its value from another component by linking to an anchored
component.

###### Markdown
Text can contain markdown and HTML. Some handy snippets:

Responsive Popout Boxed Image located at External URL
```
<img class="materialboxed responsive-img" src="https://dft1tcs2nbf7z.cloudfront.net/images/tutorials/quickstart/main_menu.gif">
```

##### Question
It can receive its value from another component by linking to an anchored
component.
Special characters include: ```_@$#±/\ |.,!?<>{}()[]'"`^#&+-*%="~```

##### CRUD Schema

The schema field in the construct designer Insert and Update CRUD dialogs must
be formatted as JSON; the objects are control component specifications, for
example:
```
[
  {
     "name": "what",
     "type": "question",
     "prompt": "What are you doing?",
     "format": "ALPHA_NUMERIC_SPECIAL",
     "icon": "play_for_work",
     "show": true
   },
  {
     "name": "where",
     "type": "question",
     "prompt": "Where are you going?",
     "format": "ALPHA_NUMERIC_SPECIAL",
     "icon": "location_on",
     "show": false
   },
  {
    "name": "who",
     "type": "question",
     "prompt": "Who are you talking to?",
     "format": "ALPHA_NUMERIC_SPECIAL",
     "icon": "people",
     "show": false
  },
 {
    "name": "expectedOutcome",
     "type": "question",
     "prompt": "How will it go?",
     "format": "ALPHA_NUMERIC_SPECIAL",
     "icon": "announcement",
     "show": true
  }
]
```
When a control is not shown, it is still asked in the CRUD-Create and CRUD-Update forms, it just is not listed in the on page summary. If an Icon is not specified, the prompt will be used instead of the icon. When an icon is specified, the prompt will be used as tooltip for the icon.

For a full list of supported icons, see [MaterializeCSS Icons](http://materializecss.com/icons.html).

##### Dynamic Controls
It is common for conversations to gather more information about data provided by the user; in order to accomplish this it mush be possible to create controls on the fly based on user data. All controls can be dynamically modified during a session by linking to a Expression that returns an object to override the parameters of the control that was specified for this control in the Construct Designer:

| Controls               | Parameter
|------------------------|--------------------
| Text, Question, Choice | value - the initial/default value of the control
| Question, Choice       | prompt - tells the user what information this control requires
| Choice                 | options - an array of option objects for the user to choose from, each option has a string label and a numeric or string value or object value.
| Dices                  | tbd
| CRUD                   | schema - a JSON array of controls defining the content of database documents managed by this control. In addition to regular control keys, each control can include the icon and show keys. If a control has shown true, it will be used to determine which elements of the document to list in the collection in the viewer.

Example expression script for overriding a Choice control; the overridden control will show the user a prompt to 'Choose a colour' and offer a choice between 'Green' and 'Red', with 'Red' being preselected for the user when the page is initially shown:
```
return {
  prompt: 'Choose a colour',
  options: [
    {
      label: 'Green',
      value: 'GREEN'
    },
    {
      label: 'Red',
      value: 'RED'
    }
  ],
  value: 'RED'
};
```

tbd - Dices Example

tbd - CRUD Example
