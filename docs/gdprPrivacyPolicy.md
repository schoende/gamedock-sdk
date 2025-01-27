# GDPR & Privacy Policy

According to European GDPR (General Data Protection Regulation) law, apps must clarify their functions to users and explicitly request user approval before tracking personal data. Both Google and Apple have their own interpretations of GDPR. In order to comply with all requirements, the Gamedock SDK includes a customisable privacy policy menu with accompanying mechanisms that ensure no 3rd party libraries are initialised and no network communication is performed before the user has accepted the privacy policy.

> [!WARNING]
> It is not allowed to have any network calls before the user accepts the privacy policy, do not initialise any libraries/make any calls before initialising the Gamedock SDK.

![github pages](_images/PrivacyPolicyPopup.png)

GDPR law will be enforced starting May 2018, meaning all apps must show a consent popup before collecting data. Besides the GDPR deadline, Google has added its own requirements, which will be enforced starting 1 Feb 2018. Apple hasn’t communicated a hard deadline yet other than May 2018, but we're taking the same approach as with Google so as to avoid any risks. This means that as of 1 Feb 2018, all games must have the GDPR popup enabled unless the account manager explicitly states it should be disabled. Please align closely with your account manager regarding this matter.

<!-- tabs:start -->

#### ** Unity **

### Enable or disable the consent popup

By default, the Gamedock SDK will use the default native template screens, if you want to use custom Unity screens select ‘Use unity prefabs’ on the GamedockSDK GameObject, also make sure to specify the correct orientation. There are 2 default template prefabs provided by the Gamedock SDK, those can be found in the ‘Resources/Gamedock/PrivacyPolicy’ directory. The 2 prefabs both have to be added to the GamedockSDK gameobject in the privacy policy slots when selecting the ‘Use unity prefabs’ option.

![github pages](_images/PrivacyPolicyEditorSettings.png)

The user should have an option to change their settings later on while playing the game. The game should offer a button for this which opens the GDPR settings screen, a default settings screen is provided by the Gamedock SDK and can be opened by calling:
	
~~~csharp
Gamedock.Instance.ShowPrivacyPolicySettings();
~~~

![github pages](_images/PrivacyPolicySettingsPopup.png)

The user has to restart the app after making changes to the GDPR settings before they take effect.


### Handling network calls and 3rd party SDK’s

By default, the Gamedock SDK handles all the network calls and 3rd party SDK’s within the Gamedock SDK. However, in case you do send network calls to your own server or load 3rd party SDK’s outside the Gamedock SDK it is important to know that this is only allowed after the user accepted the consent popup. When the user presses the accept button the following Gamedock SDK method is called for starting the SDK’s own network calls / 3rd party:

~~~csharp
//Callback informing the status of the privacy policy (if it was accepted by the user)
Gamedock.Instance.PrivacyPolicyCallbacks.OnPrivacyPolicyStatus -= OnPrivacyPolicyStatus(bool accepted);
Gamedock.Instance.PrivacyPolicyCallbacks.OnPrivacyPolicyStatus += OnPrivacyPolicyStatus(bool accepted);
~~~

### Changing the header image

If you want to set your own custom header image instead of the Azerion logo, you can do so by overwriting the appropriate image file to the following location (if it does not exist, you will need to create it):

PROJECT_PATH/Assets/Plugins/Android/res/drawable/

The image files should have the name “privacy_policy_landscape_custom.png” or “privacy_policy_portrait_custom.png” depending on your orientation. The image size is 800px x 220px or 600px x 220px.

For iOS, the header image can be replaced at Gamedock.framework/PrivacyPolicyHeader.png.

For Unity 2017.1 and above you can use the supplied project found in the SDK bundle under NativeLibraries/Android/Resources. Build this project in Android Studio and make sure to replace the necessary images.

#### ** AIR **

### Enable or disable the privacy policy popup

The privacy policy popup can be enabled/configured via parameters when calling Gamedock.GetInstance().Init():
* *showAgeGate* Boolean - If enabled, the age gate popup will always appear the first time the user opens the game **before** the GDPR/Privacy Policy popup. 
* *ageGateShouldBlock* Boolean - Gives the possibility for blocking the user from continuing to the game if the minimum age requirement is not met. 
* *ageGateMinimumAgeRequirement* Number - Minimum age in years for users to be able to pass the age gate popup.  
* *coppaEnabled* Boolean - If enabled, follows COPPA law for protection of minors and makes sure the game never shows a privacy policy menu, age gate menu or ads.

### Handling privacy policy callbacks / initialising 3rd party libraries

The SDK provides feedback information for the choice that the user has made when presented with the privacy policy. By default, the Gamedock SDK handles all the network calls and 3rd party SDK’s within the Gamedock SDK. However, in case you do send network calls to your own server or load 3rd party SDK’s outside the Gamedock SDK it is important to know that this is only allowed after the user accepted the privacy policy popup. In order to get that feedback, register the following callback:

