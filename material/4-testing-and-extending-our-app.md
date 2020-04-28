# Testing and extending our application

Now that we have established a good foundation for our project, it is time to start expanding it. In this section you can put to use all the React Native knowledge you have gained so far. Along with expanding our application we will cover some new areas, such as testing, and additional resources.

## Testing React Native applications

To start testing code of any kind, the first thing we need is a testing framework, which we can use to run a set of test cases and inspect their results. For testing a JavaScript application, [Jest](https://jestjs.io/) is a popular candidate for such testing framework. For testing an Expo based React Native application with Jest, Expo provides a set of Jest configuration in a form of [jest-expo](https://github.com/expo/expo/tree/master/packages/jest-expo) preset. In order to use Eslint in the Jest's test files, we also need the [eslint-plugin-jest](https://www.npmjs.com/package/eslint-plugin-jest) plugin for Eslint. Let's get started by installing the packages:

```shell
npm install --save-dev jest jest-expo eslint-plugin-jest
```

To use the jest-expo preset in Jest, we need to add the following Jest configuration to the _package.json_ file along with the _test_ script:

```javascript
{
  // ...
  "scripts": {
    //...
    "test": "jest"
  },
  "jest": {
    "preset": "jest-expo"
  }
}
```

To use the eslint-plugin-jest plugin in Eslint, we need to include it in the _.eslintrc_ file:

```javascript
{
  "plugins": ["react", "jest"],
  "extends": ["eslint:recommended", "plugin:react/recommended", "plugin:jest/recommended"],
  "parser": "babel-eslint",
  "env": {
    "browser": true
  },
  "rules": {
    "react/prop-types": "off"
  }
}
```

To see that the setup is working, create a directory <i>\_\_tests\_\_</i> in the _src_ directory and i n the created directory create a file _example.js_. In that file, add this simple test:

```javascript
describe('Example', () => {
  it('works', () => {
    expect(1).toBe(1);
  });
});
```

Now, let's run our example test by running `npm test`. The command's output should indicate that the test located in the `src/__tests__/example.js` file is passed.

Organizing test files in a single <i>\_\_tests\_\_</i> directory is one approach in organizing the tests. When choosing this approach, it is recommended to put the test files in their corresponding subdirectories just like the code itself. This means that for example tests related to components are in the _components_ directory, tests related to utilities are in the _utils_ directory and so on. This will result in the following structure:

```
src/
  __tests__/
  components/
    AppBar.js
    RepositoryList.js
    ...
  utils/
    authStorage.js
    ...
  ...
```

Other approach is to organize the tests near the implementation. This means that for example the test file containing tests for the `AppBar` component is in the same directory as the component's code. This will result in the following structure:

```
src/
  components/
    AppBar/
      AppBar.test.jsx
      index.jsx
    ...
  ...
```

In this example, the component's code is in the _index.jsx_ file and the test in the _AppBar.test.jsx_ file. Note that in order to Jest finding your test files you either have to put them into a <i>\_\_tests\_\_</i> directory, use the _.test_ or _.spec_ suffix, or [manually configure](https://jestjs.io/docs/en/configuration#testmatch-arraystring) the glob patterns.

## Testing components

Now that we have managed to set up Jest and run a very simple test, it is time find out how to test components. As we know, testing components requires a way to serialize a component's render output and simulate firing different kind of events, such as pressing a button. For these purposes there is the [Testing Library](https://testing-library.com/docs/intro) family, which provides libraries for testing user interface components in different platforms. All of these libraries share similar API for testing user interface components in a user-centric way.

In [part 5](/en/part5/testing_react_apps) we got familiar with one of these libraries, the [React Testing Library](https://testing-library.com/docs/react-testing-library/intro). Unfortunately, this library is only suitable for testing React web applications. Luckily, there exists a React Native counterpart for this library, which is the [Native Testing Library](https://testing-library.com/docs/native-testing-library/intro). This is the library we will be using in testing our React Native application's components. The good news it, that these libraries shares a very similar API, so there isn't too many new concepts to learn. In addition to the Native Testing Library, we need a set of React Native specific Jest matchers such as `toHaveTextContent` and `toHaveProp`. These matchers are provided by the [jest-native](https://github.com/testing-library/jest-native) library. Before getting into the details, let's install these packages:

```shell
npm install --save-dev @testing-library/react-native jest-native
```

To be able to use these matchers we need to extend the Jest's `expect` object. This can be done by using a global setup file. Create a file _setupTests.js_ in the root directory of your project, that is, the same directory where the _package.json_ file is located. In that file add the following line:

```javascript
import '@testing-library/jest-native/extend-expect';
```

Next, configure this file as a setup file in the Jest's configuration in the _package.json_ file:

```javascript
{
  // ...
  "jest": {
    "preset": "jest-expo",
    "setupFiles": ["<rootDir>/setupTests.js"]
  }
  // ...
}
```

The main concepts of the Native Testing Library are the [queries](https://www.native-testing-library.com/docs/api-queries) and [firing events](https://www.native-testing-library.com/docs/api-events). Queries are used extract a set of nodes from the component that is rendered using the [render](https://www.native-testing-library.com/docs/api-main#render-options) function. These are useful in tests where we except for example some text, such as name of a repository, to be present in the rendered component. To get hands on specific nodes easily, it is recommended to tag nodes with the `testID` prop, and query it with the [getByTestId](https://www.native-testing-library.com/docs/api-queries#bytestid) function. Every React Native core component accept the `testID` prop. Here is an example of how to use the queries:

```javascript
import React from 'react';
import { Text } from 'react-native';
import { render } from '@testing-library/react-native';

const Greeting = ({ name }) => {
  return (
    <View>
      <Text testID="greetingText">Hello {name}!</Text>
    </View>
  );
};

describe('Greeting', () => {
  it('renders a greeting message based on the name prop', () => {
    const { debug, getByTestId } = render(<Greeting name="Kalle" />);

    debug();

    expect(getByTestId('greetingText')).toHaveTextContent('Hello Kalle!');
  });
});
```

The `render` function returns the queries and additional helpers, such as the `debug` function. The [debug](https://www.native-testing-library.com/docs/api-main#debug) function is a very handy to print the rendering result. Use it if you are unsure what is being rendered with the `render` function. The `toHaveTextContent` is one of the React Native specific matchers provided by the jest-native library. The full list of available matchers can be found in the [documentation](https://github.com/testing-library/jest-native#matchers) of the jest-native library.

The second very important Native Testing Library concept is firing events. We can fire an event in a provided node by using the [fireEvent](https://www.native-testing-library.com/docs/api-events) object's methods. This is useful for for example typing text in to a text field or pressing a button. Here is an example of how to test submitting a simple form:

```javascript
import React, { useState } from 'react';
import { Text, TextField, TouchableWithoutFeedback, View } from 'react-native';
import { render, fireEvent } from '@testing-library/react-native';

const Form = ({ onSubmit }) => {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');

  const handleSubmit = () => {
    onSubmit({ username, password });
  };

  return (
    <View>
      <View>
        <TextField
          value={username}
          onChangeText={(text) => setUsername(text)}
          placeholder="Username"
          testID="usernameField"
        />
      </View>
      <View>
        <TextField
          value={password}
          onChangeText={(text) => setPassword(text)}
          placeholder="Password"
          testID="passwordField"
        />
      </View>
      <View>
        <TouchableWithoutFeedback onPress={handleSubmit} testID="submitButton">
          <Text>Submit</Text>
        </TouchableWithoutFeedback>
      </View>
    </View>
  );
};

describe('Form', () => {
  it('calls function provided by onSubmit prop after pressing the submit button', () => {
    const onSubmit = jest.fn();
    const { getByTestId } = render(<Form />);

    fireEvent.change(getByTestId('usernameField'), 'kalle');
    fireEvent.change(getByTestId('passwordField'), 'password');
    fireEvent.press(getByTestId('usernameField'), 'submitButton');

    expect(onSubmit).toHaveBeenCalledTimes(1);

    // onSubmit.mock.calls[0] contains the arguments of the first call as an array
    expect(onSubmit.mock.calls[0]).toEqual([
      { username: 'kalle', password: 'password' },
    ]);
  });
});
```

## Handling dependencies in tests

## Exercises

- Testing SignIn component in the application

## Additional resources

- https://github.com/jondot/awesome-react-native
- https://callstack.github.io/react-native-paper/
- https://styled-components.com/docs/basics#react-native

## Extending our application

## Exercises

- Single repository view
- Repository's reviews view
- Review form
- Sign up form
- Repository list sorting (optional)
- Repository list filtering (optional)

## Infinite scrolling

- https://reactnative.dev/docs/flatlist
- https://www.apollographql.com/docs/react/data/pagination/#cursor-based
- Example of adding infinite scrolling to the repository list

## Exercises

- Repository review list infinite scrolling
