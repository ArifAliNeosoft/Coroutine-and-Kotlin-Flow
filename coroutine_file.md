# Topics
## coroutines
## withContext
## launch
## async
## suspend
## dispatchers
## Coroutine Scopes
## Job
## Context
## GlobalScope
## runBlocking
<br/>



<span style="color:green;font-weight:700;font-size:30px"><U>Coroutines</span>
<span style="color:black;font-size:20px">
<br/>
 A coroutine is a concurrency design pattern that you can use on Android to simplify code that executes asynchronously.
 coroutines help to manage long-running tasks that might otherwise block the main thread and cause your app to become unresponsive</span>.
<br/><br/>
<span style="color:green;font-weight:700;font-size:30px"><U>Features</span>
<span style="color:black;font-size:20px">
<br/>
 Coroutines is our recommended solution for asynchronous programming on Android. Noteworthy features include the following:
 <br/>

 <span style="color:green;font-size:22px"><U>Lightweight:</span><span style="color:black;font-size:20px"> You can run many coroutines on a single thread due to support for suspension, which doesn't block the thread where the coroutine is running. Suspending saves memory over blocking while supporting many concurrent operations.
 <br/>
 <br/>
 <span style="color:green;font-size:22px"><U>Fewer memory leaks: </span><span style="color:black;font-size:20px"> Use structured concurrency to run operations within a scope.Built-in cancellation support: Cancellation is propagated automatically through the running coroutine hierarchy.
 <br/>

 <span style="color:green;font-size:22px"><U>Jetpack integration: </span><span style="color:black;font-size:20px">Many Jetpack libraries include extensions that provide full coroutines support. Some libraries also provide their own coroutine scope that you can use for structured concurrency.</span>
 <br/>
<br/>
<span style="color:green;font-weight:700;font-size:30px"><U>withContext</span>
<span style="color:black;font-size:20px">
<br/>
withContext is a suspend function through which we can do a task by providing the Dispatchers on which we want the task to be done.
<br/>
 withContext does not create a new coroutine, it only shifts the context of the existing coroutine and it's a suspend function whereas launch and async create a new coroutine and they are not suspend functions.
<br/>
As withContext is a suspend function, it can be only called from a suspend function or a coroutine. So, both the above functions are suspend functions.

<span style="color:green;font-weight:700;font-size:30px"><U>launch</span>  
<span style="color:black;font-size:20px">
<br/> launch is a function that creates a coroutine and dispatches the execution of its function body to the corresponding dispatcher.
<br/> The launch is basically fire and forget.
<br/> launch does not return anything.
<br/> launch cannot be used when you need the parallel execution of network calls.
<br/>launch will not block your main thread.

<span style="color:green;font-weight:700;font-size:30px"><U>Async</span>  
<span style="color:black;font-size:20px">
<br/> Async is basically performing a task and return a result.
<br/> async, which has an await() function returns the result of the coroutine.
<br/> Use async only when you need the parallel execution network calls.
<br/> Async will block the main thread at the entry point of the await() function. 

<span style="color:green;font-weight:700;font-size:30px"><U>Suspend</span>  
<span style="color:black;font-size:20px">
<br/>
 Suspend function is a function that could be started, paused, and resume. One of the most important points to remember about the suspend functions is that they are only allowed to be called from a coroutine or another suspend function.
 
 <span style="color:green;font-weight:700;font-size:30px"><U>Dispatchers </span>  
<span style="color:black;font-size:20px">
<br/>
Dispatchers help coroutines in deciding the thread on which the work has to be done. Dispatchers are passed as the arguments to the GlobalScope by mentioning which type of dispatchers we can use depending on the work that we want the coroutine to do.<br/><br/>
<U>Types of Dispatchers<br/>
There are majorly 4 types of Dispatchers.<br/>

1.Main  Dispatcher<br/>
2 IO Dispatcher<br/>
3.Default Dispatcher<br/>
4.Unconfined Dispatcher

<span style="color:green;font-weight:700;font-size:30px"><U>Coroutine Scope </span>  
<span style="color:black;font-size:20px">
<br/>To launch a coroutine, we need to use a coroutine builder like launch or async. These builder functions are actually extensions of the CoroutineScope interface. So, whenever we want to launch a coroutine, we need to start it in some scope.<br/><br/>The scope creates relationships between coroutines inside it and allows us to manage the lifecycles of these coroutines. There are several scopes provided by the kotlinx.coroutines library that we can use when launching a coroutine. There’s also a way to create a custom scope. Let’s have a look.
<br/><br/><u>
GlobalScope :-</u> The lifecycle of this scope is tied to the lifecycle of the whole application. This means that the scope will stop running either after all of its coroutines have been completed or when the application is stopped.
<br/><br/><u>
runBlocking :-</u> Another scope that comes right out of the box is runBlocking. From the name, we might guess that it creates a scope and runs a coroutine in a blocking way. This means it blocks the current thread until all childrens’ coroutines complete their executions.
<br/><br/><u>
coroutineScope :-</u> For all the cases when we don’t need thread blocking, we can use coroutineScope. Similarly to runBlocking, it will wait for its children to complete. But unlike runBlocking, this scope doesn’t block the current thread but only suspends it because coroutineScope is a suspending function.
<br/><br/><u>
Custom Coroutine Scope :-</u> There might be cases when we need to have some specific behavior of the scope to get a different approach in managing the coroutines. To achieve that, we can implement the CoroutineScope interface and implement our custom scope for coroutine handling.

<span style="color:green;font-weight:700;font-size:30px"><U>Coroutine Context </span>  
<span style="color:black;font-size:20px">
<br/>The context is a holder of data that is needed for the coroutine. Basically, it’s an indexed set of elements where each element in the set has a unique key.The important elements of the coroutine context are the Job of the coroutine and the Dispatcher.<br/>Kotlin provides an easy way to add these elements to the coroutine context using the “+” operator:


<span style="color:green;font-weight:700;font-size:30px"><U>Job in the Context</span>  
<span style="color:black;font-size:20px">
<br/>A Job of a coroutine is to handle the launched coroutine. For example, it can be used to wait for coroutine completion explicitly.Since Job is a part of the coroutine context, it can be accessed using the coroutineContext[Job] expression


