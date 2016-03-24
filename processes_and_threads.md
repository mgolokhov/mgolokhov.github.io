---
layout: page
title: Processes and threads
---

Android uses Linux processes with the single thread of execution, "main" thread (UI thread).

By default, all components of the same application starts and live in this thread.

Default behavior can be change in manifest (with android:process).

Even components from different applications can share one process.

Android OS cannot kill Activities, but might shut down a process at some point (ex. low memory). 

Long running operations can block UI thread as the result after 5 sec user gets  "application not responding" (ANR).

Andoid UI toolkit is not thread-safe (don't access the UI from outside the UI thread)

Developer can create other threads ("background" or "worker" threads).

Variants:

### Standard Java SE threads + helper methods to post result in the UI thread

* Activity.runOnUiThread(Runnable)
* View.post(Runnable)
* View.postDelayed(Runnable, long)

### AsyncTask

<center><img src="{{ site.url }}/assets/async_task.png"/></center>

Gotchas:

AsyncTask should only be used for tasks/operations that take quite few seconds.

AsyncTasks are executed serially on a single background thread ([from API 11](http://developer.android.com/reference/android/os/AsyncTask.html#execute(Params...))). So long running worker can block others.

Declaration as an inner class in an activity/fragment is a bad idea, because it creates an implicit reference to the outer class, which can then result in leaked memory.

AsyncTask tied to the lifecycle of its process, as the result can post results to the destroyed activity/fragment.

The task can be executed only once or you'll get an exception.



### HeandlerThread

### Threadpools

-----------

## Preferences

[http://developer.android.com/guide/components/processes-and-threads.html](http://developer.android.com/guide/components/processes-and-threads.html)

[http://developer.android.com/reference/android/os/AsyncTask.html](http://developer.android.com/reference/android/os/AsyncTask.html)







