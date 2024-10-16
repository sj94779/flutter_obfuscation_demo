# flutter_obfuscation_demo

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

- [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

### ..........................................................................................

Summary:

Obfuscation hides function and class names in your compiled Dart code, replacing each symbol with another symbol,
making it difficult for an attacker to reverse engineer your app.
 
a flutter app knows its function names, which can be shown using Dart's StackTrace class.

Flutter's code obfuscation works only on a release build.



Command to create obfuscated build for android
flutter build apk --obfuscate --split-debug-info=build/app/outputs/symbols  --extra-gen-snapshot-options=--save-obfuscation-map=build/app/outputs/map
flutter clean
flutter build appbundle --obfuscate --split-debug-info=build/app/outputs/symbols  --extra-gen-snapshot-options=--save-obfuscation-map=build/app/outputs/map


Several files will be created such as build/app/outputs/symbols /app.android-arm64.symbols,app.android-arm.symbols,app.android-x64.symbols

for ios product/scheme/edit scheme to release for which provisioning profile needed
Command to create obfuscated build for ios
flutter build ios --obfuscate --split-debug-info=build/app/outputs/symbols  --extra-gen-snapshot-options=--save-obfuscation-map=build/app/outputs/map

t creates file build/app/outputs/symbols/app.ios-arm64.symbols
This will also modify ios/Flutter/Generated.xcconfig to include
DART_OBFUSCATION=true
SPLIT_DEBUG_INFO=misc/mapping/1.0.0

Use Xcode menu: Product > Archive which will uses Release.xcconfig which includes updated Generated.xcconfig
#include "Generated.xcconfig"

So your uploaded Archive will now be obfuscated (you don't need to make changes to Release.xcconfig)




--obfuscate which enables obfuscation

The --split-debug-info option specifies the directory where Flutter outputs debug files. 
In the case of obfuscation, it outputs a symbol map.



To save the name obfuscation map at app build time, use --extra-gen-snapshot-options=--save-obfuscation-map=/<your-path>.
--save-obfuscation-map=<filename> which makes VM store a mapping between original names and obfuscated ones in the given filename. 
The mapping is encoded as a JSON array [original_name_0, obfuscated_name_0, original_name_1, obfuscated_name_1, ...]





To create signed apk and signed bundle using command so that you can use offuscate command as well
https://medium.com/@psyanite/how-to-sign-and-release-your-flutter-app-ed5e9531c2ac



For Android, use the following command in your terminal:
flutter build apk --obfuscate --split-debug-info=/path/to/directory/

For iOS, use:
flutter build ios --obfuscate --split-debug-info=/path/to/directory/

By executing this command, Flutter will build your app in release mode, apply obfuscation techniques to the code, and generate an APK file with the obfuscated version of your app. 
The symbol map containing the mapping between the obfuscated and original identifiers will be stored in the specified directory.


When your app crashes, the Google Play Developer Console would log this crash for you to inspect and debug.
But as the final user has an obfuscated version of your app, the report sent to you is written with meaningless names and you cannot debug it.

Now, the debug symbols map are used internally by the Play Console to resymbolize the crash report into human readable class 
names so you can debug it easily.


older post
How to obfuscate flutter apps (answer with 9 upvote describe for android and ios correctly)
https://stackoverflow.com/questions/50542764/how-to-obfuscate-flutter-apps

What symbols to upload to Play Store with Flutter and native obfuscation?
https://stackoverflow.com/questions/70404922/what-symbols-to-upload-to-play-store-with-flutter-and-native-obfuscation


How to provide symbols for obfuscated Flutter Android app bundles?
https://stackoverflow.com/questions/73967283/how-to-provide-symbols-for-obfuscated-flutter-android-app-bundles

firebase crashlytics support for flutter obfuscation
https://firebase.google.com/docs/crashlytics/get-deobfuscated-reports?platform=flutter#android-split-debug-info

apk decompile/ reverse engineering/fetch source code from apk
Steps to follow:
1) get dex tools
2) goto folder in command prompt
3) run following commands:
   chmod u+x d2j_invoke.sh
   chmod u+x d2j-dex2jar.sh
4) place .apk file in dex tools folder
5) run following command:
   sh d2j-dex2jar.sh -f -o output_jar.jar my_app.apk
6) a jar file will be created 
7) now open jd-gui tool 
8) open that jar in this tool


https://programmerworld.co/android/how-to-get-source-code-from-apk-file-of-an-android-app-api-34android-14/
to solve command not found error:
https://stackoverflow.com/questions/29744010/apk-decompile-error-d2j-dex2jar-command-not-found
http://java-decompiler.github.io


