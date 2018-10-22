# Other
## Why must UIKit operations be performed on the main thread


In Cocoa Touch, the UIApplication i.e. the instance of your application is attached to the main thread because this thread is created by UIApplicatioMain(), the entry point function of Cocoa Touch. It sets up main event loop, including the application’s run loop, and begins processing events. Application's main event loop receives all the UI events i.e. touch, gestures etc.

From docs UIApplicationMain(),

```
This function instantiates the application object from the principal class and instantiates the delegate (if any) from the given class and sets the delegate for the application. It also sets up the main event loop, including the application’s run loop, and begins processing events. If the application’s Info.plist file specifies a main nib file to be loaded, by including the NSMainNibFile key and a valid nib file name for the value, this function loads that nib file. 
```

These application UI events are further forwarded to UIResponder's following the chain of responders usually like UIApplication->UIWindow->UIViewController->UIView->subviews(UIButton,etc.)

Responders handle events like button press, tap, pinch zoom, swipe etc. which get translated as change in the UI. Hence as you can see these chain of events occur on main thread which is why UIKit, the framework which contains the responders should operate on main thread.

From docs again UIKit,

For the most part, UIKit classes should be used only from an application’s main thread. This is particularly true for classes derived from UIResponder or that involve manipulating your application’s user interface in any way.

EDIT

Why drawRect needs to be on main thread?

drawRect: is called by UIKit as part of UIView's lifecycle. So drawRect: is bound to main thread. Drawing in this way is expensive because it is done using the CPU on the main thread. The hardware accelerate graphics is provided by using the CALayer technique (Core Animation).

CALayer on the other hand acts as a backing store for the view. The view will then just display cached bitmap of its current state. Any change to the view properties will result in changes in the backing store which get performed by GPU on the backed copy. However, the view still needs to provide the initial content and periodically update view. I have not really worked on OpenGL but I think it also uses layers(I could be wrong).

I have tried to answer this to the best of my knowledge. Hope that helps!

