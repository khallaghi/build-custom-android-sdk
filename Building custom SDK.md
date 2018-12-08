# Building custom SDK
Logs of building custom SDK


## first: modifying proper files to add new API
I want to modify the `setWifiEnabled` function in `WifiManager` class in framework. actually I want to add new function with same name but with different input variables to get password string too.
default signature of function is:
```java 
public boolean setWifiEnabled(boolean enabled);
```

 and I want to add new function with this signature: 
```java
public boolean setWifiEnabledWithPassword(boolean enabled, String password);
```

and after that modify the original function to do nothing when called but for sake of backward compatibility I keep the function in the framework.

so there is some steps to do:
* first trying to add the `setWifiEnabledWithPassword` completely like the `setWifiEnabled` function in all the source code and check the functionality of both functions.
* compile the new sdk with added function and use it in Android studio to compile the proper applications.
* modify the added function with the architecture that I designed and complete the function then check the functionality.
* change old function to do nothing and turn off or even delete proper test cases that check the functionality of function
* check all the functionalities

## first: add new function
so at the first we have to add the new function with the following signature:
```java
public boolean setWifiEnabledWithPassword(boolean enabled, String password);
```

we know the path of `WifiManager` class in AOSP it’s in the:
`./frameworks/base/wifi/java/android/net/wifi/WifiManager.java`

after adding the new function below the old one you should add the new function to other parts of source code.

simply use `grep` to do so:
```bash
grep -irl "WifiManager"
```

so here is the result in `android-x86-marshmallow`

```bash

./external/robolectric/src/test/java/com/xtremelabs/robolectric/shadows/WifiManagerTest.java:    public void setWifiEnabled_shouldThrowSecurityExceptionWhenAccessWifiStatePermissionNotGranted() throws Exception {
./external/robolectric/src/test/java/com/xtremelabs/robolectric/shadows/WifiManagerTest.java:        wifiManager.setWifiEnabled(true);
./external/robolectric/src/main/java/com/xtremelabs/robolectric/shadows/ShadowWifiManager.java:    public boolean setWifiEnabled(boolean wifiEnabled) {
./packages/apps/TvSettings/Settings/src/com/android/tv/settings/connectivity/NetworkActivity.java:                mConnectivityListener.setWifiEnabled(true);
./packages/apps/TvSettings/Settings/src/com/android/tv/settings/connectivity/NetworkActivity.java:                mConnectivityListener.setWifiEnabled(false);
./packages/apps/TvSettings/Settings/src/com/android/tv/settings/connectivity/ConnectivityListener.java:    public void setWifiEnabled(boolean enable) {
./packages/apps/TvSettings/Settings/src/com/android/tv/settings/connectivity/ConnectivityListener.java:        mWifiManager.setWifiEnabled(enable);
./packages/apps/ManagedProvisioning/src/com/android/managedprovisioning/WifiConfig.java:            mWifiManager.setWifiEnabled(true);
./packages/apps/ManagedProvisioning/src/com/android/managedprovisioning/task/AddWifiNetworkTask.java:                && (mWifiManager.isWifiEnabled() || mWifiManager.setWifiEnabled(true));
./packages/apps/Settings/src/com/android/settings/wifi/WifiEnabler.java:        if (!mWifiManager.setWifiEnabled(isChecked)) {
./packages/apps/Settings/src/com/android/settings/widget/SettingsAppWidgetProvider.java:                    wifiManager.setWifiEnabled(desiredState);
./development/apps/Development/src/com/android/development/Connectivity.java:                                mWm.setWifiEnabled(false);
./development/apps/Development/src/com/android/development/Connectivity.java:                                mWm.setWifiEnabled(true);
./development/apps/Development/src/com/android/development/Connectivity.java:                    mWm.setWifiEnabled(true);
./development/apps/Development/src/com/android/development/Connectivity.java:                    mWm.setWifiEnabled(false);
Binary file ./prebuilts/sdk/sdk-annotations/annotations.zip matchesd
./prebuilts/sdk/system-api/23.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/system-api/22.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/19.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/10.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/11.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/2.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/6.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/18.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/1.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/17.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/20.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/15.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/5.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/12.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/16.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/14.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/13.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/23.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/8.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/21.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/22.txt:    method public boolean setWifiEnabled(boolean);
./prebuilts/sdk/api/9.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/7.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/4.xml:<method name="setWifiEnabled"
./prebuilts/sdk/api/3.xml:<method name="setWifiEnabled"
./frameworks/data-binding/compiler/src/main/resources/api-versions.xml:		<method name="setWifiEnabled(Z)Z" />
./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:        if (wifiEnabled) setWifiEnabled(wifiEnabled);
./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:     * see {@link android.net.wifi.WifiManager#setWifiEnabled(boolean)}
./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:    public synchronized boolean setWifiEnabled(boolean enable) {
./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:        Slog.d(TAG, "setWifiEnabled: " + enable + " pid=" + Binder.getCallingPid()
./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:            Slog.e(TAG, "Invoking mWifiStateMachine.setWifiEnabled\n");
./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:            setWifiEnabled(true);
./frameworks/base/wifi/java/android/net/wifi/IWifiManager.aidl:    boolean setWifiEnabled(boolean enable);
./frameworks/base/wifi/java/android/net/wifi/WifiManager.java:    public boolean setWifiEnabled(boolean enabled) {
./frameworks/base/wifi/java/android/net/wifi/WifiManager.java:            return mService.setWifiEnabled(enabled);
./frameworks/base/core/tests/coretests/src/android/app/DownloadManagerBaseTest.java:        manager.setWifiEnabled(enable);
./frameworks/base/core/tests/hosttests/test-apps/DownloadManagerTestApp/src/com/android/frameworks/downloadmanagertests/DownloadManagerBaseTest.java:        manager.setWifiEnabled(enable);
./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:        mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:            mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:        return mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:        return mWifiManager.setWifiEnabled(false);
./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:        if (!mWifiManager.setWifiEnabled(false)) {
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:        return mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:            mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:            mWifiManager.setWifiEnabled(true);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:        return mWifiManager.setWifiEnabled(false);
./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:        if (!mWifiManager.setWifiEnabled(false)) {
./frameworks/base/core/java/android/app/admin/DevicePolicyManager.java:     *   Use {@link android.net.wifi.WifiManager#setWifiEnabled(boolean)} instead.</li>
./frameworks/base/packages/SettingsProvider/src/com/android/providers/settings/SettingsBackupAgent.java:            mWfm.setWifiEnabled(enable);
./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(true);
./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(false);
./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(true);
./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(true);
./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(true);
./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:    protected void setWifiEnabled(boolean enabled) {
./frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/policy/NetworkController.java:    void setWifiEnabled(boolean enabled);
./frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/policy/NetworkControllerImpl.java:    public void setWifiEnabled(final boolean enabled) {
./frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/policy/NetworkControllerImpl.java:                mWifiManager.setWifiEnabled(enabled);
./frameworks/base/packages/SystemUI/src/com/android/systemui/qs/tiles/WifiTile.java:        mController.setWifiEnabled(!mState.enabled);
./frameworks/base/packages/SystemUI/src/com/android/systemui/qs/tiles/WifiTile.java:            mController.setWifiEnabled(true);
./frameworks/base/packages/SystemUI/src/com/android/systemui/qs/tiles/WifiTile.java:            mController.setWifiEnabled(state);
./frameworks/base/api/current.txt:    method public boolean setWifiEnabled(boolean);
./frameworks/base/api/system-current.txt:    method public boolean setWifiEnabled(boolean);
./frameworks/base/cmds/svc/src/com/android/commands/svc/WifiCommand.java:                    wifiMgr.setWifiEnabled(flag);
 
```

