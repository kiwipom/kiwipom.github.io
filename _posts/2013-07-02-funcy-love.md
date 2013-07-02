---
layout: post
title: Give your code just a little Func-y love
---

Doing more javascript these days, and even glancing occasionally at Haskell every so often (not often enough, sadly), I find bits of my C# code just *crying out* for a little funcy love.

.NET has had `Func<T>` and `Action<T>` for, oooh, thousands of years (or something) yet I tend to find many C# devs will avoid them in favour of a more imperitive style.

Consider the following example: We want to cache requests to a service, and the shape of our method looks like:

    - Get the value from the cache
    - If it's null:
      * do some work to actually fetch the data from some repository
      * cache the returned data
    - return the result
    
This pattern may repeat again and again, but you can't abstract the repetitive bits because of that pesky "get the data" piece in the middle of the call, right?

####Func-y time!


Let's say your initial implementation looked like this:

    public void GetOrders(int customerId)
    {
        string cacheKey = string.Format("OrdersForCustomer{0}", customerId);
        
        var result = _cacheService.Get<IEnumerable<Order>>(cacheKey);
        if (result == null)
        {
            result = _ordersRepository.GetForCustomer(customerId);
            _cacheService.Add(cacheKey, result);
        }
        return result;
    }
    
We can see that `_cacheService.Get` and `_cacheService.Add` would be easy to abstract, but what about the call to the repository? Well, that is simply code that returns an `IEnumerable<Order>` and can be expressed with a `Func<IEnumerable<Order>>`.

Personally, I would extend the cache service to do this work; I wouldn't add a method to the ICacheService interface, because then anyone who wanted to implement caching would have to implement that method. I'd do it as an extension method like this:


    public static void FuncyExtensions
    {
        public void T RetrieveAndCache<T>(this ICacheService cacheService, string cacheKey, Func<T> retrieve)
        {
            var result = cacheService.Get<T>(cacheKey);
            if (result == null)
            {
                result = retrieve();
                cacheService.Add(cacheKey, result);
            }
            return result;
        }
    }
    
    
So now, our GetOrders code looks like:

    public void GetOrders(int customerId)
    {
        string cacheKey = string.Format("OrdersForCustomer{0}", customerId);
        
        return _cacheService.RetrieveAndGet<IEnumerable<Order>>(cacheKey,
            () => {
                return _ordersRepository.GetForCustomer(customerId);
            });
    }


Et voila! The only code that's left in GetOrders is code that's concerned with actually getting the orders, and we have abstracted the caching completely out the way.

Much nicer, eh?

