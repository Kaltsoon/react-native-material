# React Native basics

Now that we have set up our development environment we can get into to React Native bascis and get started with the development of our application.

## Core components

In the previous parts we have learned that we can use React to define components as functions which receive props as an argument and returns a tree of React elements. This tree is usually represented with JSX syntax. In the browser environment we have used the [ReactDOM](https://reactjs.org/docs/react-dom.html) library to turn these components into a DOM tree that can be rendered by browser. Here is a concrete example of a very simple component:

```javascript
import React from 'react';

const HelloWorld = props => {
  return (
    <div>Hello world!</div>
  )
};
```

The _HelloWorld_ component returns a single _div_ element which is created using the JSX syntax. We might remember that this JSX syntax is actually compiled into `React.createElement` method calls, such as this: 

```javascript
React.createElement('div', null, 'Hello world!');
```

This line of code creates a _div_ element without any props and with a single child element which is a string _"Hello world"_. When we render this component into a root HTML element using the `ReactDOM.render` method the _div_ element will be rendered as the corresponding HTML element.

As we can see, React is not bound to a certain environment, such as browser environment. Instead there are libraries such as ReactDOM that can render _a set of predefined components_, such as HTML elements, in a specific environment. In React Native these predefined components are called _core components_.

[Core components](https://reactnative.dev/docs/intro-react-native-components) are set of components provided by React Native which behind the scenes utilizes platforms native components. Let's implement the previous example using React Native:

<!-- TODO: highlight changed parts -->

```javascript
import React from 'react';
import { Text } from 'react-native';

const HelloWorld = props => {
  return (
    <Text>Hello world!</Tex>
  );
};
```

So we import the [Text](https://reactnative.dev/docs/text) component from React Native and replaced the _div_ element with _Text_ element. Many familiar HTML elements have their React Native "counterparts". Here are some examples picked from the React Native's [Core Components documentation](https://reactnative.dev/docs/components-and-apis):

- [Text](https://reactnative.dev/docs/text) component is _the only_ React Native component that can have textual children. It is similar to for example the `<strong>` and the `<h1>` elements.
- [View](https://reactnative.dev/docs/view) component is the basic building block similar to the `<div>` element.
- [TextInput](https://reactnative.dev/docs/textinput) component is a text field component similar to the `<input>` element.
- [TouchableWithoutFeedback](https://reactnative.dev/docs/touchablewithoutfeedback) component (and other _Touchable*_ components) component is for capturing different press events. It is similar to for example the `<button>` element.

There are a few notable differences between core components and DOM elements. The first difference is that the _Text_ component is _the only_ React Native component that can have textual children. This means that you can't for example replace the _Text_ component with the _View_ component in the previous example.

The second notable difference is related to the event handlers. While working with the HTML elements we are used to adding event handlers such as `onClick` to basically any element such as `<div>` and `<button>`. In React Native we have to carefully read the [API documentation](https://reactnative.dev/docs/components-and-apis) to know what event handlers (as well as other props) a component accepts. For example the family of ["Touchable" components](https://reactnative.dev/docs/handling-touches#touchables) provide the capability to capture tapping gestures, and can display feedback when a gesture is recognized. One of these components is the [TouchableWithoutFeedback](https://reactnative.dev/docs/touchablewithoutfeedback) component, which accepts the `onPress` prop:


```javascript
import React from 'react';
import { View, TouchableWithoutFeedback, Alert } from 'react-native';

const Button = props => {
  return (
    <TouchableWithoutFeedback onPress={() => Alert.alert('You pressed the button!')}>
      <Text>A simple button</Text>
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

In this exercise we will implement the first version of rated repositories list. Luckily React Native provides a handy component for displaying a list of data, which is [FlatList](https://reactnative.dev/docs/flatlist). Implement components _RepositoryList_ and _RepositoryItem_ into the _components_ directory's files _RepositoryList.jsx_ and _RepositoryItem.jsx_. _RepositoryList_ component should render the _FlatList_ component and _RepositoryItem_ a single item on the list (hint: use the FlatList component's [renderItem](https://reactnative.dev/docs/flatlist#renderitem) prop). Use this as the basis for the _RepositoryList.jsx_ file:

```javascript
import React from 'react';
import { FlatList } from 'react-native';

const repositories = [
  {
    id: 'jaredpalmer.formik',
    fullName: 'jaredpalmer/formik',
    description: 'Build forms in React, without the tears',
    language: 'TypeScript',
    openIssuesCount: 565,
    stargazersCount: 21553,
    ratingAverage: 88,
    ownerAvatarUrl: 'https://avatars2.githubusercontent.com/u/4060187?v=4',
  },
  {
    id: 'rails.rails',
    fullName: 'rails/rails',
    description: 'Ruby on Rails',
    language: 'Ruby',
    openIssuesCount: 598,
    stargazersCount: 45377,
    ratingAverage: 100,
    ownerAvatarUrl: 'https://avatars1.githubusercontent.com/u/4223?v=4',
  },
  {
    id: 'django.django',
    fullName: 'django/django',
    description: 'The Web framework for perfectionists with deadlines.',
    language: 'Python',
    openIssuesCount: 220,
    stargazersCount: 48496,
    ratingAverage: 73,
    ownerAvatarUrl: 'https://avatars2.githubusercontent.com/u/27804?v=4',
  },
  {
    id: 'reduxjs.redux',
    fullName: 'reduxjs/redux',
    description: 'Predictable state container for JavaScript apps',
    language: 'TypeScript',
    openIssuesCount: 41,
    stargazersCount: 52869,
    ratingAverage: 0,
    ownerAvatarUrl: 'https://avatars3.githubusercontent.com/u/13142323?v=4',
  },
];

const RepositoryList = () => {
  return (
    <FlatList
      data={repositories}
      // other props
    />
  );
};

export default RepositoryList;
```

Render the _RepositoryList_ component in the _Main_ component which we previously added into the _Main.jsx_ file in the _components_ directory.
 
<!-- TODO: final result image -->

## Style

- Styles in general
  - CSS vs React Native styles
  - https://github.com/vhpoet/react-native-styling-cheat-sheet
  - Inline styles
  - `StyleSheet.create`
  - Conditional styles
  - Snack examples
- Theming
  - Motivation: https://getbootstrap.com/docs/4.4/getting-started/theming/ and https://material-ui.com/customization/theming/
- Custom `<Text />` component with variants
- Flexbox
  - https://css-tricks.com/snippets/css/a-guide-to-flexbox/
  - Snack examples

## Exercises

- `<AppBar />` component
- Fancy repository list

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
