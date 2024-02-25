# 11. iOS Bug Bounty

## iOS Bug Bounty Hunt

> ❗ Always refer to a HackerOne Bug Bounty program to find valid targets
>
> 🔗 [HackerOne iOS programs](https://hackerone.com/opportunities/all/search?asset_types=APPLE_STORE_APP_ID%2CTESTFLIGHT%2COTHER_IPA&ordering=Newest+programs)
>
> - 🧪  `e.g.` - [1.1.1.1 - com.cloudflare.1dot1dot1dot1](https://hackerone.com/cloudflare) Cloudflare iOS is in scope

**Static Analysis**

Install the app on the iPhone via the App Store

Pull the `ipa` from the App Store via `AnyTrans` or `iMazing` tools (Apple ID login necessary)

Import the `.ipa` into MobSF and analyze it

Rename the `.ipa` file to `.zip`, unzip it and look at the content

- `iTunesMetadata.plist` - general information, app name, etc
- Open the `.app` and look for the application content
  - `Info.plist` - look for URLs, api keys, IDs, strings etc
  - `.plist`, `.json`, config files
  - `Manifest.plist`

**Dynamic Analysis**

Jailbreak the iPhone, run the app and try to intercept its traffic using a proxy (BurpSuite, Proxyman for MacOS, Zaproxy, etc)

Proceed with SSL Unpinning using `Objection` if necessary

Dynamically test the app by joining an account, signing in and navigating the entire app

- Two accounts to test with are suggested, to test auth tokens, access to the other account, and different parts of the app

------

