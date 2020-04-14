# Prepare Android app for Google Play

## Checklist

### **Launcher Icons**

Size your image assets.

* mipmap-mdpi: 48 x 48
* mipmap-hdpi:72 x 72
* mipmap-xhdpi: 96 x 96
* mipmap-xxhdpi: 144 x 144

Replace the icons in these folder with your own icons.

* myawesomeproject/android/app/src/main/res/mipmap-hdpi/ic\_launcher.png
* myawesomeproject/android/app/src/main/res/mipmap-mdpi/ic\_launcher.png
* myawesomeproject/android/app/src/main/res/mipmap-xhdpi/ic\_launcher.png
* myawesomeproject/android/app/src/main/res/mipmap-xxhdpi/ic\_launcher.png

### SplashScreen

[https://medium.com/@101/splash-screen-in-android-769d3b0bafd0\#.cep53047v](https://medium.com/@101/splash-screen-in-android-769d3b0bafd0#.cep53047v)

### App Signing

Make sure to have pepk.jar in your directory and the release keystore

```text
├── pepk.jar
├── sixt-release-key.keystore
└── sixt_release_encrypted_private_key
```

```text
java -jar pepk.jar --keystore=sixt-release-key.keystore --alias=sixt-key-alias --output=sixt_release_encrypted_private_key --encryptionkey=eb10fe8f7c7c9df715022017b00c6471f8ba8170b13049a11e6c09ffe3056a104a3bbe4ac5a955f4ba4fe93fc8cef27558a3eb9d2a529a2092761fb833b656cd48b9de6a
```

