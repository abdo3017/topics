

# services
### what's Services?
A **Service** is an application component that can perform long-running operations in the background,\
even when the user is not interacting with your application.\
**For example** a service can handle network transactions, play music, perform file I/O, or interact with a content provider.
### what types of Services?
* **Foreground**\
  performs some operation that is noticeable to the user.\
  **For examplep** lay music and control with her notification.
* **Background**\
  performs an operation that isn't directly noticed by the user.\
  **For example** if an app used a service to compact its storag.
  > If your app targets API level 26 or higher:don't use this it,Instead, schedule your tasks using WorkManager.
* **Bound**\
  A bound service offers a client-server interface that allows components to interact with the service,\
  send requests, receive results, and even do so across processes with       interprocess communication (IPC).
  > Multiple components can bind to the service at once, but when all of them unbind, the service is destroyed.
  
#### we can bound andstart service by calling `onStartCommand()`, `onBind()`.
#### you can declare the service as private in the manifest file and block access from other applications.

### How Choosing between a service and a thread?
**service** run in **main thread** so you should still create a new thread within the service if it performs intensive or blocking operations.\
but **intentService** run in **worker thread**.
### how create a service?
you must create a subclass of **Service** or use one of its existing subclasses and implement important methods,\
**like** `onStartCommand()`,`onBind()`,`onCreate()`,`onDestroy()`.
>if you **bind** service you don't need to call `onStartCommand()` because the service runs only as long as the component is bound to it.\
>After the service is unbound from all of its clients, the system destroys it.
>*The Android system stops a service only when memory is low and it must recover system resources for the activity that has user focus.\
>If the service is bound to an activity that has user focus, it's less likely to be killed; if the service is declared to run in the foreground, it's rarely killed.\
>If the service is started and is long-running, the system lowers its position in the list of background tasks over time, and the service becomes highly susceptible to killing\ 
>if your service is started, you must design it to gracefully handle restarts by the system.\
>If the system kills your service, it restarts it as soon as resources become available, but this also depends on the value that you return from `onStartCommand()`*
### Declaring a service in the manifest!!
**`<service android:name=".ExampleService" />`**.
make your service is available to only your app by including the `android:exported` attribute equal **`false`**.

### Notes
* **the ‪`onStartCommand()`‬ method must return an integer. The integer is a value that describes how the system should continue the service in the event that the system kills it.**
1. **START_NOT_STICKY**
   Don't recreate the service unless there are pending intents to deliver.\
   This is the safest option to avoid running your service when not necessary and when your application can simply restart any unfinished jobs.
2. **START_STICKY**
   recreate the service and call `‪onStartCommand()‬`, but do not redeliver the last intent,This is suitable for **media players**.
3. **START_REDELIVER_INTENT**
   recreate the service and call `‪onStartCommand()‬` with the last intent that was delivered to the service, such as **downloading a file**.

* **When a service is started, it has a lifecycle that's independent of the component that started it.**

* **the service should stop itself when its job is complete by calling ‪`stopSelf()‬`, or another component can stop it by calling ‪`stopService()‬`.**

* **Pass intent by startservice(your intent) and receive it in `onStartcommand()`**


 > **It is not recommended to use intentservice for new apps as it will not work well starting with Android 8 Oreo, You can use *JobIntentService* as a replacement for**
 > **‪IntentService that is compatible with newer versions of Android.**

 > **If your app targets *API level 26 or higher*, the system imposes restrictions on using or creating background services unless the app itself is in the foreground.**

 > **if you want the service to send a result back, the client that starts the service can create a ‪*PendingIntent*‬ for a broadcast with ‪`getBroadcast()‬` and deliver it to the**
 > **service in the ‪Intent‬ that starts the service, The service can then use the broadcast to deliver a result.**
 
 ##LifeCycle of Services
 ![LifeCycle of Services](https://developer.android.com/images/service_lifecycle.png)
