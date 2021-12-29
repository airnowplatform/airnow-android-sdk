Airnow Media SDK: mediation IronSource
======
##### Step 1 – Adding Airnow Media SDK
Read the integration guide [here](https://github.com/airnowplatform/airnow-android-sdk/tree/main/airnow-sdk).
##### For ProGuard Users Only
If you are using ProGuard with the Airnow Media SDK, you must add the following code to your ProGuard file:
``` java
-keep class com.airnow.** {*;}
-keep public class com.google.android.gms.ads.** {
   public *;
}
-keep public class com.google.ads.mediation.** {
   public *;
}

-keepattributes JavascriptInterface
-keepclassmembers class * {
    @android.webkit.JavascriptInterface <methods>;
}
```
##### Step 2 – Adding IronSource adapters
1. Copy the library `AirnowSDK-IronSource-Adapter.jar` to your project in the folder `libs`.
2. Then add the following to the dependencies section:
```gradle
implementation files('libs/AirnowSDK-IronSource-Adapter.jar')
```
##### Step 3 - Integrate the IronSource SDK into your project
Read the integration guide [here](https://developers.is.com/ironsource-mobile/android/android-sdk).

##### Step 4 – Adding mediation
1. Contact [IronSource](https://developers.is.com/submit-a-request) to enable your IronSource account for custom adapters.

2. Add Airnow Media network and configure all ad units that you wish to support as part of ironSource mediation. Learn how to do it [here](https://developers.is.com/ironsource-mobile/general/custom-adapter-setup).

> **Note**: Use **15b95f765** as the Airnow Media network key; 
`apiKey`, `appId`, and `placementName` were given to you when registering your Android application on the Airnow Media dashboard.
