# Testing and extending our application

Now that we have established a good foundation for our project, it is time to start expanding it. In this section you can put to use all the React Native knowledge you have gained so far. Along with expanding our application we will cover some new areas, such as testing, and additional resources.

## Testing React Native applications

To start testing code of any kind, the first thing we need is a testing framework, which we can use to run a set of test cases and inspect their results. For testing a JavaScript application, [Jest](https://jestjs.io/) is a popular candidate for such testing framework. For testing an Expo based React Native application with Jest, Expo provides a set of Jest configuration in a form of [jest-expo](https://github.com/expo/expo/tree/master/packages/jest-expo) preset. Let's get started by installing Jest and jest-expo packages:

```shell
npm install jest jest-expo --save-dev
```

To use the jest-expo preset, we need to add the following Jest configuration to the _package.json_ file along with the _test_ script:

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

To see that the setup is working, create a directory <i>\_\_tests\_\_</i> in the _src_ directory and create a file _example.js_. In that file, add this simple test:

```javascript
describe('Example', () => {
  it('works', () => {
    expect(1).toBe(1);
  });
});
```

Now, let's run our example test by running `npm test`. 

- https://github.com/callstack/react-native-testing-library

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
