# Problem
Error: `An item with the same key has already been added. Key: Users`

## Details
When you're working in a multi-thread application, there is a chance that many `async` methods trying to update same `Dictionary<T,T>` object.

In our scenario, we were using `Dictionary<string,Container>` to cache list of Cosmos DB containers. Whenever someone needs an intance of container, it will first check in the cache, and if not found, will create a new instance and add in cache.

The problem arises when multiple Integration Tests running in parallel and doesn't find `Container` in the cache. It then tries to add a new instance of `Container` in cache. However, Dictionary complains that when you're trying to create a new instance, another thread already added a an instance into cache

# Environment
Visual Studio 2022, .Net 6, C#,

# Solution
For multi-threaded application, it is recommended to use `ConcurrentDictionary<string, Container>`.

Moreover, in order to add a new key/value in dictionary, `TryAdd()` method should be used as a safeguard instead of `Add()`

## Sample Code
```
public class CosmosContainerFactory : ICosmosContainerFactory
    {
        private readonly CosmosClient _cosmosClient;
        private readonly ConcurrentDictionary<string, Database> _databases;
        private readonly ConcurrentDictionary<string, Container> _containers;

        public CosmosContainerFactory(CosmosClient cosmosClient)
        {
            _cosmosClient = cosmosClient;
            _databases = new ConcurrentDictionary<string, Database>();
            _containers = new ConcurrentDictionary<string, Container>();
        }

        public async Task<Container> GetContainer(string databaseName, string containerName, string partitionKey)
        {
            if (_containers.ContainsKey(containerName))
            {
                return _containers[containerName];
            }
            var database = await EnsureDatabase(databaseName);
            var container = database.GetContainer(containerName);
            _containers.TryAdd(containerName, container);

            return _containers[containerName];
        }

        private async Task<Database> EnsureDatabase(string databaseName)
        {
            if (_databases.ContainsKey(databaseName))
            {
                return _databases[databaseName];
            }
            var database = await _cosmosClient.GetDatabase(databaseName).ReadAsync();
            _databases.TryAdd(databaseName, database);

            return _databases[databaseName];
        }
    }

```
