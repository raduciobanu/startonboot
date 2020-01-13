# startonboot
**Sample Android project for starting an app on boot**

Here are the steps to start an Android app on boot:

1. Create a broadcast receiver (as seen in StartMyActivityAtBootReceiver.java)
2. In the manifest file (AndroidManifest.xml), add the following permission:
```
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
```

3. Also in the manifest, add the broadcast receiver as shown below (you need to mark it as enabled and exported, and set the permission, and then you specify the intents it receives broadcasts from - all of them refer to booting or rebooting the device):
```
<receiver android:name="ro.pub.acs.startonboot.StartMyActivityAtBootReceiver" android:enabled="true" android:exported="true" android:permission="android.permission.RECEIVE_BOOT_COMPLETED">
    <intent-filter>
        <category android:name="android.intent.category.DEFAULT" />
        <action android:name="android.intent.action.BOOT_COMPLETED" />
        <action android:name="android.intent.action.ACTION_BOOT_COMPLETED" />
        <action android:name="android.intent.action.QUICKBOOT_POWERON" />
        <action android:name="com.htc.intent.action.QUICKBOOT_POWERON" />
    </intent-filter>
</receiver>
```
4. Then, go back to your BroadcastReceiver (StartMyActivityAtBootReceiver.java) and start the main activity on boot, by writing the following in the `onReceive` function:

```
if (Intent.ACTION_BOOT_COMPLETED.equals(intent.getAction())) {
    Log.d(TAG, "Receive boot completed broadcast");
    Intent activityIntent = new Intent(context, MainActivity.class);
    activityIntent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    context.startActivity(activityIntent);
}
```

5. Finally, after installing the app, you need to start it manually at least once, in order for Android to accept the broadcast receiver and send the intents
