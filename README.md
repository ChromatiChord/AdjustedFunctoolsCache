# ChromatiCache
 
Adds a *'key'* parameter to the  *functools* python cache module.  

By default, the Cache functionality of the *functools* python module looks at all arguments of a decorated function to determine if the result should be cached or extracted from the cache. 
For example, when the function 'func()' is wrapped by the functools cache decorator:

```
@functools.lru_cache(maxsize=None)  
def func(int_value, list_value, string_value):
  ...
```

Only calls to func() with the same 'int_value' **and** 'list_value' AND 'string_value' will be extracted from the cache.  
i.e. Once:
  ```
  func(5, [], "dummy")
  ```
is called, the output will be stored in memory (cached).  
Calling:
  ```
  func(5, [], "dummy")
  ```
again will not re-run the function, instead it will just check the result it got from the cache previously.


However, what if we only wanted to only check a subset of function parameters for caching?  
For example:
  ```
  func(3, [], "HELLO")
  ```
and
  ```
  func(3, [], "GOODBYE")
  ```
Would be cached as different results, even if we might not want them to be. 

With the new feature, by specifying the *'key'* parameter in the initial decorator, the user gains finer controler over how the function's key is stored. 

### Inputting an integer as a key
If the supplied key argument is only an integer, then only that subset of the function arguments are looked at when caching. So now at setup, we can specify that we only want the first two arguments to be cached:

```
@chromaticache.lru_cache(maxsize=None, key=2)  
def func(int_value, list_value, string_value):
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
