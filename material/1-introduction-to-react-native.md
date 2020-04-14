# Introduction to React Native

Traditionally developing native iOS and Android applications has required the developer to use platform specific programming language and development envinronment. For iOS development this means using Switch and for Android development using JVM based languages such as Java, Scala or Kotlin. Releasing an application for both these platforms technically requires to develop two separate applications with different programming languages. This requires lots of development resources.

One of the popular approaches to unify the platform specific development has been to utilize browser as the rendering engine. [Cordova](https://cordova.apache.org/) is one of the most popular platforms for building cross platform applications. It allows to develop multi platform applications using standard web technologies - HTML5, CSS3, and JavaScript. However, Cordova applications are running within a browser window in user's device. That is why the can not achieve the performance of native applications which utilize actual native user interface components.

[React Native](https://reactnative.dev/) is a framework for developing native Android and iOS applications using JavaScript and React. Using React Native allows us to bring all the familiar features of React such as JSX, components, props, state and hooks into native application development. On top of that we are able to utilize many familiar libraries in React ecosystem such as [react-redux](https://react-redux.js.org/), [react-apollo](https://github.com/apollographql/react-apollo), [react-router](https://reacttraining.com/react-router/core/guides/quick-start) and many more.

The speed of development and gentle learning curve for developers familiar with React are one of the most important benefits of React Native. Here's a motivational quote from Coinbase's article [Onboarding thousands of users with React Native](https://blog.coinbase.com/onboarding-thousands-of-users-with-react-native-361219066df4) on the benefits of React Native:

> If we were to reduce the benefits of React Native to a single word, it would be “velocity”. On average, our team was able to onboard engineers in less time, share more code (which we expect will lead to future productivity boosts), and ultimately deliver features faster than if we had taken a purely native approach.

##  About this part

During this part we will learn how to build an actual React Native application from bottom up. We will learn concepts such what are React Native's core components, how to create beautiful user interfaces, how to communicate with a server and how to test a React Native application.

We will be building an application for rating [GitHub](https://github.com/) repositories. Our application will have features such as, sorting and filttering rated repositories, registering an user, logging in and creating a rating for a repository. The back end for the application will be provided for us so that we can solely focus on the React Native development.

<!--
TODO: application gif
-->

All the exercises in this part need to submitted into _a single GitHub repository_ which will eventually contain the entire source code of your application. There will model solutions available for each section of this part which you can use to fill in incomplete submissions. This part is structured based on the idea that you develop your application as you progress in the material. So _do not_ wait until the exercises to start the development. Instead develop your application in the same pace as the material progresses.

<!-- TODO: palautussovellus info -->

This part will heavily rely on concepts of covered in the previous parts. Before starting this part you will need basic knowledge of JavaScript, React and GraphQL. Deep knowledge of server side development is not required and all the server side code is provided for you. However, we will be making network requests from your React Native applications for example using GraphQL queries. The recommended parts to complete before this part are [part 1](/en/part1), [part 2](/en/part2), [part 5](/en/part5), [part 7](/en/part7) and [part 8](/en/part8).

## Initializing the application

To get started with our application we need to set up our development environment. We have learned from previous parts that there are useful tools for setting up React applications quickly such as Create React App. Luckily React Native has these kind of tools as well.

For the development of our application we will be using [Expo](https://docs.expo.io/versions/latest/). Expo is a platform that eases the setup, development, building, and deployment of React Native applications. Let's get started with Expo by installing the _expo-cli_ command line interface:

```shell
npm install --global expo-cli
```

Next we can initialize our project within a _rate-repository-app_ directory:

```shell
expo init rate-repository-app
```

After running this command Expo will ask you choose a template for the project. Within the _Managed workflow_ section, choose the _blank_ option (the one with description "a minimal app as clean as an empty canvas"). 

Now that our application has been initialized, open the created _rate-repository-app_ directory with an editor such as [Visual Studio Code](https://code.visualstudio.com/). The structure should be more or less the following:

![Project structure](images/1.png)

We might spot some familiar files and directories such as _package.json_ and _node\_modules_. On top of those the most relevant files are _app.json_ file which contains Expo related configuration and _App.js_ which is the root component of our application. _Do not_ rename or move the _App.js_ file because by default Expo imports it to [register the root component](https://docs.expo.io/versions/latest/sdk/register-root-component/).

Let's have look at _scripts_ section of the _package.json_ file which has the following scripts:

```javascript
{
  // ...
  "scripts": {
    "start": "expo start",
    "android": "expo start --android",
    "ios": "expo start --ios",
    "web": "expo start --web",
    "eject": "expo eject"
  },
  // ...
}
```

Running the script `npm start` starts the [Metro bundler](https://facebook.github.io/metro/) which is a JavaScript bundler for React Native. It can be described as the [Webpack](https://webpack.js.org/) of the React Native ecosystem. In addition to the Metro bundler, _Expo DevTools_ should be open within a browser window at [http://localhost:19002]. Expo DevTools is a useful tool for viewing the application logs and starting the application in an emulator or in Expo's mobile application. We will get to emulators and Expo's mobile application soon, but first let's start our application within a web browser by clicking the _Run in web browser_ link:

![Expo DevTools](images/2.png)

After clicking the link we should soon see the text defined in the _App.js_ file within a browser window. Open the _App.js_ file with an editor and make a small change into the text within the _Text_ component. After saving the file you should be able to see that the changes you have made into the code are visible within the browser window.
 
## Setting up the development environment

We have had the first glance of our application using Expo's browser view. Although the browser view is quite usable, it is still a quite poor simulation of the native environment. Let's have a look at the alternatives we have regarding the development envinronment.

Android and iOS devices such as tables and phones can be emulated in computers using specific _emulators_. This is very useful for developing native applications. MacOS users can use both Android and iOS emulators with their computers. Users of other operating systems such as Linux or Windows have to settle for Android emulators. Next, depending on your operating system follow one of these instructions on setting up an emulator:

- [Set up Android emulator with Android Studio](https://docs.expo.io/versions/v37.0.0/workflow/android-studio-emulator/) (any operating system)
- [Set up iOS simulator with Xcode](https://docs.expo.io/versions/v37.0.0/workflow/ios-simulator/) (macOS operating system)

After you have set up the emulator and it is running, start the Expo development tools as we did before, by running `npm start`. Depending on the emulator you are running either click the _Run on Android device/emulator_ or _Run on iOS simulator_ link. After clicking the link, Expo should connect to the emulator and you should eventually see the application in your emulator. Be patient, this might take a while.

In addition to emulators, there is one extremely useful way to develop React Native applications with Expo, the Expo mobile app. With Expo mobile app you can preview your application using your actual mobile device, which provides a bit more concrete development experience compared to emulators. To get started, install the Expo mobile app by following the instructions the [Expo's documentation](https://docs.expo.io/versions/latest/get-started/installation/#2-mobile-app-expo-client-for-ios).

When the Expo mobile app is finished installing, open it up. Next, if the Expo development tools is not already running, start it by running `npm start`. In the bottom left corner of the development tools you should be able to see a QR code. Within the Expo mobile app, press _Scan QR Code_ and scan the QR code displayed in the development tools. The Expo mobile app should start building the JavaScript bundle and after it is finished you should be able to see your application. Now, every time you want to reopen your application in the Expo mobile app, you should be able to access the application without scanning the QR code by pressing it in the _Recently opened_ list in the _Projects_ view.

## Exercise 1.1.

Initialize your application with Expo command line interface and set up the development environment either using an emulator or Expo's mobile app. It is preferred to try both and find out which development environment is the most suitable for you. The name of the application is not that relevant, you can for example go with _rate-repository-app_.

To submit this exercise and all the future exercises you need to [create a new GitHub repostitory](https://github.com/new). The name of the repository can be for example the name of the application you initialized with `expo init`. Now that the repository is created, run `git init` within your application's root directory to make sure that the directory is initialized as a Git repository. Next, to add the created repository as the remote run `git remote add origin git@github.com:<YOUR_GITHUB_USERNAME>/<NAME_OF_YOUR_REPOSITORY>.git` (remember to replace the placeholder values in the command). Finally, just commit and push your changes into the repository and you are all done.

## Eslint

Now that we are somewhat familiar with the development environment let's enhance our development experience even further by configuring a linter. We will be using [eslint](https://eslint.org/) which is already familiar to us from the previous parts. Let's get started by installing the dependencies:

```shell
npm install --save-dev eslint babel-eslint eslint-plugin-react
```

Next, let's add a eslint configuration into a _.eslintrc_ file into the _rate-repository-app_ directory:

```javascript
{
  "plugins": ["react"],
  "extends": ["eslint:recommended", "plugin:react/recommended"],
  "parser": "babel-eslint",
  "env": {
    "browser": true
  },
  "rules": {
    "react/prop-types": "off",
  }
}
```

And finally, let's add a `lint` script to the _package.json_ file to check the linting rules in specific files:

```javascript
{
  // ...
  "scripts": {
    // ...
    "lint": "eslint ./src/**/*.{js,jsx} App.js --no-error-on-unmatched-pattern"
  },
  // ...
}
```

Now we can check that the linting rules are obeyd in JavaScript files within the _src_ directory and in the _App.js_ file by running `npm run lint`. We will be adding our future code into the _src_ directory but because we haven't added any files there yet, we need the `no-error-on-unmatched-pattern` flag. Also if possible integrate eslint with your editor. If you are using Visual Studio Code you can do that by, going to the extensions section and checking that the Eslint extension is installed and enabled:

![Visual Studio Code Eslint extensions](images/3.png)

The provided eslint configuration contains only the basis for the configuration. Feel free to improve the configuration and add new plugins if you feel like it. 

## Exercise 1.2.

Set up eslint in your project so that you can perfom linter checks by running `npm run lint`. 

## Debugging with Expo

<!-- TODO -->
https://docs.expo.io/versions/v37.0.0/workflow/debugging/
