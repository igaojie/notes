# Android

## adb

0. [How to use ADB Shell when Multiple Devices are connected? Fails with "error: more than one device and emulator"](https://stackoverflow.com/questions/14654718/how-to-use-adb-shell-when-multiple-devices-are-connected-fails-with-error-mor)

1. adb devices

   ```shell
   $ cd /Library/Android/sdk/platform-tools
   $ ./adb devices
   List of devices attached
   
   9JB7N19404001426	device
   emulator-5554	device
   ```

   

2. adb shell

   ```shell
   $ ./adb  -s 9JB7N19404001426 shell
   HWLYA:/ $
   HWLYA:/ $
   ```

   

## Storage

0. https://developer.android.com/training/data-storage/app-specific

1. 写文件

   ```java
   FileOutputStream out = null;
   BufferedWriter writer = null;
   try {
     out = context.openFileOutput("ksAd.log", Context.MODE_APPEND);
     writer = new BufferedWriter(new OutputStreamWriter(out));
     writer.write(jsonString);
   }catch (IOException e){
     e.printStackTrace();
   } finally {
     try {
       if (writer != null){
         writer.close();
       }
     } catch (IOException e){
       e.printStackTrace();
     }
   }
   ```

   

2. 查看文件

   Android Studio 右下角 Device File Explorer  -> data -> data -> com.xx.xxx(包名)-> files  -> ksAd.log 能查看存储的文件内容。 

3. 2

## 常见问题

1. java.lang.SecurityException: getUniqueDeviceId: The user 12029 does not meet the requirements to access device identifiers.

   ```java
   @SuppressLint("MissingPermission")
       public static String getIMEI(Context context) {
           String deviceId;
   
           if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
               deviceId = Settings.Secure.getString(context.getContentResolver(), Settings.Secure.ANDROID_ID);
           } else {
               final TelephonyManager mTelephony = (TelephonyManager) context.getSystemService(Context.TELEPHONY_SERVICE);
               if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
                   if (context.checkSelfPermission(Manifest.permission.READ_PHONE_STATE) != PackageManager.PERMISSION_GRANTED) {
                       return "";
                   }
               }
               assert mTelephony != null;
               if (mTelephony.getDeviceId() != null) {
                   if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
                       deviceId = mTelephony.getImei();
                   } else {
                       deviceId = mTelephony.getDeviceId();
                   }
               } else {
                   deviceId = Settings.Secure.getString(context.getContentResolver(), Settings.Secure.ANDROID_ID);
               }
           }
           return deviceId;
       }
   ```

   

2. Android Studio Arctic Fox | 2020.3.1 中文显示成方块□□解决方案

   设置 Preferences -> Appearance & Behavior -> Appearance -> 勾选 `Use custom font`，点击 OK。

3. xx

4. 1

5. 2

6. 3

7. 4

8. 4

9. 4

10. 4

11. 4