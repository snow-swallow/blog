## Cordova 和 IBM Worklight

1. 整体架构 - Architecture Overview Diagram from Cordova website

    ![Cordova App Architecture Overview](../images/cordovaapparchitecture.png)

    从图中，我们可以看到它提供了Web APP、WebView、Cordova Plugins。

    **Web APP**

    这是存放应用程序代码的地方，体现是你的具体业务逻辑模块。应用的实现是通过web页面，默认的本地文件名称是是index.html，这个本地文件应用CSS,JavaScript,图片，媒体文件和其他运行需要的资源。应用执行在原生应用包装的WebView中，这个原生应用是你分发到app stores中的。

    **WebView**

    Cordova启用的WebView可以给应用提供完整用户访问界面。在一些平台中，他也可以作为一个组件给大的、混合应用，这些应用混合和Webview和原生的应用组件。

    **Cordova Plugins**

    插件是Cordova生态系统的重要组成部分。他提供了Cordova和原生组件相互通信的接口并绑定到了标准的设备API上，这使你能够通过JavaScript调用原生代码。

2. Cordova 工作原理

    其实Cordova通过命令来添加项目的，但是可以选择哪个平台去编译，比如我们添加Android平台，在Android默认mainActivity类，我们可以看到它其实继承CordovaActivity类，一切初始化条件是从loadUrl方法开始。
    ```
    package com.example.hello;

    import android.os.Bundle;
    import org.apache.cordova.*;

    public class MainActivity extends CordovaActivity
    {
        @Override
        public void onCreate(Bundle savedInstanceState)
        {
            super.onCreate(savedInstanceState);

            // enable Cordova apps to be started in the background
            Bundle extras = getIntent().getExtras();
            if (extras != null && extras.getBoolean("cdvStartInBackground", false)) {
                moveTaskToBack(true);
            }

            // Set by <content src="index.html" /> in config.xml
            loadUrl(launchUrl);
        }
    }
    ```

    当Cordova框架启动时候，CordovaActivity类中的onCreate方法调用loadUrl方法即可启动，最终在SystemWebViewEngine类的init方法中，会调用webView的addJavascriptInterface方法，看到这个方法是不是很熟悉，我们常规让webView支持开启JavaScript调用接口也是使用此特性。
    ```
    private static void exposeJsInterface(WebView webView, CordovaBridge bridge) {
        if ((Build.VERSION.SDK_INT < Build.VERSION_CODES.JELLY_BEAN_MR1)) {
            LOG.i(TAG, "Disabled addJavascriptInterface() bridge since Android version is old.");
            // Bug being that Java Strings do not get converted to JS strings automatically.
            // This isn't hard to work-around on the JS side, but it's easier to just
            // use the prompt bridge instead.
            return;
        }
        SystemExposedJsApi exposedJsApi = new SystemExposedJsApi(bridge);
        webView.addJavascriptInterface(exposedJsApi, "_cordovaNative");
    }
    ```
    那么SystemExposedJsApi类new出来的对象就等同抛出“_cordovaNative”对象给JS端调用，进去看下SystemExposedJsApi类包含哪些内容。
    其中最关键是exec方法，其中bridgeSecret代表选择哪个桥接方式，service一般对应着你本地Java文件类名，action代表java文件中方法名，callbackId代表回调函数的Id，也就是句柄，arguments代表传递的参数。看出其中设计思想了没，service往往是本地能力集的类名，比如web端想调用相机，一般起个Camera类代表这个相机服务类，然后在这个类中定义方法，也就是action参数，这个action名称可扩展，因为方法名称可各种各样，适合自定义功能扩展。
    ```
    class SystemExposedJsApi implements ExposedJsApi {
        private final CordovaBridge bridge;

        SystemExposedJsApi(CordovaBridge bridge) {
            this.bridge = bridge;
        }

        @JavascriptInterface
        public String exec(int bridgeSecret, String service, String action, String callbackId, String arguments) throws JSONException, IllegalAccessException {
            return bridge.jsExec(bridgeSecret, service, action, callbackId, arguments);
        }

        @JavascriptInterface
        public void setNativeToJsBridgeMode(int bridgeSecret, int value) throws IllegalAccessException {
            bridge.jsSetNativeToJsBridgeMode(bridgeSecret, value);
        }

        @JavascriptInterface
        public String retrieveJsMessages(int bridgeSecret, boolean fromOnlineEvent) throws IllegalAccessException {
            return bridge.jsRetrieveJsMessages(bridgeSecret, fromOnlineEvent);
        }
    }
    ```

3. 插件开发流程

4. IBM Worklight 介绍

5. hybrid app 如何实现 SSO

> References
- [浅谈Cordova框架的一些理解](https://www.cnblogs.com/cr330326/p/7082821.html)