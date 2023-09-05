# RewardsSDK 
​
RewardsSDK by CloudCard
​
## Installation
​
Maven repository is a directory where all the dependencies such as jars, library files, plugins, or other artifacts that are required by the projects are stored. To integrate RewardsSDK into your android project, please specify it in your build.gradle:
​
```bash
implementation 'io.bitbucket.ccstech.mobile.rewards.sdk:rewards-sdk:1.1.0-rc'
```
    
## Usage
​
```kotlin
import com.cloudcardinc.rewards.sdk.Rewards
​
Rewards.initialize(
            applicationContext: Application,
            origin: String,
            cardHolderId: String,
            headerTitle: String? = null,
            userProfileName: String? = null,
            userProfileImage: String? = null,
            summaryContent: String? = null,
            userCurrentLocation: Location? = null,
            uiOptions: RewardsUiOptions? = null,
            customCallbacks: List<String>? = null,
            onInitSuccess: ((rewards: Rewards) -> Unit),
            onInitFail: ((exception: Exception) -> Unit)? = null
        )
```
​
## Parameters
​
```kotlin
applicationContext: Application
```
applicationContext parameter requires Application type value. It will be used for open RewardsSDK.
​
```kotlin
origin: String
```
origin parameter requires string type value. It will be provided by CloudCard to activate the RewardsSDK.
​
```kotlin
cardholderId: String
```
cardholderId parameter requires cardholder id of logged In user in the app e.g ch_xxxxxxxxx. 
​
```kotlin
headerTitle: String
```
headerTitle is optional and can be send as null to initialize function. It support string value to display title. 
​
```kotlin
userProfileName: String
```
userProfileName parameter is optional. It can take null or can be filled with string value of logged In user name. 
​
```kotlin
userProfileImage: String
```
userProfileImage parameter is optional. It can be null or can be filled with URL of image for user profile picture.
​
```kotlin
summaryContent: String
```
summaryContent parameter is optional. It can be send as null or with string value.
​
```kotlin
userCurrentLocation: Location,
```
userCurrentLocation is optional parameter and can be send as null to initialize function. It support Location with user coordinates to display nearby offers.
​
```kotlin
uiOptions: RewardsUiOptions? = null,,
```
uiOptions is optional parameter and can be send as null to initialize function. RewardsUiOptions is explained below.
​
```kotlin
onInitSuccess: ((rewards: Rewards) -> Unit),
```
onInitSuccess is required parameter and trigger when rewards SDK initialize successfully.
​
```kotlin
environment: Environment? = null,
```
select enviorment for sandbox or production

```kotlin
onInitFail: ((exception: Exception) -> Unit)? = null,
```
onInitFail is optional parameter and can be send as null to initialize function. If its not null, then it will be triggered when rewards SDK initialization fails due to any reason.
​
## RewardsUiOptions
​
```kotlin
data class RewardsUiOptions(
    val feedTitle: String,
    val logoStyle: RewardLogoStyle,
    val brandDetailsHeaderStyle: RewardBrandDetailsHeaderStyle,
    @AnimRes val enterAnimationId: Int? = null,
    @AnimRes val exitAnimationId: Int? = null
)
```
​
#### RewardLogoStyle
​
```kotlin
enum class RewardLogoStyle {
    CIRCLE, RECTANGLE
}
```
​
#### RewardLandingScreen
​
```kotlin
enum class RewardLandingScreen {
    REWARDS_FEED, ACCOUNT_SUMMARY
}
```
#### EnvironmentSelection
​
```kotlin
enum class Environment {
    SANDBOX, PRODUCTION
}
```
​
## Rewards Error Callbacks
​
```kotlin
fun setErrorCallbacks(
        onErrorOccurred: ((exception: RewardException) -> Unit)
    )
​
RewardsManager.instance?.setErrorCallback(
    onErrorOccurred = { exception: RewardException ->
        if (exception is RewardException.NetworkError) {
            Log.e("REWARDS-ERROR", exception.message, exception.cause)
        } else {
            Log.e("REWARDS-ERROR", exception.message ?: "")
        }
    }
)
```
RewardsSDK provide setErrorCallback function in case of error occured in SDK.
​
## Rewards Custom Callbacks 
​
```kotlin
fun setSupportedCallbacks(
        customCallbacks: List<String>,
        onCallBackTrigger: (name: String, payload: String?) -> Boolean
    )
​
RewardsManager.instance?.setSupportedCallbacks(
    listOf("my_custom_feature"),
    onCallBackTrigger = { name: String, payload: String? ->
        if (name == "my_custom_feature") {
            // Present your custom feature
        }
        return true // return true to dismiss the activity, false to keep it in the backstack
    }
)
```
RewardsSDK provide function in case of custom callback received in SDK.
​
## RewardsSDKTheme 
​
To support this in your Android app, you can also override the color and font values that are in the Rewards.Default.Main by specifying what values you would like to override in your colors.xml file
​
In your colors.xml resource file
```xml
<public name="reward_core_header" type="color" />
<public name="reward_core_primary" type="color" />
<public name="reward_core_interactive" type="color" />
```
​
And to override the fonts, you can upload your own font type in your font folder. Afterwards, you would need to create a font-family resource file for each font you would like to override from us.
​
Our font family names are:
```
rewards_font_bold
rewards_font_light
rewards_font_medium
rewards_font_regular
```
​
To be able to use your own fonts, you'll need to create a font family with the same name as one of the above font family names. For example, if you wanted to override our rewards_font_bold you would create a resource in the font folder by the name rewards_font_bold.xml and specify your custom font within the attributes.
​
```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android">
    <font
        android:font="@font/your_font_name"
        android:fontWeight="400"
        android:fontStyle="bold"/>
</font-family>
```
​
The SDK also supports theming the navigation bar used throughout the SDK experience. If the SDK theming is not provided, then the navigation bar defaults to the primary theme color as the background with white UI elements.
​
```xml
<public name="reward_core_navigation_bar_title" type="color" />
<public name="reward_core_navigation_bar_background" type="color" />
<public name="reward_core_navigation_bar_back_button" type="color" />
<public name="reward_core_navigation_bar_separator" type="color" />
```
​
Our Activity uses the theme, Rewards.Default.Main. If you would like to customize items, like the status bar or navigation bar color, you can do so by overriding our theme and then the items you would like customized.
​
```xml
<style name="Rewards.Default.Main">
    <item name="android:statusBarColor">@color/colorPrimaryDark</item>
    <item name="android:navigationBarColor">@color/colorPrimaryDark</item>
</style>
```
​
#### Supported Theme Overrides
Note: currently we do not support the ability to customize all of our Rewards.Default.Main theme. The following are a list of items that can be safely customized.
​
```
android:statusBarColor
android:navigationBarColor
android:windowLightNavigationBar
```
    
