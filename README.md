PhoneGap iOS
=============================================================
PhoneGapLib is a static library that enables users to include PhoneGap in their iOS application projects easily, and also create new PhoneGap based iOS application projects through a Xcode project template.
<br />

Pre-requisites
-------------------------------------------------------------
Make sure you have installed the latest iOS SDK. Download it at [http://developer.apple.com/ios](http://developer.apple.com/ios)
<br />

Build and install the Installer Package
-------------------------------------------------------------
You don't need to do this if you downloaded the installer from [phonegap.com](http://phonegap.com), this is only for developers that need to compile the source.

1. Launch "Terminal.app"
2. Navigate to the folder where the Makefile is
3. Type in "make" then press Enter

<br />

The installer should build "PhoneGapInstaller.dmg" into the **dist** folder, mount the .dmg, then:

1. Quit Xcode
2. Launch "PhoneGapInstaller.pkg" from the mounted .dmg, to install PhoneGapLib, the PhoneGap framework and the PhoneGap Xcode Templates.

<br />

Create a PhoneGap project (Xcode 3)
-------------------------------------------------------------

1. Launch Xcode, then under the File menu, select "New Project...".
2. Navigate to the "User Templates" section, select PhoneGap, then in the right pane, select "PhoneGap-based Application"
3. Select the "Choose..." button, name your project and choose the location where you want the new project to be.
4. Modify the contents of the "www" directory to add your HTML, CSS and Javascript.

<br />

Create a PhoneGap project (Xcode 4)
-------------------------------------------------------------

1. Launch Xcode, then under the File menu, select "New Project...".
2. Navigate to the "iOS" section, under "Applications" - then in the right pane, select "PhoneGap-based Application"
3. Select the "Next" button, name your project and company idenfifier, then select the "Next" button again.
4. Choose the location where you want the new project to be.
5. Run the project at least once to create the "www" folder in your project folder.
6. Drag and drop this "www" folder into your project in Xcode, and add it as a **folder reference** (**BLUE** folder).
7. Modify the contents of the "www" directory to add your HTML, CSS and Javascript.

<br />

Uninstalling PhoneGapLib, PhoneGap.framework and the Xcode Templates
--------------------------------------------------------------------

Use the "Uninstall PhoneGap" app included in the PhoneGap iOS DMG file, OR:

1. Launch "Terminal.app"
2. Navigate to the folder where Makefile is (this folder)
3. Type in "make uninstall" then press Enter

<br />

**NOTE:** 

It will ask you to confirm whether you want to delete the installed PhoneGapLib directory (just in case you made changes there) as well as the PhoneGap framework. It will not ask for confirmation in deleting the installed Xcode templates.

Unit Tests
--------------------------------------------------------------------
1. **Create** a new PhoneGap-based Application project
2. **Download** the code from the **[mobile-spec](https://github.com/apache/incubator-cordova-mobile-spec)** and put all of it in the root of your **www** folder
3. **Modify phonegap.js** to point to your correct phonegap-X.X.X.js version
4. **Run** the project

Installer Notes
-------------------------------------------------------------
This installer will only install items under your home folder (signified by ~)

Items that will be installed:

1. Xcode global var in _~/Library/Preferences/com.apple.Xcode.plist _ (which will be listed under Xcode Preferences -> Source Trees)
2. PhoneGapLib Xcode static library project under _~/Documents/PhoneGapLib_
3. Xcode project template in _~/Library/Application Support/Developer/Shared/Xcode/Project Templates/PhoneGap_
4. Xcode 4 project template in _~/Library/Developer/Xcode/Templates/Project Templates/Application_
5. PhoneGap Xcode static framework under _/Users/Shared/PhoneGap/Frameworks/PhoneGap.framework_ (may change in future updates)
6. Symlink to the framework in (5) under _~/Library/Frameworks_

<br />

To uninstall:

Delete the files listed above, or use the "Uninstall PhoneGap" app included in the PhoneGap iOS DMG file.

FAQ
---

**1. In Xcode 4, I get an error that "The Start Page 'www/index.html' was not found."?**

This is a known issue with the Xcode 4 Template - we can't specify a folder reference. You need to build the project at least once, then go to the folder where your project is in, and drag and drop in the __www__ folder, then add it as a __folder reference__ (will end up as a __blue__ folder, not yellow), then run the project again. Check your project warnings as well for clues.

**2. When I run the Installer, the installation fails?** 

Usually it's a folder permissions issue with the templates folder, changed by other third-party installers. You can trouble-shoot using the [instructions here](http://wiki.phonegap.com/PhoneGap-Installer-Fails).

**3. When I add Plugins, they are not found or won't compile?** 

Check your Xcode Run Log for clues.

This can be because of:

1. You did not add the plugin mapping in __PhoneGap.plist/Plugins__ (contact the plugin creator for the proper mapping). The __key__ is the service name used in the JavaScript interface and the __value__ is the classname used in the Objective-C interface. Often the key and value are the same.
2. You did _not_ add the plugin code as a "group" (__yellow__ folder) but added it as a "folder reference" (blue folder) 
3. You are having #import problems - see [this article](http://wiki.phonegap.com/PhoneGap%20iOS%20Plugins%20Problems). 

<br />  

**4. I get this error in my Xcode Run Log - 'ERROR whitelist rejection: url='http://&lt;MYHOSTNAME&gt;/'**

This error occurs because of the new white-list feature in version 1.1.

You will have to add any hosts your app uses or connects to (hostnames/IP addresses only, **without** the protocol) in __PhoneGap.plist/ExternalHosts__. Wildcards are supported.

This includes external http/https/ftp/ftps links in:

1. HTML anchor tags
2. connections in JavaScript (i.e through XMLHttpRequest)
3. connections through Objective-C plugins 
  
<br />

**5. How do I effectively upgrade my project?**

Starting with PhoneGap 1.4, follow the instructions in the **"PhoneGap Upgrade Guide"** document that is included with the distribution.

<br />

**6. I've got 'symbol not found' errors during runtime? Usually it's because I'm deploying to an iOS 3.x device.**

With version 0.9.6, we implemented the W3C Media Capture API, which requires use of some iOS 4 APIs and frameworks. If you are deploying to an iOS 3.x device, you will need to "weak/optional" link three frameworks: __UIKit__, __CoreMedia__, and __AVFoundation__. 

If you get a "Symbol not found: _NSConcreteGlobalBlock_", you will have to weak link libSystem through a command-line option. 

This is because the LLVM compiler strong links NSConcreteGlobalBlock, but gcc weak links (correctly). Add a linker flag in "Other Linker Flags" in your project target: _"-weak_library /usr/lib/libSystem.B.dylib"_

Starting with version 1.1, when creating a new project, the weak-linking is added through a linker flag so you will not need to do this manually.

**7. How do I override the location of the start page www/index.html?** 

Starting with PhoneGap **1.4**, you can set this directly in the function **application:didFinishLaunchingWithOptions:** in your project's **AppDelegate.m** file.

Modify these lines appropriately:

1. self.viewController.wwwFolderName = @"www";
2. self.viewController.startPage = @"index.html";

**8. What's the difference between the Xcode 3 and Xcode 4 templates?**

The PhoneGapLib static library is only used by the Xcode 3 template. The Xcode 4 template uses PhoneGap.framework (a static framework) because of Xcode 4's template limitations. Both are based off the same code, just packaged differently.

You can still create projects using the command line if you want to use the Xcode 3 Template in Xcode 4. This is particularly useful for developers debugging the PhoneGap core.

Link:

1. [https://raw.github.com/apache/incubator-cordova-ios/1.0.0/create_project.sh](https://raw.github.com/apache/incubator-cordova-ios/1.0.0/create_project.sh)

<br />

**9. In Xcode 3, I want to have a project-specific copy of PhoneGapLib for my project, not a global one. How do I do this?** 

In your project, there should be a _PhoneGapBuildSettings.xcconfig_ file. Modify the _PHONEGAPLIB_ variable in the file to point to your project specific PhoneGapLib folder. You can use relative paths, off $(PROJECT_DIR).

**10. In Xcode 4, I want to have a project-specific copy of PhoneGap.framework for my project, not a global one. How do I do this?** 

A. Remove the existing PhoneGap.framework from your project, and drag and drop your own PhoneGap.framework in, that's all there is to it. To compile your own version of PhoneGap.framework, go to _~/Documents/PhoneGapLib_ and run the Xcode project with the _UniversalFramework_ target. You might need to modify the _USER_FRAMEWORK_SEARCH_PATHS_ in your project as well.

**11. I've got other PhoneGap-specific issues not covered here?**

A. Older pre-1.0 issues have been put in the [PhoneGap iOS FAQ](http://wiki.phonegap.com/w/page/41631150/PhoneGap-for-iOS-FAQ) on the [Wiki](http://wiki.phonegap.com).      

**12. On an iOS 3.2 iPad, and launching an iPhone only app, when I use the Media Capture API, the user interface shown is iPad sized, not iPhone sized?**

A. You must delete the *~ipad.png images from **Capture.bundle** if they want to build an iPhone only app and have captureAudio() display properly on an iPad. This additional fix is just for iPad running iOS 3.2 - if the requested *~ipad.png is not available it returns the iPhone sized image.  

**13. I get this linker error: "ld: warning: ignoring file libPhoneGap.a, file was built for archive which is not the architecture being linked (armv7)"** 

A. In your project's Build Settings, set **"Build for Active Architecture Only"** to **NO**. This has been fixed in PhoneGap 1.2 for newly created projects. This is usually because Xcode 4 will only build for armv7 by default, and not armv6.

BUGS?
-----
File them at the [Cordova-iOS Issue Tracker](https://issues.apache.org/jira/browse/CB)      
<br />

MORE INFO
----------
* [http://docs.phonegap.com](http://docs.phonegap.com)
* [http://wiki.phonegap.com](http://wiki.phonegap.com)
* [http://groups.google.com/group/phonegap](http://groups.google.com/group/phonegap)
* \#phonegap channel on [Freenode IRC](http://freenode.net/)

<br />