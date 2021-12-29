Airnow Media SDK
======
> **Android SDK 11.4.4 Documentation**

## Overview
The Airnow Media SDK is the most powerful app monetization solution in the industry, providing developers with fantastic performance and weekly payouts. Airnow Media developers are consistently achieving more revenue than alternative ad networks due to our advanced ad units that outperform other available solutions within the industry. This document covers installation instructions, available ad units and their features, optimization, and best practices. It is written for developers with the assumption that they are familiar with Android development.

## Ad Units
There are 4 types of ad units available in this SDK.

#### In-App and Inline Banner Ads
In-App and Inline Banner Ads are a staple of the mobile advertising world. Combined with the rest of Airnow Media’s industry leading ad types, In-App Banner Ads enable Android developers to monetise their users at every point in their mobile experience and maximise their revenue.

#### Interstitial Ads
Interstitial Ads are full-screen ads that cover the interface of their main application. They are usually displayed at points of natural transition in the flow of the application, for example, between levels in a game. 
Airnow Media SDK supports static and animated Interstitial ads types, as well as video VAST ads.

#### Rewarded Ads
Rewarded ads are an excellent way to keep users engaged in your app while earning ad revenue. The reward usually comes in the form of in-game currency (coins, lives, gold, etc.). The user gets it after a successful ad completion. 

## Installation Instructions

The Airnow Media Android SDK contains the code necessary to install Airnow Media ads in your application.

##### Airnow Media Android SDK Requirements:
1. Android Studio
2. JDK 1.8
3. Android 4.1 (API Level: 16) or later 
4. Google Play services Android Advertising ID (AAID)

##### Step 1 – Adding Airnow Media SDK
Airnow Media supports both Gradle dependencies and manual download mechanisms to integrate our SDK:
##### Gradle
1. Add the following to your app’s **build.gradle** file inside repositories section
``` gradle
repositories {
    mavenCentral()
}
```
2. Then add the following to the dependencies section:

``` gradle
dependencies {
    implementation 'io.github.airnowplatform:airnow-android-sdk:11.4.0' 
}
```
##### Manual
1. Copy the library `AirnowSdk.aar` to your project in the folder `libs`.
2. Then add the following to the dependencies section:
```gradle
implementation files('libs/AirnowSdk.aar')
```
##### Optional: Update AndroidManifest.xml 
Mandatory Manifest Permissions and Activities are included in the AAR. You can add optional permissions to the manifest:
```xml
<uses-permission android:name=“android.permission.ACCESS_COARSE_LOCATION” />
<uses-permission android:name=“android.permission.ACCESS_FINE_LOCATION” />
```

##### Step 2. Google identifier permissions
1. Add the Play Services dependencies into the dependencies block, to allow Google Advertising ID information to be retrieved.

```gradle
dependencies { 
    implementation 'com.google.android.gms:play-services-ads-identifier:17.0.0' 
}
```
2. Apps updating their target API level to 31 (Android 12) will need to declare a Google Play services normal permission in the manifest file as follows:
``` xml
<uses-permission android:name="com.google.android.gms.permission.AD_ID"/>
``` 

