---
layout: page
title: Processes and threads
position: 1
---

Android uses Linux processes with the single thread of execution, "main" thread (UI thread).

By default, all components of the same application starts and live in this thread.

Every widget-based UI modification goes through a message queue. When you use callbackes like onCreate() or onClick(), your code executed on the main thead, Android isn't processing message queue, sceen does not update, you see on logcat "Skipped xx frames! The application may be doing too much work on its main thread." After 5 sec of long running operation in UI thread user gets  "application not responding" (ANR).

Default behavior can be change in manifest (with android:process).

Even components from different applications can share one process.

At "low memory" situations Android OS doesn't kill Activities, but shuts down processes.

Starting and communicating between processes is slow, and not an efficient way of achieving asynchronous execution. To achieve higher throughput and better performance, an application should utilize multiple threads within each process.

A developer has multiple choices to create other threads ("background" or "worker" threads).

Android UI toolkit is not thread-safe (don't access the UI from outside the UI thread).

For really long running operations use service, because it has higher priority (later in killing order list) then activity [[1]](http://developer.android.com/guide/components/processes-and-threads.html#Lifecycle).

For a task "fire and forget" all is pretty easy, real fun comes with thread communication. The regular Java mechanisms - pipes, shared memory, blocking queues - are available to Android applications but impose problems for the UI thread because of their tendancy to block. Hence, the Android platform defines its own message passing mechanism with nonblocking consumer-producer pattern.
The pattern is made from four main ingredients: Handler, Looper, MessageQueue and Messages/Runnables. 

You may associate a Looper, which contains a MessageQueue, with a thread [[how]](http://developer.android.com/reference/android/os/Looper.html)(UI thread and HeandlerThread already have one). Most interaction with a message loop is through the Handler class, which provides interface for posting messages. When you create a new Handler, it is bound to the thread and message queue of the thread that is creating it.

Producers send Messages or Runnables through Handler, which inserts them in the MessageQueue. The Looper (runs in the consumer thread) recieves messages in a sequentional order (by timestamp) and dispatches them to the correct Handler. In other words true work happens in the consumer thread and in the producers thread we use interface to form and send messages. 

<center><img src="{{ site.url }}/assets/handler.png"/></center>

## How to hand off tasks to the background?

### Standard Java SE threads + helper methods to post result in the UI thread

* Activity.runOnUiThread(Runnable)
* View.post(Runnable)
* View.postDelayed(Runnable, long)

### AsyncTask

<center><img src="{{ site.url }}/assets/async_task.png"/></center>

Gotchas:

AsyncTask should only be used for tasks/operations that take quite few seconds.

AsyncTasks are executed serially on a single background thread ([from API 11](http://developer.android.com/reference/android/os/AsyncTask.html#execute(Params...))). So long running worker can block others.

Declaration as an inner class in an activity/fragment can lead to memory leak, because it creates an implicit reference to the outer class.

AsyncTask tied to the lifecycle of its process, as the result can post results to the destroyed activity/fragment.

As well as Thread cannot be reused after the job is completed, AsyncTask is one-shot task or you'll get an exception.

AsyncTask.cancel() doesn't kill the Thread with no regard for the consequences. All it does is set the AsyncTask to a “cancelled” state.
It's up to the developer of AsyncTask to adhere cancel state.

### HeandlerThread

HandlerThread is a thread with a message queue that incorporates a Thread, a Looper, and a MessageQueue. It is constructed and started in the same way as a Thread. Once it is started, HandlerThread sets up queuing through a Looper and MessageQueue and then waits for incoming messages to process.



### Threadpools

-----------

## Preferences

[http://developer.android.com/guide/components/processes-and-threads.html](http://developer.android.com/guide/components/processes-and-threads.html)

[http://developer.android.com/reference/android/os/AsyncTask.html](http://developer.android.com/reference/android/os/AsyncTask.html)

[https://blog.nikitaog.me/2014/10/11/android-looper-handler-handlerthread-i/](https://blog.nikitaog.me/2014/10/11/android-looper-handler-handlerthread-i/)

[http://blog.danlew.net/2014/06/21/the-hidden-pitfalls-of-asynctask/](http://blog.danlew.net/2014/06/21/the-hidden-pitfalls-of-asynctask/)

[http://codetheory.in/android-handlers-runnables-loopers-messagequeue-handlerthread/](http://codetheory.in/android-handlers-runnables-loopers-messagequeue-handlerthread/)







