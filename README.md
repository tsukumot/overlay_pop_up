# overlay_pop_up

A new Flutter plugin to display pop ups or screens over other apps in Android even when app is closed or killed.

# Demo

[![Preview](https://github.com/diegohzea/diegohzea/raw/main/overlay_pop_up_demo.gif)](https://github.com/diegohzea/diegohzea/raw/main/overlay_pop_up_demo.gif)

## Android

add this to your AndroidManifest.xml

```dart
 <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />

 <application>
        ...
        <service
           android:name="com.requiemz.overlay_pop_up.OverlayService"
           android:exported="false" />
    </application>
```

### Android 14

applications that target SDK 34 and use foreground service should include foregroundServiceType attribute([see documentation](https://developer.android.com/about/versions/14/changes/fgs-types-required)).

```dart
 <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />

 <application>
        ...
        <service
           android:name="com.requiemz.overlay_pop_up.OverlayService"
           android:exported="false"
           <!-- add this -->
           android:foregroundServiceType="camera, dataSync, location, etc" />
    </application>
```

## Flutter implementation

configure your main.dart entry point a widget to display (make sure to add @pragma('vm:entry-point'))

**NOTE:**
Now you can pass as parameter the dart entry point method name when showOverlay is called

```dart
@pragma("vm:entry-point")
void overlayPopUp() {
  WidgetsFlutterBinding.ensureInitialized();
  runApp(const MaterialApp(
    debugShowCheckedModeBanner: false,
    home: Text('Hello Pub.dev!'),
  ));
}
```

## Overlay Methods

  returns true when overlay permission is alreary granted if permission is not granted then open app settings

  ```dart
  await OverlayPopUp.requestPermission();
  ```

  returns true or false according to permission status

  ```dart
  await OverlayPopUp.checkPermission();
  ```

  display your overlay and return true if is showed

*PARAMS*

- `height` is not required by default is MATCH_PARENT
- `width` is not required by default is MATCH_PARENT
- `verticalAlignment` is not required by default is CENTER for more info see: <https://developer.android.com/reference/android/view/Gravity>
- `horizontalAlignment` is not required by default is CENTER for more info see: <https://developer.android.com/reference/android/view/Gravity>
- `backgroundBehavior` by default is focusable flag that is you can take focus inside a overlay for example inside a textfield and [tapThrough] you can tap through the overlay background even if has MATCH_PARENT sizes.
- `screenOrientation` by default orientation is portrait.
- `closeWhenTapBackButton` by default when user presses back button the overlay no has any action if you pass true then back button will close overlay.
- `isDraggable`  by default is false therefore the overlay can´t be dragged.
- `entryPointMethodName` by default is 'overlayPopUp' if you want you can change it

  ```dart
  await OverlayPopUp.showOverlay();
  ```

  returns true if overlay closed correctly or already is closed

  ```dart
  await OverlayPopUp.closeOverlay();
  ```

  returns the overlay status true = open, false = closed

  ```dart
  await OverlayPopUp.isActive();
  ```

  returns the last overlay position if drag is enabled

  ```dart
  await OverlayPopUp.getOverlayPosition();
  ```

  share dynamic data to overlay

  ```dart
  await OverlayPopUp.sendToOverlay({'data':'hello!'});
  await OverlayPopUp.sendToOverlay('hello');
  ```

  receive the data from flutter as stream

  ```dart
  await OverlayPopUp.dataListener();
  ```
