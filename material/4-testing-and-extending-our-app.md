# Testing and extending our application

Now that we have established a good foundation for our project, it is time to start expanding it. In this section you can put to use all the React Native knowledge you have gained so far. Along with expanding our application we will cover some new areas, such as testing, and additional resources.

## Testing React Native applications

To start testing code of any kind, the first thing we need is a testing framework, which we can use to run a set of test cases and inspect their results. For testing a JavaScript application, [Jest](https://jestjs.io/) is a popular candidate for such testing framework. For testing an Expo based React Native application with Jest, Expo provides a set of Jest configuration in a form of [jest-expo](https://github.com/expo/expo/tree/master/packages/jest-expo) preset. In order to use Eslint in the Jest's test files, we also need the [https://www.npmjs.com/package/eslint-plugin-jest](https://www.npmjs.com/package/eslint-plugin-jest) plugin for Eslint. Let's get started by installing the dependencies:

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

Now that we have managed to set up Jest and run a very simple test, it is time find out how to test components. As we know, testing components requires a way to serialize a component's render output and simulate firing different kind of events, such as pressing a button. For these purposes there is the [Testing Library](https://testing-library.com/docs/intro) family, which provides libraries for testing user interface components in different platforms. All of these libraries share similar API for testing user interface components in a user-centric way. One of these libraries is the [Native Testing Library](https://testing-library.com/docs/native-testing-library/intro), which we will use for testing our React Native components. Before getting into the details, let's install the package:

```shell
npm install --save-dev @testing-library/react-native
```

- https://testing-library.com/docs/native-testing-library/intro

## Exercises

- Testing component X in the application

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
