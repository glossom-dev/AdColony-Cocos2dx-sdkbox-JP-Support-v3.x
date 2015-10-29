#SDKBOX: AdColony Installation with Cocos2d-x v3.x and C++#

##概要##
このドキュメントでは[Cocos2d-x SDKBOX](http://cocos2d-x.org/sdkbox#)のAdColonyの導入方法について説明します。正式版のドキュメント（英語）は[こちら](http://sdkbox-doc.github.io/en/plugins/adcolony/v3-cpp/)からご参照ください。

**参照**：[http://cocos2d-x.org/sdkbox/adcolony](http://cocos2d-x.org/sdkbox/adcolony)

##Step 0: Prerequisites##
* Glossomにてapp ID、zone IDを発行しお渡し致します。

##Step 1: Integration

ターミナルを開いて以下のコマンドを実行し、SDKBOXのAdColonyプラグインをインストールしてください。[SDKBOXのインストーラー](http://www.cocos2d-x.org/sdkbox/adcolony)が正しくセットアップされていることを確認してください。

	$ cd ~/MyGame
	$ sdkbox import adcolony

###iOS 9サポート###
Xcode 7をご利用の場合、下記の手順を実装する必要があります。

####App Transport Securityを無効に####
下記をplistに設定してください。

        <key>NSAppTransportSecurity</key>
        <dict>
            <key>NSAllowsArbitraryLoads</key>
            <true/>
        </dict>


####Bitcodeを無効に####
Bitcodeサポートを無効にしてください。有効の場合cocos2d-xのビルドがエラーになります。

####Requires full screenを有効に####
アプリは全方向対応でない場合は、Targets -> General -> Deployment Info -> Requires full screen にチェックしてください。チェックをしない場合、ストア申請時にエラーになります。

####canOpenURLを許可するURLリストの設定####
こちらは、利用しているプラグインによりますが、AdColonyを利用してる場合、[こちら](https://github.com/glossom-dev/AdColony-iOS-SDK-JP-Support/blob/master/iOS-9.md#canopenurlについて)を参照してください。
##Step 2: Configuration

SDKBOXが自動的に`./Resource/sdkbox_config.json`を作成してくれるので、アプリで使用をする前にその設定を修正しなければいけません。

以下がAdColonyの設定のサンプルです。`<app id>`と`<zone id>`をGlossomにて発行がされたものに置き換えてください。

```json
{
    "android": {
        "AdColony": {
            "debug": true, 
            "id": "<app id>", 
            "ads": {
                "v4vc": {
                    "v4vc": true, 
                    "pre_popup": true, 
                    "zone": "<zone id>", 
                    "post_popup": true
                }, 
                "video": {
                    "v4vc": false, 
                    "zone": "<zone id>"
                }
            }
        }
    }, 
    "ios": {
        "AdColony": {
            "debug": true, 
            "id": "<app id>", 
            "ads": {
                "v4vc": {
                    "v4vc": true, 
                    "pre_popup": true, 
                    "zone": "<zone id>", 
                    "post_popup": true
                }, 
                "video": {
                    "v4vc": false, 
                    "zone": "<zone id>"
                }
            }
        }
    }
}
```

##Step 3: Usage

###3.1 Initialize AdColony

適当な場所でプラグインを初期化してください。`AppDelegate::applicationDidFinishLaunching()`もしくは`AppController:didFinishLaunchingWithOptions()`を推奨しています。適切なヘッダーファイルをインクルードするのを忘れないようにしてください。

```cpp
#include "PluginAdColony/PluginAdColony.h"
AppDelegate::applicationDidFinishLaunching()
{
     sdkbox::PluginAdColony::init();
}
```

###3.2 Showing Ads

広告の種類を指定し、任意の場所で広告を表示してください。

```cpp
sdkbox::PluginAdColony::show("video");
```

もしくは

```cpp
sdkbox::PluginAdColony::show("v4vc");
```

###3.3 Catch AdColony events (optional)

プラグインはAdColonyのイベントをキャッチすることができるので、動画を見たユーザに対して報酬を付与するといったような処理を実行することができます。

* 下記のように、sdkbox::AdColonyListenerを継承して実装してください。

```cpp
#include "PluginAdColony/PluginAdColony.h"
class MyClass : public sdkbox::AdColonyListener
{
private:
  void onAdColonyChange(const sdkbox::AdColonyAdInfo& info, bool available);
  void onAdColonyReward(const sdkbox::AdColonyAdInfo& info,
        const std::string& currencyName, int amount, bool success);
  void onAdColonyStarted(const sdkbox::AdColonyAdInfo& info);
  void onAdColonyFinished(const sdkbox::AdColonyAdInfo& info);
};
```

* コールバックを処理するリスナーを作成してください。

```cpp
sdkbox::PluginAdColony::setListener(this);
```

##Manual Integration

もしSDKBOXのインストーラーが正常に処理を完了しない場合、SDKBOXをマニュアルで統合することが可能です。インストーラーが正常に処理を完了した場合は、これ以降のドキュメントの内容は必要ありません。

この手順は殆ど必要とされないのでドキュメントの最後に記載をしています。以下の手順を行った場合、完了されたあとに、その他の統合項目について、再度上記のドキュメントの確認をお願いいたします。

###Manual Integration For iOS

*  **plugins/ios**フォルダーから下記のフレームワークをXcodeプロジェクトにドラッグ&ドロップしてください。追加するときに、`Copy items if needed`をチェックしてください。
	* sdkbox.framework
	* PluginAdColony.framework
	* AdColony.framework

		![import frame-works image](assets/installation_guide-1_1.png)

* 上記のフレームワークは下記にのシステムフレームワークに依存していますので、プロジェクトに追加されなかったら追加をしてください。
    * libz.1.2.5.dylib
    * AdSupport.framework (Set to Optional)
    * AudioToolbox.framework
    * AVFoundation.framework
    * CoreGraphics.framework
    * installation_guide-html.html
    * CoreMedia.framework
    * CoreTelephony.framework
    * EventKit.framework
    * EventKitUI.framework
    * MediaPlayer.framework
    * MessageUI.framework
    * QuartzCore.framework
    * Security.framework
    * Social.framework (Set to Optional)
    * StoreKit.framework (Set to Optional)
    * SystemConfiguration.framework
    * WebKit.framework (Set to Optional)

		![import frame-works image](assets/installation_guide-2_2.png)

* **Target > Build Settings > Linking > Other Linker Flags**を開いて、下記の二つのフラグを設定してください。

        -force_load AdColony.framework/AdColony
        -fobjc-arc (AdColonyではARCを設定していないプロジェクトでも利用することができます。)

###Manual Integration For Android.

**2.1 Copy Files**

* **plugin/android/libs**フォルダーから下記の**jar**ファイルをプロジェクトの**proj.android/libs**下にコピーしてください。
	* adcolony.jar
	* PluginAdColony.jar
	* sdkbox.jar

* **plugin/android/jni**から`pluginadcolony` and `sdkbox`フォルダーをプロジェクトの**proj.android/jni/**にコピーしてください。

**2.2 Edit AndroidManifest.xml**

* `AndroidManifest.xml`ファイルに下記のパーミッションを追加してください。

        <uses-permission android:name="android.permission.INTERNET" /\>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" /\>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /\>
        <uses-permission android:name="android.permission.VIBRATE" /\>

* **application**タグに下記のようにハードウェアアクセラレーションを有効にしてください。

        <android:hardwareAccelerated="true" /\>

* 下記のように、Acitivityの設定を**application**タグの間にコピーしてください。

        <activity android:name="com.jirbo.adcolony.AdColonyOverlay"
        	android:configChanges="keyboardHidden|orientation"
        	android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" /\>
        <activity android:name="com.jirbo.adcolony.AdColonyFullscreen"
        	android:configChanges="keyboardHidden|orientation"
        	android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen" /\>
        <activity android:name="com.jirbo.adcolony.AdColonyBrowser"
        	android:configChanges="keyboardHidden|orientation"
        	android:theme="@android:style/Theme.Black.NoTitleBar.Fullscreen" /\>

**注意:** アプリがAPI レベル13以下に対応している場合、上記`configChanges`設定から`screenSize`を消してください。

**2.3 Edit Android.mk**

下記の通りに、**proj.android/jni/Android.mk** ファイルを設定してください。

* `LOCAL_STATIC_LIBRARIES`　の設定：

        LOCAL_STATIC_LIBRARIES += PluginAdColony
        LOCAL_STATIC_LIBRARIES += sdkbox

* 下記を一番最初の**import-module**コードの前に追加してください。

        $(call import-add-path,$(LOCAL_PATH))

* 一番後ろに下記の*import-module**コードを追加してください。

        $(call import-module, ./sdkbox)
        $(call import-module, ./pluginadcolony)

**注意：** 既存に`$(call import-module,./prebuilt-mk)`がある場合、上記をその前に追加してください。

**2.4  `Application.mk`の設定（Cocos2d-x v3.0 から v3.2 までのみ必要**

 **proj.android/jni/Application.mk** を開いて、**APP_STL**を下記のように設定してください。

    APP_STL := gnustl_static

**2.5 `Cocos2dxActivity.java`の設定**

**注意：** Cocos2d-xのインストール方法によって`Cocos2dxActivity.java`は二つの場所にある可能性があります。プリービルドされたライブラリーを利用してる場合**proj.android/src/org/cocos2dx/lib/Cocos2dxActivity.java**にあります。
ソースコードからインストールした場合、**cocos2d/cocos/platform/android/java/src/org/cocos2dx/lib/Cocos2dxActivity.java**にあります。

* まず、**Cocos2dxActivity.java**に必要なパケージをインポート：

        import android.content.Intent;

        import org.cocos2dx.plugin.PluginWrapper;

*  次に、`Cocos2dxActivity`の`onCreate(final  Bundle savedInstanceState)`に下記のようにプライグインの初期化コードを記述してください。
 `PluginWrapper.init(this);` は必ず `onLoadNativeLibraries();`の後に記述する必要があります。

    onLoadNativeLibraries();
    PluginWrapper.init(this);

* 最後に、下記のようにオバーライド関数を実装してください。

        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            if (!PluginWrapper.onActivityResult(requestCode, resultCode, data)) {
                super.onActivityResult(requestCode, resultCode, data);
            }
        }

        @Override
        protected void onStart() {
            super.onStart();
            PluginWrapper.onStart();
        }

        @Override
        protected void onStop() {
            super.onStop();
            PluginWrapper.onStop();
        }

        @Override
        protected void onResume() {
            super.onResume();
            PluginWrapper.onResume();
        }

        @Override
        protected void onPause() {
            super.onPause();
            PluginWrapper.onPause();
        }

        @Override
        public void onBackPressed() {
            if (!PluginWrapper.onBackPressed()) {
                super.onBackPressed();
            }
        }

**Step 2.6: Proguardの設定 (リリース版, 任意)**

* 下記のように、`project.properties` にProguardの設定ファイルを指定してください。

        proguard.config=proguard.cfg

*指定されたファイルに下記を追加してください。

        -dontwarn android.webkit.**

**注意：** Proguardはリリース版のビルドにしか利用することが出来ません。
