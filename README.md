# 🎮 Godot Android Continuous Deployment 🚀

This repository provides a continuous deployment pipeline for Godot Android games using GitHub Actions. When you push to the `prod` branch, your APK is built and sent to you on Telegram. 📬

## 🛠️ Setup

1. **Use this template**: Press the button on the top right corner. Make sure you include all branches. 📂

2. **Add Your Game**: Place your Godot game inside the "game" folder. Make sure you don't delete the file named `export_presets.android.example`. You can edit it to include your own export configs later. Everything else in the game folder is related to the demo and can be deleted.🎲

3. **Set the Game Info**: You must set your current version of the game in the "version" file. The first line must be Version_Name-Version_Code(e.g. 1.0.1-12). Note that builds may fail with versions below 1. You also have to set the values for `APP_NAME`, `UNIQUE_NAME` and `PACKAGE_NAME`. 🏷️

4. **Create Android Keystore Files**: If you already have your Android keystore files, skip this step. If you don't, you can use `keytool` from JDK to create them. 🔑

   ```bash
   keytool -keyalg RSA -genkeypair -alias androiddebugkey -keypass android -keystore debug.keystore -storepass android -dname "CN=Android Debug,O=Android,C=US" -validity 9999 -deststoretype pkcs12
   ```
   ```bash
   keytool -v -genkey -keystore keystore.keystore -alias your_alias_name -keyalg RSA -validity 10000 -keypass your_keypass -storepass your_storepass
   ```
   
5. **Setup Repository Secrets for GitHub Actions**: Set the following secrets at https://github.com/yourhandle/yourrepo/settings/secrets/actions. 🕵️‍♀️

   - `ANDROID_KEYSTORE_BASE64`: base64 encoding of keystore.keystore (Can be done on bash with `base64 keystore.keystore -w 0`)
   - `ANDROID_KEYSTORE_DEBUG_BASE64`: base64 encoding of debug.keystore (Can be done on bash with `base64 debug.keystore -w 0`)
   - `ANDROID_KEYSTORE_ALIAS`: your_alias_name
   - `ANDROID_STORE_PASSWORD`: your_storepass
   - `ANDROID_KEYSTORE_PASSWORD`: your_keypass
   - `GROUP_ID`: Telegram ChatID of the group you want the bot to send the APK. You can find it by forwarding a message from the group/chat to this bot https://t.me/chatIDrobot
   - `TELEGRAM_TOKEN`: API Token of the bot that's going to send you the APK. You can create a bot by talking to https://t.me/BotFather. Also, make sure to add the bot to your group chat!

6. **Trigger the Build**: Make a change to the "version" file and when you push to `prod`, your APK is built and sent to you on Telegram. 📲

## 💡 Tips

- Do not commit to the `prod` branch! The correct way to use this is to merge your main branch to your `prod` branch and when the version number changes, this action will be triggered. 🔄
- There seems to be a bug that fails the build when `keypass` and `storepass` are different values. 🐛
- Repo secrets can cause so much trouble! Be careful and always double-check them! 🔐
- If you are on Windows, you can find keytool.exe here: `C:\Program Files\Java\jdk-*\bin\`

## 🤝 Community

Contributions are very welcome! Also if you had any problems implementing this pipeline for your app, start a discussion in this repository and I'll try to help you. 💬
