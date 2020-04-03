# React Native basics

## Core components

- https://reactnative.dev/docs/intro-react-native-components
- HTML elements and their React Native counterparts with example code
  - Basic building blocks such as `<div />` and `<View />`, https://reactnative.dev/docs/view
  - Elements that contain text such as `<strong />` and `<Text />`, https://reactnative.dev/docs/text
    - Difference: only `<Text />` component can contain text
  - `<input />` and `<TextInput /> `, https://reactnative.dev/docs/textinput
  - Elements that respond to press events such as `<button />` and `<TouchableWithoutFeedback />`, https://reactnative.dev/docs/touchablewithoutfeedback
    - Difference: Most HTML elements respond to click events, however in React Native specific components need to be used to capture e.g. press events. Always check the [API documentation](https://reactnative.dev/docs/components-and-apis) for core component's available events
  - https://reactnative.dev/docs/components-and-apis
- iOS and Android specific core components
- Snack examples

## Exercises

- Create a simple repository list with mock data

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
