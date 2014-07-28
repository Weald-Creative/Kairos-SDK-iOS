Kairos SDK (iOS)
==============

Kairos is the easist way include **Face-Recognition** technology in your iOS apps. This is the iOS wrapper for the Kairos Face-Recognition API. The package includes both the **SDK** as well as an **example app project**. Continue reading to learn how to integrate Kairos into your own iOS app.



## What You'll Need
* An iOS app targeting at least iOS version 6.1


## Quick Test Run
* If you just want to do a quick test of Kairos SDK, the example xcode project with the SDK is ready to test out of the box. (1) Just [sign up for a free developer account](https://developer.kairos.io/signup), (2) create an application and copy your appId/appKey, and (3) paste them into the authentication method in AppDelegate.m, (4) run the app, and (5) tap a test button and you'll get a demonstration of one of the API methods.



## Installing the Kairos SDK in your App

1. [Create your free Kairos developer account](https://developer.kairos.io/signup) if you don't already have one.
2. Log into the [dashboard](https://developer.kairos.io/admin/applications) and create a new app.
3. Copy your **App ID** & **App Key** (you'll need them later).
3. [Download](https://github.com/kairosinc/Kairos-SDK-iOS) the SDK and unzip the package.
4. Open the folder named **Kairos-SDK-iOS** containing the SDK library and header files.
5. Drag the SDK library file (**libKairosSDK.a**) into your xcode project.
6. Drag the SDK header file (**KairosSDK.h**) into your xcode project.
5. In the "Build Phases" section of your project target, navigate to "**Link Binary with Libraries**" and add libKairosSDK.a to the list (if not already there).
6. While you're still in "Link Binary with Libraries" add the frameworks: 
	* UIKit.framework
	* Foundation.framework
	* CoreImage.framework
	* CoreMedia.framework
	* CoreGraphics.framework
	* AVFoundation.framework
	* AudioToolbox.framework


7. **IMPORTANT**: In the "Build Settings" section of your project target, navigate to "Other Linker Flags" and add '-all_load' if not already present.
  
8. Import the framework header wherever you want to use the SDK

```
 #import "KairosSDK.h"
```


## Authenticate

Before you can make API calls you'll need to authenticate via your appId and appKey (You only need to do this once). Paste your **App Id** and **App Key** into this method:    

```
[KairosSDK initWithAppId:@"appID" appKey:@"appKey"];
```



## Image-Capture & 'Enroll'

The **Enroll** method **registers a face for later recognitions**. Here's an example of enrolling a face (subject) using one of the image-capture methods. This method displays an image-capture view in your app, captures an image of a face, and enrolls it:    

```
[KairosSDK imageCaptureEnrollWithSubjectId:@"12" 
                               galleryName:@"gallery1" 
                                   success:^(NSDictionary *response, UIImage *image) {
                                    
                                       NSLog(@"%@", response); 
                                   } 
                                   failure:^(NSError *error, UIImage *image) {
                                    
                                       NSLog(@"%@", error.localizedDescription); 
                                   }];
```


## Image-Capture & 'Recognize'

The **Recognize** method takes an image of a subject and **attempts to match it against a given gallery of previously-enrolled subjects**. Here's an example of recognizing a subject using an image-capture method. This method displays an image-capture view in your app, captures an image of a face, sends it to the API, and returns a match and confidence value:    

```
[KairosSDK imageCaptureRecognizeWithThreshold:@".75"
                                  galleryName:@"gallery1"
                                      success:^(NSDictionary *response, UIImage *image){
                                      
											NSLog(@"%@", response);
									   } 
									   failure:^(NSError *error, UIImage *image) {
									   
                                           NSLog(@"%@", error.localizedDescription);     
                                      }];
```
    
    
    
## Image-Capture & 'Detect'

The **Detect** method takes an image of a subject and **returns various attributes pertaining to the face features**. The detect methods also accept an optional 'selector' parameter, allowing you to tweak the scope of the response ([see docs](https://developer.kairos.io/docs) for more info on the detect selector). Here's an example of using detect via an image-capture method to retrieve the face attributes:    

```
[KairosSDK imageCaptureDetectWithSelector:nil
                                  success:^(NSDictionary *response, UIImage *image){
                                  
                                  		NSLog(@"%@", response);
                                  }
                                  failure:^(NSError *error, UIImage *image){
                                  
                                      NSLog(@"%@", error.localizedDescription);
                                  }];
```
    
## Standard Methods

The three methods introduced above are all 'Image-Capture' methods. Meaning, they all present a view that captures an image from the camera for you. But if you'd like to provide your own images, or want to develop your own image capture view, Kairos provides unwrapped versions of these methods as well. Below you'll see an example of two flavors of the recognize method, one accepts an image (UIImage), another accepts a URL (NSString) to an external image.

```
// This recognize method accepts an image
UIImage *localImage = [UIImage imageNamed:@"sample.jpg"];
[KairosSDK recognizeWithImage:localImage
                    threshold:@".75"
                  galleryName:@"gallery1"
                  maxResults:@"10"
                     success:^(NSDictionary *response) {
                              
                          NSLog(@"%@", response);
                      } 
                      failure:^(NSError *error) {
                              
                          NSLog(@"%@", error.localizedDescription);
                      }];
                      
                          
// This recognize method accepts a url                          
NSString *imageURL = @"http://media.kairos.com/liz.jpg";
[KairosSDK recognizeWithImageURL:imageURL
                       threshold:@".75"
                     galleryName:@"gallery1"
                      maxResults:@"10"
                         success:^(NSDictionary *response) {
                                 
                             NSLog(@"%@", response);
                         } 
                         failure:^(NSError *error) {
                                 
                             NSLog(@"%@", error.localizedDescription);
                         }];
```
    
    
## Optional Customization

The Kairos SDK offers options for configuring and customizing the tool to fit your use-case. You're able to specify colors, font size, transition duration and type, camera preferences, localization strings, and more. Below are just a few examples of how you can configure Kairos. (See KairosSDK.h for the full list of available configuration options):    

```
    [KairosSDK setPreferredCameraType:KairosCameraFront];
    [KairosSDK setEnableFlash:YES];
    [KairosSDK setEnableCropping:NO];
    [KairosSDK setEnableShutterSound:NO];
    [KairosSDK setStillImageTintColor:@"DBDB4D"];
    [KairosSDK setProgressBarTintColor:@"FFFF00"];
    [KairosSDK setErrorMessageMoveCloser:@"ちょっと近づいてね"];
```


    
## Available Notification Events
Optionally register for Kairos SDK events by adding your controller as an observer to the following SDK notifications:



#####KairosWillShowImageCaptureViewNotification
	Fires before the image-capture view is displayed

#####KairosWillHideImageCaptureViewNotification
	Fires before the image-capture view is hidden

#####KairosDidShowImageCaptureViewNotification
	Fires after the image-capture view has been displayed

#####KairosDidHideImageCaptureViewNotification
	Fires after the image-capture view has been hidden

#####KairosDidCaptureImageNotification
	Fires after an image has been captured



## View the Examples

Also see provided example app 'KairosSDKExampleApp' included in the SDK download bundle. It contains clear examples on how to use all of the available methods. Also check out the API documentation at [https://developer.kairos.io/docs](https://developer.kairos.io/docs)


##Support 
Have an issue? [Contact us](mailto:eric@kairos.com) or [create an issue on GitHub](https://github.com/kairosinc/Kairos-SDK-iOS)
