# AdjustedFunctoolsCache

By default, the Functools Cache looks at all arguments of a wrapped function for results to remember. For example, when the function 'func()' is wrapped by the functools cache decorator:

```
@lru_cache(maxsize=None)  
def func(value, list, dummy_string):
  ...
```

All calls to f with the same value AND list AND dummy_string will be extracted from the cache. i.e. Once:
  ```
  func(5, [], "dummy")
  ```
is called, the output will be stored in memory (cached). Calling 
  ```
  func(5, [], "dummy")
  ```
again will not re-run the function, instead it will just check the result it got from the cache previously.


However, what if we only wanted to only check a subset of function parameters for caching? For example:
  ```
  func(3, [], "HELLO")
  ```
and
  ```
  func(3, [], "GOODBYE")
  ```
Would be cached as different results, even if we might not want them to be. 

With the new feature, by specifying the 'arg_num' parameter in the initial decorator, only that subset of the function arguments are looked at when caching. So now at setup, we can specify that we only want the first two arguments to be cached:

```
@lru_cache(maxsize=None, arg_num=2)  
def func(value, list, dummy_string):
  ...
```

Now:
  ```
  func(3, [], "HELLO")
  ```
and
  ```
  func(3, [], "GOODBYE")
  ```
Will be cached as having the same result!
