# android-hybridloc-sdk

## About

The Hybridloc Software Development Kit (SDK) enables seamless integration of Wanzl's Hybridloc functionalities into your existing application. The SDK communicates with the backend to obtain keys required for unlocking the Hybridlocs via NFC. This SDK is designed to manage the majority of the workload, simplifying the implementation steps for you.

## Installation

Add the dependency to your project:

```kotlin
repositories {
    mavenCentral()
}

dependencies {
    implementation("de.wanzl:hybridloc:1.0.0")
}
```

## Usage

### Create the HyblidlocClient object:

```kotlin
val context = //Activity context
val clientId = "123456"
val clientSecret = "123456"
val userId = "sdk_user"


hybridlocClient = HybridlocClient(
    context,
    clientId,
    clientSecret,
    userId
)
```

The example above creates a HybridlocClient object using the Client Secret and Client Id.
These credentials were provided to you by Wanzl. If you do not have any credentials yet, please get in touch with your contact at Wanzl.

Please be advised that the credentials are sensitive data used to authenticate against the backend. They should not be declared as plain text as was done in the example. Please ensure to store them safely to prevent unwanted access.

The userId can be chosen freely by you.

### Fetch the keys:

```kotlin
hybridlocClient.loadDigitalKeys()
```

This method retrieves the keys from the backend and stores them locally. Opening a lock depends on having the latest keys in your local storage, otherwise you may not be able to open all locks. If an error occurs a ```HybridlocClientException``` is thrown.

We recommend loading the keys on every app start, to be always up to date with the newest keys.

### Start the lock opening process:

```kotlin
hybridlocClient.openLock()
```

The ```openLock()``` method initiates NFC tag listening. The method will return once a lock has been succesfully unlocked. If an error occurs (e.g., NFC tag was removed) a ```HybridlocClientException``` is thrown. After returning or an exception is thrown the app will automatically stop listening for NFC tags.

This method can trigger ```loadDigitalKeys()``` if either no keys are stored, or the keys have expired. Please be advised that by using this feature, it may happen that someone will try to open a lock before the keys are fetched. We recommend loading the keys beforehand by using ```loadDigitalKeys()``` for a smooth user experience.

### Cancel the lock opening process:

```kotlin
hybridloclient.cancelLockOpening
```

This method can be used to stop NFC tag listening. If ```cancelLockOpening()``` is called, ```openLock()``` will throw an ```HybridlocClientException```.

## License

This software is available under the [MIT]("https://github.com/wanzl-gmbh/android-hybridloc-sdk/blob/main/LICENSE") licence.