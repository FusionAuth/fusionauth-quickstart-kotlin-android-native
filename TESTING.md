# Testing
<!--
tag::forDocSiteTesting[]
-->
Using OAuth2 with a browser popup for Authentication can be tricky to use during automated testing in an [Android Emulator](https://developer.android.com/studio/run/emulator)., we've made some efforts in the development of the FusionAuth Android SDK and made the test available in this Quickstart.

In this doc, we go in to detail on how to create the test with a real FusionAuth backend and what to look for in an automated testing environment.
<!--
end::forDocSiteTesting[]
-->
## Test
<!--
tag::forDocSiteTest[]
-->
TODO

The Quickstart includes a full End 2 End Test that uses all the different functionalities provided by the example App.
<!--
end::forDocSiteTest[]
-->
## Kickstart Details
<!--
tag::forDocSiteKickstart[]
-->
FusionAuth will be initially configured with these settings:

* An update to the Tenant Theme including the ChangeBank Theme to make the look and feel of the login the same as the ChangeBank App.
* The Tenant Issuer will be set to `http://10.0.2.2:9011` to allow for testing with [Android Emulator](https://developer.android.com/studio/run/emulator).
* Two Applications `Example Android App` and `Secondary Application` to test users with and without access to the Android App.
* Your client secret is: `super-secret-secret-that-should-be-regenerated-for-production`
* You'll have three example usernames available with slightly different user profiles `richard@example.com`, `monica@example.com` and `gilfoyle@example.com`. All having access to `Example Android App` where the password for all three is `password`.
* And an example user without access `erlich@example.com` to the `Example Android App`
* Your FusionAuth admin username is `admin@example.com` and your password is `password`.
* Your fusionAuthBaseUrl to access FusionAuth is `http://localhost:9011/`
<!--
end::forDocSiteKickstart[]
-->
## Automated End 2 End Test
<!--
tag::forDocSiteE2ETest[]
-->
TODO

- Freshly setup Android Emulator Browser issue.
- Which image to use.
- Recording UI.

```
adb shell pm clear com.android.chrome
adb shell am set-debug-app --persistent com.android.chrome
adb shell 'echo "chrome --disable-fre --no-default-browser-check --no-first-run" > /data/local/tmp/chrome-command-line'
adb emu screenrecord start --time-limit 300 ./recording_video.webm
./gradlew clean connectedAndroidTest
```

<!--
end::forDocSiteE2ETest[]
-->