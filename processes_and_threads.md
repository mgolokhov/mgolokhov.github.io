---
layout: page
title: Processes and threads
---

Android uses Linux processes with the single thread of execution, "main" thread (UI thread).

By default, all components of the same application starts and live in this thread.

Every widget-based UI modification goes through a message queue. When you use callbackes like onCreate() or onClick(), your code executed on the main thead, Android isn't processing message queue, sceen does not update, you see on logcat "Skipped xx frames! The application may be doing too much work on its main thread." After 5 sec of long running operation in UI thread user gets  "application not responding" (ANR).

Default behavior can be change in manifest (with android:process).

Even components from different applications can share one process.

At "low memory" situations Adroid OS doesn't kill Activities, but shut down processes.

Starting and communicating between processes is slow, and not an efficient way of achieving asynchronous execution. To achieve higher throughput and better performance, an application should utilize multiple threads within each process.

A developer has multiple choices create other threads ("background" or "worker" threads).

Andoid UI toolkit is not thread-safe (don't access the UI from outside the UI thread).

For really long running operations use service, because it has higher priority (later in killing order list) then activity [[1]](http://developer.android.com/guide/components/processes-and-threads.html#Lifecycle).

## Choices:

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

The task can be executed only once or you'll get an exception.

AsyncTask.cancel() doesn't kill the Thread with no regard for the consequences. All it does is set the AsyncTask to a “cancelled” state.


### HeandlerThread

### Threadpools

-----------

## Preferences

[http://developer.android.com/guide/components/processes-and-threads.html](http://developer.android.com/guide/components/processes-and-threads.html)

[http://developer.android.com/reference/android/os/AsyncTask.html](http://developer.android.com/reference/android/os/AsyncTask.html)

[http://blog.danlew.net/2014/06/21/the-hidden-pitfalls-of-asynctask/](http://blog.danlew.net/2014/06/21/the-hidden-pitfalls-of-asynctask/)







