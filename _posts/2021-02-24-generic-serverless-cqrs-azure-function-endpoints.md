---
title: Generic Serverless CQRS Azure Functions Endpoints
toc: false
note: false
updated: 2021-03-24 19:54
teaser: In this post I will briefly describe how you can implement a serverless, generic CQRS Http Endpoint using Azure Functions and MediatR.
---

Let’s imagine we have an ICommand and an IQuery<T> interface. Here we implement them by extending existing MediatR contracts:

```csharp
namespace Core.Common
{
    public interface ICommand : IRequest
    {
    }

    public interface IQuery<out T> : IRequest<T>
    {
    }
}
```

Then, we can implement two generic Functions which are triggered by a Http request and will send those Commands and Queries via MediatR to it’s handlers:

```csharp
namespace Functions
{
    public class CqrsFunctions
    {
        private readonly IMediator _mediator;

        public CqrsFunctions(IMediator mediator)
        {
            _mediator = mediator;
        }

        [FunctionName(nameof(Command))]
        public async Task Command(
            [HttpTrigger(AuthorizationLevel.Anonymous, "post", Route = "command/{commandName}")]
            HttpRequest req, string commandName)
        {
            var commandType = typeof(ICommand);

            var type = commandType.Assembly.GetTypes()
                .Where(x => !x.IsAbstract)
                .Where(x => !x.IsInterface)
                .Where(x => commandType.IsAssignableFrom(x))
                .FirstOrDefault(x => x.FullName.Contains(commandName));

            var body    = await req.ReadAsStringAsync();
            var cmd     = JsonConvert.DeserializeObject(body, type);

            await _mediator.Send(cmd);
        }

        [FunctionName(nameof(Query))]
        public async Task<IActionResult> Query(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = "query/{queryName}")]
            HttpRequest req, string queryName)
        {
            var queryType = typeof(IQuery<>);
            var type = queryType.Assembly.GetTypes()
                .Where(x => !x.IsAbstract)
                .Where(x => !x.IsInterface)
                .Where(x => x.GetInterfaces().Any(x => x.IsGenericType && x.GetGenericTypeDefinition() == queryType))
                .FirstOrDefault(x => x.FullName.Contains(queryName));

            var queryDictionary = req.Query.ToDictionary(x => x.Key, x => x.Value.ToString());
            var queryJson       = JsonConvert.SerializeObject(queryDictionary);
            var query           = JsonConvert.DeserializeObject(queryJson, type);

            var restult = await _mediator.Send(query);

            return new OkObjectResult(restult);
        }
    }
}
```

Then you can simply send commands and queries from a client to the server which are executed in a serverless manner:

![Command Example](/assets/images/cqrs-functions-1.png)

![Query Example](/assets/images/cqrs-functions-2.png)

This implementation is extremely flexible and can be easily adjusted and extended. For example you can simple generate TypeScript representations of those commands and queries (which is what I have done in some projects).

For a more complete example (yes — it’s yet another TodoList sample), take a look at <a href="https://github.com/Cogax/azure-functions-todolist" target="_blank">this GitHub Repository</a>.

<div class="divider"></div>
