# SpringBootCoroutines

#### GlobalScope

- You can use GlobalScope to start a coroutine at the top level of your application. This is often discouraged because
  it can lead to unmanaged coroutines that outlive the application's lifecycle.

```kotlin
    GlobalScope.launch { /* code to run in a coroutine */ }
```

#### CoroutineScope

- You can create your own CoroutineScope and launch coroutines within it.This is the recommended way to start
  coroutines especially when working within a structured component like a ViewModel or a service .

```kotlin
val myScope = CoroutineScope(Dispatchers.Default)
myScope.launch {}
myScope.async {}
myScope.async {}.await()
myScope.withContext {}
```

#### async

- The async builder is used to start a coroutine that computes a result asynchronously and returns a Deferred object.
  You can use await() on the Deferred object to retrieve the result .

```kotlin
val result = async { /* asynchronous computation */ }.await()
```

#### withContext

- You can use the withContext builder to switch to a different coroutine context temporarily for a block of code. This
  is commonly used for switching between dispatchers for specific tasks.

```kotlin
val result = withContext(Dispatchers.Default) { /* code to run in Default dispatcher */ }
```

#### runBlocking

- The runBlocking builder is used to start a new coroutine and block the current thread until the coroutine completes.
  It's typically used in testing or in the main function of a Kotlin program.

```kotlin
runBlocking {/* code to run in a new coroutine */ }
```

#### runCatching

- runCatching is an extension function available in the kotlinx.coroutines library that allows you to launch a
  coroutine while capturing and handling exceptions that may occur within the coroutine. It returns a
  CancellableContinuation object that can be used to handle the result or exception.

```kotlin
val job = GlobalScope.launchCatching {
    // Coroutine code that may throw an exception
}

job.invokeOnCompletion { result ->
    if (result.isFailure) {
        val exception = result.exceptionOrNull()
        // Handle the exception here
    }
}
```

### Closeable

#### use function

- The `use` function is an extension function that comes from the Closeable interface. It is used for managing resources
  that need to be closed when they are no longer needed.
- Use function used because consumer need to be closed after use landsEventConsumer.close()

```kotlin
landsEventConsumer.use { landsEventConsumer ->
    val job = launch {
        while (isActive) {
            val records = landsEventConsumer.poll(pollTimeout)
            for (record in records) {
                val landResponse = record.value()
                lands.addAll(landResponse.lands ?: emptyList())
            }
        }
    }

    job.join()
}
```

#### Join function

- The job.join() call is used to wait for the completion of a coroutine represented by the job object.
- Function `join()` that can be called on a Job object returned by launch. It blocks the current thread until the
  coroutine represented by the Job completes.

