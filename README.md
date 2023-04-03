## Here's an example of how you can use axios with Promise.all to fetch data in parallel and also support cancellation:

```java
Aimport axios from "axios";
import { useQuery } from "react-query";

const fetchData = async (url, signal) => {
  const response = await axios.get(url, { signal });
  return response.data;
};

const useParallelFetch = (urls) => {
  const abortController = new AbortController();
  const { signal } = abortController;

  const fetchers = urls.map((url) => fetchData(url, signal));

  const promise = Promise.all(fetchers);

  promise.cancel = () => {
    abortController.abort();
  };

  return useQuery(["parallelFetch", urls], () => promise, {
    refetchOnWindowFocus: false,
    retry: false,
  });
};
```
# mapokemon
