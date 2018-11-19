## Where to Store Android KeyStore File in CI/CD Cycle?
This repo is created for <a href="https://medium.com/@farukcankaya/where-to-store-android-keystore-file-in-ci-cd-cycle-2365f4e02e57">Where to Store Android KeyStore File in CI/CD Cycle</a> article in medium. You can take a look to get the story.
Here are the storage solutions:
- <a href="https://github.com/farukcankaya/ciappsigningexample/tree/feature/encode-keystore">Encode keystore file as an environment variable</a>
- <a href="https://github.com/farukcankaya/ciappsigningexample/tree/feature/encrypted-keystore">Store encrypted keystore file in version control system</a>
- Download keystore from external source like AWS S3, Google Drive

<img src="https://github.com/farukcankaya/ciappsigningexample/raw/development/art/art.png" />

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

### Feel free to contribute
You can open an issue or send pull request about my faults.

### License
Documentation (guides, references, and associated images) is licensed as Creative Commons Attribution-NonCommercial-ShareAlike CC BY-NC-SA. The full license can be found [here](http://creativecommons.org/licenses/by-nc-sa/4.0/legalcode), and the human-readable summary [here](http://creativecommons.org/licenses/by-nc-sa/4.0/).

Everything in this repository not covered above is licensed under the [included MIT license](LICENSE).
