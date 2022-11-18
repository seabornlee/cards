- Add dependency in build.gradle
- ```
  implementation 'com.alphawallet:alphawallet-library:3.58.8'
  ```
- Add GPR repository
- Add key and user in gradle.properties
-
- open build.gradle under project root folder, add on the top
  ```
  buildscript {
      dependencies {
          classpath 'com.google.dagger:hilt-android-gradle-plugin:2.40.5'
      }
  }
  ```
-
- Add Gradle plugins:
- ```
  id 'dagger.hilt.android.plugin'
  id 'realm-android'
  ```
-
- Add Hilt dependencies:
  ```
  implementation "com.google.dagger:hilt-android:2.40.5"
  annotationProcessor "com.google.dagger:hilt-compiler:2.40.5"
  ```
-
- Add blow code to settings.gradle -> dependencyResolutionManagement -> repositories
  ```
  maven { url 'https://jitpack.io' }
  maven {
      url = uri("https://maven.pkg.github.com/alphawallet/alpha-wallet-android")
      credentials {
          username = getGitHubUsername()
          password = getPAT()
      }
  }
  maven {
  	url = uri("https://maven.pkg.github.com/trustwallet/wallet-core")
  	credentials {
  		username = getGitHubUsername()
  		password = getPAT()
  	}
  }
  ```
-
- Ensure ./gradle.properties inclueds:
  ```
  android.useAndroidX=true
  android.enableJetifier=true
  ```
-
- **Errors fix**
	- error: attribute destination not found
		- Open the related layout xml file
		- Search below line and delete "-auto"
		- ```
		  xmlns:app="http://schemas.android.com/apk/res-auto"
		  ```
		-
	- errer: 4 files found with path 'META-INF/gradle/incremental.annotation.processors'.
		- Solution:
			- ```
			  packagingOptions {
			      pickFirst 'META-INF/gradle/incremental.annotation.processors'
			  }
			  ```
			-