~~~actionscript
//Callback informing the choice for the privacy policy
Gamedock.GetInstance().addEventListener(SDKEvents.PRIVACY_POLICY_STATUS, onPrivacyPolicyStatusEvent);

private function onPrivacyPolicyStatusEvent(evt:PrivacyPolicyStatusEvent) : void
{
	if(evt.accepted)
	{
		trace("Privacy policy accepted, initialising 3rd party libraries.");
		// Initialise third party libraries like Google Play Games, Game Center etc.
	}
}
~~~

The variables returned are:
 * *accepted* Boolean - Informs the game if the privacy policy was accepted.

### Changing the header image

If you want to set your own custom header image instead of the Azerion logo, you can do so by replacing the appropriate image files in the GamedockResources.ANE:

Android:
- GamedockResources.ane\META-INF\ANE\Android-ARM\sdk-resources-res\drawable\
- GamedockResources.ane\META-INF\ANE\Android-ARM64\sdk-resources-res\drawable\
- GamedockResources.ane\META-INF\ANE\Android-x86\sdk-resources-res\drawable\

iOS:
- Gamedock.ane\META-INF\ANE\iPhone-ARM\
- Gamedock.ane\META-INF\ANE\iPhone-x86\

For Android, the image files are named “privacy_policy_landscape_custom.png” and “privacy_policy_portrait_custom.png”, for iOS they are called "PrivacyPolicyHeader.png" and "PrivacyPolicyHeaderLandscape.png", each file is used for landscape/portrait resp. The image size is 800px x 220px or 600px x 220px.

### Showing the privacy policy settings in-game

The user should also have an option to change their privacy policy settings while playing the game. The game should offer a button for this which opens the GDPR settings screen, a default settings screen is provided by the Gamedock SDK and can be opened by calling:
	
~~~csharp
Gamedock.GetInstance().ShowPrivacyPolicySettings();
~~~

![github pages](_images/PrivacyPolicyEditorSettings.png)

The user has to restart the app after making changes to the GDPR settings before they take effect.

#### ** Cordova **

### Enable or disable the privacy policy popup

The GDPR privacy policy popup can be enabled via parameters when calling gamedockSDK.initialise.

### Handling privacy policy callbacks / initialising 3rd party libraries

The SDK provides feedback information for the choice that the user has made when presented with the privacy policy. By default, the Gamedock SDK handles all the network calls and 3rd party SDK’s within the Gamedock SDK. However, in case you do send network calls to your own server or load 3rd party SDK’s outside the Gamedock SDK it is important to know that this is only allowed after the user accepted the privacy policy popup. In order to get that feedback, register the following callback:

~~~javascript
//Callback informing the choice for the age gate
gamedockSDK.on('PrivacyPolicyStatus', (privacyPolicyStatus) => {
    console.log('PrivacyPolicyStatus with data: ', JSON.stringify(privacyPolicyStatus));
});
~~~

### Changing the header image

If you want to set your own custom header image instead of the Azerion logo, you can do so by adding the following images (PNG):

Android:
* platforms/android/app/src/main/res/drawable/privacy_policy_landscape_custom.png (if your game is in landscape)
* platforms/android/app/src/main/res/drawable/privacy_policy_portrait_custom.png (if your game is in portrait)

iOS:
*
*

### Showing the privacy policy settings in-game

The user should also have an option to change their privacy policy settings while playing the game. The game should offer a button for this which opens the GDPR settings screen, a default settings screen is provided by the Gamedock SDK and can be opened by calling:
	
~~~javascript
gamedockSDK.showPrivacyPolicySettings();
~~~

The user has to restart the app after making changes to the GDPR settings before they take effect.

<!-- tabs:end -->


## Changing the consent popup text

Note that the text and translations of the popup are kept in the Gamedock SDK Game config feature, which should be by default integrated into your game. Note that in case you are working on an update you can fetch the new game config in Unity (we already updated the game config contents). Only in case of explicit requests by your account manager the default text and translations may be changed. Please don’t change this text on your own as this must be legally correct.

## Manually passing GDPR information

If you don't intend on using the SDK popups, you can also pass and retrieve the GDPR information to the SDK using the following methods:

<!-- tabs:start -->

#### ** Unity **

~~~csharp
Gamedock.Instance.SetGDPRSettings(withPersonalisedAds, withPersonalisedContent);
//Dictionary contains two keys (withPersonalisedAds, withPersonalisedContent) with the information.
Dictionary<string, bool> gdprSettings = Gamedock.Instance.GetGDPRSettings();
~~~

#### ** AIR **

> This feature is not supported for AIR.

#### ** Cordova **

~~~javascript
gamedockSDK.setGDPRSettings(true, true);
var settings = gamedockSDK.getGDPRSettings();
~~~

<!-- tabs:end -->