## Encode keystore file as an environment variable in CI [![CircleCI](https://circleci.com/gh/farukcankaya/ciappsigningexample/tree/feature%2Fencode-keystore.svg?style=svg)](https://circleci.com/gh/farukcankaya/ciappsigningexample/tree/feature%2Fencode-keystore) [![Build Status](https://travis-ci.org/farukcankaya/ciappsigningexample.svg?branch=feature%2Fencode-keystore)](https://travis-ci.org/farukcankaya/ciappsigningexample)
This repo is created for <a href="https://medium.com/@farukcankaya/where-to-store-android-keystore-file-in-ci-cd-cycle-2365f4e02e57">Where to Store Android KeyStore File in CI/CD Cycle</a> article in medium. You can take a look to get the story.
There are 3 branches in this repo and you are in the <a href="https://github.com/farukcankaya/ciappsigningexample/tree/feature/encode-keystore">feature/encode-keystore</a>

### Build project in your local machine
- Clone this repo and checkout the feature/encode-keystore branch:
   ```
   git clone https://github.com/farukcankaya/ciappsigningexample.git
   cd ciappsigningexample
   git fetch origin feature/encode-keystore
   git checkout feature/encode-keystore
   ```
   
- Create keystore files

   We create two keystore files for **debug** and **release** build types. If you don't know how to create keystore, you create with following instructions <a href="https://developer.android.com/studio/publish/app-signing#generate-key">here</a>.
   We used following informations to create keystores:

   **Debug**
   - Alias: Debug
   - Password: 1234567
   - Keystore file name: debug.keystore.jks
   - Store password: 1234567
   
   **Release**
   - Alias: Prod
   - Password=12345678
   - Keystore file name: release.keystore.jks
   - Store password: 12345678
- Create **keystore.properties** file in the project root(/ciappsigningexample)

   Create **keystore.properties** file and put the informations below to **keystore.properties** file.
   
   ```
   debugKeyAlias=Debug
   debugKeyPassword=1234567
   debugKeyStore=debug.keystore.jks
   debugStorePassword=1234567
   releaseKeyAlias=Prod
   releaseKeyPassword=12345678
   releaseKeyStore=release.keystore.jks
   releaseStorePassword=12345678
   ```
   
- Finally build the project!
   You can build the project and create an APK with following command:
   
   `./gradlew clean assembleDebug`
   It generates signed an APK named **app-debug.apk** in **/ciappsigningexample/app/build/outputs/apk/debug/** directory.


### Build project in Continuous Integration tools
Circle CI (<a href="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encode-keystore/.circleci/config.yml">.circleci/config.yml</a>) and Travis CI (<a href="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encode-keystore/.travis.yml">.travis.yml</a>) are configured. You just need to generate base64 encoded string for debug keystore and release keystore. Then, put those encoded keys into CI tools as environment variables.
- openssl base64 -A -in debug.keystore.jks
  Save generated key in environment variable name DEBUG_KEYSTORE_BASE64
- openssl base64 -A -in release.keystore.jks
  Save generated key in environment variable name RELEASE_KEYSTORE_BASE64
- Finally add other environment variables to CI:

  ```
   debugKeyAlias=Debug
   debugKeyPassword=1234567
   debugKeyStore=debug.keystore.jks
   debugStorePassword=1234567
   releaseKeyAlias=Prod
   releaseKeyPassword=12345678
   releaseKeyStore=release.keystore.jks
   releaseStorePassword=12345678
   ```
   
Environment variables should looked like below in **Circle CI**:
<img src="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encode-keystore/art/circleci-env.png?raw=true" />

Environment variables should looked like below in **Travis CI**:
<img src="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encode-keystore/art/travis-env.png?raw=true" />

### Feel free to contribute
You can open an issue or send pull request about my faults.

### License
Documentation (guides, references, and associated images) is licensed as Creative Commons Attribution-NonCommercial-ShareAlike CC BY-NC-SA. The full license can be found [here](http://creativecommons.org/licenses/by-nc-sa/4.0/legalcode), and the human-readable summary [here](http://creativecommons.org/licenses/by-nc-sa/4.0/).

Everything in this repository not covered above is licensed under the [included MIT license](LICENSE).
