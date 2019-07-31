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

  
**:exclamation: If you have in-app purchases, you must send them using one of the following methods:**

If you're using the plugin for purchases from Unity3D, you can call a universal method for Android and iOS:
	
	GuruTraff.PurchaseEvent(PurchaseEventArgs eventArgs);
*or*

	GuruTraff.PurchaseEvent(Product product);
If you're using other plugins, call one of the following methods:

For Android:

	PurchaseEventAndroid (double price, string currency, string receipt, string signature)
For iOS:

	PurchaseEventIOS (double price, string currency, string transactionId)
  
  

### Step 3: Showing advertising

a) We recommend to pre-cache ads using the method:
	
	GuruTraff.CacheVideo ("placementId");

b) You can check whether cached ads are available using the method:

	GuruTraff.IsReadyVideo ("placementId");

c) You can show ads using the method:

	GuruTraff.ShowVideo ("placementId");

d) You can subscribe to the events, to track changes in the advertising states (conditions)
  

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



## * Reward users on install
To improve the score of the traffic, we recommend rewarding players for installing the advertised games. 
With our API you can get the necessary information to reward the player with virtual currency (such as crystals, coins...)

To request the information regarding the reward when starting the game and after switching from the background to the active mode, we recommend calling the method:

	GuruTraff.RequestMotivatedRewards();

To process the received information, you can subscribe to the events:
	
	//On receiving successful information from the server
	GuruTraff.ReceiveMotivatedRewards += ReceiveMotivatedRewardsHandler;
	//In case of error.
	GuruTraff.FailedRequestMotivatedRewards += FailedRequestMotivatedRewardsHandler; 
You must subscribe to the events before requesting for the rewards GuruTraff.RequestMotivatedRewards ()

The handler of received information(ReceiveMotivatedRewardsHandler) you will receive an array of data MotivatedRewardInfo[], where each element contains:
	- RewardId - reward id
	- AppName - the name of the installed application
	- AmountOfReward - the amount of virtual currency that needs to be credited to the player
The handler returns a bool value, where true - automatically remove received rewards from the GuruTraff server, false - if you want to remove it manually.

If your reward handler returns false, you need to manually call the method

	GuruTraff.RemoveMotivatedRewards(rewardIdsToRemove); 
where rewardIdsToRemove is an array of reward identifiers received in ReceiveMotivatedRewardsHandler.

 **:exclamation: For the rewards to work correctly, it is necessary to install AppUserId - the user ID on your server, before requesting advertising, if this is not done the users' motivation will not work.**