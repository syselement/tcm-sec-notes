# 4. Android Static Analysis

## Injured Android

> 🔗 [InjuredAndroid](https://github.com/B3nac/InjuredAndroid)
>
> 🔗 [InjuredAndroid walk-throughs](https://github.com/B3nac/InjuredAndroid/blob/master/InjuredAndroid-FlagWalkthroughs.md)
>
> 1. Download the latest release [injuredandroid.apk](https://github.com/B3nac/InjuredAndroid/releases/tag/v1.0.12) from the releases or Google Play.
> 2. Enable USB debugging on your Android test phone.
> 3. Connect your phone and your pc with a USB cable.
> 4. Install via `adb` if installing from releases. (You need to use the absolute path to the .apk file or be in the same directory)
>
> 📌 Packages name: `b3nac.injuredandroid`

`InjuredAndroid` is a *a vulnerable Android application that shows simple examples of vulnerabilities in a ctf style*.

```bash
mkdir ~/apks
cd ~/apks

wget -O InjuredAndroid.apk https://github.com/B3nac/InjuredAndroid/releases/download/v1.0.12/InjuredAndroid-1.0.12-release.apk

adb install InjuredAndroid.apk
```

> **EXTRA**
>
> - Check for app's path (and pull `base.apk` if necessary)
>
> ```bash
> adb shell
> pm list packages | grep injured
> 	package:b3nac.injuredandroid
> pm path b3nac.injuredandroid
> 	package:/data/app/b3nac.injuredandroid-Ms4WCz1i9EefZuncV6Xnpw==/base.apk
> 
> # Pull the apk from the path into the host OS
> exit # the adb shell
> adb pull /data/app/b3nac.injuredandroid-Ms4WCz1i9EefZuncV6Xnpw==/base.apk InjuredAndroid_base.apk
> ```

🔬 Open `InjuredAndroid.apk` with `jadx-gui` for analysis.

![](.gitbook/assets/2024-01-05_20-20-52_292.png)

## [AndroidManifest.xml](https://developer.android.com/guide/topics/manifest/manifest-intro)

The `AndroidManifest.xml` file contains essential information about the app, declaring the components of the app like minSDKVersion, Permissions, Activities, Services, Content Providers, Intent Filters, Debugging Info, etc.

- [Permissions](https://developer.android.com/reference/android/Manifest.permission) (Network, Internet, Phone, RW External Storage, etc)
- [Activities](https://developer.android.com/guide/components/activities/intro-activities) (UI elements for user's interaction)
  - hidden screens protected with *intent-filters*
  - outside exposed activity - `android:exported="true"` 
- [Content Providers](https://developer.android.com/guide/topics/providers/content-providers) (sharing data between apps)
  - dangerous if exported

![AndroidManifest.xml](.gitbook/assets/2024-01-05_19-44-52_291.png)

- Look for
  - `minSdkVersion`
  - `android.permission`
  - `activity` & `provider`
  - `android:exported` flags
  - various normal strings
  - backup options

```xml
<!-- # Min and Target SDK  -->
<uses-sdk android:minSdkVersion="21" android:targetSdkVersion="29"/>

<!-- # Permissions -->
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>

<!-- # Activities -->
<activity android:theme="@style/AppTheme.NoActionBar" android:label="@string/title_activity_flag_eighteen" android:name="b3nac.injuredandroid.FlagEighteenActivity" android:exported="true"/>
<provider android:name="androidx.core.content.FileProvider" android:exported="false" android:authorities="b3nac.injuredandroid.fileprovider" android:grantUriPermissions="true">
    <meta-data android:name="android.support.FILE_PROVIDER_PATHS" android:resource="@xml/file_paths"/>
</provider>
<activity android:theme="@style/AppTheme.NoActionBar" android:label="@string/title_activity_flag_seventeen" android:name="b3nac.injuredandroid.FlagSeventeenActivity"/>
```

---

## Manual Static Analysis

> [Apktool CLI Parameters](https://apktool.org/docs/cli-parameters)

 🔬Decompile the app using [`apktool`](https://apktool.org/)

```bash
apktool d InjuredAndroid.apk
```

![](.gitbook/assets/2024-01-05_20-24-11_293.png)

- `lib` - directory where source code is changed for injection (`.so` files = shared objects)
- `original` - check for sensitive information in the files, `AndroidManifest.xml`
- `res` - resources directory, check the `values/strings.xml` file
- `smali` - directory where the app's source code is stored (Smali is not human readable, use `dex2jar` converter or  `jadx-gui` that decompile the files directly into Java)
- `AndroidManifest.xml` - important for static analysis

```bash
# Read .so files' strings, e.g.
strings ~/apks/InjuredAndroid/lib/x86_64/libencrypt.so

# Read strings.xml, AndroidManifest.xml files
cat ~/apks/InjuredAndroid/res/values/strings.xml

cat ~/apks/InjuredAndroid/AndroidManifest.xml
```

##  Hardcoded Strings

**Hardcoded strings** refer to strings or text values that are directly written into the source code of a program, typically without being stored in a separate configuration file or resource file.

- Found in `Resources/strings.xml` and in Activity source code

Threat vector can be login bypass with hardcoded credentials, exposed URLs and API Keys, Firebase URLs, etc. 

🔬 Open `InjuredAndroid.apk` with `jadx-gui` for analysis, open `Resources/resources.arsc/res/values/*.xml` files and search for hardcoded strings.

![strings.xml](.gitbook/assets/2024-01-06_11-24-45_295.png)

```bash
# strings.xml

<string name="AWS_ID" />
<string name="AWS_SECRET" />

<string name="firebase_database_url">https://injuredandroid.firebaseio.com</string>

<string name="google_api_key">AIzaSyCUImEIOSvqAswLqFak75xhskkB6illd7A</string>
<string name="google_app_id">1:430943006316:android:d97db57e11e42a1a037249</string>
<string name="google_crash_reporting_api_key">AIzaSyCUImEIOSvqAswLqFak75xhskkB6illd7A</string>
<string name="google_storage_bucket">injuredandroid.appspot.com</string>

```

Use the `Text search` tool to search the source code. Search for useful info like API, URLs, ids, passwords, SQL, Firebase, HTTP/HTTPS, secrets, sensitive data, etc.

![](.gitbook/assets/2024-01-06_11-36-39_296.png)

## Injured Android Flags

### Flag1

Using `jadx-gui` open `Source code\b3nac.injuredandroid\FlagOneLoginActivity`

![FlagOneLoginActivity](.gitbook/assets/2024-01-06_11-45-43_297.png)

> The `submitFlag` method verifies user input against the string "**F1ag_0n3**" and, if matched, navigates to the `FlagOneSuccess` activity, with associated actions related to flags and UI.

> 🚩 `F1ag_0n3` - Flag found in the `submitFlag` method.

### Flag2

*There is a way to bypass the main activity and invoke other activities that are exported*. Activities can be accessed with `adb`.

In `jadx-guid`, search for `android:exported="true"` in the `AndroidManifest.xml` file.

- Take the `<activity android:name="b3nac.injuredandroid.b25lActivity" android:exported="true"/>` activity.
- `b3nac.injuredandroid.b25lActivity` can be accessed from anywhere on the phone

```bash
# Open terminal
adb shell
su

# Start the activity in the app
am start b3nac.injuredandroid/.b25lActivity
```

![](.gitbook/assets/2024-01-06_12-03-09_299.png)

> 🚩 `S3c0nd_F1ag`

### Flag3

*R stands for Resources. Check for `xml` files.*

Using `jadx-gui` open `Source code\b3nac.injuredandroid\FlagThreeActivity`

![FlagThreeActivity](.gitbook/assets/2024-01-06_12-08-00_300.png)

> The `submitFlag` method checks if the user input matches the string resource `cmVzb3VyY2VzX3lv`. If true, it directs to the `FlagOneSuccess` activity, with related flag handling and UI.
>
> - The hardcoded string (containing the flag) is stored in the `strings.xml` file - easy to reverse engineer.

Search for the `cmVzb3VyY2VzX3lv` string in the `strings.xml` file.

![](.gitbook/assets/2024-01-06_12-10-03_301.png)

> 🚩 `F1ag_thr33`

### Flag4

*Classes and imports.*

Using `jadx-gui` open `Source code\b3nac.injuredandroid\FlagFourActivity`

![FlagFourActivity](.gitbook/assets/2024-01-06_12-13-32_302.png)

> The `submitFlag` method compares the user input with a string decoded from a byte array obtained by the `decoder.getData()` method. If there's a match, it launches the `FlagOneSuccess` activity, updating flag status and UI.

Search the obfuscated `g` class.

![](.gitbook/assets/2024-01-06_12-25-25_303.png)

- [CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)&input=TkY5dmRtVnlaRzl1WlY5dmJXVnNaWFJ6) online tool can be used to decode the string or Terminal `base64 -d` command.

```bash
echo "NF9vdmVyZG9uZV9vbWVsZXRz" | base64 -d
	4_overdone_omelets
```

> The `g` class provides a byte array obtained by decoding a Base64-encoded string. The decoded byte array represents the value "**4_overdone_omelets**" and is accessible through the `a()` method.

> 🚩 `4_overdone_omelets`

### Flag8

*AWS Cli, Profiles and Credentials.*

> 🔗 Tools
>
> - [cloud_enum](https://github.com/initstring/cloud_enum) - *Multi-cloud OSINT tool. Enumerate public resources in AWS, Azure, and Google Cloud.*
> - [AWS CLI](https://aws.amazon.com/cli/)

```bash
# cloud_enum

cd ~/repo
git clone https://github.com/initstring/cloud_enum.git
cd ~/repo/cloud_enum
pip3 install -r requirements.txt
```

- Run `cloudenum.py`

```bash
python3 cloud_enum.py -k injuredandroid
```

Check the [http://injuredandroid.s3.amazonaws.com/](http://injuredandroid.s3.amazonaws.com/) bucket, the enumeration reveals the flag. Instead of attempting to crack the AWS login secret, one of the discovered URLs will get the flag.

![](.gitbook/assets/2024-01-06_12-49-23_304.png)

> 🚩 `C10ud_S3cur1ty_lol`

> EXTRA - Previous AWS with ID & SECRET strings.
>
> ```bash
> # If bucket exists and access is necessary with ID & SECRET, AWS CLI can be used
> sudo apt install -y awscli
> 
> aws configure --profile injuredandroid
> aws s3 ls s3://injuredandroid --profile injuredandroid
> ```

### Flag9

*Use .json trick with database URL.*

> 🔗 Tools
>
> - [firebaseEnum](https://github.com/Sambal0x/firebaseEnum) - *Enumerate exposed firebase databases*

Open `strings.xml` and search for `firebase`.

Found `<string name="firebase_database_url">https://injuredandroid.firebaseio.com</string>`.

Open [https://injuredandroid.firebaseio.com](https://injuredandroid.firebaseio.com) in a browser - Google login.

> - Enumerating Firebase database (if not found in the strings)
>
> ```bash
> # firebaseEnum
> 
> cd ~/repo
> git clone https://github.com/Sambal0x/firebaseEnum.git
> cd ~/repo/firebaseEnum
> pip3 install -r requirements.txt
> ```
>
> - Run `firebaseEnum.py`
>
> ```bash
> python3 firebaseEnum.py -k injuredandroid
> # nothing found
> ```
>
> ![](.gitbook/assets/2024-01-06_13-17-35_308.png)

Using `jadx-gui` open `Source code\b3nac.injuredandroid\FlagNineFirebaseActivity`

![FlagNineFirebaseActivity](.gitbook/assets/2024-01-06_13-11-56_305.png)

![FlagNineFirebaseActivity](.gitbook/assets/2024-01-06_13-12-40_306.png)

```bash
# Decode the string found in the FlagNineFirebaseActivity method
echo "ZmxhZ3Mv" | base64 -d
	flags/
```

Open [https://injuredandroid.firebaseio.com/flags/.json](https://injuredandroid.firebaseio.com/flags/.json) in the browser and find the flag

- this Firebase directory is not protected.

![](.gitbook/assets/2024-01-06_13-14-56_307.png)

Base64 encode the found flag `[nine!_flag]` and input in the app.

```bash
echo -n '[nine!_flag]' | base64
	W25pbmUhX2ZsYWdd
```

> 🚩 `W25pbmUhX2ZsYWdd`



---

## MobSF Automated Analysis

> 🔗 [MobSF](https://github.com/MobSF/Mobile-Security-Framework-MobSF) - *a security research platform for mobile applications in Android, iOS and Windows Mobile*

Run `MobSF` (with Docker) and import the `InjuredAndroid.apk` into it for Static Analysis.

```bash
docker run -it --rm --name mobsf -p 8000:8000 -v ~/docker/mobsf:/home/mobsf/.MobSF opensecurity/mobile-security-framework-mobsf:latest
```

![MobSF - InjuredAndroid.apk](.gitbook/assets/2024-01-06_14-53-44_321.png)

![MobSF Application Security Scorecard - InjuredAndroid 1.0.9](.gitbook/assets/2024-01-06_14-37-31_311.png)

![](.gitbook/assets/2024-01-06_14-38-13_312.png)

![](.gitbook/assets/2024-01-06_14-38-34_313.png)

![](.gitbook/assets/2024-01-06_14-39-52_314.png)

![](.gitbook/assets/2024-01-06_14-47-50_318.png)

![](.gitbook/assets/2024-01-06_14-49-11_319.png)

![](.gitbook/assets/2024-01-06_14-40-58_315.png)

![](.gitbook/assets/2024-01-06_14-50-11_320.png)

![](.gitbook/assets/2024-01-06_14-41-44_316.png)

![](.gitbook/assets/2024-01-06_14-42-41_317.png)



------

