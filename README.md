## Store encrypted keystore file in version control system
This repo is created for <a href="https://medium.com/@farukcankaya/where-to-store-android-keystore-file-in-ci-cd-cycle-2365f4e02e57">Where to Store Android KeyStore File in CI/CD Cycle</a> article in medium. You can take a look to get the story.
There are 3 branches in this repo and you are in the <a href="https://github.com/farukcankaya/ciappsigningexample/tree/feature/encrypted-keystore">feature/encrypted-keystore</a>

### Build project in your local machine
- Clone this repo and checkout the feature/encode-keystore branch:
   ```
   git clone https://github.com/farukcankaya/ciappsigningexample.git
   cd ciappsigningexample
   git fetch origin feature/encrypted-keystore
   git checkout feature/encrypted-keystore
   ```
   
- Create keystore files
   
   **(Skip this part if you want to use encrypted keystores which is described in <a href="https://github.com/farukcankaya/ciappsigningexample/new/feature/encrypted-keystore?readme=1#decrypt-stored-keystores">decrypt-stored-keystore section</a>.)**
   
   Create two keystore files for **debug** and **release** build types. If you don't know how to create keystore, you can create with following instructions <a href="https://developer.android.com/studio/publish/app-signing#generate-key">here</a>.
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

### Decrypt stored keystores
This repository has encrypted keystores named <a href="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encrypted-keystore/debug.keystore.jks.encrypted">debug.keystore.jks.encrypted</a> and <a href="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encrypted-keystore/release.keystore.jks.encrypted">release.keystore.jks.encrypted</a>. You cannot use those keystores directly to sign APK cause they are encrypted.
First, you need to decrypt these files and then you can use them to sign your APK.

- Decrypt **debug.keystore.jks.encrypted** 
`openssl aes-256-cbc -d -in debug.keystore.jks.encrypted -k 9924EB4441F72EC0642D8F109DE5294FC6A3F4D388C1D9974FCEE611A7683472 >> debug.keystore.jks`
*9924EB4441F72EC0642D8F109DE5294FC6A3F4D388C1D9974FCEE611A7683472* is the secret key which is used for encrypting debug.keystore.jks.

- Decrypt **release.keystore.jks.encrypted** 
`openssl aes-256-cbc -d -in release.keystore.jks.encrypted -k 72E9E4103F3D35279894C4C2935FEC640F420AD40CA7993AD3304FD0F7B09C7F >> release.keystore.jks`
*72E9E4103F3D35279894C4C2935FEC640F420AD40CA7993AD3304FD0F7B09C7F* is the secret key which is used for encrypting release.keystore.jks.

Now, **debug.keystore.jks** and **release.keystore.jks** keystores should be in the root of the project directory.

### Build project in Continuous Integration tools
Circle CI (<a href="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encrypted-keystore/.circleci/config.yml">.circleci/config.yml</a>) and Travis CI (<a href="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encrypted-keystore/.travis.yml">.travis.yml</a>) are configured.

You just need to put secret keys into CI tools as environment variables.
- **DEBUG_ENCRYPT_SECRET_KEY** : 9924EB4441F72EC0642D8F109DE5294FC6A3F4D388C1D9974FCEE611A7683472
- **RELEASE_ENCRYPT_SECRET_KEY** : 72E9E4103F3D35279894C4C2935FEC640F420AD40CA7993AD3304FD0F7B09C7F
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
<img src="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encrypted-keystore/art/circleci-env.png?raw=true" />

Environment variables should looked like below in **Travis CI**:
<img src="https://github.com/farukcankaya/ciappsigningexample/blob/feature/encrypted-keystore/art/travis-env.png?raw=true" />

### Create encrypted files
- Generate a secret key for you in a secure way with following command:
  `openssl enc -aes-256-cbc -k ciappsigningexample-secret -P -md sha1`
  **ciappsigningexample-secret** should be any string that you want. It generates output below:
  ```
  salt=8B1EFA25E475B004
  key=9924EB4441F72EC0642D8F109DE5294FC6A3F4D388C1D9974FCEE611A7683472
  iv =FB23273B6125ECC1A10BA7436D912690
  ```
 

- Encrypt your keystore with your secret key:

  `openssl aes-256-cbc -e -in debug.keystore.jks -out debug.keystore.jks.encrypted -k 9924EB4441F72EC0642D8F109DE5294FC6A3F4D388C1D9974FCEE611A7683472`

  It generates encrypted keystore file named **debug.keystore.jks.encrypted**. Now, you can store this file in version control system. It is usable just with your secret key.


### Feel free to contribute
You can open an issue or send pull request about my faults.

### License
Documentation (guides, references, and associated images) is licensed as Creative Commons Attribution-NonCommercial-ShareAlike CC BY-NC-SA. The full license can be found [here](http://creativecommons.org/licenses/by-nc-sa/4.0/legalcode), and the human-readable summary [here](http://creativecommons.org/licenses/by-nc-sa/4.0/).

Everything in this repository not covered above is licensed under the [included MIT license](LICENSE).
