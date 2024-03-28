# Quickstart: Android app with FusionAuth Android SDK

This repository contains an Android app extracted from the FusionAuth Android SDK that works with a locally running instance of [FusionAuth](https://fusionauth.io/), the authentication and authorization platform.

## Setup

### Prerequisites
- [Android Studio](https://developer.android.com/studio): The official IDE for Android will help you develop and install necessary tools to set it up.
  - At least Java 17 (which you can install via Android Studio)
- [Docker](https://www.docker.com): The quickest way to stand up FusionAuth. Ensure you also have [docker compose](https://docs.docker.com/compose/) installed.
  - (Alternatively, you can [Install FusionAuth Manually](https://fusionauth.io/docs/v1/tech/installation-guide/)).


### FusionAuth Installation via Docker

In the root of this project directory (next to this README) is a [FusionAuth folder](./fusionauth). Assuming you have Docker installed on your machine, you can stand up FusionAuth up on your machine with:

```
cd fusionauth/
docker compose up -d
```

The FusionAuth configuration files also make use of a unique feature of FusionAuth, called [Kickstart](https://fusionauth.io/docs/v1/tech/installation-guide/kickstart): when FusionAuth comes up for the first time, it will look at the [Kickstart file](./kickstart/kickstart.json) and mimic API calls to configure FusionAuth for use when it is first run. 

> **NOTE**: If you ever want to reset the FusionAuth system, delete the volumes created by docker compose by executing `docker compose down -v`. 

FusionAuth will be initially configured with these settings:

* tbd

You can log into the [FusionAuth admin UI](http://localhost:9011/admin) and look around if you want, but with Docker/Kickstart you don't need to.

### Running the Android App

tbd

#### Automated End 2 End Test

tbd

### Further Information

tbd

### Troubleshooting

tbd

<!--
How to create the example App manually:

The example App is a copy from https://github.com/FusionAuth/fusionauth-android-sdk/tree/main/app by:

1. Create a new Android project with Kotlin and Gradle
2. copy the app/src folder from the sdk in to the app/ folder
3. copy the app/build.gradle.kts from the sdk in to the app/ folder
4. remove lint configuration for sarifReport from it
5. copy the fusionauth/<latest version>/ from the sdk to fusionauth/
6. make sure gradlew is on the same version as the sdk by running e.g. ./gradlew wrapper --gradle-version 8.6
-->