the file of output is:
<a href='output.txt'>output.txt</a>

that should change each file in the following format:

```bash


# skipped because they use the old method
./external/robolectric/src/test/java/com/xtremelabs/robolectric/shadows/WifiManagerTest.java:    public void setWifiEnabled_shouldThrowSecurityExceptionWhenAccessWifiStatePermissionNotGranted() throws Exception {
./external/robolectric/src/test/java/com/xtremelabs/robolectric/shadows/WifiManagerTest.java:        wifiManager.setWifiEnabled(true);
./external/robolectric/src/main/java/com/xtremelabs/robolectric/shadows/ShadowWifiManager.java:    public boolean setWifiEnabled(boolean wifiEnabled) {
./packages/apps/TvSettings/Settings/src/com/android/tv/settings/connectivity/NetworkActivity.java:                mConnectivityListener.setWifiEnabled(true);
./packages/apps/TvSettings/Settings/src/com/android/tv/settings/connectivity/NetworkActivity.java:                mConnectivityListener.setWifiEnabled(false);
./packages/apps/TvSettings/Settings/src/com/android/tv/settings/connectivity/ConnectivityListener.java:    public void setWifiEnabled(boolean enable) {
./packages/apps/TvSettings/Settings/src/com/android/tv/settings/connectivity/ConnectivityListener.java:        mWifiManager.setWifiEnabled(enable);
./packages/apps/ManagedProvisioning/src/com/android/managedprovisioning/WifiConfig.java:            mWifiManager.setWifiEnabled(true);
./packages/apps/ManagedProvisioning/src/com/android/managedprovisioning/task/AddWifiNetworkTask.java:                && (mWifiManager.isWifiEnabled() || mWifiManager.setWifiEnabled(true));
./packages/apps/Settings/src/com/android/settings/wifi/WifiEnabler.java:        if (!mWifiManager.setWifiEnabled(isChecked)) {
./packages/apps/Settings/src/com/android/settings/widget/SettingsAppWidgetProvider.java:                    wifiManager.setWifiEnabled(desiredState);
./development/apps/Development/src/com/android/development/Connectivity.java:                                mWm.setWifiEnabled(false);
./development/apps/Development/src/com/android/development/Connectivity.java:                                mWm.setWifiEnabled(true);
./development/apps/Development/src/com/android/development/Connectivity.java:                    mWm.setWifiEnabled(true);
./development/apps/Development/src/com/android/development/Connectivity.java:                    mWm.setWifiEnabled(false);


# skipped because it was binary  file 
Binary file ./prebuilts/sdk/sdk-annotations/annotations.zip matchesd



# in the ./prebuilts/sdk/api/NUM.txt
method public boolean setWifiEnabledWithPassword(boolean, java.lang.String);

# in the ./prebuilts/sdk/api/NUM.xml
<method name="setWifiEnabledWithPassword"
 return="boolean"
 abstract="false"
 native="false"
 synchronized="false"
 static="false"
 final="false"
 deprecated="not deprecated"
 visibility="public"
>
<parameter name="enabled" type="boolean">
</parameter>

<parameter name="password" type="java.lang.String">
</parameter>
</method>

# in the ./frameworks/data-binding/compiler/src/main/resources/api-versions.xml:
 <method name="setWifiEnabledWithPassword(Z;Ljava/lang/String)Z" />
 
 # these files skipped
 ./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:        if (wifiEnabled) setWifiEnabled(wifiEnabled);
 ./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:     * see {@link android.net.wifi.WifiManager#setWifiEnabled(boolean)}
 ./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:    public synchronized boolean setWifiEnabled(boolean enable) {
 ./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:        Slog.d(TAG, "setWifiEnabled: " + enable + " pid=" + Binder.getCallingPid()
 ./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:            Slog.e(TAG, "Invoking mWifiStateMachine.setWifiEnabled\n");
 ./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:            setWifiEnabled(true);
 
 
 
 
 
 
 # in the ./frameworks/base/wifi/java/android/net/wifi/IWifiManager.aidl
 
 boolean setWifiEnabledWithPassword(boolean enabled, String password);
 
 
 # skipped beacuse of tests
 
 ./frameworks/base/core/tests/coretests/src/android/app/DownloadManagerBaseTest.java:        manager.setWifiEnabled(enable);
 ./frameworks/base/core/tests/hosttests/test-apps/DownloadManagerTestApp/src/com/android/frameworks/downloadmanagertests/DownloadManagerBaseTest.java:        manager.setWifiEnabled(enable);
 ./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:        mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:            mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:        return mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:        return mWifiManager.setWifiEnabled(false);
 ./frameworks/base/core/tests/bandwidthtests/src/com/android/bandwidthtest/util/ConnectionUtil.java:        if (!mWifiManager.setWifiEnabled(false)) {
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/unit/WifiClientTest.java:        mWifiManager.setWifiEnabled(false);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:        return mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:            mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:            mWifiManager.setWifiEnabled(true);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:        return mWifiManager.setWifiEnabled(false);
 ./frameworks/base/core/tests/ConnectivityManagerTest/src/com/android/connectivitymanagertest/ConnectivityManagerTestBase.java:        if (!mWifiManager.setWifiEnabled(false)) {
 
 
 
 
 
 #skipped because that was just a simple html file

 ./frameworks/base/core/java/android/app/admin/DevicePolicyManager.java:     *   Use {@link android.net.wifi.WifiManager#setWifiEnabled(boolean)} instead.</li>
 
 
 
 # skipped because they was for systemUI and setting application in android
 
 ./frameworks/base/packages/SettingsProvider/src/com/android/providers/settings/SettingsBackupAgent.java:            mWfm.setWifiEnabled(enable);
 ./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(true);
 ./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(false);
 ./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(true);
 ./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(true);
 ./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:        setWifiEnabled(true);
 ./frameworks/base/packages/SystemUI/tests/src/com/android/systemui/statusbar/policy/NetworkControllerWifiTest.java:    protected void setWifiEnabled(boolean enabled) {
 ./frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/policy/NetworkController.java:    void setWifiEnabled(boolean enabled);
 ./frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/policy/NetworkControllerImpl.java:    public void setWifiEnabled(final boolean enabled) {
 ./frameworks/base/packages/SystemUI/src/com/android/systemui/statusbar/policy/NetworkControllerImpl.java:                mWifiManager.setWifiEnabled(enabled);
 ./frameworks/base/packages/SystemUI/src/com/android/systemui/qs/tiles/WifiTile.java:        mController.setWifiEnabled(!mState.enabled);
 ./frameworks/base/packages/SystemUI/src/com/android/systemui/qs/tiles/WifiTile.java:            mController.setWifiEnabled(true);
 ./frameworks/base/packages/SystemUI/src/com/android/systemui/qs/tiles/WifiTile.java:            mController.setWifiEnabled(state);
 
 
 
 # in the ./frameworks/base/api/current.txt
 method public boolean setWifiEnabledWithPassword(boolean, java.lang.String);
 
 
 # in the ./frameworks/base/api/system-current.txt
 method public boolean setWifiEnabledWithPassword(boolean, java.lang.String);
 
 
 #skipped because it uses the old one 
 ./frameworks/base/cmds/svc/src/com/android/commands/svc/WifiCommand.java                wifiMgr.setWifiEnabled(flag);
 
 
 # in the file ./frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java in line 615
 added this code
 /**
     * see {@link android.net.wifi.WifiManager#setWifiEnabledWithPassword(boolean, String)}
     * @param enable {@code true} to enable, {@code false} to disable.
     * @param password to check the password from user with provided password from admin user
     * @return {@code true} if the enable/disable operation was
     *         started or is already in the queue.
     */
 public synchronized boolean setWifiEnabledWithPassword(boolean enable, String password) {
      enforceChangePermission();
      Slog.d(TAG, "setWifiEnabled: " + enable + " pid=" + Binder.getCallingPid()
                  + ", uid=" + Binder.getCallingUid());
      if (DBG) {
          Slog.e(TAG, "Invoking mWifiStateMachine.setWifiEnabled\n");
      }

      /*
      * Caller might not have WRITE_SECURE_SETTINGS,
      * only CHANGE_WIFI_STATE is enforced
      */

      long ident = Binder.clearCallingIdentity();
      try {
          if (! mSettingsStore.handleWifiToggled(enable)) {
              // Nothing to do if wifi cannot be toggled
              return true;
          }
      } finally {
          Binder.restoreCallingIdentity(ident);
      }

      mWifiController.sendMessage(CMD_WIFI_TOGGLED);
      return true;
  }
  
  
```