Read more about Google Advertising ID changes [here](https://support.google.com/googleplay/android-developer/answer/6048248?hl=en).

##### Step 3 – Configure your project to receive ads.
1. Call `init` method before showing ads:
```java
AirnowSdk.init(context, “yourApiKey”, “yourAppId”);
```
`YourApiKey` and `yourAppId` were given to you when registering your Android application on Airnow Media. It's a numeric code that can be found by locating your app in the apps dashboard.

For testing, you can use next ones: 
```java
AirnowSdk.init(context, "1638978300298486440", "471710");
```

##### For ProGuard Users Only
If you are using ProGuard with the Airnow Media SDK, you must add the following code to your ProGuard file:
``` java
-keep class com.airnow.** {*;}
-keep public class com.google.android.gms.ads.** {
   public *;
}

-keepattributes JavascriptInterface
-keepclassmembers class * {
    @android.webkit.JavascriptInterface <methods>;
}
```
##### Step 4 – Code Integration.
Airnow Media SDK currently supports 4 types of advertising: `Inline` and `In-App banners`, `Interstitial Ads`, `Rewarded Ads`.

#### Banner Ads
Banner ads usually appear at the top or bottom of your app’s screen. Airnow Media SDK support two types of banners: Inline and In-App banners.
##### Inline banner
The Airnow Media SDK provides a custom View subclass, `AirnowBanner`, which handles requesting and loading ads. Start by including this XML block to your Activity’s or Fragment’s layout:

```xml
<com.airnow.AirnowBanner
    android:id="@+id/container"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```
Next, in your Activity or Fragment code, declare an instance variable for your AirnowBanner:
```java
private AirnowBanner inlineBanner;
```
In your Activity’s `onCreate()` or your Fragment’s `onCreateView()` method get the view object from the layout, then simply call `loadAd()` to fetch and display the ad
```java
inlineBanner = ((AirnowBanner) findViewById(R.id.container));
```
Optionally you can set an ad listener to receive relevant callbacks:
```java
inlineBanner.setBannerAdListener(new AirnowBannerAdListener() {
    @Override
    public void onAdLoadSuccess() {
        // Sent when the banner has successfully retrieved an ad.
    }
    @Override
    public void onAdLoadFailure(String reason) {
        // Sent when the banner has failed to retrieve an ad with a reason for failure.
    }
    @Override
    public void onAdClicked() {
        // Sent when the user has tapped on the banner.
    }
    @Override
    public void onAdOpened() {
        // Sent when the ad starts playing.
    }
    @Override
    public void onAdClosed() {
        // Sent when the ad is closed.
    }
    @Override
    public void onAdCompleted() {
        // Sent when the ad is completed.
    }
    @Override
    public void onAdLeaveApplication() {
        // Sent when a banner event causes another application to open.
    }
});
```
You can specify a desired maximum ad size, and the Airnow Media SDK will request an ad for that size: 
```java
inlineBanner.setSize(320, 50);
```
We recommend using the following banner sizes:
**320x50 (Banner)**
**468x60 (Full Banner)**
**728x90 (Leaderboard)**
**300x250 (Medium Rectangle)**

If you created a placement for your banner in the apps dashboard, you can specify it:
```java
inlineBanner.setPlacementName(yourBannerPlacementName);
```
Request a banner:
```java
inlineBanner.load();
```
> **Note**: Ad requests must be made on the main thread.
The banner will reload automatically until it is destroyed. 
When the hosting Activity or Fragment is destroyed, be sure to also destroy the banner by calling:

```java
inlineBanner.destroy();
```

##### In-App banner
In-app banner ads appear at the bottom of your app’s screen and do not need to be added to the layout.
The Airnow Media SDK provides a custom View subclass, `AirnowAppBanner`, which handles requesting and loading ads.

In the `Activity` in which you want to show the In-app banner ad, declare a `AirnowAppBanner` instance variable:
```java
private AirnowAppBanner inappBanner;
```
In your Activity’s `onCreate()` method, instantiate the `AirnowAppBanner` using the context.
```java
inappBanner = new AirnowAppBanner(this);
```
Optionally you can set an ad listener to receive relevant callbacks:
```java
inappBanner.setBannerAdListener(new AirnowBannerAdListener() {
    @Override
    public void onAdLoadSuccess() {
        // Sent when the banner has successfully retrieved an ad.
    }
    @Override
    public void onAdLoadFailure(String reason) {
        // Sent when the banner has failed to retrieve an ad with a reason for failure.
    }
    @Override
    public void onAdClicked() {
        // Sent when the user has tapped on the banner.
    }
    @Override
    public void onAdOpened() {
        // Sent when the ad starts playing.
    }
    @Override
    public void onAdClosed() {
        // Sent when the ad is closed.
    }
    @Override
    public void onAdCompleted() {
        // Sent when the ad is completed.
    }
    @Override
    public void onAdLeaveApplication() {
        // Sent when a banner event causes another application to open.
    }
});
```
You can specify a desired maximum ad size, and the Airnow Media SDK will request an ad for that size: 
```java
inappBanner.setSize(320, 50);
```
We recommend using the following banner sizes:
**320x50 (Banner)**
**468x60 (Full Banner)**
**728x90 (Leaderboard)**
**300x250 (Medium Rectangle)**

If you created a placement for your banner in the apps dashboard, you can specify it:
```java
inappBanner.setPlacementName(yourBannerPlacementName);
```
Request a banner:
```java
inappBanner.load();
```
> **Note**: Ad requests must be made on the main thread.
The banner will reload automatically until it is destroyed. 
When the hosting Activity or Fragment is destroyed, be sure to also destroy the banner by calling:

```java
inlineBanner.destroy();
```
##### Best Practices

    • Only create a banner ad view when you display it to the user.
    
    • If the user navigates from a screen with a banner on it to a screen that does not have a banner, destroy or hide the banner.

#### Interstitial Ads

The Airnow Media SDK provides a custom class, `AirnowInterstitial`, that handles fetching and displaying fullscreen interstitial ads. To ensure a smooth experience, you should pre-fetch the content as soon as your Activity is created, then display it if the fetch was successful.

In the `Activity` in which you want to show the interstitial ad, declare a `AirnowInterstitial` instance variable:
```java
private AirnowInterstitial interstitial;
```
In your Activity’s `onCreate()` method, instantiate the `AirnowInterstitial` using the context.
```java
interstitial = new AirnowInterstitial(this);
```
If you created a `placement` for your interstitial ad in the apps dashboard, you should use this method:
```java
interstitial = new AirnowInterstitial(this, yourInterstitialPlacementName);
```
> We recommend setting an ad listener to receive appropriate callbacks and display ads correctly:

```java
interstitial.setInterstitialAdListener(new AirnowInterstitialAdListener() {
    @Override
    public void onAdLoadSuccess() {
        // Sent when the interstitial ad has been cached and is ready to be shown.
    }
    @Override
    public void onAdLoadFailure(String reason) {
        // Sent when the interstitial ad fails to load with the reason for the failure to load.
    }
    @Override
    public void onAdClicked() {
        // Sent when the user has tapped on the interstitial ad.
    }
    @Override
    public void onAdOpened() {
        // Sent when the interstitial ad starts playing.
    }
    @Override
    public void onAdClosed() {
        // Sent when the interstitial ad is closed. At this point your application should resume.
    }
    @Override
    public void onAdCompleted() {
        // Sent when the ad is completed.
    }
    @Override
    public void onAdLeaveApplication() {
        // Sent when a interstitial event causes another application to open.
    }
    @Override
    public void onAdShowFailure(String reason) {
        //  Sent when the interstitial ad fails to show with the reason for the failure to show
    }
});
```
In your `onCreate()` method, call `load()` to begin caching the ad. It’s important to fetch interstitial ad content well before you plan to show it, because it often incorporates rich media and may take some time to load. We suggest caching when your `Activity` is first created, but you may also choose to do it based on events in your app, such as the start of a game level. Load an interstitial ad:
```java
interstitial.load();
```
> **Note**: Ad requests must be made on the main thread.

When you’re ready to display your ad, use `AirnowInterstitialAdListener#onAdLoadSuccess()` callback and the `AirnowInterstitial.isLoaded()` to check whether the interstitial ad was successfully cached.

After that you can show the ad:
```java
interstitial.show();
```
The interstitial ad does not automatically reload. Call `load()` every time you want to reload your ad.

> **Note**: When the hosting Activity is destroyed, be sure to also destroy the interstitial ad by calling:

```java
interstitial.destroy();
```

#### Rewarded Ads

The Airnow Media SDK provides a custom class, `AirnowRewarded`, that handles fetching and displaying rewarded ads. To ensure a smooth experience, you should pre-fetch the content as soon as your Activity is created, then display it if the fetch was successful.

In the `Activity` in which you want to show the rewarded ad, declare a `AirnowRewardedinstance` variable:
```java
private AirnowRewarded rewarded;
```
In your Activity’s `onCreate()` method, instantiate the `AirnowRewarded` using the context and placement:
```java
AirnowRewarded rewarded = new AirnowRewarded(this, yourRewardedPlacementName);
```
You can provide a unique user ID for identification on your server:
```java
rewarded.setUserId(yourUserId);
```
You can set an ad listener to receive appropriate callbacks and display ads correctly:
```java
rewarded.setRewardedAdListener(new AirnowRewardedAdListener() {
    @Override
    public void onAdLoadSuccess() {
        // Sent when the rewarded ad has been cached and is ready to be shown.
    }
    @Override
    public void onAdLoadFailure(String reason) {
        // Sent when the rewarded ad fails to load with the reason for the failure to load.
    }
    @Override
    public void onAdClicked() {
        // Sent when the user has tapped on the rewarded ad.
    }
    @Override
    public void onAdOpened() {
        // Sent when the rewarded ad starts playing.
    }
    @Override
    public void onAdClosed() {
        // Sent when the rewarded ad is closed. At this point your application should resume.
    }
    @Override
    public void onAdCompleted() {
        // Sent when the ad is completed.
    }
    @Override
    public void onAdLeaveApplication() {
        // Sent when a rewarded event causes another application to open.
    }
    @Override
    public void onAdShowFailure(String reason) {
        //  Sent when the rewarded ad fails to show with the reason for the failure to show
    }
    @Override
    public void onRewardedAdStarted() {
        // Sent when the rewarded ad is started.
    }
    @Override
    onReceivedReward(AnReward reward) {
        // Sent when the rewarded ad is completed and the user should be rewarded.
        // You can use the reward object with boolean 
        // isSuccessful(), String getLabel(), and int getAmount().
    }
});
```
In your `onCreate(`) method, call `load()` to begin caching the ad. It’s important to fetch rewarded ad content well before you plan to show it, because it incorporates rich media and may take some time to load. We suggest caching when your `Activity` is first created, but you may also choose to do it based on events in your app, such as the start of a game level. Load a rewarded ad:
```java
rewarded.load();
```
> **Note**: Ad requests must be made on the main thread.

