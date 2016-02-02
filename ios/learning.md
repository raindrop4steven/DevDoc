## The App Life Cycle

### Execution States for Apps

Most state transitions are accompanied by a corresponding call to the methods of your app delegate object. These methods are your chance to respond to state changes in an appropriate way. These methods are listed below, along with a summary of how you might use them.

- `application:willFinishLaunchingWithOptions`:—This method is your app’s first chance to execute code at launch time.
- `application:didFinishLaunchingWithOptions`:—This method allows you to perform any final initialization before your app is displayed to the user.
- `applicationDidBecomeActive`:—Lets your app know that it is about to become the foreground app. Use this method for any last minute preparation.
- `applicationWillResignActive`:—Lets you know that your app is transitioning away from being the foreground app. Use this method to put your app into a quiescent state.
- `applicationDidEnterBackground`:—Lets you know that your app is now running in the background and may be suspended at any time.
- `applicationWillEnterForeground`:—Lets you know that your app is moving out of the background and back into the foreground, but that it is not yet active.
- `applicationWillTerminate`:—Lets you know that your app is being terminated. This method is not called if your app is suspended.


### Background Execution

1. Apps that start a short task in the foreground can ask for time to finish that task when the app moves to the background.
2. Apps that initiate downloads in the foreground can hand off management of those downloads to the system, thereby allowing the app to be suspended or terminated while the download continues.
3. Apps that need to run in the background to support specific types of tasks can declare their support for one or more background execution modes.

#### 1. Executing Finite-Length Tasks

> beginBackgroundTaskWithName:expirationHandler: or beginBackgroundTaskWithExpirationHandler: 


忧伤还是快乐 


