# ExpertBox
[ExpertBox.com](https://ExpertBox.com) is used by experts from different knowledge domains to create constructs of systematic conversations that converse unattended with consumers, for free or for a small fee. In other words everyone can build their own chatbot for free, and then charge people to talk to it.

Constructs are automated conversations and business process automation flows, created on [ExpertBox.com](https://ExpertBox.com) using the construct designer. Users can have paid or free or subscribe to sessions with constructs and access these session via the Construct Viewer.

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

Expressions, when evaluated, that have a value that is a defined value or true is consider to be true. Expressions, when evaluated, that have a value that is undefined or false is considered to be false.

In order to make constructs more managable and easier to understand in the Construct Designer, it is advised to keep each expression as short and compact as possible and rather to break up large expressions into multiple linked expressions.

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
  return api._.groupBy(colourList, function(colour){ return  colourType(colour)});
```

#### Controls
Controls are GUI (Graphical User Interface) components used to present or collect information from the construct user in the viewer. Controls can only be added to pages and all controls on  a page is shown to the user in the construct viewer at the same time.

| Control    | Purpose
|------------|-------------------------------------------------------|
| Text       | present text information to user                      |
| Question   | ask the user an open question with a text answer      |
| Choice     | ask the user a closed question from a list of options |


##### Dynamic Controls
It is common for conversations to gather more information about data provided by the user; in order to accomplish this it mush be possible to create controls on the fly based on user data. All controls can be dynamically modified during a session by linking to a Expression that returns an object to override the parameters of the control that was specified for this control in the Construct Designer:

| Controls               | Parameter
|------------------------|--------------------
| Text, Question, Choice | value - the initial/default value of the control
| Question, Choice       | prompt - tells the user what information this control requires
| Choice                 | options - an array of option objects for the user to choose from

Example Expression script for overriding a Choice control; the overridden control will show the user a prompt to 'Choose a colour' and offer a choice between 'Green' and 'Red', with 'Red' being preselected for the user when the page is initially shown:
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

## Sessions
Once a construct is published by its handler, users can start creating sessions with that construct. Constructs can be published privately, publicly and within organisations.

To start a session with a construct the user must locate the construct on their stage, where it will appear if the construct was published to this user or their organisation or to the public. If the handler charges a fee for using the construct, the user will require a sufficient amount of credits to start the session.

In order to ensure that users always are using fresh knowledge, sessions expire automatically after a period of time determined by the handler. the expiration period is determined by the handler based on the nature of the knowledge, for example, day trading advice will expire before agricultural planning advice because day trading markets change fast and agricultural seasons changes slowly.

Once a session was started, the construct viewer will be shown for the construct in question. While a session has not expired, it can be found using the Sessions menu. Once expired, only reports of a session will be available, until archived or deleted by the user.

## Construct Viewer
The construct viewer looks a lot like a software installation wizard, and was inspired by that design. It has 3 main areas, the current page of the session on the left for presentation of information and collection of information from the user, the timeline on the right to show what pages have been completed and which pages are expected next (this changes all the time due to new information provided by the user), and the navigation at the bottom of the page for the user to go forward and backward between pages.
