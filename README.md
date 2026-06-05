# hello-android
howto build an android helloworld app for your phone using the cmdline in a gentoo box.

1.  emerge --ask dev-java/openjdk-bin:17 dev-util/android-sdk-cmdline-tools dev-util/android-tools dev-java/gradle-bin
2.  gpasswd -a <USER> android
3.  sdkmanager --licenses
4.  export MY_ANDROID="~/your_local_path"
5.  mkdir -p "${MY_ANDROID}/hello-android/app/src/main/java/net.cloc3/helloworld; cd "${MY_ANDROID}/hello-android"
6.  add settings.gradle, build.gradle and app/build.gradle files
7.  put source code in ${MY_ANDROID}app/src/main/java/net.cloc3/helloworld/MainActivity.java
8.  make the manifest in ${MY_ANDROID}app/src/main/java/net/cloc3/helloworld/AndroidManifest.xml
9. gradle clean assembleDebug
10. cd ${MY_ANDROID}/app/build/outputs/apk/debug
11. python3 -m http.server 8080 # opens an http server on your pc
12. open the brower on your phone at the address: http://<yourPcLocalAddress>:8080
13. download the file helloworld-debug.apk
14. double clic over helloworld-debug.apk and follow installing instructions of your device.
15. test the debug version of your app. if it runs, go away to build the release version.
16. mkdir ~/androidKeys; chown 700 ~/androidKeys;
17. keytool -genkey -v -keystore ~/androidKeys/helloworld.jks -keyalg RSA -keysize 2048 -validity 10000 -alias helloKey
18. echo "yourPassword" > ~/androidKeys/${USER}; chown 600 ~/androidKeys/${USER}
19. keytool -genkey -v -keystore ~/androidKeys/helloworld.jks -keyalg RSA -keysize 2048 -validity 10000 -alias helloKey
20. cd ~/github/hello-android; gradle clean assembleRelease \
    -Pandroid.injected.signing.store.file=${HOME}/androidKeys/helloworld.jks \
    -Pandroid.injected.signing.store.password=$(<~/androidKeys/${USER}) \
    -Pandroid.injected.signing.key.alias=helloKey \
    -Pandroid.injected.signing.key.password=$(<~/androidKeys/${USER})
21. cd app/build/outputs/apk/release; python3 -m http.server 8080

enjoy your helloworld.apk available for download on your phone.
