# SpringBootCoroutines

#### GlobalScope

- You can use GlobalScope to start a coroutine at the top level of your application. This is often discouraged because
  it can lead to unmanaged coroutines that outlive the application's lifecycle.
    ```kotlin
    GlobalScope.launch { /* code to run in a coroutine */ }
    ```

#### CoroutineScope

- You can create your own CoroutineScope and launch coroutines within it. This is the recommended way to start
  coroutines,
  especially when working within a structured component like a ViewModel or a service.
    ```kotlin
    val myScope = CoroutineScope(Dispatchers.Default)
    myScope.launch {}
    myScope.async{}
    myScope.withContext{}
    myScope.runBlocking{}
    ```

#### async

The async builder is used to start a coroutine that computes a result asynchronously and returns a Deferred object. You
can use await() on the Deferred object to retrieve the result.

```kotlin
val result = async { /* asynchronous computation */ }.await()
```

#### withContext

You can use the withContext builder to switch to a different coroutine context temporarily for a block of code. This is
commonly used for switching between dispatchers for specific tasks.

```kotlin
val result = withContext(Dispatchers.Default) { /* code to run in Default dispatcher */ }
```

#### runBlocking
The runBlocking builder is used to start a new coroutine and block the current thread until the coroutine completes.
It's typically used in testing or in the main function of a Kotlin program.
```kotlin
runBlocking {/* code to run in a new coroutine */ }
```

