# MacOS Distribution Process

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

# AOO Server Setup Process

:ballot_box_with_check:  Set up essej/aooserver

:ballot_box_with_check: Daemonized with below instructions

:ballot_box_with_check: Logs: journalctl -u aooserver

:ballot_box_with_check: Elastic IP allocated: 18.190.82.203

To create a persistent systemd service unit file for your binary `/usr/local/bin/aooserver` and configure it to start on server boot, follow these steps:

1. **Create the Service Unit File**:
   Open a terminal and use a text editor to create the service unit file:
   ```bash
   sudo nano /etc/systemd/system/aooserver.service
   ```
2. **Add Configuration**:
   Inside the text editor, add the following configuration to the `aooserver.service` file:
   ```ini
   [Unit]
   Description=Your Service Description
   After=network.target

   [Service]
   Type=simple
   ExecStart=/usr/local/bin/aooserver
   Restart=always
   RestartSec=3

   [Install]
   WantedBy=multi-user.target
   ```
   Replace `Your Service Description` with a meaningful description for your service.
3. **Reload systemd**:
   After creating or editing the service unit file, reload systemd to make it aware of the new service:
   ```bash
   sudo systemctl daemon-reload
   ```
4. **Enable and Start the Service**:
   Enable the service to start on server boot and start it:
   ```bash
   sudo systemctl enable aooserver.service
   sudo systemctl start aooserver.service
   ```
5. **Check Service Status**:
   You can check the status of your service using:
   ```bash
   sudo systemctl status aooserver.service
   ```
6. **Restart, Stop, and Disable**:
   - To restart the service: `sudo systemctl restart aooserver.service`
   - To stop the service: `sudo systemctl stop aooserver.service`
   - To disable the service from starting on boot: `sudo systemctl disable aooserver.service`

Now your `aooserver` binary will run as a persistent systemd service and will automatically start when the server boots up. The service will also be managed by systemd, providing features like automatic restarts in case of failure.

Please replace placeholders like service descriptions and paths with your actual values.
