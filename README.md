# MemoryCahce
## Logica
```C#
using System;
using System.Runtime.Caching;

namespace Cache
{
    public static class Caching
    {
        /// <summary>
        /// Un método genérico para obtener y configurar objetos en la memoria caché.
        /// </summary>
        /// <typeparam name="T">El tipo de objeto a devolver.</typeparam>
        /// <param name="cacheItemName">El nombre que se utilizará al almacenar este objeto en el caché.</param>
        /// <param name="cacheTimeInMinutes">¿Cuánto tiempo hay que guardar en caché este objeto?.</param>
        /// <param name="objectSettingFunction">Una función sin parámetros a la que llamar si el objeto no está en el caché y necesita configurarlo.</param>
        /// <returns>Un objeto del tipo que pediste</returns>
        public static T GetObjectFromCache<T>(string cacheItemName, int cacheTimeInMinutes, Func<T> objectSettingFunction)
        {
            ObjectCache cache = MemoryCache.Default;
            var cachedObject = (T)cache[cacheItemName];
            if (cachedObject == null)
            {
                CacheItemPolicy policy = new CacheItemPolicy();
                policy.AbsoluteExpiration = DateTimeOffset.Now.AddMinutes(cacheTimeInMinutes);
                cachedObject = objectSettingFunction();
                cache.Set(cacheItemName, cachedObject, policy);
            }
            return cachedObject;
        }
    }
}
```
## Uso
```C#
int resultOfExpensiveQuery = GetObjectFromCache<int>("resultOfExpensiveQuery", 10, CallSlowQueryFromDatabase);
```
