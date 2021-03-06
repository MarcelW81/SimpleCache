# SimpleCache

- Allows you to check if some value exists in cache and if not, acquire the value and put it in cache, using 1 method call.
- Takes care of locking while the value is being acquired when it does not yet exist in cache.
- Generates cachekeys including the type name being cached so you can use the same key (ie FindAll) for different types.

[![Build status](http://img.shields.io/appveyor/ci/mwijnands/simplecache.svg?style=flat)](https://ci.appveyor.com/project/mwijnands/simplecache) [![NuGet version](http://img.shields.io/nuget/v/XperiCode.SimpleCache.svg?style=flat)](https://www.nuget.org/packages/XperiCode.SimpleCache)

## Installation

The `SimpleCache` package is available at [NuGet](https://www.nuget.org/packages/XperiCode.SimpleCache). To install `SimpleCache`, run the following command in the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console):

> #### Install-Package XperiCode.SimpleCache

## Examples

Get a value from cache and if it does not exist in cache yet, acquire it and put it in cache:

```csharp
var person = MemoryCache.Default.Get("Find", () => _personRepository.Find());
```

Same as above but the value will stay in the cache for only 15 minutes:

```csharp
var person = MemoryCache.Default.Get("Find", TimeSpan.FromMinutes(15), () => 
{
    return _personRepository.Find();
});
```

There are methods available to invalidate cache at an absolute expirationdate or when a certain file has changed as well. For all these methods there are `async` versions available in case the acquire method is `async`:

```csharp
var person = await MemoryCache.Default.GetAsync("Find", () => 
{
    return _personRepository.FindAsync();
});
```

If the acquire method takes a long time to finish, the extension methods take care of locking (even the `async` versions) so the acquire method will not be called multiple times after the value is acquired. So if 10 visitors would visit a webpage with the following code at the same time, the `SomeLongRunningMethod` method and `_personRepository.Find()` call will only be called **once**:

```csharp
var person = MemoryCache.Default.Get("Find", () =>
{
    SomeLongRunningMethod();
    return _personRepository.Find();
});
```

## Project page

http://simplecache.xpericode.com/

Please report issues if you find any. Pull requests are welcome!
