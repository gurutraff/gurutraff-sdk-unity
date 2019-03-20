# GuruTraff SDK

Welcome to the GuruTraff SDK release repository.

## Builds
Builds can be found from the [releases page](https://github.com/gurutraff/gurutraff-sdk/releases).

 ## Minimum requirements:
- Unity 3D version 5.6 or higher.
- Android 4.1 API 16 or higher.
- iOS 8 or higher.

## Integration steps

### Step 1: Import the GuruTraffSDK Unity package into the project.

  

If your project is designed only for iOS, you don’t have to import the package contents in the Plugins/Android folder.

Likewise, if your project is designed only for Android, you don’t have to import the package contents in the Plugins/iOS folder;

To import the GuruTraffSDK plugin:

- Select in the menu Unity Assets/Import Package/Custom Package;

- Select GuruTraffSDK.unitypackage;

Content of GuruTraffSDK.unitypackage:

- /Assets/Plugins/GuruTraff: C# code for using GuruTraff;

- /Assets/Plugins/Android/gurutraffsdk.aar: GuruTraff library for Android;

- /Assets/Plugins/Android/gurutraffunity.aar: Interlayer for calling methods GuruTraff for Android using C# code;

- /Assets/Plugins/Android/play-services-basement-11.0.4.aar: library for Android, required to obtain Google Advertise Id.

**:exclamation:If your project already has this library, but the version is different, you need to keep only one version of the library.**

- /Assets/Plugins/iOS/libGuruTraffSDKUnity.a: GuruTraff iOS library

  

### Step 2:  Initialize the GuruTraff SDK in code.

  

To initialize the GuruTraff SDK, you need to call in your code: 

    GuruTraff.Initialize ("YOUR_APP_ID");


> We also recommend setting the player ID according to your backend
> server version. To do this, call: GuruTraff.AppUserId = "YOUR_USER_ID;

AppUserId will be passed on from the ad server to your pre-registered server to reward the user for viewing the advertisements.

  

**:exclamation:If you have in-app purchases, you must send them using the following method:**

For Android:

	PurchaseEventAndroid (double price, string currency, string receipt, string signature)

For iOS:

	PurchaseEventIOS (double price, string currency, string transactionId)

  
  

### Step 3: Showing advertising

a) We recommend to pre-cache the appropriate type of advertising using the methods:

	GuruTraff.CacheInterstitial("placementId"); //For interstitial ads
	
	GuruTraff.CacheVideo ("placementId"); //For video ads

b) You can check whether cached ads are available using the methods:

	GuruTraff.IsReadyInterstitial ("placementId"); //For interstitial ads
	
	GuruTraff.IsReadyVideo ("placementId"); //For video ads

c) You can show ads using methods:

	GuruTraff.ShowInterstitial ("placement Id"); //For interstitial ads

	GuruTraff.ShowVideo ("placementId"); //For video ads

d) You can subscribe to the events, to track changes in the advertising states (conditions)

**For interstitial ads:**

	InterstitialDidCache; //resources were cached to display interstitial ads

	InterstitialDidFailToLoad (AdError); //error loading interstitial advertising

	InterstitialWindowWillDisplay; //called before opening the advertisement window

	InterstitialWindowDidDisplay; //called after the advertisement window has opened on the display

	InterstitialWindowWillClose; // alled before closing the advertising window

	InterstitialWindowDidClose; //called after the advertisement window was closed

	InterstitialDidClick; //player tapped on the advertisement

	InterstitialReceiveReward; //player can be rewarded for viewing ads

  

**For video advertising:**

	VideoDidCache; //resources were cached for displaying video ads

	VideoDidFailToLoad (AdError); //error loading video ads

	VideoWindowWillDisplay; //called before opening the advertisement window

	VideoWindowDidDisplay; //called after the advertisement window was opened on the screen

	VideoDidDismiss; //after hiding the video, the post image is shown

	VideoWindowWillClose; //called before closing the advertising window

	VideoWindowDidClose; //called after the advertisement window was closed

	VideoPostViewDidClick; //player tapped on the ads

	VideoReceiveReward; //player can be rewarded for successfully watching video ads

  

Possible errors while loading ads:

- ErrorNoAds - no ads available

- ErrorNoFreeDiskSpace - there is no space on the disk

> Note:
> 
> It is possible not to cache the advertisement in advance, but in this
> case, it is impossible to predict when it will be loaded and
> displayed.

> On Android, when ads are displayed, events will not be called right
> away, they will be called up after returning to the application.

Before showing ads, you need to pause all the sounds in the game.

### Step 4: Done! 

Now you can show ads in your application.