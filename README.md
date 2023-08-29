# CoLabsPlus

## MacOS Distribution Process

*CoLabs repo*

### Store credentials in keychain

1- Create an app-specific password on app store connect (instructions can be found by googling)

```
xcrun altool --store-password-in-keychain-item "Notarization-PASSWORD" -u "login@appleid.email" -p "YOUR-PASS-WORD-HERE"
```

Replace these:

- `login@appleid.email` with your Apple ID login
- `YOUR-PASS-WORD-HERE` with the app-specific passwrd you generated (you have to copy it right after generating)

2- Make sure you have these 2 certs and 2 private keys:

<img width="372" alt="image" src="https://github.com/AmunsonAudio/CoLabsPlus/assets/5114111/285e120c-ab8a-4c65-b9a9-dc25786b0040">

Otherwise you'll get random codesigning errors. 

### Distribution script

You'll need to install **Packages** for the packagebuild command and **DropDMG** ($25 download from Mac App Store) for the dropdmg command. 

```
cd release

APPLEASC=M3Y658T3JB APPLEID=dennis.s.lysenko@gmail.com INSTSIGNID=B7CFE6EC9EAE1AE69B4DA6BA9E417C579491613C ./distmac.sh 1.0
```

Replace these:

- `APPLEASC`: refers to apple dev team (Amunson Audio LLC)
- `APPLEID`: replace with your own apple ID email of your account on App Store Connect

**INSTSIGNID** is the SHA-1 hash of the Developer ID Installer certificate above. Do not replace this unless it changes. In which case you can look up how to get the SHA-1 hash from Keychain Access.

At the end, DropDMG will open, just direct it to the `release` folder and it'll continue running the script.