the output file is:
<a href='logsOfAddingNewMethodToAndroid.txt'>logsOfAddingNewMethodToAndroid.txt</a>


## second: compile new SDK to compile proper applications

useful links to build custom SDKs 
https://android.googlesource.com/platform/sdk/+/master/docs/howto_build_SDK.txt

page 135 of Embedded Android 

useful link for navigating andorid
https://www.youtube.com/watch?v=NsqFOSzoYE8

____
2 chrome extensions 
sdk reference search --> prefix is ad or ab
android resource navigator --> arn
___


first: I don't have any fucking idea
started to search around to find a solution 
I just thought that I have to force android studio to build the program
but that was totally false because if it can not find the framework obviously can't link the function. so I have to add the new SDK to android studio


find three things:

### First 
a document in google git about how to build custom API that doesn't work for me.
https://github.com/miracle2k/android-platform_sdk/blob/master/docs/howto_build_SDK.txt
https://android.googlesource.com/platform/sdk/+/master/docs/howto_build_SDK.txt

### Second
after that I found how to make custom SDK in page 135 of Embedded Android that was same as the first solution and doesn’t work for me.

### Third
after that I found a stack overflow post 
[android - In Androird how to expose a library API in AOSP to 3rd party developers - Stack Overflow](https://stackoverflow.com/questions/17339613/in-androird-how-to-expose-a-library-api-in-aosp-to-3rd-party-developers)

so I use `make update-api` and after that `make sdk` 
in second phase I faced some issues 

___
PS: to open android source code in android studio you could use this link [How to import android source code(AOSP) into Android studio? - Stack Overflow](https://stackoverflow.com/questions/24531006/how-to-import-android-source-codeaosp-into-android-studio)
and in this video https://www.youtube.com/watch?v=NsqFOSzoYE8
they show how to dive into android source code it could be useful
for example two useful #chrome_extensions introduced. 
___

#### Problems
I got faced two problems that I posted here 
[Android-x86 Google Groups - Error while building SDK because of lack gralloc_drm.h](https://groups.google.com/forum/?utm_source=digest&utm_medium=email#!topic/android-x86/Ey2A2Eu3jOY)

> I want to build custom SDK to do that I run this command first to add new API  
`make update-api` 
that  runs successfully  after that I run:
`make sdk`
but I faced an error that `#include "gralloc_drm.h"` not found in the:
` frameworks/native/services/surfaceflinger/DisplayDevice.cpp `
I compared code in this file with original AOSP and did not find `#include "gralloc_drm.h"` in the original code either I did not find the `gralloc_drm.h` in the appropriate directory.
so I removed the included line. 
but after that I faced this error:
```
frameworks/native/services/surfaceflinger/DisplayDevice.cpp:263:29: error: use of undeclared identifier 'GRALLOC_MODULE_PERFORM_LEAVE_VT'
            gr->perform(gr, GRALLOC_MODULE_PERFORM_LEAVE_VT);
                            ^
frameworks/native/services/surfaceflinger/DisplayDevice.cpp:268:29: error: use of undeclared identifier 'GRALLOC_MODULE_PERFORM_ENTER_VT'
            gr->perform(gr, GRALLOC_MODULE_PERFORM_ENTER_VT);
                            ^
target  C++: libsurfaceflinger <= frameworks/native/services/surfaceflinger/DisplayHardware/FramebufferSurface.cpp
2 errors generated.
build/core/binary.mk:706: recipe for target 'out/target/product/generic_x86/obj/SHARED_LIBRARIES/libsurfaceflinger_intermediates/DisplayDevice.o' failed
make: *** [out/target/product/generic_x86/obj/SHARED_LIBRARIES/libsurfaceflinger_intermediates/DisplayDevice.o] Error 1
make: *** Waiting for unfinished jobs....

make failed to build some targets (02:58 (mm:ss)) 
```
that obviously is because of lack of `gralloc_drm.h` file.
I still amazed why `gralloc_drm.h` is not in the source code while some codes using it.

and I got this answer
	Mauro Rossi	

9:36 PM (1 hour ago)


> Hi Mohammad,  
>   
> the gralloc_drm.h header is in external/drm_gralloc folder,  

> it is also exported with libgralloc_drm shared library, so you could try to add the following line in the Android.mk of target that is giving you the build error.  
> ` LOCAL_SHARED_LIBRARIES += libgralloc_drm`  

> Mauro  

so I added this line `LOCAL_SHARED_LIBRARIES += libgralloc_drm` to `/root/android-x86-marshmallow/frameworks/native/services/surfaceflinger/Android.mk`
and build it again.

after these steps I got this error:

```
make: *** No rule to make target 'out/target/product/generic_x86/obj/SHARED_LIBRARIES/libgralloc_drm_intermediates/export_includes', needed by 'out/target/product/generic_x86/obj/SHARED_LIBRARIES/libsurfaceflinger_intermediates/import_includes'.  Stop.
make: *** Waiting for unfinished jobs....
Export includes file: frameworks/native/services/surfaceflinger/Android.mk -- out/target/product/generic_x86/obj/SHARED_LIBRARIES/libsurfaceflinger_intermediates/export_includes

#### make failed to build some targets (09:33 (mm:ss)) ####
```

I changed it and just add the `libgralloc_drm` like other libraries and it worked as mentioned bellow

```
LOCAL_SHARED_LIBRARIES := \
    libcutils \
    liblog \
    libdl \
    libhardware \
    libutils \
    libEGL \
    libGLESv1_CM \
    libGLESv2 \
    libbinder \
    libui \
    libgui \
    libpowermanager \
    libgralloc_drm
```

after compiling it I faced another issue:

```
prebuilts/gcc/linux-x86/host/x86_64-linux-glibc2.15-4.8//x86_64-linux/bin/ld: out/host/linux-x86/obj32/STATIC_LIBRARIES/libLLVMProfileData_in
termediates/libLLVMProfileData.a(SampleProfWriter.o): previous definition here
prebuilts/gcc/linux-x86/host/x86_64-linux-glibc2.15-4.8//x86_64-linux/bin/ld: error: out/host/linux-x86/obj32/STATIC_LIBRARIES/libLLVMProfile
Data_intermediates/libLLVMProfileData.a(SampleProfWriter.o): multiple definition of 'vtable for llvm::sampleprof::SampleProfileWriterText'
prebuilts/gcc/linux-x86/host/x86_64-linux-glibc2.15-4.8//x86_64-linux/bin/ld: out/host/linux-x86/obj32/STATIC_LIBRARIES/libLLVMProfileData_in
termediates/libLLVMProfileData.a(SampleProfWriter.o): previous definition here
prebuilts/gcc/linux-x86/host/x86_64-linux-glibc2.15-4.8//x86_64-linux/bin/ld: error: out/host/linux-x86/obj32/STATIC_LIBRARIES/libLLVMProfile
Data_intermediates/libLLVMProfileData.a(SampleProfWriter.o): multiple definition of 'vtable for llvm::sampleprof::SampleProfileWriterBinary'
prebuilts/gcc/linux-x86/host/x86_64-linux-glibc2.15-4.8//x86_64-linux/bin/ld: out/host/linux-x86/obj32/STATIC_LIBRARIES/libLLVMProfileData_in
termediates/libLLVMProfileData.a(SampleProfWriter.o): previous definition here

clang: error: linker command failed with exit code 1 (use -v to see invocation)
build/core/host_shared_library_internal.mk:51: recipe for target 'out/host/linux-x86/obj32/lib/libLLVM.so' failed
make: *** [out/host/linux-x86/obj32/lib/libLLVM.so] Error 1

#### make failed to build some targets (01:03:57 (hh:mm:ss)) ####

```



after one more time compiling I got same error as expected:

```
clang: error: linker command failed with exit code 1 (use -v to see invocation)
build/core/host_shared_library_internal.mk:51: recipe for target 'out/host/linux-x86/obj32/lib/libLLVM.so' failed
make: *** [out/host/linux-x86/obj32/lib/libLLVM.so] Error 1
make: *** Waiting for unfinished jobs....

#### make failed to build some targets (01:43 (mm:ss)) ####
```


I found this link to resolve this issue:
[Android build error on Ubuntu 16.04 LTS - Viva La Vida](http://oopsmonk.github.io/blog/2016/06/07/android-build-error-on-ubuntu-16-04-lts)

it seems AOSP codes before May 7 2016 has this issue on Ubuntu 16.04 (my environment) and to resolve it I should change `/build/core/clang/HOST_x86_common.mk` with a newer one after May 7 2016

and at all in the android source site mentioned that it’s better to use Ubuntu 14.04 to compile the Android-Marshmallow

So I first change this file and if I got problem I change my machine.

so I changed `/build/core/clang/HOST_x86_common.mk` file with the same file in android nougat-x86 to see the changes.

the new file is
<a href='HOST_x86_common.mk'>HOST_x86_common.mk</a>

after compiling I got exact same error:
```
clang: error: linker command failed with exit code 1 (use -v to see invocation)
build/core/host_shared_library_internal.mk:51: recipe for target 'out/host/linux-x86/obj32/lib/libLLVM.so' failed
make: *** [out/host/linux-x86/obj32/lib/libLLVM.so] Error 1
make: *** Waiting for unfinished jobs....

#### make failed to build some targets (01:59 (mm:ss)) ####
```

___

So I decided to get a 14.04 vps and do all the stuff from the top
___

here is how I did it:

1. first got a VPS from AarvanCloud. 
2. add my own ip to route from default gateway because I want to use vpn on my vps and if I don’t do that I lost my connection to my vps
3. setup a vpn server with `steisand` effect repository at: 
https://github.com/StreisandEffect/streisand 
4. run this command on your own vps: 
``` bash
ip route x.x.x.x/24 via [default-gateway]
```
 that x.x.x.x is ip of the computer using to connect to remote host. easily can find your ip by search **my ip** in Google
5. and run this: 
```bash 
openconnect —cafile ca.crt [server-ip] -u [your-username]
```
6. after this start cloning procedure from the first



to establish a build environment on ::Ubuntu 14.04::

* install `openjdk-7-jdk`
```bash
add-apt-repository ppa:openjdk-r/ppa  
apt-get update   
apt-get install openjdk-7-jdk 
```

* install requirements:
```bash
apt-get install git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev libxml2-utils xsltproc unzip
```

on ubuntu 16.04 run this:
```bash
apt-get install yasm python-pip libyaml-dev bc genisoimage syslinux-utils git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev libgl1-mesa-dev libxml2-utils xsltproc libssl-dev ncurses-dev xz-utils kernel-package unzip
```

* install proper pip packages:
```bash
pip install prettytable Mako pyaml dateutils
```



so I setup a 14.04 machine from DigitalOcean

on bare source code run 
```bash
source build/envsetup.sh
lunch sdk_x86-eng
make -j6 sdk
``` 

and I got failed.

so I try other method:
```bash
source build/envsetup.sh
lunch sdk_x86-eng
make -j6 update-api
make -j6 PRODUCT-sdk-sdk
``` 
that after `update-api` phase was successful but the last `make` command failed with the following error:

```
/bin/bash: prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/bin/arm-linux-androideabi-gcc: No such file or directory
/bin/bash: prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/bin/arm-linux-androideabi-gcc: No such file or directory
make: *** [out/target/product/generic/obj/lib/crtbegin_dynamic1.o] Error 127
make: *** Waiting for unfinished jobs....
make: *** [out/target/product/generic/obj/lib/crtbrand.o] Error 127
Note: Some input files use unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.

#### make failed to build some targets (02:00 (mm:ss)) ####
```

I tried to build sdk with default android-marshmallow AOSP on Ubuntu 14.04 machine and android-x86 simultaneously	


Ubuntu 16.04 - android-x86-marshmallow - error
```bash
host C++: libclangAnalysis_32 <= external/clang/lib/Analysis/PseudoConstantAnalysis.cpp
host C++: libclangAnalysis_32 <= external/clang/lib/Analysis/ReachableCode.cpp
host C++: libclangAnalysis_32 <= external/clang/lib/Analysis/ScanfFormatString.cpp
host C++: libclangAnalysis_32 <= external/clang/lib/Analysis/ThreadSafetyCommon.cpp
host C++: libclangAnalysis_32 <= external/clang/lib/Analysis/ThreadSafety.cpp
host C++: libclangAnalysis_32 <= external/clang/lib/Analysis/ThreadSafetyLogical.cpp
host C++: libclangAnalysis_32 <= external/clang/lib/Analysis/ThreadSafetyTIL.cpp
host C++: libclangAnalysis_32 <= external/clang/lib/Analysis/UninitializedValues.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/APValue.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/ASTConsumer.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/ASTContext.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/ASTDiagnostic.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/ASTDumper.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/ASTImporter.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/ASTTypeTraits.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/AttrImpl.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/CommentBriefParser.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/CommentCommandTraits.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/Comment.cpp
host C++: libclangAST_32 <= external/clang/lib/AST/CommentLexer.cpp
clang: error: linker command failed with exit code 1 (use -v to see invocation)
build/core/host_shared_library_internal.mk:51: recipe for target 'out/host/linux-x86/obj32/lib/libLLVM.so' failed
make: *** [out/host/linux-x86/obj32/lib/libLLVM.so] Error 1
make: *** Waiting for unfinished jobs....

#### make failed to build some targets (04:08 (mm:ss)) ####
```

one useful link 
[makefile - Building Android from sources: unsupported reloc 43 - Stack Overflow](https://stackoverflow.com/questions/36048358/building-android-from-sources-unsupported-reloc-43)
I added `-no-integrated-as` at the end of line 16 of 
` build/core/clang/HOST_x86_common.mk` 

```bash
  ifeq ($(HOST_OS),linux)
  CLANG_CONFIG_x86_LINUX_HOST_EXTRA_ASFLAGS := \
    --gcc-toolchain=$($(clang_2nd_arch_prefix)HOST_TOOLCHAIN_FOR_CLANG) \
    --sysroot=$($(clang_2nd_arch_prefix)HOST_TOOLCHAIN_FOR_CLANG)/sysroot \
    -B$($(clang_2nd_arch_prefix)HOST_TOOLCHAIN_FOR_CLANG)/x86_64-linux/bin \
    -no-integrated-as
 
```

in this pdf file explains a little about the issue:

<a href='LPC16%20-%20AOSP's%20switch%20to%20Clang(2).pdf'>LPC16%20-%20AOSP's%20switch%20to%20Clang(2).pdf</a>

and nothing has changed on ubuntu 16.04 :sad:
I’ve got exactly the same error

___
compiling SDK on Ubuntu 14.04 on AOSP marshmallow was successful

but on the same machine android-x86-marshmallow got some issues:
I do this command:
```
lunch sdk-eng
make -j6 sdk
```

and I got this error message
```bash
/bin/bash: prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/bin/arm-linux-androideabi-gcc: No such file or directory
make: *** [out/target/product/generic/obj/lib/crtbegin_dynamic1.o] Error 127
make: *** Waiting for unfinished jobs....
/bin/bash: prebuilts/gcc/linux-x86/arm/arm-linux-androideabi-4.9/bin/arm-linux-androideabi-gcc: No such file or directory
make: *** [out/target/product/generic/obj/lib/crtbrand.o] Error 127
Note: Some input files use unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
Note: Some input files use or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
Note: Some input files use unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
Note: Some input files use unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
1 warning
Note: Some input files use or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
Note: Some input files use unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.

#### make failed to build some targets (01:24 (mm:ss)) ####
```

from the error it seems that the x86 source don’t have arm based compiler and because of this it failed. 
I will transfer those file from AOSP to x86 and hope it will work.

before transferring arm files from AOSP to x86 repo I try this command:

```
lunch sdk_x86-eng
make sdk
```

to build specifically SDK for x86 machine 
but I got this error message

```bash

/root/android-x86-marshmallow/external/busybox/scripts/basic/split-include.c: In function ‘main’:
/root/android-x86-marshmallow/external/busybox/scripts/basic/split-include.c:134:11: warning: ignoring return value of ‘fgets’, declared with attribute warn_unused_result [-Wunused-result]
      fgets(old_line, buffer_size, fp_target);
           ^
  HOSTCC  scripts/kconfig/conf.o
/root/android-x86-marshmallow/external/busybox/scripts/kconfig/conf.c: In function ‘conf_askvalue’:
/root/android-x86-marshmallow/external/busybox/scripts/kconfig/conf.c:106:8: warning: ignoring return value of ‘fgets’, declared with attribute warn_unused_result [-Wunused-result]
   fgets(line, 128, stdin);
        ^
/root/android-x86-marshmallow/external/busybox/scripts/kconfig/conf.c: In function ‘conf_choice’:
/root/android-x86-marshmallow/external/busybox/scripts/kconfig/conf.c:354:9: warning: ignoring return value of ‘fgets’, declared with attribute warn_unused_result [-Wunused-result]
    fgets(line, 128, stdin);
         ^
  HOSTCC  scripts/kconfig/kxgettext.o
  HOSTCC  scripts/kconfig/mconf.o
  SHIPPED scripts/kconfig/zconf.tab.c
  SHIPPED scripts/kconfig/lex.zconf.c
  SHIPPED scripts/kconfig/zconf.hash.c
/root/android-x86-marshmallow/external/busybox/scripts/kconfig/mconf.c: In function ‘show_textbox’:
/root/android-x86-marshmallow/external/busybox/scripts/kconfig/mconf.c:847:7: warning: ignoring return value of ‘write’, declared with attribute warn_unused_result [-Wunused-result]
  write(fd, text, strlen(text));
       ^
/root/android-x86-marshmallow/external/busybox/scripts/kconfig/mconf.c: In function ‘exec_conf’:
/root/android-x86-marshmallow/external/busybox/scripts/kconfig/mconf.c:481:6: warning: ignoring return value of ‘pipe’, declared with attribute warn_unused_result [-Wunused-result]
  pipe(pipefd);
      ^
  HOSTCC  scripts/kconfig/zconf.tab.o
  HOSTLD  scripts/kconfig/conf
scripts/kconfig/conf -s Config.in
#
# using defaults found in .config
#
  SPLIT   include/autoconf.h -> include/config/*
  HOSTCC  applets/usage
  HOSTCC  applets/applet_tables
/root/android-x86-marshmallow/external/busybox/applets/usage.c: In function ‘main’:
/root/android-x86-marshmallow/external/busybox/applets/usage.c:52:8: warning: ignoring return value of ‘write’, declared with attribute warn_unused_result [-Wunused-result]
   write(STDOUT_FILENO, usage_array[i].usage, strlen(usage_array[i].usage) + 1);
        ^
  GEN     include/bbconfigopts.h
/root/android-x86-marshmallow/external/busybox/applets/applet_tables.c: In function ‘main’:
/root/android-x86-marshmallow/external/busybox/applets/applet_tables.c:144:9: warning: ignoring return value of ‘fgets’, declared with attribute warn_unused_result [-Wunused-result]
    fgets(line_old, sizeof(line_old), fp);
         ^
  GEN     include/usage_compressed.h
  GEN     include/applet_tables.h
  HOSTCC  applets/usage_pod
  CC      applets/applets.o
  LD      applets/built-in.o
make[1]: Leaving directory `/root/android-x86-marshmallow/external/busybox'

#### make failed to build some targets (01:11 (mm:ss)) ####
``` 

I am trying to run 
``` 
lunch sdk_x86-eng
make -j6 sdk
```

on AOSP source code as mentioned in the Android on X86 book page 55
you can find the book here:
https://doc.lagout.org/programmation/Android/Android%20on%20x86_%20An%20Introduction%20to%20Optimizing%20for%20Intel%20Architecture%20%5BKrajci%20%26%20Cummings%202013-12-26%5D.pdf

and it was successful


so I modified all the files in the source code 
and run:
```bash
lunch sdk_x86-eng
make update-api
make sdk
```

but I got this error:

```bash
ERROR: /root/android-marshmallow/frameworks/opt/net/wifi/service/java/com/android/server/wifi/WifiServiceImpl.java:116: The type WifiServiceImpl must implement the inherited abstract method IWifiManager.setWifiEnabledWifiEnabled(boolean, String)
make: *** [out/target/common/obj/JAVA_LIBRARIES/wifi-service_intermediates/with-local/classes.dex] Error 41
make: *** Waiting for unfinished jobs....

#### make failed to build some targets (01:19:43 (hh:mm:ss)) ####
```

so it seems that I add `setWifiEnabledWifiEnabled` instead of `setWifiEnabledWithPassword` that this error occurred.
after I do `grep -ir setWifiEnabledWifiEnabled` and found the suspect
```
frameworks/base/wifi/java/android/net/wifi/IWifiManager.aidl:    boolean setWifiEnabledWifiEnabled(boolean enable, String password);
```

so I changed it and do make command again.

I can finally  build SDK successfully 
the SDK zip file can be found in `/root/android-marshmallow/out/host/linux-x86/sdk/sdk_x86/android-sdk_eng.root_linux-x86.zip`

I download the zip file and trying to import it to android-studio on a linux machine because  SDK was build for linux systems only and there were no `darwin-x86` directory in `/root/android-marshmallow/out/host/` 

I set up an Ubuntu 18.04 machine with android studio installed and transfer the `android-sdk_eng.root_linux-x86.zip` to it then extract it.

I changed the android-studio SDK path to the new SDK directory.
in android-studio **File -> Project Structure -> SDK Location**
change the path of SDK location to the downloaded SDK.

![](Building%20custom%20SDK/Screen%20Shot%201397-05-29%20at%201.42.31%20PM.png)

::Note: images may be look like the procedure have be done on a Mac computer but I tried all the steps on a Ubuntu 18.04 machine and the screen shots are just for better understanding.::

after this changes still nothing good happens and the `WifiManager.setWifiEnabledWithPassword(boolean enabled, String password)` is unknown to android-studio.

you should change the  
**File -> Project Structure**
in Modules pane. go to app and change the **Compile Sdk Version** to `API 23: Android 6.0 (Marshmallow)` 


![](Building%20custom%20SDK/Screen%20Shot%201397-05-29%20at%201.53.14%20PM.png)

after this step `WifiManager.setWifiEnabledWithPassword(boolean enabled, String password)` will be known to android-studio finally but something else happens and `R.id.s_wifi_status` become unknown to android-studio with this error:
```
Error:(50, 38) error: reference to findViewById is ambiguous
both method findViewById(int) in Activity and method <T>findViewById(int) in AppCompatActivity match
where T is a type-variable:
T extends View declared in method <T>findViewById(int)
```

in this line:
```java
mWifiStatusSwitch = (Switch) findViewById(R.id.s_wifi_status);
```


![](Building%20custom%20SDK/Screen%20Shot%201397-05-29%20at%201.59.54%20PM.png)

as shown in the image.

I just create new project using custom marshmallow SDK but after compiling the application I got this errors:


![](Building%20custom%20SDK/Screen%20Shot%201397-05-29%20at%204.09.06%20PM.png)


```
/Users/mohammad/.gradle/caches/transforms-1/files-1.1/design-26.1.0.aar/20243a57aeaaed99bf35971df2337c59/res/values-v26/values-v26.xml
Error:(3, 5) error: style attribute 'android:attr/keyboardNavigationCluster' not found.

/Users/mohammad/.gradle/caches/transforms-1/files-1.1/appcompat-v7-26.1.0.aar/74c0b00c055dfbc0e4f01f66ee7315f4/res/values-v26/values-v26.xml
Error:(9, 5) error: resource android:attr/colorError not found.
Error:(13, 5) error: resource android:attr/colorError not found.
Error:(17, 5) error: style attribute 'android:attr/keyboardNavigationCluster' not found.


/Users/mohammad/.gradle/caches/transforms-1/files-1.1/appcompat-v7-26.1.0.aar/74c0b00c055dfbc0e4f01f66ee7315f4/res/values/values.xml
Error:(246, 5) error: resource android:attr/keyboardNavigationCluster not found.


Error:resource android:style/TextAppearance.Material.Widget.Button.Borderless.Colored not found.
Error:resource android:style/TextAppearance.Material.Widget.Button.Colored not found.

/Users/mohammad/Projects/AndroidProjects/WifiChanger/app/build/intermediates/incremental/mergeDebugResources/merged.dir/values-v26/values-v26.xml
Error:(7) resource android:attr/colorError not found.
Error:(11) resource android:attr/colorError not found.
Error:(15) style attribute 'android:attr/keyboardNavigationCluster' not found.
Error:(18) style attribute 'android:attr/keyboardNavigationCluster' not found.


/Users/mohammad/Projects/AndroidProjects/WifiChanger/app/build/intermediates/incremental/mergeDebugResources/merged.dir/values/values.xml
Error:(197) resource android:attr/keyboardNavigationCluster not found.

Error:failed linking references.
Error:java.util.concurrent.ExecutionException: java.util.concurrent.ExecutionException: com.android.tools.aapt2.Aapt2Exception: AAPT2 error: check logs for details
Error:java.util.concurrent.ExecutionException: com.android.tools.aapt2.Aapt2Exception: AAPT2 error: check logs for details
Error:com.android.tools.aapt2.Aapt2Exception: AAPT2 error: check logs for details
Error:Execution failed for task ':app:processDebugResources'.
> Failed to execute aapt

Information:BUILD FAILED in 1s

```

find this link:
[Configure eclipse to use my own Android SDK (framework.jar) - Stack Overflow](https://stackoverflow.com/questions/14729793/configure-eclipse-to-use-my-own-android-sdk-framework-jar)

```
Error:A problem occurred configuring project ':app'.
> Failed to find Build Tools revision 26.0.2
```

So here is a brief explanation of what I did:

after changing the sdk directory I thought maybe gradle script is problem.
I using android marshmallow with API version 23 but gradle insists to use api ver 26 so I headed to change my gradle  script to force it to use api ver 23 

here is my first gradle script
```gradle
apply plugin: 'com.android.application'

android {
    compileSdkVersion 26
    defaultConfig {
        applicationId "com.example.wifichangerfour"
        minSdkVersion 23
        targetSdkVersion 26
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:26.0.0-beta1'
    implementation 'com.android.support.constraint:constraint-layout:1.0.2'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}

```


___
[android - How to downgrade to older version of Gradle - Stack Overflow](https://stackoverflow.com/questions/33892760/how-to-downgrade-to-older-version-of-gradle)
___

so I change `compileSdkVersion`  and `targerSdkVersion` from 26 to 23.

and as this link mentioned  I should change the `implementation` scripts as well.

it seems that the problem is because of android sdk version 

___
I’ve got too many problems to import compiled SDKs to android-studio 
after above bugs I thought maybe I can handle the bugs in gradle.build script.

## Gradle
Gradle is a build tool for java projects that android-studio use it too. 
there are three major things in build script:
* `targetSdkVersion` 
* `minSdkVersion`
* `buildToolsVersion`
* support library
the three options : `targetSdkVersion` and `buildToolsVersion` and support library version should be same as each others.

### targetSdkVersion and minSdkVersion
these two options in build.gradle script determine the version of sdk that we want to compile out application.
the `targetSdkVersion` is the target sdk that we want to compile application with it and the `minSdkVersion` is the minimum sdk level that we want to out application be compatible with it.

### buildToolsVersion
build tool is building tools that gradle use to build your application and version of it should be same as `targetSdkVersion` 

### Support library
something to support older libraries

____________________________________________________________________
I just compiled android oreo sdk and import it to android-studio 
set the every three major thing to 27 as shown in bellow gradle script

```
apply plugin: 'com.android.application'

android {
    buildToolsVersion "27.0.1"
    compileSdkVersion 27
    defaultConfig {
        applicationId "com.example.wifichangerseventh"
        minSdkVersion 27
        targetSdkVersion 27
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:26.1.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.2'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}


```                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              

but I’ve got new error

```
Error:java.util.concurrent.ExecutionException: java.lang.RuntimeException: No server to serve request. Check logs for details.
Error:Execution failed for task ':app:mergeDebugResources'.
> Error: java.util.concurrent.ExecutionException: java.lang.RuntimeException: No server to serve request. Check logs for details.
```






so I’ve done that on the Ubuntu machine

and got this error
```
Gradle build daemon disappeared unexpectedly (it may have been killed or may have crashed)```



____
# Compile Android with feature
```
Install: out/target/product/x86_64/system/bin/rild
target Pack Relocations: gralloc.drm_32 (out/target/product/x86_64/obj_x86/SHARED_LIBRARIES/gralloc.drm_intermediates/PACKED/gralloc.drm.so)
INFO: Compaction                 : 0 bytes
INFO: Too few relocations to pack after alignment
target Pack Relocations: hwcomposer.drm_32 (out/target/product/x86_64/obj_x86/SHARED_LIBRARIES/hwcomposer.drm_intermediates/PACKED/hwcomposer.drm.so)
INFO: Compaction                 : 0 bytes
INFO: Too few relocations to pack after alignment
frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1453: error: cannot find symbol
    public static final Uri BASE_CONTENT_URI = Uri.parse("content://" + AUTHORITY);
                                               ^ 
symbol:   variable Uri
  location: class ManagerContract
frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1516: error: cannot find symbol
            Cursor passwordRecord = getContentResolver()
            ^
  symbol:   class Cursor
  location: class WifiManager
frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1521: error: cannot find symbol
                            ManagerContract.PasswordManagerEntry._ID);
                                                                ^
  symbol:   variable _ID
  location: class PasswordManagerEntry
frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1516: error: cannot find symbol
            Cursor passwordRecord = getContentResolver()
                                    ^
  symbol:   method getContentResolver()
  location: class WifiManager
frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1527: error: cannot find symbol
            if (password.isEqual(resourcePassword)) {
                        ^
  symbol:   method isEqual(String)
  location: variable password of type String
target Unpacked: gralloc.drm (out/target/product/x86_64/obj/SHARED_LIBRARIES/gralloc.drm_intermediates/PACKED/gralloc.drm.so)
target Unpacked: hwcomposer.drm (out/target/product/x86_64/obj/SHARED_LIBRARIES/hwcomposer.drm_intermediates/PACKED/hwcomposer.drm.so)
Note: Some input files use or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
Note: Some input files use unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
8 errors
target Unpacked: libgui (out/target/product/x86_64/obj/SHARED_LIBRARIES/libgui_intermediates/PACKED/libgui.so)
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1453: Uri cannot be resolved to a type
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1453: Uri cannot be resolved
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1457: BaseColumns cannot be resolved to a type
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1459: Uri cannot be resolved to a type
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1460: Uri cannot be resolved to a type
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1516: Cursor cannot be resolved to a type
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1516: The method getContentResolver() is undefined for the type WifiManager
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1517: Uri cannot be resolved to a type
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1521: _ID cannot be resolved or is not a field
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1527: The method isEqual(String) is undefined for the type String
build/core/java.mk:394: recipe for target 'out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/classes-full-debug.jar' failed
make: *** [out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/classes-full-debug.jar] Error 41
make: *** Waiting for unfinished jobs....
build/core/java.mk:643: recipe for target 'out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/with-local/classes.dex' failed
make: *** [out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/with-local/classes.dex] Error 41

#### make failed to build some targets (11:15 (mm:ss)) ####

root@androidbuilder:~/android-x86-marshmallow#
```



after adding new code to android source code and resolve errors I got an error that need special acts to resolve

```
ERROR: /root/android-x86-marshmallow/frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1519: The method getContentResolver() is undefined for the type WifiManager
build/core/java.mk:643: recipe for target 'out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/with-local/classes.dex' failed
make: *** [out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/with-local/classes.dex] Error 41
make: *** Waiting for unfinished jobs....
frameworks/base/wifi/java/android/net/wifi/WifiManager.java:1519: error: cannot find symbol
            Cursor passwordRecord = getContentResolver()
                                    ^
  symbol:   method getContentResolver()
  location: class WifiManager
Note: Some input files use or override a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
Note: Some input files use unchecked or unsafe operations.
Note: Recompile with -Xlint:unchecked for details.
1 error
build/core/java.mk:394: recipe for target 'out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/classes-full-debug.jar' failed
make: *** [out/target/common/obj/JAVA_LIBRARIES/framework_intermediates/classes-full-debug.jar] Error 41

#### make failed to build some targets (03:00 (mm:ss)) ####
```

method `getContentResolver` from `Context` class could not resolved in framework although `Context` class was added to source code and I can use it in android studio.
so let’s go to find the problem

```
vim core/java/com/android/internal/widget/RotarySelector.java
```


