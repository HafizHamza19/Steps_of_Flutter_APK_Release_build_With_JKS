# Steps_of_Flutter_APK_Release_build_With_JKS
Steps of Create JKs file in flutter and release APK build for play store.

First change the app name [pubget(rename: ^2.0.1)]:

Run these cmd:
flutter pub global activate rename
flutter pub global run rename --appname "App name" 

Register Package name:
flutter pub global run rename --bundleId com.application.xyz

Generet file with name key.properties on this path:
project\android\key.properties

past this on that file(key.properties)

storePassword=your password
keyPassword=your pasward
keyAlias=upload        note:if you change this so you have to also change on generate JKS file cmd.
storeFile=<location of the key store file, such as /Users/<user name>/upload-keystore.jks>



Generate JKS file through this cmd: note change USER_NAME as per your PC Name
keytool -genkey -v -keystore c:\Users\USER_NAME\upload-keystore.jks -storetype JKS -keyalg RSA -keysize 2048 -validity 10000 -alias upload

then past the JKs on this path project/android/app


Some changes in [project]/android/app/build.gradle file.

 def keystoreProperties = new Properties()
   def keystorePropertiesFile = rootProject.file('key.properties')
   if (keystorePropertiesFile.exists()) {
       keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
   }

   android {
         ...
   }


signingConfigs {
       release {
           keyAlias keystoreProperties['keyAlias']
           keyPassword keystoreProperties['keyPassword']
           storeFile keystoreProperties['storeFile'] ? file(keystoreProperties['storeFile']) : null
           storePassword keystoreProperties['storePassword']
       }
   }
   buildTypes {
       release {
           signingConfig signingConfigs.release
       }
   }


Run this cmd:
flutter build appbundle

goto project->android->.gitignore
past this => app/upload-keystore.jks
  
  
  
Flutter build:
https://docs.flutter.dev/deployment/android
Video Link:
https://www.youtube.com/watch?v=g0GNuoCOtaQ

