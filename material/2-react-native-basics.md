# React Native basics

Now that we have set up our development environment we can get into to React Native bascis and get started with the development of our application.

## Core components

In the previous parts we have learned that we can use React to define components as functions which receive props as an argument and returns a tree of React elements. This tree is usually represented with JSX syntax. In the browser environment we have used the [ReactDOM](https://reactjs.org/docs/react-dom.html) library to turn these components into a DOM tree that can be rendered by a browser. Here is a concrete example of a very simple component:

```javascript
import React from 'react';

const HelloWorld = props => {
  return (
    <div>Hello world!</div>
  );
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

const HelloWorld = props => {
  return (
    <Text>Hello world!</Text>
  );
};
```

So we import the [Text](https://reactnative.dev/docs/text) component from React Native and replaced the _div_ element with _Text_ element. Many familiar DOM elements have their React Native "counterparts". Here are some examples picked from the React Native's [Core Components documentation](https://reactnative.dev/docs/components-and-apis):

- [Text](https://reactnative.dev/docs/text) component is _the only_ React Native component that can have textual children. It is similar to for example the `<strong>` and the `<h1>` elements.
- [View](https://reactnative.dev/docs/view) component is the basic building block similar to the `<div>` element.
- [TextInput](https://reactnative.dev/docs/textinput) component is a text field component similar to the `<input>` element.
- [TouchableWithoutFeedback](https://reactnative.dev/docs/touchablewithoutfeedback) component (and other _Touchable*_ components) component is for capturing different press events. It is similar to for example the `<button>` element.

There are a few notable differences between core components and DOM elements. The first difference is that the _Text_ component is _the only_ React Native component that can have textual children. This means that you can't for example replace the _Text_ component with the _View_ component in the previous example.

The second notable difference is related to the event handlers. While working with the HTML elements we are used to adding event handlers such as `onClick` to basically any element such as `<div>` and `<button>`. In React Native we have to carefully read the [API documentation](https://reactnative.dev/docs/components-and-apis) to know what event handlers (as well as other props) a component accepts. For example the family of ["Touchable" components](https://reactnative.dev/docs/handling-touches#touchables) provide the capability to capture tapping gestures, and can display feedback when a gesture is recognized. One of these components is the [TouchableWithoutFeedback](https://reactnative.dev/docs/touchablewithoutfeedback) component, which accepts the `onPress` prop:

```javascript
import React from 'react';
import { Text, TouchableWithoutFeedback, Alert } from 'react-native';

const TouchableText = props => {
  return (
    <TouchableWithoutFeedback onPress={() => Alert.alert('You pressed the text!')}>
      <Text>You can press me</Text>
    </TouchableWithoutFeedback>
  );
}
```

Now that we have a basic understanding of the core components, let's start to give our project some structure. Create a _src_ directory into the root directory of your project and into the _src_ directory create a _components_ directory. Into the _components_ directory create a file _Main.jsx_ with the following content:

```javascript
import React from 'react';
import Constants from 'expo-constants';
import { Text, StyleSheet, View } from 'react-native';

const styles = StyleSheet.create({
  container: {
    marginTop: Constants.statusBarHeight,
  }
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
  return (
    <Main />
  );
};

export default App;
```

<!-- TODO: iOS and Android specific core components & Snack examples -->

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
 
<!-- TODO: final result image -->

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
    isBig && styles.bigText
  ];

  return (
    <Text style={textStyles}>
      {children}
    </Text>
  );
};

const Main = () => {
  return (
    <>
      <FancyText>Simple text</FancyText>
      <FancyText isBlue>Blue text</FancyText>
      <FancyText isBig>Big text</FancyText>
      <FancyText isBig isBlue>Big blue text</FancyText>
    </>
  );
};
```

In the example we use the `&&` operator with statement `condition && exprIfTrue`. This statement yields `exprIfTrue` if the `condition` evaluates to true, otherwise it will yield `condition`, which in that case is a value that evaluates to false. This is an extremely widely used and handy shorthand. Another option would be to use for example the [conditional operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator), `condition ? exprIfTrue : exprIfFalse`.

## Consistent user interface with theming

Let's stick with the concept of styling but with a bit wider perspective. Most of us have a used a multitude of differenct applications and might agree that one trait that makes a good user interface is _consistency_. This means that the appearance of user interface components such as their font size, font family and color follow a consistent pattern. To achieve this we have somehow _parametrize_ values of different style properties. This method is commonly known as _theming_.

Users of popular user interface libraries such as [Bootstrap](https://getbootstrap.com/docs/4.4/getting-started/theming/) and [Material UI](https://material-ui.com/customization/theming/) might already be quite familiar with theming. Eventhough the theming implementations differ the main idea is always to use variables such as `colors.primary` instead of ["magic numbers"](https://en.wikipedia.org/wiki/Magic_number_(programming)) such as `#0366d6` when defining styles. This leads to increased consistency and flexibility.

Let's see how theming could work in practice in our application. We will be using a lot text with different variations, such as different font sizes and color. Because React Native does not support global styles we should create our own _Text_ component to keep the textual content consistent. Let's  get started by adding the following theme configuration object in the `theme.js` file in the `src` directory:

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
    main: 'Arial',
  },
  fontWeights: {
    normal: 400,
    bold: 700,
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

  return (
    <NativeText style={textStyle} {...props} />
  );
}

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
      <Text fontWeight="bold" fontSize="subheading">Bold subheading</Text>
      <Text color="textSecondary">Text with secondary color</Text>
    </>
  );
};

export default Main;
```

Feel free to extend or modify this component if you feel like it. It might also be a good idea to create reusable text components such as _Subheading_ which use the _Text_ component. Also keep on extending and modifying the theme configuration as your application progresses.

## Using flebox for layout

The last concept we will cover related to styling is implementing layouts with [flexbox](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Flexbox). Those who are familiar with CSS know that flexbox is not only related to React Native instead it has many use cases in web development as well. In fact those who know how flexbox works in web development won't probably learn that much from this section. Nevertheless, let's learn or revise the basics of flexbox. 

Flexbox is a layout entity consisting of two separete components: a _flex container_ and inside it a set of _flex items_. Flex container has a set of properties which control the flow of its items. To make a component a flex container it must have the style property `display` set as `flex`:

```javascript
import React from 'react';
import { View, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  flexContainer: {
    display: 'flex',
  },
});

const FlexboxExample = () => {
  return (
    <View style={styles.flexContainer}>
      {/* ... */}
    </View>
  );
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
        <Text>
          Flex item A
        </Text>
      </View>
      <View style={styles.flexItemB}>
        <Text>
          Flex item B
        </Text>
      </View>
    </View>
  );
};
```

One of the most commonly used properties of flex item is the [flexGrow](https://css-tricks.com/almanac/properties/f/flex-grow/) property. It accepts a unitless value which defines the ability for a flex item to grow if necessary. If every flex items have a `flexGrow` of `1`, they will share all the available space evenly. If a flex item have a `flexGrow` of `0` it will only use the space its content requires and leave rest of the space for other flex items.

<!-- TODO: Snack -->

Next, read the article [A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) which has comprehensive visual examples of flexbox. It is also a good idea to play around with the flexbox properties in the [Flexbox Playground](https://demos.scotch.io/visual-guide-to-css3-flexbox-flexbox-playground/demos/). Remember that in React Native the property names are the same as the ones in CSS with the exception of the _camelCase_ naming. However, the _property values_ such as `flex-start` and `space-between` are exactly the same.

## Exercises

- `<AppBar />` component
- Fancy repository list
  - Style
  - Owner avatar image

## Routing

- Reference: https://fullstackopen.com/osa7/react_router
- https://reacttraining.com/react-router/native/guides/quick-start

## Exercises

- Setup routes for repository list and sing in views and add links to app bar

## Forms

- https://jaredpalmer.com/formik/
- Custom `<TextInput />` component
- Example of `<FormikTextInput />` component
- Example form
- Snack example

## Exercises

- Sign in form

## Platform specific code

- `Platform.OS`
- `Platform.select`
- Platform specific imports (`.ios.js` & `.android.js`)

## Exericses

- Use platform specific sans-serif font in the theme configuration (iOS Arial, Android Ubuntu)
