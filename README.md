# BiliShare
[![Apache 2.0 License](https://img.shields.io/badge/license-Apache%202.0-blue.svg?style=flat)](http://www.apache.org/licenses/LICENSE-2.0.html)
[ ![Download](https://api.bintray.com/packages/zyhang/maven/BiliShare/images/download.svg?version=1.0.0-alpha1) ](https://bintray.com/zyhang/maven/BiliShare/1.0.0-alpha1/link)
[![API](https://img.shields.io/badge/API-15%2B-blue.svg?style=flat)](https://developer.android.com/about/versions/android-4.0.3)

> 本项目fork自bilibili/BiliShare

支持分享到QQ聊天、QQ空间、微信聊天、微信朋友圈，系统分享等。

## 使用姿势

### 配置

 - 在build.gradle里添加依赖.

```
allprojects {
    repositories {
        jcenter()
    }
}

dependencies {
    implementation 'com.zyhang:bilishare:lastest-version'
}
```

 - 配置QQ分享，在AndroidManifest文件里添加如下配置，注意在scheme里添加你的appId。

```
<activity
    android:name="com.tencent.tauth.AuthActivity"
    android:launchMode="singleTask"
    android:noHistory="true">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="tencent你的AppId" />
   </intent-filter>
</activity>
```

 - 配置微信分享，在{root package}/wxapi/下添加WXEntryActivity，并且配置到AndroidManifest文件里。

```java
public class WXEntryActivity extends BaseWXEntryActivity {
    @Override
    protected String getAppId() {
        return .....;
    }
}
```
```xml
<activity
    android:name=".wxapi.WXEntryActivity"
    android:configChanges="keyboardHidden|orientation|screenSize"
    android:exported="true"
    android:theme="@android:style/Theme.Translucent.NoTitleBar"/>
```

### 使用

 - 示例代码

 ```java
 BiliShareConfiguration configuration = new BiliShareConfiguration.Builder(context)
                .qq(appId) //配置qq
                .weixin(appId) //配置微信
                .imageDownloader(new ShareFrescoImageDownloader()) //图片下载器
                .build();

    //global client全局共用，也可以用BiliShare.get(name)获取一个特定的client，以便业务隔离。
    BiliShare shareClient = BiliShare.global();
    shareClient.config(configuration); //config只需要配置一次

    shareClient.share(context, socializeMedia, shareParam, shareListener);
 ```

 - 具体参考/sample/src/main/java/com/bilibili/socialize/sample/MainActivity.java