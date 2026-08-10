# Fireside Mobile SDK

Run a Fireside Interview inside your own mobile app.

This repository holds two things: the **licence**, and the package names below.
The SDK itself is published to each platform's normal package registry.

**Ask your Fireside representative for the full install guide.** It is not
published with the package. Without it, the four points below are the ones that
will block you.

## Install

| Platform | Where it comes from | Package name |
|---|---|---|
| Android | Maven Central | `ai.fireside:sdk-android:<version>` |
| iOS | Swift Package Manager | `https://github.com/fireside-hq/fireside-ios-sdk.git` |

Both have the same version number.

**Flutter is not published.** This release puts no `fireside_flutter` package on
pub.dev. So there is no package name to install. Talk to your Fireside
representative if you need Flutter.

## Before it will work

### Send your app ids to Fireside — do this first

The ids Fireside needs are your Android **`applicationId`** and your iOS **bundle
identifier**, on whichever platforms you ship.

**Your key works only from the app ids that are on it. Only Fireside can add an
app id.** "Register" means Fireside puts your app id on the list of app ids
stored on your key. There is no web page where you can do this yourself. Send
your ids to your Fireside representative. Then wait until they tell you the ids
are on your key. Ask them how long that usually takes.

Two things here are easy to get wrong:

- **Each platform needs its own registration.** You may use the same id on
  Android and on iOS. That still counts as two registrations. Send the id once
  for each platform you ship.
- **Each build variant is a different id.** A build variant is one build of your
  app, like debug or staging. If a variant adds a suffix such as `.debug` or
  `.staging`, its id is not the id you registered. Send us every id you ship.

If the id of the running app is not on your key, the build still works. Nothing
looks wrong. But the SDK makes one call of its own, started by `init`, and that
call checks your key and your app id together. The server answers that call with
an HTTP **403** error. Your code never sees it. What you do see is this: every
later call to `present` and `fetchInterviewStatuses` gives back an **integration
error**.

A wrong key gives you the **same** integration error. So does a key that Fireside
turned off. The only difference is that the server answers with a 401 error, not
a 403. You cannot see that either.

So the error does not tell you which problem you have. Do this: first check the
app id of the exact build variant you are running. If that id is right, then
check the key.

### Then three more

1. **Declare camera and microphone yourself.** The SDK declares neither of them,
   so you add them in your own app. **Get this wrong and it is not a small
   problem: on iOS your app crashes the first time the interview uses the camera
   or the microphone, and on Android the prompt never appears and any question
   that records the participant fails with no error message.** The install guide
   lists the exact keys and permissions for each platform.
2. **Meet the minimum versions.** The SDK sets a floor for the iOS deployment
   target, and for Android `minSdk`, `compileSdk`, and the JDK. The install guide
   has the numbers. Below any of them the build fails.
3. **Use the right key for each build.**

**Fireside gives you two keys, not one.** One key starts with `test_`. The other
starts with `live_`. The same two keys work on Android and on iOS.

Use the **`test_`** key in your debug builds. Use the **`live_`** key in your
release builds.

Both keys reach the same Fireside servers. The difference is where your interview
data is kept. Data that arrives with a `test_` key is stored apart from your real
data, so your testing does not mix into your real numbers.

   The key is publishable, which means it is safe to ship inside your app. What
   protects it is the list of app ids above, not secrecy.

## Licence

Commercial. See [`LICENSE`](LICENSE).

The Software is available to Fireside customers. It is available in compiled form
only, which means built files and no source code. Contact your Fireside
representative for licensing.