When you’re ready to display your ad, use `AirnowRewardedAdListener#onAdLoadSuccess()` callback and the `AirnowRewarded.isLoaded()` to check whether the rewarded ad was successfully cached.

After that you can show the ad:
```java
rewarded.show();
```
The rewarded ad does not automatically reload. Call `load()` every time you want to reload your ad.

> **Note**: When the hosting Activity is destroyed, be sure to also destroy the rewarded ad by calling:
```java
rewarded.destroy();
```

### Airnow Media SDK Test Mode
You can set test mode to customize ads in your application. In test mode, you will only receive test ads.
To enable or disable test mode, use the method:
```java
AirnowSdk.setTestMode(mode);
```
### Airnow Media Opt-Out Ads
An Android mobile device may provide a "Limit Ad Tracking" or "Opt out of interest-based advertising" setting. If a user opts out of using this option on a device, Airnow Media will not use the information collected from that device to identify his interests or to display ads on that device that target his intended interests.

The Airnow Media SDK also contains built-in functionality to manage personalized ad opt-outs. Application developers can use them to provide this functionality at the application level.

To manage personalized ad opt-outs, follow the below:
```java
AirnowSdk.setOptOut(context, mode); 
AirnowSdk.isOptOutEnabled(context);
```

If a user opts out, he will still see the same amount of Airnow Media ads on his mobile device; however, these ads may be less relevant because they will not be relevant to his interests.

