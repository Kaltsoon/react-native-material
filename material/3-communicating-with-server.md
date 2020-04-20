# Communicating with server

So far we have implemented features to our application without any actual server communication. For example the rated repositories list we have implemented uses mock data and the sign in form doesn't send the user's credentials to any authorization end point.

<!-- TODO: fullstack/rate-repository-api repo -->

Soon we we will learn how to communicate with a server in our application. Before we get to that, we need a server to communicate with. For this purpose we have a completed server implementation in the [https://github.com/Kaltsoon/rate-repository-api](rate-repository-api) repository. The _rate-repository-api_ server fulfills all our application's API needs during this part. It uses [SQLite](https://www.sqlite.org/index.html) database which doesn't need any set up and provides a Apollo GraphQL API along with a few REST API end points.

Before heading further into the material, set up the _rate-repository-api_ server by following the set up instructions in the repository's [README](https://github.com/Kaltsoon/rate-repository-api/blob/master/README.md). Note that if you are using an emulator for development it is recommended to run the server and the emulator _on the same computer_. This eases network requests considerably.

## HTTP requests

React Native provides [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) for making HTTP requests in our applications. In addition React Native also supports the good old [XMLHttpRequest API](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) which makes it possible to use third party libraries such as [Axios](https://github.com/axios/axios). These APIs are exactly the same as the one in the browser environment and they are globally available without need of an import.

Users who have used both Fetch API and XMLHttpRequest API most likely agree that the Fetch API is easier to use and more modern. However, this doesn't mean that XMLHttpRequest API doesn't have its uses. For the sake simplicity, we will be only using the Fetch API in our examples.

Sending a HTTP requests using the Fetch API can be done using the _fetch_ function. The first argument of the function is the URL of the resource:

```javascript
fetch('https://my-api.com/get-end-point');
```

The default request method is _GET_. The second argument of the _fetch_ function is an options object, which you can use to for example specify a different request method, request headers, or request body:

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

Now that we know the end point's URL let's use actual server provided data in our rated repositories list. We are currently using mock data stored in the `repositories` variable. Remove the `repositories` variable and replace that mock data with this piece of code in the _RepositoryList.jsx_ file in the _components_ directory:

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

We are using the React's _useState_ hook to maintain the repository list state and the _useEffect_ hook to to call the _fetchRepositories_ function when the _RepositoryList_ component is mounted. We extract the actual repositories into the `repositoryNodes` variable and replace the previously used `repositories` variable in the _FlatList_ component's _data_ prop with it. Now you should be able to see actual server provided data in the rated repositories list.

It is usually a good idea to log the server's response to be able to inspect it as we did in the _fetchRepositories_ function. You should be to see this log message in the Expo development tools if you navigate to your device's logs as we learned in the [Viewing logs]() section. If you are using the Expo's mobile app for development and the network request is failing make sure that the computer you are using to run the server and your phone iare _connected to the same Wi-Fi network_. If that's not possible either use an emulator in the same computer as the server is running in or set up a tunnel to the localhost for example using [Ngrok](https://ngrok.com/).

The current data fetching code in the _RepositoryList_ component could do some refactoring. For instance, the component is aware of the network request's details such as the end point's URL. In addition, the data fetching code has lots of reuse potential. Let's refactor the component's code by extract the data fetching code into its own hook. Create a directory _hooks_ in the _src_ directory and in that _hooks_ directory create a file _useRepositories.js_ with the following content:

```javascript
import React, { useState, useEffect } from 'react';

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

Now that we have a clean abstraction for fetching the rated repositories, let's use it in the _RepositoryList_ component:

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

- Reference: https://fullstackopen.com/osa8/react_ja_graph_ql
- Setting up the Apollo client in the application

## Exercises

- Repository list query

## Environment variables

- https://github.com/zetachang/react-native-dotenv

## Exercises

- `APOLLO_URI` from environment variable

## Storing data in user's device

- AsyncStorage, https://github.com/react-native-community/async-storage

## Exercises

- `AuthStorage` service for storing access token
- Sign in mutation
- Sign out
