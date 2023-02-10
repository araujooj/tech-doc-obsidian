# isFetching vs isLoading

`isFetching` and `isLoading` are two important states provided by React Query to manage the status of a query or a mutation. Understanding the difference between these two states is crucial to effectively use React Query and provide a smooth user experience.

`isFetching` is a state that indicates whether a query is currently being executed. In other words, it means that the async function associated with the query has not yet resolved. This state is useful for indicating to the user that the application is currently fetching data from the server and that they need to wait for the response. When `isFetching` is `true`, the component using the query should display a loading indicator.

`isLoading` is a state that indicates whether a query is currently in a loading state. This state is more comprehensive than `isFetching` and takes into account two factors: (1) the absence of cached data and (2) the value of  `isFetching`. In other words, `isLoading` is `true` when there is no cached data and `isFetching` is `true`.

When a query is `isLoading`, it means that the component using the query should display a loading indicator. This is because the query has either not started yet or it is currently being executed, and the application is waiting for a response from the server.

In conclusion, `isFetching` and `isLoading` are two different states provided by React Query to manage the status of a query or a mutation. By using these states, you can effectively manage the loading state of an application and ensure that the user is informed about the status of the query. This helps provide a smooth and responsive user experience, even when the application is fetching data from the server.

```jsx
import { useQuery } from '@tanstack/react-query';

const fetchPosts = async () => {
  const response = await fetch('https://jsonplaceholder.typicode.com/posts');
  return response.json();
};

const PostsList = () => {
  const { isLoading, isFetching, data, error } = useQuery(['posts'], fetchPosts);

  if (error) {
    return <div>An error has occurred: {error.message}</div>;
  }

  if (isLoading) {
    return <div>Loading...</div>;
  }

  if (isFetching) {
    return <div>Fetching new data...</div>;
  }

  return (
    <ul>
      {data.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
};
```

In this example, `isLoading` is `true` when there is no cached data and the fetching of data has not started yet. In this case, the component displays a "Loading..." message to inform the user that the application is waiting for data.

`isFetching` is `true` when the fetching of data has started, but the async function associated with the query has not yet resolved. In this case, the component displays a "Fetching new data..." message to inform the user that the application is currently fetching data from the server and that they need to wait for the response.

In the case where an error has occurred, the component displays an error message to inform the user that something went wrong.

# isError

The `isError` state in React Query allows you to determine whether an error has occurred during the execution of a query or a mutation. This state is a boolean value that is set to `true` if there was an error, and `false` otherwise.

The `error` object, on the other hand, contains information about the error that occurred. This object can be used to display more detailed error information to the user. For example, you could display the error message, status code, or other details associated with the error.

Here is an example of how you can use the `isError` and `error` states in your React component:

```jsx
import { useQuery } from '@tanstack/react-query';

const fetchPosts = async () => {
  const response = await fetch('https://jsonplaceholder.typicode.com/posts');
  return response.json();
};

const PostsList = () => {
  const { isLoading, isFetching, isError, error, data } = useQuery(['posts'], fetchPosts);

  if (isError) {
    return <div>An error has occurred: {error.message}</div>;
  }

  if (isLoading) {
    return <div>Loading...</div>;
  }

  if (isFetching) {
    return <div>Fetching new data...</div>;
  }

  return (
    <ul>
      {data.map(post => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  );
};

```

# Retries

React Query will automatically retry a failed query or mutation up to three times by default. This is done to ensure that the request has a better chance of succeeding if it initially fails due to network issues or other temporary problems.

However, you can modify this behavior by specifying the `retry` option when calling `useQuery`. For example, if you want to retry a query only once, you can set the `retry` option to `1`:

```js
const { data, error } = useQuery(['posts'], fetchPosts, { retry: 1 });
```

In some cases, you may also want to specify a custom retry logic. You can do this by passing a function as the `retry` option. The function will receive the previous attempts as an argument, and it should return the number of remaining retries. For example, you can use a custom retry logic to only retry if the error code is specific:

```js
const retry = (attempts) => {
  if (attempts[attempts.length - 1].error.code === '429') {
    return 1;
  }

  return 0;
};

const { data, error } = useQuery(['posts'], fetchPosts, { retry: retry });
```

# onError

React Query also provides a way to handle errors that occur during a query or mutation. You can use the `onError` option to specify a callback function that will be called when an error occurs. The function will receive the error as an argument.

For example, you can use the `onError` option to display an error message to the user:

```js
const onError = (error) => {
  console.error(error);
  alert('An error occurred while fetching the data. Please try again later.');
};

const { data, error } = useQuery('posts', fetchPosts, { onError });
```

In this example, the `onError` callback function is called whenever an error occurs, and it logs the error to the console and displays an error message to the user using an `alert` function.

Note that the `onError` option is only called if an error occurs, and it does not affect the retry behavior of React Query. If you want to modify the retry behavior, you can use the `retry` option as described in the previous paragraph.