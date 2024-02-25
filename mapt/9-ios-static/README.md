# 9. iOS Static Analysis

> ❗ Always refer to a HackerOne Bug Bounty program to find valid targets
>
> 🔗 [HackerOne iOS programs](https://hackerone.com/opportunities/all/search?asset_types=APPLE_STORE_APP_ID%2CTESTFLIGHT%2COTHER_IPA&ordering=Newest+programs)
>
> - 🧪  `e.g.` - [1.1.1.1 - com.cloudflare.1dot1dot1dot1](https://hackerone.com/cloudflare) Cloudflare iOS is in scope

- Unzip an `.ipa` file and check the various files inside it
  - Inside the `Payload` folder check for `plist` files, `Framekworks` folder for app's source code, other `json` files on different folders
  - Check for hardcoded strings in the files.

![](.gitbook/assets/2024-02-25_00-38-23_421.png)

![](.gitbook/assets/2024-02-25_00-47-43_422.png)

![Info.plist](.gitbook/assets/2024-02-25_00-48-49_423.png)

- Run MobSF and import the `.ipa` file into it for local static analysis
  - Check the sections, look for different files, strings and look through interesting information about the app

![MobSF - 1.1.1.1.ipa](.gitbook/assets/2024-02-25_01-25-16_424.png)

![MobSF Application Security Scorecard - 1.1.1.1 6.23](.gitbook/assets/2024-02-25_01-36-57_428.png)

![](.gitbook/assets/2024-02-25_01-28-44_425.png)

![](.gitbook/assets/2024-02-25_01-29-28_426.png)

![](.gitbook/assets/2024-02-25_01-31-45_427.png)

------

