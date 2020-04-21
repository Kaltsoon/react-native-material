# Communicating with server

So far we have implemented features to our application without any actual server communication. For example the rated repositories list we have implemented uses mock data and the sign in form doesn't send the user's credentials to any authorization end point.

<!-- TODO: fullstack/rate-repository-api repo -->

Soon we we will learn how to communicate with a server in our application. Before we get to that, we need a server to communicate with. For this purpose we have a completed server implementation in the [https://github.com/Kaltsoon/rate-repository-api](rate-repository-api) repository. The _rate-repository-api_ server fulfills all our application's API needs during this part. It uses [SQLite](https://www.sqlite.org/index.html) database which doesn't need any set up and provides a Apollo GraphQL API along with a few REST API end points.

Before heading further into the material, set up the _rate-repository-api_ server by following the set up instructions in the repository's [README](https://github.com/Kaltsoon/rate-repository-api/blob/master/README.md). Note that if you are using an emulator for development it is recommended to run the server and the emulator _on the same computer_. This eases network requests considerably.

## HTTP requests

React Native provides [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) for making HTTP requests in our applications. In addition React Native also supports the good old [XMLHttpRequest API](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) which makes it possible to use third party libraries such as [Axios](https://github.com/axios/axios). These APIs are exactly the same as the ones in the browser environment and they are globally available without need of an import.

Users who have used both Fetch API and XMLHttpRequest API most likely agree that the Fetch API is easier to use and more modern. However, this doesn't mean that XMLHttpRequest API doesn't have its uses. For the sake simplicity, we will be only using the Fetch API in our examples.

Sending a HTTP requests using the Fetch API can be done using the `fetch` function. The first argument of the function is the URL of the resource:

```javascript
fetch('https://my-api.com/get-end-point');
```

The default request method is _GET_. The second argument of the `fetch` function is an options object, which you can use to for example to specify a different request method, request headers, or request body:

```javascript
fetch('https://my-api.com/post-end-point', {
  method: 'POST',
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    firstParam: 'firstValue',
    secondParam: 'secondValue',
  }),
});
```

Note that these URLs are made up and won't (most likely) send a response to your requests. In comparison to Axios, the Fetch API operates on a bit lower level. For example there isn't any request or response body serialization and parsing. This means that you have to for example set the _Content-Type_ header by yourself and use `JSON.stringify` method to serialize the request body.

The `fetch` function returns a promise which resolves a [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response) object. Note that error status codes such as 400 and 500 _are not rejected_ like for example in Axios. In case of a JSON formatted response we can parse the response body using the `Response.json` method:

```javascript
const fetchMovies = async () => {
  const response = await fetch('https://reactnative.dev/movies.json');
  const json = await response.json();

  return json;
};
```

For a more detailed introduction to the Fetch API, read the [Using Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch) article in the MDN web docs.

Next, let's try the Fetch API in practice. The _rate-repository-api_ server provides an end point for returning a paginated list of rated repositories. Once the server is running, you should be able to access the end point at [http://localhost:5000/api/repositories](http://localhost:5000/api/repositories). The data is paginated in a common [cursor based pagination format](https://graphql.org/learn/pagination/). The actual repository data is behind the _node_ key in the _edges_ array.

Unfortunately we can't access the server directly in our application by using the _http://localhost:5000/api/repositories_ URL. To make a request to this end point in our application we need to access the access the server using its IP address in its local network. To find out what it is, open the Expo development tools by running `npm start`. In the development tools you should be able to see an url starting with _exp://_ above the QR code:

![Development tools](images/10.png)

Copy the IP address between the _exp://_ and _:_, which is in this example _192.168.100.16_. Construct an URL in format _http://<IP_ADDRESS>:5000/api/repositories_ and open it in the browser. You should see the same response as you did with the _localhost_ URL.

Now that we know the end point's URL let's use actual server provided data in our rated repositories list. We are currently using mock data stored in the `repositories` variable. Remove the `repositories` variable and replace the usage of the mock data with this piece of code in the _RepositoryList.jsx_ file in the _components_ directory:

```javascript
import React, { useState, useEffect } from 'react';

// ...

const RepositoryList = () => {
  const [repositories, setRepositories] = useState();

  const fetchRepositories = async () => {
    // Replace the IP address part with your own IP address!
    const response = await fetch('http://192.168.100.16:5000/api/repositories');
    const json = await response.json();

    console.log(json);

    setRepositories(json);
  };

  useEffect(() => {
    fetchRepositories();
  }, []);

  // Get the nodes from the edges array
  const repositoryNodes = repositories
    ? repositories.edges.map((edge) => edge.node)
    : [];

  return (
    <FlatList
      data={repositoryNodes}
      // Other props
    />
  );
};

export default RepositoryList;
```

<!-- TODO: Viewiling logs linkki -->

We are using the React's `useState` hook to maintain the repository list state and the `useEffect` hook to to call the `fetchRepositories` function when the _RepositoryList_ component is mounted. We extract the actual repositories into the `repositoryNodes` variable and replace the previously used `repositories` variable in the _FlatList_ component's _data_ prop with it. Now you should be able to see actual server provided data in the rated repositories list.

It is usually a good idea to log the server's response to be able to inspect it as we did in the `fetchRepositories` function. You should be to see this log message in the Expo development tools if you navigate to your device's logs as we learned in the [Viewing logs]() section. If you are using the Expo's mobile app for development and the network request is failing make sure that the computer you are using to run the server and your phone iare _connected to the same Wi-Fi network_. If that's not possible either use an emulator in the same computer as the server is running in or set up a tunnel to the localhost for example using [Ngrok](https://ngrok.com/).

The current data fetching code in the _RepositoryList_ component could do some refactoring. For instance, the component is aware of the network request's details such as the end point's URL. In addition, the data fetching code has lots of reuse potential. Let's refactor the component's code by extract the data fetching code into its own hook. Create a directory _hooks_ in the _src_ directory and in that _hooks_ directory create a file _useRepositories.js_ with the following content:

```javascript
import { useState, useEffect } from 'react';

const useRepositories = () => {
  const [repositories, setRepositories] = useState();
  const [loading, setLoading] = useState(false);

  const fetchRepositories = async () => {
    setLoading(true);

    // Replace the IP address part with your own IP address!
    const response = await fetch('http://192.168.100.16:5000/api/repositories');
    const json = await response.json();

    setLoading(false);
    setRepositories(json);
  };

  useEffect(() => {
    fetchRepositories();
  }, []);

  return { repositories, loading, refetch: fetchRepositories };
};

export default useRepositories;
```

Now that we have a clean abstraction for fetching the rated repositories, let's use the _useRepositories_ hook in the _RepositoryList_ component:

```javascript
import React from 'react';

// ...

import useRepositories from '../hooks/useRepositories';

const RepositoryList = () => {
  const { repositories } = useRepositories();

  const repositoryNodes = repositories
    ? repositories.edges.map((edge) => edge.node)
    : [];

  return (
    <FlatList
      data={repositoryNodes}
      // Other props
    />
  );
};

export default RepositoryList;
```

That's it, now the _RepositoryList_ component is no longer aware of the way the repositories are acquired. Maybe in the future we will acquire them through a GraphQL API instead of a REST API. We will see what happens.

## GraphQL and Apollo client

In [part 8](https://fullstackopen.com/en/part8) we learned about GraphQL and how to make GraphQL queries to a Apollo Server using the [Apollo Client](https://www.apollographql.com/docs/react/) in React applications. Good news is that, we can use the Apollo Client in a React Native application exactly as we would with a React web application.

As mentioned earlier, the _rate-repository-api_ server provides a GraphQL API which is implemented with Apollo Server. Once the server is running, you can access the [GraphQL Playground](https://www.apollographql.com/docs/apollo-server/testing/graphql-playground/#gatsby-focus-wrapper) at [http://localhost:5000/graphql](http://localhost:5000/graphql). GraphQL Playground is a development tool for making GraphQL queries and inspecting the GraphQL APIs schema and documentation. If you need to make a query in your application _always_ test it with the GraphQL Playground first before implementing it in the code. It is much easier to debug possible problems in the query in the GraphQL Playground than in the application. If you are uncertain what the available queries are or how to use them, click the _DOCS_ tab to open the documentation:

![GraphQL Playground](images/11.png)

In our React Native application, we will be using the [Apollo Boost](https://www.npmjs.com/package/apollo-boost) library wich is a zero-config way to start using Apollo Client. As a React integration, we will be using the [@apollo/react-hooks](https://www.apollographql.com/docs/react/api/react-hooks/) library, which provides hooks such as [useQuery](https://www.apollographql.com/docs/react/api/react-hooks/#usequery) and [useMutation](https://www.apollographql.com/docs/react/api/react-hooks/#usemutation) for using the Apollo Client. Let's get started by installing the dependencies:

```shell
npm install apollo-boost @apollo/react-hooks graphql
```

Next, let's create a utility for creating the Apollo Client with the required configuration. Create a _utils_ directory in the _src_ directory and in that _utils_ directory create a file _apolloClient.js_. In that file configure the Apollo Client to connect to the Apollo Server:

```javascript
import ApolloClient from 'apollo-boost';

const createApolloClient = () => {
  return new ApolloClient({
    // Replace the IP address part with your own IP address!
    uri: 'http://192.168.100.16:5000/graphql',
  });
};

export default createApolloClient;
```

The URL used to connect to the Apollo Server is otherwise the same as the one you used with the Fetch API expect the path is _/graphql_. Lastly, we need to provide the Apollo Client using the [ApolloProvider](https://www.apollographql.com/docs/react/api/react-hooks/#apolloprovider) context. We will add it to the _App_ component in the _App.js_ file:

```javascript
import React from 'react';
import { NativeRouter } from 'react-router-native';
import { ApolloProvider } from '@apollo/react-hooks';

import Main from './src/components/Main';
import createApolloClient from './utils/apolloClient';

const apolloClient = createApolloClient();

const App = () => {
  return (
    <NativeRouter>
      <ApolloProvider client={apolloClient}>
        <Main />
      </ApolloProvider>
    </NativeRouter>
  );
};

export default App;
```

It is up to you how to organize the GraphQL related code. It is recommended to define queries, mutations and fragments, and possible other entities in their owns files. Here is an example of a structure you can use to get started:

![GraphQL structure](images/12.png)

You can import the [gql](https://www.apollographql.com/docs/apollo-server/api/apollo-server/#gql) template literal tag used to define GraphQL queries from the Apollo Boost library like this:

```javascript
import { gql } from 'apollo-boost';

const GET_REPOSITORIES = gql`
  ${/* ... */}
`;
```

## Evolving the structure

Once our application grows larger there might times when certain files grow too large to manage. For example we have component _A_ which renders components _B_ and _C_. All these components are defined in a file _A.jsx_ in a _components_ directory. We would like to extract components _B_ and _C_ in to their own files _B.jsx_ and _C.jsx_ without major refactors. We have two options:

1. Create files _B.jsx_ and _C.jsx_ in the _components_ directory. This results to the following structure:

```
components/
  A.jsx
  B.jsx
  C.jsx
  ...
```

2. Create a directory _A_ in the _components_ directory and create files _B.jsx_ and _C.jsx_ there. To avoid breaking components that import the _A.jsx_ file, move the _A.jsx_ file to the _A_ directory and rename it to _index.jsx_. This results to the following structure:

```
components/
  A/
    B.jsx
    C.jsx
    index.jsx
  ...
```

The first option is fairly decent, however if components _B_ and _C_ are not reusable outside the component _A_, it is useless to bloat the _components_ directory by adding them as separate files. The second option is quite modular and doesn't break any imports because importing path such as _./A_ will match both _A.jsx_ and _A/index.jsx_.

## Exercises

We want to replace the Fetch API implementation in the _useRepositories_ hook with a GraphQL query. Open the GraphQL Playground at [http://localhost:5000/graphql](http://localhost:5000/graphql) and open to documentation by clicking the _DOCS_ tab. Look up the _repositories_ query. The query has some arguments, however all of these are optional so you don't need to specify them. In the GraphQL Playground form a query for fetching the repositories with the fields you are currently displaying in the application. The result will be paginated and it contains the up to first 30 results by default. For now, you can ignore the pagination entirely.

Once the query is working in the GraphQL Playground, use it to replace the Fetch API implementation in the _useRepositories_ hook. This can be achieved using the [useQuery](https://www.apollographql.com/docs/react/api/react-hooks/#usequery) hook. The `gql` template literal tag can be imported from the Apollo Boost as instructed earlier. Consider using the structure recommended ealier for the GraphQL related code. To avoid future caching issues, use the _cache-and-network_ [fetch policy](https://www.apollographql.com/docs/react/api/react-apollo/#optionsfetchpolicy) in the query. It can be used with the _useQuery_ hook like this:

```javascript
useQuery(MY_QUERY, {
  fetchPolicy: 'cache-and-network',
  // Other options
});
```

This change in the _useRepositories_ hook should not affect the _RepositoryList_ component in any way.

## Environment variables

Every application will most likely run in more than one environment. Two obvious candidates for these environments are the development environment and the production environment. Out of these two, the development envinroment is the one we are running the application right now. Different environments usually have different depencies, for example server we are developing locally might use a local database where as server that is deployed to the production environment uses the production database. To make the code environment independant we need to parametrize these dependencies. At the moment we are using one very environment dependant hard coded value in our application: the url of the server.

We have previously learned that we can provided running programmes with environment variables. These variables can be defined in the command line or using environment configuration files such as _.env_ files and third party libraries such as Dotenv. Unfortunately, React Native doesn't have direct support for environment variables. However, we can use the [Babel](https://babeljs.io/) compiler to inject desired environment variables _at build time_. These variables can be defined in environment specific _.env_ files.

Babel preset [react-native-dotenv](https://github.com/zetachang/react-native-dotenv) lets you import environment variables from a _.env_ file in React Native without any native code integration. Start by installing the dependency:

```shell
npm install react-native-dotenv --save-dev
```

Once installed, we can use the preset in the Babel compiler by including it in the configuration in the _babel.config.js_ file which is located in the root directory of your project:

```javascript
module.exports = function(api) {
  api.cache(true);
  return {
    presets: ['babel-preset-expo', 'module:react-native-dotenv'],
  };
};
```

The way the preset works is that the environment variables are defined in _.env_ files. The _.env_ file contains development enviroment related variables and _.env.production_ production environment related variables. Add a file _.env_ in the root directory of your project with the following content:

```
ENV=development
```

Similarly, create a file _.env.production_ with the following content:

```
ENV=production
```

These variables can be accessed in the code by importing them from the _react-native-dotenv_ module. Let's try this by logging the _ENV_ variable in the _App_ component:

```javascript
import React from 'react';
import { NativeRouter } from 'react-router-native';
import { ApolloProvider } from '@apollo/react-hooks';
import { ENV } from 'react-native-dotenv';

import Main from './src/components/Main';
import createApolloClient from './utils/apolloClient';

const apolloClient = createApolloClient();

const App = () => {
  console.log(ENV);

  return (
    <NativeRouter>
      <ApolloProvider client={apolloClient}>
        <Main />
      </ApolloProvider>
    </NativeRouter>
  );
};

export default App;
```

You should see the value of the _ENV_ variable in the logs. You can remove the log message and the import, once you have succesfully logged the variable's value. Note that it is _never_ a good idea to put sensitive data into the React Native environment variables. The reason for this is that once a user has downloaded your application, they can at least theoretically reverse engineer your application and figure out the sensitive data you have stored into the code.

## Exercise

Instead of the hard coded Apollo Server's URL, use an environment variable when initializing the Apollo Client. You can name the variable for example to _APOLLO\_URI_. For now, it is enough that you only define the variable in the _development environment_, because we aren't aware of the production URL of the Apollo Server.

## Storing data in user's device

- AsyncStorage, https://github.com/react-native-community/async-storage

## Exercises

- `AuthStorage` service for storing access token
- Sign in mutation
- Sign out
