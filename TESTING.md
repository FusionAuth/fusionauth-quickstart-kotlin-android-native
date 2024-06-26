A full tutorial on FusionAuth Android SDK Test.

# Table of Contents

- [Testing](#testing)
    - [Test Cases](#test-cases)
    - [Test Data](#test-data)
    - [Test Automation](#test-automation)

# Testing
<!--
tag::forDocSiteTesting[]
-->
Using OAuth2 with a browser popup for Authentication can be tricky to use during automated testing in an [Android Emulator](https://developer.android.com/studio/run/emulator). We made some efforts in the development of the FusionAuth Android SDK and made the test available in this Quickstart as well.

In this doc, we go in to detail on how to create the test with a real FusionAuth backend and what to look for in an automated testing environment.
<!--
end::forDocSiteTesting[]
-->
## Test Cases
<!--
tag::forDocSiteTest[]
-->
We intend to test all use cases relevant for this SDK, which include: `Login` `Refresh Token` `User Info` `Logout` in combination with certain [Test Data](#test-data).

At first glance, these may seem like trivial tests, but we are seeking specific results for each use case and use them repeatedly in automated testing.

### Login

* Does it redirect to FusionAuth?
* Does a successful login result in a redirect to the Home screen?

### Refresh Token

* Is it refreshing the Token?

### User Info

* Is the User Info received?
* Depending on the different User Info returned, are all the different data sets handled properly?

### Logout

* Does it redirect to FusionAuth?
* Does it invalidate the user session?
* Does it return to the Login screen?

<!--
end::forDocSiteTest[]
-->
## Test Data

All the relevant data for testing is defined in FusionAuth which includes multiple flavors of Applicatoins and Users.

### Kickstart Details
<!--
tag::forDocSiteKickstart[]
-->
To be able to test the different scenarios FusionAuth will be initially configured with these settings:

* An update to the Tenant Theme including the ChangeBank Theme to make the look and feel of the login the same as the ChangeBank App.
* The Tenant Issuer will be set to `http://10.0.2.2:9011` to allow for testing with [Android Emulator](https://developer.android.com/studio/run/emulator).
* Two Applications `Example Android App` and `Secondary Application` to test users with and without access to the Android App.
* Your client secret is: `super-secret-secret-that-should-be-regenerated-for-production`
* You will have three example usernames available with slightly different user profiles `richard@example.com`, `monica@example.com` and `gilfoyle@example.com`. All having access to `Example Android App` where the password for all three is `password`.
* And an example user without access `erlich@example.com` to the `Example Android App`
* Your FusionAuth admin username is `admin@example.com` and your password is `password`.
* Your fusionAuthBaseUrl to access FusionAuth is `http://localhost:9011/`
<!--
end::forDocSiteKickstart[]
-->
## Test Automation
<!--
tag::forDocSiteE2ETest[]
-->
The Quickstart includes a [Full End 2 End Test](complete-application/app/src/androidTest/java/io/fusionauth/sdk/FullEnd2EndTest.kt) that uses all the different functionalities provided by the example App.

### Test Automation Prerequisites

#### Emulator Image

Starting the emulator requires the right image for the test, we’re using the `google_apis` images for the last 5 API level.

In particular 29, 30, 31, 33, and 34, the 32 API level is skipped as it is a special [Android 12L release](https://blog.google/products/android/12l-larger-screens/) for tablets and foldables with different layouts we're not concerned about.

Android Studio offers you to configure and handle emulators within the IDE, but you can go via the [emulator commandline](https://developer.android.com/studio/run/emulator-commandline) too.

If you’re automating your tests with a test matrix, it is important to know the change of the architecture starting at API Level 31 to `x86_64` from previously `x86`.

Which results in our case in the following five emulator configurations:

| api-level | target      | arch   |
|-----------|-------------|--------|
| 29        | google_apis | x86    |
| 30        | google_apis | x86    |
| 31        | google_apis | x86_64 |
| 33        | google_apis | x86_64 |
| 34        | google_apis | x86_64 |

An example of such an automation you can find in the [e2e-test workflows](https://github.com/FusionAuth/fusionauth-android-sdk/tree/main/.github/workflows) of the FusionAuth Android SDK.

#### Browser

Every time an emulator is started for the first time the Browser setup is not suitable for Testing as it will popup different modals like `Welcome to Chrome` or `Sign in to Chrome`, which are changing with every Chrome version.

To prevent chrome displaying any of these modals no matter the version, we execute the following commands once the emulator is started:

```
adb shell pm clear com.android.chrome
adb shell am set-debug-app --persistent com.android.chrome
adb shell 'echo "chrome --disable-fre --no-default-browser-check --no-first-run" > /data/local/tmp/chrome-command-line'
```

This is something you have to do only once for your emulator. But gets important during fully automated testing scenarios in a workflow, where the emulator is setup from scratch with every test.

With Android Studio you can start the emulator and start chrome manually to skip the modals, or use the `adb` command ([Android Debug Bridge](https://developer.android.com/tools/adb)) which can be found in your Android SDK installation:

```
$HOME/Android/Sdk/platform-tools/adb
```

#### Recording

All thought there are a lot of log details in your IDE and automated workflow to help to debug a failed test. It makes sense to watch the test in the emulator or to record the test as a video for further debugging input and context.

If you automate your test in a workflow you can pass this before starting the test command:

```
adb emu screenrecord start --time-limit 300 ./recording_video.webm
```

Depending on the build time of your App you might see only a Mobile screen for some time untill your App is started and displayed.

### Test Automation Setup

The `setUp` test initialization includes the following steps:
- Initializes `Intents`.
- Sets up `uiAutomation` to interact with the system UI.

### Automated End-to-End Test (`e2eTest`)

This test validates the application by executing the following steps, as a user would:

1. Taps the login button(`start_auth`).
2. Waits for the login form to appear.
3. Fills in the username(`richard@example.com`) and password(`password`) in the login form.
4. Submits the form using the enter key.
5. Waits for the token view to be shown(`sign_out`).
6. Checks the refresh token functionality by comparing the token expiration time before and after the refresh.
7. Taps the logout button(`sign_out`).
8. Waits for the login activity to be displayed again.
9. Repeats steps 3-8 for a second user, using username `gilfoyle@example.com` and their password.

This test is then repeated a second time with `RepeatRule` to ensure logout was successful and the login form is displayed again.

Helper methods are used throughout the test to interact with the UI:

- `closeKeyboardIfOpen`: Closes the system's keyboard if it's open, preventing it from obscuring the UI elements being interacted with.

### Test Teardown

The test concludes by releasing the initialized intents in the `tearDown` method.

### Constants

The constants used in the test include:
- `USERNAME`, `PASSWORD`: Credentials of the first user.
- `USERNAME2`, `PASSWORD2`: Credentials of the second user.
- `TIMEOUT_MILLIS`: The duration for which the test waits for the UI elements to appear in the system, expressed in milliseconds.

Please note that the username, password, and timeouts would typically be environment-specific and not part of the test code.
<!--
end::forDocSiteE2ETest[]
-->