### Regulation Advanced Settings
##### GDPR – Managing Consent
The Airnow Media SDK supports a client API that allows publishers to pass consent on behalf of their end users.

To use the Airnow Media API to update the user's consent status, use this function:

```java
AirnowSdk.setGdprConsent(state);
```
If the user has given consent, set the state `AirnowConsent.ACCEPTED`.
If the user did not consent, set the state `AirnowConsent.REJECTED`.
Otherwise, set the state to `AirnowConsent.NOCONSENT`.

> **Note**: `AirnowConsent.NOCONSENT` is used as the default state.

##### CCPA Compliance
The Airnow Media SDK supports publishers to restrict the sale of end users personal information under the California Consumer Privacy Act (CCPA).

If the user has opted out of “sale” of personal information:
```java
AirnowSdk.setCcpaConsent(false);
```

If “sale” of personal information is permitted:
```java
AirnowSdk.setCcpaConsent(true);
```

##### User-Level Settings for Child-Directed Apps with Age Gates
The Airnow Media SDK enables publishers of child-directed apps to flag specific end-users as children, as may be permitted or required by applicable law (e.g. COPPA, GDPR, etc.).  Publishers of child-directed apps are responsible for determining whether an app is permitted to flag at the end-user level or must treat all end-users as children.  Publishers should consult with their legal counsel accordingly.

If the end-user is a child (as defined by applicable regulations):
```java
AirnowSdk.setChildDirected(true);
```
If the end-user is not a child:
```java
AirnowSdk.setChildDirected(false);
```

### Sample Application Code and Support
You can download the demo project **"Android SDK Integration DemoApp"** using the following [link](https://github.com/airnowplatform/airnow-android-sdk-demo-app) . This is code demonstration on how to integrate Airnow Media Android SDK in your app.
Demo app will work without any modifications on simulator. 