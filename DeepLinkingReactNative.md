# Guide to React Native Deep Linking

Before we go through the **HOW** part where we will be adding *Deep Linking* in our React Native app, Let's first go through the **WHAT** and **WHY** to better grasp the concept. Let's start.
# What is Deep Linking?
A *Deep Link* is simply an *intent filter system* that allows the user to access a certain activity in an Android app with a URL.  
Let us suppose that we saw a certain product (for example a shoe) on an e-commerce website (example: Amazon) and we want to share it with a friend. So a Deep Link will allow us to share a URL that will enable the receiver to open that exact shoe product page in just one click.  
Now this definition will be clearer:
>Deep linking consists of using a uniform resource identifier (URI) that links to a specific location within a mobile app rather than simply launching the app. Deferred deep linking allows users to deep link to content even if the app is not already installed.

# Why Deep Linking?
Well, we already went through one example in *What* part above but there can be many use cases where a *Deep Link* can come very handy. Think of marketing strategies, referral links, sharing a certain product, etc.  
The greatest benefit of mobile deep linking is the ability for marketers and app developers to bring users directly into the specific location within their app with a dedicated link. Just as deep links made the web more usable, mobile deep links do the same for mobile apps.
# How to add Deep Linking?
Finally, how to create one. There are just 3 simple steps involved. Which are:
## Steps involved:-
1. Create a Project
2. Edit AndroidManifest.xml
3. Build Project

## Step 1. Create a project
Create a React Native project by running this command:
```shell
react-native init deeplinkdemo
```
Now that we have a project to finally tweak, let's move on to step 2.
## Step 2. Edit AndroidManifest.xml
We have to add `intent-filter` inside `AndroidManifest.xml` to specify the incoming links to launch this particular app. 
```xml
<activity
 android:name=".MainActivity"
 android:label="@string/app_name"
 android:configChanges="keyboard|keyboardHidden|orientation|screenSize"
 android:windowSoftInputMode="adjustResize">
 <intent-filter>
 <action android:name="android.intent.action.MAIN" />
 <category android:name="android.intent.category.LAUNCHER" />
 </intent-filter>
 <!--Copy & Paste code from here-->
 <intent-filter android:label="@string/app_name">
 <action android:name="android.intent.action.VIEW" />
 <category android:name="android.intent.category.DEFAULT" />
 <category android:name="android.intent.category.BROWSABLE" />
 <data android:scheme="app"
 android:host="deeplink" />
 </intent-filter>
 <!--To here-->
 </activity>
```
I hope the comments are clearly specifying what to do. Let's understand the `intent-filter` a little better. 
> An `intent filter` is an expression in an app's manifest file that specifies the type of intents that the component would like to receive.

Get a closer look on `<data>` tag inside `<intent-filter>`. There are two properties that we have to care about. 
Consider `scheme` as a type of incoming link and `host` as the URL. 
>Our Deep Link will look something like this: `app://deeplink`

Read Google Docs for more info: 
https://developer.android.com/training/app-links/deep-linking

## Step 3. Build Project
Go to your root directory and run this command:
```shell
react-native run-android
```
Wait for the project to build and then we will test if our Deep Link is working correctly or not.
# Test it out
Make sure your app is in background and run this command: 
```shell
adb shell am start -W -a android.intent.action.VIEW -d app://deeplink com.deeplinkdemo
```

If your package has a different name then edit command as follows:
```shell
$ adb shell am start -W -a android.intent.action.VIEW -d <URI> <PACKAGE>
```

>*Note: Take a closer look at `app://deeplink`. This is our link added inside `intent-filter` to specify our app.* 

If your App opened successfully then our Deep Linking is working as expected. Yay!  
!['yay'](https://media.giphy.com/media/hZj44bR9FVI3K/giphy.gif)

# How to open with URL
We used the `app` scheme earlier and now we are going to use `https` scheme. 
Edit `<data>` tag inside `intent-filter` attribute as follows:
```xml
<data android:scheme="https" android:host="www.deeplinkdemo.com" />
```
Run this command:
```shell
 adb shell am start -W -a android.intent.action.VIEW -d https://www.deeplinkdemo.com com.deeplinkdemo
```
Cheers if your app appears right in front of you.

## Note:
You can use multiple `<data>` tags inside `intent-filter` so something like this is totally okay.
```xml
<data android:scheme="app" android:host="deeplink" />
<data android:scheme="https" android:host="www.deeplinkdemo.com" />
```
>The URIs app://deeplink and “https://www.deeplinkdemo.com” both resolve to this activity.

You can also create a HTML file with these two links like this and *test*:
```html
<html>
<a href="app://deeplink">DeepLink with app scheme</a>
<a href="https://www.deeplinkdemo.com">DeepLink with https scheme</a>
</html>
```
Access the file via localhost or place it on device. Click the link and this will hopefully launch your app.  
