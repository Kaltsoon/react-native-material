# 1. Introduction to React Native

##  What is React Native

- General info
- Benefits of React Native
- What are the other similar options and how they compare to React Native
  - https://www.electronjs.org/
  - https://flutter.dev/

##  About this part

- Structure of the part
- What kind of application we are building during the part
- Prerequisities
  - Parts 1, 2, 5, 7, 8

## Setting up the development environment

- https://docs.expo.io/versions/latest/
- Development with Expo mobile application
- Android or iOS emulator
  - https://docs.expo.io/versions/v37.0.0/workflow/android-studio-emulator/
  - https://docs.expo.io/versions/v37.0.0/workflow/ios-simulator/

## Eslint

## Debugging with Expo

- https://docs.expo.io/versions/v37.0.0/workflow/debugging/

## Exercises

- Setup Expo project and play around with the development environment 
- Setup eslint

# 2. React Native basics

## Core components

- https://reactnative.dev/docs/intro-react-native-components

## Exercises

- Create a simple repository list with mock data

## Style

- Styles in general
  - https://github.com/vhpoet/react-native-styling-cheat-sheet
  - Inline styles, `StyleSheet.create`, conditional styles
- Theming
  - Examples: https://getbootstrap.com/docs/4.4/getting-started/theming/ and https://material-ui.com/customization/theming/
- Custom `<Text />` component with variants
- Flexbox
  - https://css-tricks.com/snippets/css/a-guide-to-flexbox/

## Exercises

- `<AppBar />` component
- Fancy repository list

## Routing

- https://reacttraining.com/react-router/native/guides/quick-start

## Exercises

- Setup routes for repository list and sing in views and add links to app bar

## Forms

- https://jaredpalmer.com/formik/

## Exercises

- Sign in form

## Platform specific code

- `Platform.OS` & `Platform.select`
- Platform specific imports (`.ios.js` & `.android.js`)

## Exericses

- Use platform specific sans-serif font in the theme configuration (iOS Arial, Android Ubuntu)

# 3. Communicating with server

##  HTTP requests

- https://reactnative.dev/docs/network

## GraphQL and Apollo client

## Exercises

- Repository list query

## Environment variables

- https://github.com/zetachang/react-native-dotenv

## Exercises

- `APOLLO_URI` from environment variable and configuration module

## Storing data in user's device

- AsyncStorage, https://github.com/react-native-community/async-storage

## Exercises 

- `AuthStorage` service for storing access token
- Sign in mutation
- Sign out 

# 4. Testing and extending our application

## Testing React Native applications

- https://github.com/callstack/react-native-testing-library

## Exercises

- Testing component X in the application

## Additional resources

- https://github.com/jondot/awesome-react-native
- https://callstack.github.io/react-native-paper/
- https://styled-components.com/docs/basics#react-native

## Extending our application

## Exercises

- Sign up form
- Single repository view
- Review form
- Repository list sorting (optional)
- Repository list filtering (optional)

## Implementing infine scrolling

- https://reactnative.dev/docs/flatlist
- https://www.apollographql.com/docs/react/data/pagination/#cursor-based
- Example of adding infinite scrolling to the repository list

## Exercises

- Repository review list infinite scrolling
