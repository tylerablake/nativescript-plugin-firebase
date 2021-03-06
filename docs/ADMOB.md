<img src="https://raw.githubusercontent.com/EddyVerbruggen/nativescript-plugin-firebase/master/docs/images/features/admob.png" height="85px" alt="AdMob"/>

## Enabling AdMob
Since plugin version 3.10.0 you can use Firebase _AdMob_ features.

_AdMob_ lets you show banners or interstitials (full screen ads) in your app so you can earn some money.

### iOS

#### App Transport Security
Open `app/App_Resources/iOS/Info.plist` and add this to the bottom:

```xml
<key>NSAppTransportSecurity</key>
<dict>
  <key>NSAllowsArbitraryLoads</key>
  <true/>
  <key>NSAllowsArbitraryLoadsForMedia</key>
  <true/>
  <key>NSAllowsArbitraryLoadsInWebContent</key>
  <true/>
</dict>
```

[More info on this subject.](https://firebase.google.com/docs/admob/ios/app-transport-security)

## Functions

### admob.showBanner
Go [manage your AdMob app](https://apps.admob.com/#account/appmgmt:) and grab the banner, then show it in your app:

```js
  firebase.admob.showBanner({
    size: firebase.admob.AD_SIZE.SMART_BANNER, // see firebase.admob.AD_SIZE for all options
    margins: { // optional nr of device independent pixels from the top or bottom (don't set both)
      bottom: 10,
      top: 0
    },
    androidBannerId: "ca-app-pub-9517346003011652/7749101329",
    iosBannerId: "ca-app-pub-9517346003011652/3985369721",
    testing: true, // when not running in production set this to true, Google doesn't like it any other way
    iosTestDeviceIds: [ //Android automatically adds the connected device as test device with testing:true, iOS does not
        "45d77bf513dfabc2949ba053da83c0c7b7e87715", // Eddy's iPhone 6s
        "fee4cf319a242eab4701543e4c16db89c722731f"  // Eddy's iPad Pro
    ],
    keywords: ["keyword1", "keyword2"] // add keywords for ad targeting
  }).then(
      function () {
        console.log("AdMob banner showing");
      },
      function (errorMessage) {
        dialogs.alert({
          title: "AdMob error",
          message: errorMessage,
          okButtonText: "Hmmkay"
        });
      }
  );
```

### admob.hideBanner
Easy peasy:

```js
  firebase.admob.hideBanner().then(
      function () {
        console.log("AdMob banner hidden");
      },
      function (errorMessage) {
        dialogs.alert({
          title: "AdMob error",
          message: errorMessage,
          okButtonText: "Hmmkay"
        });
      }
  );
```

### admob.showInterstitial
This is a fullscreen ad, so you can earn extra credit on the eternal ladder of annoyance.

Note that an interstitial is supposed to be hidden by clicking the close button, so there's no function to do it programmatically.

There's two ways how you can use this function:

* **RECOMMENDED**: without arguments, and after the Promise of `preloadInterstitial` resolves. This will show the interstitial immediately.
* **DEPRECATED**: with arguments (same as `preloadInterstitial`). This will preload and *then* show the interstitial, so a delay will be noticable by the user, which is against Google's policies.

```js
  firebase.admob.preloadInterstitial({
    iosInterstitialId: "ca-app-pub-9517346003011652/6938836122",
    androidInterstitialId: "ca-app-pub-9517346003011652/6938836122",
    testing: true, // when not running in production set this to true, Google doesn't like it any other way
    iosTestDeviceIds: [ // Android automatically adds the connected device as test device with testing:true, iOS does not
        "45d77bf513dfabc2949ba053da83c0c7b7e87715", // Eddy's iPhone 6s
        "fee4cf319a242eab4701543e4c16db89c722731f"  // Eddy's iPad Pro
    ]
  }).then(
      function () {
        console.log("AdMob interstitial preloaded, you can now call 'showInterstitial' at any time to show it without delay.");
      },
      function (errorMessage) {
        dialogs.alert({
          title: "AdMob error",
          message: errorMessage,
          okButtonText: "Hmmkay"
        });
      }
  );
```

After the preload Promise resolved successfully, you can show the interstitial at any time you want:

```js
  firebase.admob.showInterstitial().then(
      function () {
        console.log("AdMob interstitial showing.");
      },
      function (errorMessage) {
        dialogs.alert({
          title: "AdMob error",
          message: errorMessage,
          okButtonText: "Hmmkay"
        });
      }
  );
```

## What about the nativescript-admob plugin?
There's currently no functional difference between the AdMob features in the Firebase plugin and
[nativescript-admob](https://github.com/EddyVerbruggen/nativescript-admob).

The main advantage of using the version in the Firebase plugin is to avoid a gradle build conflict
in the Android build you may encounter when including both plugins in your app.
