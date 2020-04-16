# React Native basics

Now that we have set up our development environment we can get into to React Native bascis and get started with the development of our application.

## Core components

In the previous parts we have learned that we can use React to define components as functions which receive props as an argument and returns a tree of React elements. This tree is usually represented with JSX syntax. In the browser environment we have used the [ReactDOM](https://reactjs.org/docs/react-dom.html) library to turn these components into a DOM tree that can be rendered by a browser. Here is a concrete example of a very simple component:

```javascript
import React from 'react';

const HelloWorld = (props) => {
  return <div>Hello world!</div>;
};
```

The _HelloWorld_ component returns a single _div_ element which is created using the JSX syntax. We might remember that this JSX syntax is actually compiled into `React.createElement` method calls, such as this:

```javascript
React.createElement('div', null, 'Hello world!');
```

This line of code creates a _div_ element without any props and with a single child element which is a string _"Hello world"_. When we render this component into a root DOM element using the `ReactDOM.render` method the _div_ element will be rendered as the corresponding DOM element.

As we can see, React is not bound to a certain environment, such as browser environment. Instead there are libraries such as ReactDOM that can render _a set of predefined components_, such as DOM elements, in a specific environment. In React Native these predefined components are called _core components_.

[Core components](https://reactnative.dev/docs/intro-react-native-components) are a set of components provided by React Native which behind the scenes utilize platforms native components. Let's implement the previous example using React Native:

<!-- TODO: highlight changed parts -->

```javascript
import React from 'react';
import { Text } from 'react-native';

const HelloWorld = (props) => {
  return <Text>Hello world!</Text>;
};
```

So we import the [Text](https://reactnative.dev/docs/text) component from React Native and replaced the _div_ element with _Text_ element. Many familiar DOM elements have their React Native "counterparts". Here are some examples picked from the React Native's [Core Components documentation](https://reactnative.dev/docs/components-and-apis):

- [Text](https://reactnative.dev/docs/text) component is _the only_ React Native component that can have textual children. It is similar to for example the `<strong>` and the `<h1>` elements.
- [View](https://reactnative.dev/docs/view) component is the basic building block similar to the `<div>` element.
- [TextInput](https://reactnative.dev/docs/textinput) component is a text field component similar to the `<input>` element.
- [TouchableWithoutFeedback](https://reactnative.dev/docs/touchablewithoutfeedback) component (and other _Touchable\*_ components) component is for capturing different press events. It is similar to for example the `<button>` element.

There are a few notable differences between core components and DOM elements. The first difference is that the _Text_ component is _the only_ React Native component that can have textual children. This means that you can't for example replace the _Text_ component with the _View_ component in the previous example.

The second notable difference is related to the event handlers. While working with the HTML elements we are used to adding event handlers such as `onClick` to basically any element such as `<div>` and `<button>`. In React Native we have to carefully read the [API documentation](https://reactnative.dev/docs/components-and-apis) to know what event handlers (as well as other props) a component accepts. For example the family of ["Touchable" components](https://reactnative.dev/docs/handling-touches#touchables) provide the capability to capture tapping gestures, and can display feedback when a gesture is recognized. One of these components is the [TouchableWithoutFeedback](https://reactnative.dev/docs/touchablewithoutfeedback) component, which accepts the `onPress` prop:

```javascript
import React from 'react';
import { Text, TouchableWithoutFeedback, Alert } from 'react-native';

const TouchableText = (props) => {
  return (
    <TouchableWithoutFeedback
      onPress={() => Alert.alert('You pressed the text!')}
    >
      <Text>You can press me</Text>
    </TouchableWithoutFeedback>
  );
};
```

Now that we have a basic understanding of the core components, let's start to give our project some structure. Create a _src_ directory into the root directory of your project and into the _src_ directory create a _components_ directory. Into the _components_ directory create a file _Main.jsx_ with the following content:

```javascript
import React from 'react';
import Constants from 'expo-constants';
import { Text, StyleSheet, View } from 'react-native';

const styles = StyleSheet.create({
  container: {
    marginTop: Constants.statusBarHeight,
  },
});

const Main = () => {
  return (
    <View style={styles.container}>
      <Text>Rate Repository Application</Text>
    </View>
  );
};

export default Main;
```

Next, let's use the _Main_ component in the _App_ component in the _App.js_ file which is located in our project's root directory. Replace the current content of the file with this:

```javascript
import React from 'react';

import Main from './src/components/Main';

const App = () => {
  return <Main />;
};

export default App;
```

## Exercise 10.3.

In this exercise we will implement the first version of rated repositories list. The list should contain the repository's full name, description, language, number of forks, number of stars, rating average and number of reviews. Luckily React Native provides a handy component for displaying a list of data, which is the [FlatList](https://reactnative.dev/docs/flatlist) component. Implement components _RepositoryList_ and _RepositoryItem_ into the _components_ directory's files _RepositoryList.jsx_ and _RepositoryItem.jsx_. _RepositoryList_ component should render the _FlatList_ component and _RepositoryItem_ a single item on the list (hint: use the FlatList component's [renderItem](https://reactnative.dev/docs/flatlist#renderitem) prop). Use this as the basis for the _RepositoryList.jsx_ file:

```javascript
import React from 'react';
import { FlatList, View, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  separator: {
    height: 10,
  },
});

const repositories = [
  {
    id: 'jaredpalmer.formik',
    fullName: 'jaredpalmer/formik',
    description: 'Build forms in React, without the tears',
    language: 'TypeScript',
    forksCount: 1589,
    stargazersCount: 21553,
    ratingAverage: 88,
    reviewCount: 4,
    ownerAvatarUrl: 'https://avatars2.githubusercontent.com/u/4060187?v=4',
  },
  {
    id: 'rails.rails',
    fullName: 'rails/rails',
    description: 'Ruby on Rails',
    language: 'Ruby',
    forksCount: 18349,
    stargazersCount: 45377,
    ratingAverage: 100,
    reviewCount: 2,
    ownerAvatarUrl: 'https://avatars1.githubusercontent.com/u/4223?v=4',
  },
  {
    id: 'django.django',
    fullName: 'django/django',
    description: 'The Web framework for perfectionists with deadlines.',
    language: 'Python',
    forksCount: 21015,
    stargazersCount: 48496,
    ratingAverage: 73,
    reviewCount: 5,
    ownerAvatarUrl: 'https://avatars2.githubusercontent.com/u/27804?v=4',
  },
  {
    id: 'reduxjs.redux',
    fullName: 'reduxjs/redux',
    description: 'Predictable state container for JavaScript apps',
    language: 'TypeScript',
    forksCount: 13902,
    stargazersCount: 52869,
    ratingAverage: 0,
    reviewCount: 0,
    ownerAvatarUrl: 'https://avatars3.githubusercontent.com/u/13142323?v=4',
  },
];

const ItemSeparator = () => <View style={styles.separator} />;

const RepositoryList = () => {
  return (
    <FlatList
      data={repositories}
      ItemSeparatorComponent={ItemSeparator}
      // other props
    />
  );
};

export default RepositoryList;
```

_Do not_ alter the contents of the `repositories` variable, it should contain everything you need to complete this exercise. Render the _RepositoryList_ component in the _Main_ component which we previously added into the _Main.jsx_ file in the _components_ directory. The rated repository list should roughly look something like this:

![Application preview](images/5.png)

## Style

Now that we have a basic understanding how core components work and we can use them to build a simple user interface it is time to add some style. In [part 2](/en/part2/adding_styles_to_react_app) we learned that in the browser environment we can define React component's style properties using CSS. We had an option to either define these styles inline using the `style` prop or in a CSS file with a suitable selector.

There are many similarties in the way style properties are attached to React Native's core components and CSS. In React Native most of the core components accept a prop called `style`. The `style` prop accepts an object with style attributes and their values. These style properties are in most cases the same as in CSS, however property names are in _camelCase_. This means that CSS properties such as `padding-top` and `font-size` written as `paddingTop` and `fontSize`. Here is a simple example of how to use the `style` prop:

```javascript
import React from 'react';
import { Text, View } from 'react-native';

const BigBlueText = () => {
  return (
    <View style={{ padding: 20 }}>
      <Text style={{ color: 'blue', fontSize: 24, fontWeight: 700 }}>
        Big blue text
      <Text>
    </View>
  );
};
```

On top of the property names, you might have noticed another difference in the example. In CSS numerical property values commonly have a unit such as _px_, _em_ or _rem_. In React Native all dimension related property values such as `width`, `height`, `padding`, and `margin` as well as font sizes are _unitless_. These unitless numeric values represent _density-independent pixels_. In case you are wondering what are the available style properties for certain core component, check the [React Native Styling Cheat Sheet](https://github.com/vhpoet/react-native-styling-cheat-sheet).

<!-- TODO: avaa tarkemmin density-independent pixels -->

In general defining styles directly in the `style` prop is not consired such a great idea, because it makes components bloated and unclear. Instead we should define styles outside the component's render function using the [StyleSheet.create](https://reactnative.dev/docs/0.53/stylesheet#create) method. The `StyleSheet.create` method accepts a single argument which is an object consisting of named style objects and creates a StyleSheet style reference from the given object. Here is an example of how refactor the previous example using `StyleSheet.create`:

```javascript
import React from 'react';
import { Text, View, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    padding: 20,
  },
  text: {
    color: 'blue',
    fontSize: 24,
    fontWeight: 700,
  },
});

const BigBlueText = () => {
  return (
    <View style={styles.container}>
      <Text style={styles.text}>
        Big blue text
      <Text>
    </View>
  );
};
```

We create two named style objects, _container_ and _text_. Inside the component we can
we can access specific style object the same way we would access any key in a plain object.

In addition to an object, the `style` prop also accepts an array of objects. In case of an array, the objects are merged from left to right so that latter style properties takes presence. This works recursively, so we can have for example an array containing an array of styles and so forth. If an array contains values that evaluate to false, such as `null` or `undefined`, these values are ignored. This makes it easy to define _conditional styles_ for example based on a value of prop. Here is an example of conditional styles:

```javascript
import React from 'react';
import { Text, View, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  text: {
    color: 'grey',
    fontSize: 14,
  },
  blueText: {
    color: 'blue',
  },
  bigText: {
    fontSize: 24,
    fontWeight: 700,
  },
});

const FancyText = ({ isBlue, isBig, children }) => {
  const textStyles = [
    styles.text,
    isBlue && styles.blueText,
    isBig && styles.bigText,
  ];

  return <Text style={textStyles}>{children}</Text>;
};

const Main = () => {
  return (
    <>
      <FancyText>Simple text</FancyText>
      <FancyText isBlue>Blue text</FancyText>
      <FancyText isBig>Big text</FancyText>
      <FancyText isBig isBlue>
        Big blue text
      </FancyText>
    </>
  );
};
```

In the example we use the `&&` operator with statement `condition && exprIfTrue`. This statement yields `exprIfTrue` if the `condition` evaluates to true, otherwise it will yield `condition`, which in that case is a value that evaluates to false. This is an extremely widely used and handy shorthand. Another option would be to use for example the [conditional operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator), `condition ? exprIfTrue : exprIfFalse`.

## Consistent user interface with theming

Let's stick with the concept of styling but with a bit wider perspective. Most of us have a used a multitude of differenct applications and might agree that one trait that makes a good user interface is _consistency_. This means that the appearance of user interface components such as their font size, font family and color follow a consistent pattern. To achieve this we have somehow _parametrize_ values of different style properties. This method is commonly known as _theming_.

Users of popular user interface libraries such as [Bootstrap](https://getbootstrap.com/docs/4.4/getting-started/theming/) and [Material UI](https://material-ui.com/customization/theming/) might already be quite familiar with theming. Eventhough the theming implementations differ the main idea is always to use variables such as `colors.primary` instead of ["magic numbers"](<https://en.wikipedia.org/wiki/Magic_number_(programming)>) such as `#0366d6` when defining styles. This leads to increased consistency and flexibility.

Let's see how theming could work in practice in our application. We will be using a lot text with different variations, such as different font sizes and color. Because React Native does not support global styles we should create our own _Text_ component to keep the textual content consistent. Let's get started by adding the following theme configuration object in the _theme.js_ file in the _src_ directory:

```javascript
const theme = {
  colors: {
    textPrimary: '#24292e',
    textSecondary: '#586069',
    primary: '#0366d6',
  },
  fontSizes: {
    body: 14,
    subheading: 16,
  },
  fonts: {
    main: 'System',
  },
  fontWeights: {
    normal: '400',
    bold: '700',
  },
};

export default theme;
```

Next we should create the actual _Text_ component which uses this theme configuration. Create a _Text.jsx_ file into the _components_ directory where we already have our other components with the following content:

```javascript
import React from 'react';
import { Text as NativeText, StyleSheet } from 'react-native';

import theme from '../theme';

const styles = StyleSheet.create({
  text: {
    color: theme.colors.textPrimary,
    fontSize: theme.fontSizes.body,
    fontFamily: theme.fonts.main,
    fontWeight: theme.fontWeights.normal,
  },
  colorTextSecondary: {
    color: theme.colors.textSecondary,
  },
  colorPrimary: {
    color: theme.colors.primary,
  },
  fontSizeSubheading: {
    fontSize: theme.fontSizes.subheading,
  },
  fontWeightBold: {
    fontWeight: theme.fontWeights.bold,
  },
});

const Text = ({ color, fontSize, fontWeight, style, ...props }) => {
  const textStyle = [
    styles.text,
    color === 'textSecondary' && styles.colorTextSecondary,
    color === 'primary' && styles.colorPrimary,
    fontSize === 'subheading' && styles.fontSizeSubheading,
    fontWeight === 'bold' && styles.fontWeightBold,
    style,
  ];

  return <NativeText style={textStyle} {...props} />;
};

export default Text;
```

Now we have implemented our own text component with consistent color, font size and font weight variants which we can use anywhere in our application. We can get different text variations using different props like this:

```javascript
import React from 'react';

import Text from './Text';

const Main = () => {
  return (
    <>
      <Text>Simple text</Text>
      <Text style={{ paddingBottom: 10 }}>Text with custom style</Text>
      <Text fontWeight="bold" fontSize="subheading">
        Bold subheading
      </Text>
      <Text color="textSecondary">Text with secondary color</Text>
    </>
  );
};

export default Main;
```

Feel free to extend or modify this component if you feel like it. It might also be a good idea to create reusable text components such as _Subheading_ which use the _Text_ component. Also keep on extending and modifying the theme configuration as your application progresses.

## Using flebox for layout

The last concept we will cover related to styling is implementing layouts with [flexbox](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox). Those who are familiar with CSS know that flexbox is not only related to React Native instead it has many use cases in web development as well. In fact those who know how flexbox works in web development won't probably learn that much from this section. Nevertheless, let's learn or revise the basics of flexbox.

Flexbox is a layout entity consisting of two separete components: a _flex container_ and inside it a set of _flex items_. Flex container has a set of properties which control the flow of its items. To make a component a flex container it must have the style property `display` set as `flex` which is actually the default value for the `display` property. Here is an example of a flex container:

```javascript
import React from 'react';
import { View, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  flexContainer: {
    flexDirection: 'row',
  },
});

const FlexboxExample = () => {
  return <View style={styles.flexContainer}>{/* ... */}</View>;
};
```

Perhaps the most important properties of a flex container are the following:

- [flexDirection](https://css-tricks.com/almanac/properties/f/flex-direction/) property controls the direction in which the flex items are laid out within the container. Possible values for this property are `row`, `row-reverse`, `column` (default value) and `column-reverse`. Flex direction `row` will lay out the flex items from left to right, whereas `column` from top to bottom. `*-reverse` directions will just reverse the order of the flex items.
- [justifyContent](https://css-tricks.com/almanac/properties/j/justify-content/) property controls the aligment of flex items along the main axis (defined by the `flexDirection` property). Possible values for this property are `flex-start` (default value), `flex-end`, `center`, `space-between`, `space-around` and `space-evenly`.
- [alignItems](https://css-tricks.com/almanac/properties/a/align-items/) property does the same as `justifyContent` but for the opposite axis. Possible values for this property are `flex-start`, `flex-end`, `center`, `baseline` and `stretch` (default value).

Let's move on to flex items. As mentioned, a flex container can contain one or many flex items. Flex items have properties which control how they behave in respect of other flex items in the same flex container. To make a component a flex item all you have to do is to set it as a immediate child of a flex container:

```javascript
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  flexContainer: {
    display: 'flex',
  },
  flexItemA: {
    flexGrow: 0,
    backgrundColor: 'green',
  },
  flexItemB: {
    flexGrow: 1,
    backgroundColor: 'blue',
  },
});

const FlexboxExample = () => {
  return (
    <View style={styles.flexContainer}>
      <View style={styles.flexItemA}>
        <Text>Flex item A</Text>
      </View>
      <View style={styles.flexItemB}>
        <Text>Flex item B</Text>
      </View>
    </View>
  );
};
```

One of the most commonly used properties of flex item is the [flexGrow](https://css-tricks.com/almanac/properties/f/flex-grow/) property. It accepts a unitless value which defines the ability for a flex item to grow if necessary. If every flex items have a `flexGrow` of `1`, they will share all the available space evenly. If a flex item have a `flexGrow` of `0` it will only use the space its content requires and leave rest of the space for other flex items.

Here is a more interactive and concreate example of how to use flexbox to implement a simple card component with header, body and footer:

<!--
TODO: embedded snack

<div data-snack-id="@kalleilv/flexbox-example" data-snack-platform="web" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#fafafa;border:1px solid rgba(0,0,0,.08);border-radius:4px;height:505px;width:100%"></div>
<script async src="https://snack.expo.io/embed.js"></script>
-->

Next, read the article [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) which has comprehensive visual examples of flexbox. It is also a good idea to play around with the flexbox properties in the [Flexbox Playground](https://demos.scotch.io/visual-guide-to-css3-flexbox-flexbox-playground/demos/) to see how different flexbox properties affect the layout. Remember that in React Native the property names are the same as the ones in CSS with the exception of the _camelCase_ naming. However, the _property values_ such as `flex-start` and `space-between` are exactly the same.

## Exercises 10.4. - 10.5.

### Exercise 10.4.

We will soon need to navigate between different views in our application. That is why need an [app bar](https://material.io/components/app-bars-top/) to display tabs for switching between different views. Create a file _AppBar.jsx_ into the _components_ folder with the following content:

```javascript
import React from 'react';
import { View, ScrollView } from 'react-native';
import Constants from 'expo-constants';

const styles = StyleSheet.create({
  container: {
    paddingTop: Constants.statusBarHeight,
    // ...
  },
  // ...
});

const AppBar = () => {
  return (
    <View style={styles.container}>
      <ScrollView horizontal>{/* ... */}</ScrollView>
    </View>
  );
};

export default AppBar;
```

The [ScrollView](https://reactnative.dev/docs/scrollview) component enables horizontal scrolling when there is too much content to fit the screen. Now that the _AppBar_ component will prevent the status bar from overlapping the content, you can remove the `marginTop` style we added for the _Main_ component earlier in the _Main.jsx_ file. The _AppBar_ component should currently contain a tab with text "Repositories". Make the tab touchable by using the [TouchableWithoutFeedback](https://reactnative.dev/docs/touchablewithoutfeedback) component but you don't have to handle the `onPress` event in any way. Add the _AppBar_ component to the _Main_ component so that it is the uppermost component in the screen. The _AppBar_ component should look something like this:

![Application preview](images/6.png)

The background color of the app bar in the image is `#24292e` but you can use any other color as well. It might be a good idea to add the app bar's background color into the theme configuration so that it is easy to change it if needed. Another good idea might be to separate the app bar's tab into its own component such as _AppBarTab_ so that it is easy to add new tabs in the future.

### Exercise 10.5.

The current version of the rated repositories list looks quite grim. Modify the _RepositoryListItem_ component so that it also displays reporitory author's avatar image. You can implement this by using the [Image](https://reactnative.dev/docs/image) component. Counts, such as number of stars and forks, larger than or equal to 1000 should be displayed in thousands with precision of one decimal and with a "k" suffix. This means that for example fork count of 8439 should be displayed as "8.4k". Also polish the overall look of the component so that the rated repositories list looks something like this:

![Application preview](images/7.png)

In the image, the _Main_ component's background color is set to `#e1e4e8` whereas _RepositoryListItem_ component's background color is set to `white`. Remember to exploit the _Text_ component we implemented earlier. Also when needed, split the _RepositoryListItem_ component into smaller components.

## Routing

When we start to expand our application we will need a way to transition between different views such as the repositories view and the sign in view. In [part 7](https://fullstackopen.com/en/part7/react_router) we got familiar with [React router](https://reacttraining.com/react-router/native/guides/quick-start) library and learned how use it to implement routing in a web application.

Routing in a React Native application is a bit different to routing in a web application. The main difference is that we can't reference pages with URLs which we type in to the browser's address bar and can't navigate back and forth through user's history using the browsers [history API](https://developer.mozilla.org/en-US/docs/Web/API/History_API). However, this is just the matter of the router interface we are using.

With React Native we can use the entire React router's core, including the hooks and components. The only difference to the browser environment is that we must replace the _BrowserRouter_ with React Native compatible [NativeRouter](https://reacttraining.com/react-router/native/api/NativeRouter) provided by the [react-router-native](https://reacttraining.com/react-router/native/guides/quick-start) library. Let's get started by installing the _react-router-native_ library:

```shell
npm install react-router-native
```

Next, open the _App.js_ file and add the _NativeRouter_ component into the _App_ component:

<!-- TODO: highlight -->

```javascript
import React from 'react';
import { NativeRouter } from 'react-router-native';

import Main from './src/components/Main';

const App = () => {
  return (
    <NativeRouter>
      <Main />
    </NativeRouter>
  );
};

export default App;
```

Now that the router is in place, let's add our first route into the _Main_ componenent in the _Main.jsx_ file:

<!-- TODO: highlight -->

```javascript
import React from 'react';
import { StyleSheet, View } from 'react-native';
import { Route, Switch, Redirect } from 'react-router-native';

import RepositoryList from './RepositoryList';
import AppBar from './AppBar';
import theme from '../theme';

const styles = StyleSheet.create({
  container: {
    backgroundColor: theme.colors.mainBackground,
  },
});

const Main = () => {
  return (
    <View style={styles.container}>
      <AppBar />
      <Switch>
        <Route path="/" exact>
          <RepositoryList />
        </Route>
        <Redirect to="/" />
      </Switch>
    </View>
  );
};

export default Main;
```

## Exercise 10.6.

Create the file _SignIn.jsx_ into the _components_ directory with the following content:

```javascript
import React from 'react';

import Text from './Text';

const SignIn = () => {
  return <Text>The sign in view</Text>;
};

export default SignIn;
```

Set up a route for this _SignIn_ component in the _Main_ component. Also add a tab with text "Sign in" in to the app bar next to the "Repositories" tab. User should be able to navigate between the two views by pressing the tabs (hint: use the [Link](https://reacttraining.com/react-router/native/api/Link) component and its [component](https://reacttraining.com/react-router/native/api/Link/component-func) prop).

## Forms

Now that we have a placeholder sign in view the next step would be to implement the sign form. Before we get to that let's talk about forms in a more wider perspective.

Implementation of forms relies heavily on the state management. Using the React's _useState_ hook for the state management might get the job done for smaller forms, however it will quickly make the state management quite tedious with more complex forms. Luckily there are many good libraries in the React ecosystem that eases the state management of forms. One of these libraries is [Formik](https://jaredpalmer.com/formik/).

The main concepts of Formik are the _Formik context_ and a _field_. The Formik context is provided by the [Formik](https://jaredpalmer.com/formik/docs/api/formik) component that it contains and mantains the form's state. The state consists of information of form's fields. This information includes for example the value and validation errors of each field. State's fields can be referenced by their name using the [useField](https://jaredpalmer.com/formik/docs/api/useField) hook or the [Field](https://jaredpalmer.com/formik/docs/api/field) component.

Let's see how this actually works by creating a form for calculation the [body mass index](https://en.wikipedia.org/wiki/Body_mass_index):

```javascript
import React from 'react';
import {
  Text,
  TextInput,
  TouchableWithoutFeedback,
  Alert,
  View,
} from 'react-native';
import { Formik, useField } from 'formik';

const initialValues = {
  mass: '',
  height: '',
};

const getBodyMassIndex = (mass, height) => {
  return Math.round(mass / Math.pow(height, 2));
};

const BodyMassIndexForm = ({ onSubmit }) => {
  const [massField, massMeta, massHelpers] = useField('mass');
  const [heightField, heightMeta, heightHelpers] = useField('height');

  return (
    <View>
      <TextInput
        placeholder="Weight (kg)"
        value={massField.value}
        onChangeText={(text) => massHelpers.setValue(text)}
      />
      <TextInput
        placeholder="Height (m)"
        value={heightField.value}
        onChangeText={(text) => heightHelpers.setValue(text)}
      />
      <TouchableWithoutFeedback onPress={onSubmit}>
        <Text>Calculate</Text>
      </TouchableWithoutFeedback>
    </View>
  );
};

const BodyMassIndexCalculator = () => {
  const onSubmit = (values) => {
    console.log(values);

    const mass = parseFloat(values.mass);
    const height = parseFloat(values.height);

    if (!isNaN(mass) && !isNaN(height) && height !== 0) {
      console.log(`Your body mass index is: ${getBodyMassIndex(mass, height)}`);
    }
  };

  return (
    <Formik initialValues={initialValues} onSubmit={onSubmit}>
      {({ handleSubmit }) => <BodyMassIndexForm onSubmit={handleSubmit} />}
    </Formik>
  );
};
```

We define the _Formik_ context in the _BodyMassIndexCalculator_ and provided it with initial values and a submit callback. Initial values are provided through the [initialValues](https://jaredpalmer.com/formik/docs/api/formik#initialvalues-values) prop as an object with field names as keys and the corresponding initial values as values. The submit callback is provided through the [onSubmit](https://jaredpalmer.com/formik/docs/api/formik#onsubmit-values-values-formikbag-formikbag--void--promiseany) prop and it is called when the `handleSubmit` function is called with the condition that there isn't any validation errros. Children of the _Formik_ component is a function which is called with [props](https://jaredpalmer.com/formik/docs/api/formik#formik-render-methods-and-props) including state related information and actions such as the `handleSubmit` function.

The _BodyMassIndexForm_ component contains the state bindings between the context and text inputs. We use the [useField](https://jaredpalmer.com/formik/docs/api/useField) hook to get the value of a field and to change it. _useField_ hooks has one argument which is the name of the field and it returns an array with three values, `[field, meta, helpers]`. The [field object](https://jaredpalmer.com/formik/docs/api/useField#fieldinputpropsvalue) contains the value of the field, the [meta object](https://jaredpalmer.com/formik/docs/api/useField#fieldmetapropsvalue) contains field meta information such as a posibble error message and the [helpers object](https://jaredpalmer.com/formik/docs/api/useField#fieldhelperprops) contains different actions for changing the state of field such as the `setValue` function.

Here is an interactive version of our previous example:

<!-- TODO: embedded snack

<div data-snack-id="@kalleilv/formik-example" data-snack-platform="web" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#fafafa;border:1px solid rgba(0,0,0,.08);border-radius:4px;height:505px;width:100%"></div>
<script async src="https://snack.expo.io/embed.js"></script>
-->

In the previous example using the _useField_ hook with the _TextInput_ component causes repetitive code. Let's extract this repetitive code into a _FormikTextInput_ component and create a custom _TextInput_ component to make text inputs a bit more visually pleasing. First, create a file _TextInput.jsx_ into the _components_ directory with the following content:

```javascript
import React from 'react';
import { TextInput as NativeTextInput, StyleSheet } from 'react-native';

const styles = StyleSheet.create({});

const TextInput = ({ style, error, ...props }) => {
  const textInputStyle = [style];

  return <TextInput style={textInputStyle} {...props} />;
};

export default TextInput;
```

Next, let's move on to the _FormikTextInput_ component. Create a file _FormikTextInput.jsx_ into the _components_ directory with the following content:

```javascript
import React from 'react';
import { StyleSheet } from 'react-native';
import { useField } from 'formik';

import TextInput from './TextInput';
import Text from './Text';

const styles = StyleSheet.create({
  errorText: {
    marginTop: 5,
  },
});

const FormikTextInput = ({ name, ...props }) => {
  const [field, meta, helpers] = useField(name);
  const showError = meta.touched && meta.error;

  return (
    <>
      <TextInput
        onChangeText={(value) => helpers.setValue(value)}
        value={field.value}
        error={showError}
        {...props}
      />
      {showError && <Text style={styles.errorText}>{meta.error}</Text>}
    </>
  );
};
export default FormikTextInput;
```

## Exercises

- Sign in form

## Platform specific code

- `Platform.OS`
- `Platform.select`
- Platform specific imports (`.ios.js` & `.android.js`)

## Exericses

- Use platform specific sans-serif font in the theme configuration (iOS Arial, Android Ubuntu)
