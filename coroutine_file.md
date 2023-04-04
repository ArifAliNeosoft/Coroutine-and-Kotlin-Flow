# Markdown Panel
# Coroutines->
### A coroutine is a concurrency design pattern that you can use on Android to simplify code that executes asynchronously.
### coroutines help to manage long-running tasks that might otherwise block the main thread and cause your app to become unresponsive. 
# Features
### Coroutines is our recommended solution for asynchronous programming on Android. Noteworthy features include the following:

### Lightweight: You can run many coroutines on a single thread due to support for suspension, which doesn't block the thread where the coroutine is running. Suspending saves memory over blocking while supporting many concurrent operations.
Fewer memory leaks: Use structured concurrency to run operations within a scope.
Built-in cancellation support: Cancellation is propagated automatically through the running coroutine hierarchy.
Jetpack integration: Many Jetpack libraries include extensions that provide full coroutines support. Some libraries also provide their own coroutine scope that you can use for structured concurrency.

withContext->
withContext is a suspend function through which we can do a task by providing the Dispatchers on which we want the task to be done.
withContext does not create a new coroutine, it only shifts the context of the existing coroutine and it's a suspend function whereas launch and async create a new coroutine and they are not suspend functions.
As withContext is a suspend function, it can be only called from a suspend function or a coroutine. So, both the above functions are suspend functions.

launch ->
launch is a function that creates a coroutine and dispatches the execution of its function body to the corresponding dispatcher.
The launch is basically fire and forget.
launch{} does not return anything.
launch{} cannot be used when you need the parallel execution of network calls.
launch{} will not block your main thread.

Async ->
Async is basically performing a task and return a result.
async{ }, which has an await() function returns the result of the coroutine.
Use async only when you need the parallel execution network calls.
Async will block the main thread at the entry point of the await() function. 

Suspend ->
Suspend function is a function that could be started, paused, and resume. One of the most important points to remember about the suspend functions is that they are only allowed to be called from a coroutine or another suspend function.
