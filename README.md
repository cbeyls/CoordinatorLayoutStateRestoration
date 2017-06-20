# CoordinatorLayout behaviors state restoration fails on API 9-12 devices

A sample project to demonstrate that the state restoration of ```CoordinatorLayout``` ```Behavior``` classes fails on API 9-12 devices, causing the app to crash after the activity has been temporarily unloaded.
This is due to a bug in the Material Components library for Android.

## About the issue

On API<13 devices, we always get a ```null``` ClassLoader in the ```CoordinatorLayout.SavedState``` constructor. A default one must be provided in that case so the Behavior states can be properly restored instead of crashing with a "```ClassNotFoundException when unmarshalling ...```" error.

Other components of the support library like ```ViewPager``` and ```RecyclerView``` already do this.

- Related issues on the Android bug tracker: [#196430](https://issuetracker.google.com/issues/37073849), [#232471](https://issuetracker.google.com/issues/37133281)
- Provided fix: [Pull request](https://github.com/material-components/material-components-android/pull/31).

## Steps to reproduce

- Launch this project on a device running **API 9 to 12**
- Press the device "home" button to switch to the home screen without leaving the Activity
- In the Android Monitor in Android Studio, press the "Terminate Application" button to kill the app process
- Switch back to the app using the task manager. It will attempt to restore its state.
- The app will crash with the following error: ```android.os.BadParcelableException: ClassNotFoundException when unmarshalling: android.support.design.widget.AppBarLayout$Behavior$SavedState```

No such crash will occur on API 13+ devices.
