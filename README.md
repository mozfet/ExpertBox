# ExpertBox
ExpertBox.com is a knowledge automation platform that is used by experts from different domains to create constructs of systematic conversations that converse with consumer, for free or for a small fee.

Constructs are automated conversations and business process automation flows, created on ExpertBox.com using the construct designer.

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
Each component has a specific logical functions, many of these functions are inspired by software development.

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

#### Controls
Controls are GUI (Graphical User Interface) components used to present or collect information from the construct user in the viewer. Controls can only be added to pages and all controls on  a page is shown to the user in the construct viewer at the same time.

| Control    | Purpose
|------------|-------------------------------------------------------|
| Text       | present text information to user                      |
| Question   | ask the user an open question with a text answer      |
| Choice     | ask the user a closed question from a list of options |

A controls can be dynamically modified during a session by linking to a Expression that returns an object to override the parameters of the control:

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

#### Expressions
Expressions are JavaScript expressions, enabling simple data evaluation, calculation and assignment, but also complex logic, algorithms, formulas and access to other data sources on the internet.

Expressions have special functions depending on which components they are added to, however all expressions can be linked to regardless of their special functions.

Expressions, when evaluated, that have a value that is a defined value or true is consider to be true. Expressions, when evaluated, that have a value that is undefined or false is considered to be false.

In order to make constructs more managable and easier to understand in the Construct Designer, it is advised to keep each expression as short and compact as possible and rather to break up large expressions into multiple linked expressions.

| Components | Expression Special Function                                 |
|------------|-------------------------------------------------------------|
| Serie      | n/a                                                         |
| Leaf       | first leaf of a branch with true expression is branch value |
| Loop       | stop loop iteration if expression is true                   |

## Construct Viewer
The construct viewer looks a lot like a software wizard, and was inspired by that design. It has 3 main areas, the page view of the left, the timeline on the right, and the navigation buttons at the bottom of the page.
