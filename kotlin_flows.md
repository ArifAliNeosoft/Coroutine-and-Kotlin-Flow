# Topics
### &nbsp;&nbsp;&nbsp;&nbsp; 1. Flow Builder
### &nbsp;&nbsp;&nbsp;&nbsp; 2. FlowCollector
### &nbsp;&nbsp;&nbsp;&nbsp; 3. flowOn &nbsp;
### &nbsp;&nbsp;&nbsp;&nbsp; 4. Map
### &nbsp;&nbsp;&nbsp;&nbsp; 5. filter
### &nbsp;&nbsp;&nbsp;&nbsp; 6. flatMap
### &nbsp;&nbsp;&nbsp;&nbsp; 7. Android Click Debounce
### &nbsp;&nbsp;&nbsp;&nbsp; 8. Debounce
### &nbsp;&nbsp;&nbsp;&nbsp; 9. distinctUntilChanged
### &nbsp;&nbsp;&nbsp;&nbsp; 10. FlatMapLatest
### &nbsp;&nbsp;&nbsp;&nbsp; 11. FlatMapConcat
### &nbsp;&nbsp;&nbsp;&nbsp; 11. Cold flows
### &nbsp;&nbsp;&nbsp;&nbsp; 11. Hot Flow

## <u>Flow Builder :-</u>

The flow builder function creates a new flow where you can manually emit new values into the stream of data using the emit function.
The flow builder is executed within a coroutine. Thus, it benefits from the same asynchronous APIs, but some restrictions apply.
Flows are sequential. As the producer is in a coroutine, when calling a suspend function, the producer suspends until the suspend function returns. 
With the flow builder, the producer cannot emit values from a different CoroutineContext. Therefore, don't call emit in a different CoroutineContext by creating new coroutines or by using withContext blocks of code. You can use other flow builders such as callbackFlow in these cases.

## <u>FlowCollector :-</u>

FlowCollector is used as an intermediate or a terminal collector of the flow and represents an entity that accepts values emitted by the Flow.

This interface should usually not be implemented directly, but rather used as a receiver in a flow builder when implementing a custom operator, or with SAM-conversion. Implementations of this interface are not thread-safe.
#### Example of usage:
val flow = getMyEvents()
try {
    flow.collect { value ->
        println("Received $value")
    }
    println("My events are consumed successfully")
} catch (e: Throwable) {
    println("Exception from the flow: $e")
}

## <u>flowOn :- </u>
 flowOn changes the CoroutineContext of the upstream flow, meaning the producer and any intermediate operators applied before (or above) flowOn. The downstream flow (the intermediate operators after flowOn along with the consumer) is not affected and executes on the CoroutineContext used to collect from the flow.
 
 ## <u>Map :-</u>
 The map function returns a list with elements changed according to the function from the argument:
 val list = listOf(1,2,3).map { it * 2 }    println(list) // Prints: [2, 4, 6]
 
 ## <u>filter :-</u>
 The filter function allows only the elements that match the provided predicate:
 val list = listOf(1,2,3,4,5).map { it > 2 }     println(list) // Prints: [3, 4, 5] 
 
 ## <u>flatMap :-</u>
 The flatMap function returns a single list of all elements yielded by the transform function, which is invoked on each element of the original collection:
 val list = listOf(10, 20).flatMap { listOf(it, it+1, it + 2) } println(list) // Prints: [10, 11,...]
 
 ## <u>Android Click Debounce :-</u>
 Sooner or later every developer comes across a problem with too many function calls in a short amount of time. A perfect example is user mashing the very same button which triggers multiple click events. The most popular solution is to introduce debouncing mechanism, which basically omits any calls in given time after previous invoke.
 
 As android platform is quite mature at this point, there are several approaches. For managing click events we can use external libraries or create our own implementation. Unfortunately ready solutions require us to use different api than basic onClickListener.
 
 ## <u>Debounce :- </u>
 After function call, we want to ignore next invocations for certain amount of time. For that we will use Kotlin coroutines. Main reason is they can be lifecycle aware. This is very important when working with asynchronous functions that works on view.

Let’s create empty function that will take three arguments. First two are pretty self explanatory. Action is our original function that takes generic T parameter and returns Unit. It is that we want to have possibility to pass arguments and typically debounced function like onClick doesn’t return value.

## <u>distinctUntilChanged:-</u>
distinctUntilChanged is a filtering operator, emitting a subset of elements from the upstream flow. In this case, the filtering rule is "eliminate consecutive duplicates".

So, in this case, the 1, 1, 1 portion of our flow will result in a single 1 being emitted downstream, as the duplicates are ignored. Similarly, 2, 2 results in a single 2 being emitted. However, the duplicates have to be consecutive, so the trailing 2 and 1 at the end of the flow are not skipped, despite both 1 and 2 having been in the flow previously.

There is another form of this function, that takes a lambda expression or other function type. That lambda is passed two consecutive values from the flow, and that lambda should return true if those two values are equivalent and the newer one should be ignored.


## <u>FlatMapLatest:-</u>
This operator cares only about the latest emitted results and does not process old emissions. Every time the outerFlow emits a value, it is flatMapped with the latest innerFlow value. Every time the innerFlow emits a value, it is flatMapped with the latest outerFlow value. Thus the final emission count is a value between zero and innerFlow emissions times outerFlow emissions.

## <u>FlatMapConcat :-</u>

This operator is sequential and paired. Once the outerFlow emits once, the innerFlow must emit once before the final result is collected. Once either flow emits a Nth time, the other flow must emit a Nth time before the Nth flatMapResult is collected.

## <u>Cold flows:-</u>
flows are by default cold (created by flow{..} ) builders.
code block inside flow{..} is not active until any terminal operator call is made (for instance collect). Its not active before the collect and after ending the stream of data.

Can have only one subscriber. Any new subscriber would create a new execution of flow{..}.


## <u>Hot Flow:-</u>
Shared between multiple collectors; only one instance of flow runs for all multiple collectors. (for instance, a flow that provides a steam of location updates. It make sense to have 1 instance of flow so that all collectors gets the updated and most recent location.)

* SharedFlow and StateFlow are HOT flows as they propagates items to multiple consumers. StateFlow is a special shareFlow that maintain the current state(by .value ).

 * Collecting from the hot flow doesn’t trigger any producer code (code block that updates the value of shared or stateFlow).

 * Analogy for hot stream : live video feed, all viewers will get to see exact same timeline of video being played.