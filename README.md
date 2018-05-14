# TLSv1.2 安全升级技术指导

### PayPal TLSv1.2升级是什么？ 

为确保我们能够提供最高安全标准，按照支付卡行业安全标准协会（PCI协会）对付款处理商的要求，PayPal将升级SSL连接协议，升级后，您的服务器必须使用TLSv1.2版本协议才能与PayPal进行通信。

为避免服务中断，您必须在2018年6月之前验证您的系统是否支持TLSv1.2，并做必要的组件库升级以满足要求。

请按照以下步骤，选择匹配您的服务器、开发语言了解详细自查步骤：

* [自查前提](#prerequisites)
* [Java](#java)
* [.NET](#net)
* [PHP / Python / Ruby / Node.js](#openssl)
* [原生移动APP](#native-mobile-apps)

* * *
<a id='prerequisites'></a>
### 自查前提 

* 请在您的生产环境服务器上执行以下检查，如果您的服务器为托管服务，请联系托管商或管理员执行检查
* 如果此此文档中的内容没有涵盖您的服务器类型，或者自查测试中出现错误，请联系PayPal技术支持，并提供以下具体信息方便排查：

    * 您的服务器操作系统及版本 / 使用的开发语言 / 相关组件的版本信息
    * 自查测试中遇到的错误信息，完整的命令行输出信息，错误截屏等。

* * *

### Java

* [Java 版本要求](#java-version)
* [运行PayPal JAVA TLSv1.2检查](#to-verify-your-java-and-tls-versions)
* [PayPal Java SDK版本要求](#supported-sdks-java)

<a id='java-version'></a>
#### Java版本要求

> **注意:** 由于JAVA 8 中默认使用TLSv1.2连接协议，所以我们推荐升级您的JAVA到版本8。

| Java版本号 | TLSv1.2协议支持 | 合规要求 |
|:--------------|:-----------------|:--------------|
| 5或者更低版本 | 不支持 | 升级至Java 6或以上 |
| 6 - 7 | 支持 | <p>JAVA 6,7版本支持TLSv1.2协议，但是并没有默认启用TLSv1.2。您需要在代码中显式声明使用TLSv1.2</p><p>在JAVA启动参数中加入：<p>```-Dhttps.protocols=TLSv1.2```</p><p>或在代码中设置[`SSLContext`](http://docs.oracle.com/javase/7/docs/api/javax/net/ssl/SSLContext.html)：</p>```SSLContext context = SSLContext.getInstance("TLSv1.2");```<br>```SSLContext.setDefault(context);``` |
| 8 | 默认启用 | 无需任何操作 |

<a id='to-verify-your-java-and-tls-versions'></a>
#### 运行PayPal JAVA TLSv1.2检查

1. 下载PayPal [TlsCheck.jave类文件 或  TlsCheck.jar包](https://github.com/paypal/TLS-update/tree/master/java)。

1. 在您的生产环境服务器上运行.jar包，检查输出信息:

    ```
    > java -jar TlsCheck.jar
    ```

    * 检查成功:

        ```
        Successfully connected to TLS 1.2 endpoint.
        ```

    * 检查失败:

        ```
        Failed to connect to TLS 1.2 endpoint.
        ```

<a id="supported-sdks-java"></a>
#### PayPal Java SDK版本要求

除了满足以上Java环境版本要求之外，如果您使用了PayPal / Braintree JAVA SDK集成，请确保SDK也升级至合规版本或更新版本
* PayPal JAVA SDK合规版本列表：
    
    SDK | TLSv1.2 支持版本
    --- | -------
    [REST Java-SDK](https://github.com/paypal/PayPal-Java-SDK) | [1.4.0](https://github.com/paypal/PayPal-Java-SDK/releases)
    [sdk-core](https://github.com/paypal/sdk-core-java) | [1.7.0](https://github.com/paypal/sdk-core-java/releases/tag/v1.7.0)
    [adaptivepayments](https://github.com/paypal/adaptivepayments-sdk-java) | [2.9.117](https://github.com/paypal/adaptivepayments-sdk-java/releases/tag/v2.9.117)
    [adaptiveaccounts](https://github.com/paypal/adaptiveaccounts-sdk-java) | [2.6.106](https://github.com/paypal/adaptiveaccounts-sdk-java/releases/tag/2.6.106)
    [invoice](https://github.com/paypal/invoice-sdk-java) | [2.7.117](https://github.com/paypal/invoice-sdk-java/releases/tag/v2.7.117)
    [buttonmanager](https://github.com/paypal/buttonmanager-sdk-java) | [2.7.106](https://github.com/paypal/buttonmanager-sdk-java/releases/tag/2.7.106)
    [permissions](https://github.com/paypal/permissions-sdk-java) | [2.6.109](https://github.com/paypal/permissions-sdk-java/releases/tag/v2.6.109)
    [merchant (merchant 2.x)](https://github.com/paypal/merchant-sdk-java) | [2.14.117](https://github.com/paypal/merchant-sdk-java/releases/tag/v2.14.117)
    [legacy (merchant 1.x)](https://github.com/paypal/PayPal-Legacy-Java-SDK/) | [1.1.0](https://github.com/paypal/PayPal-Legacy-Java-SDK/releases/tag/v1.1.0)
* Braintree JAVA SDK合规版本：

    SDK | TLSv1.2 支持版本
    --- | -------
    [braintree_java](https://github.com/braintree/braintree_java/) | [2.67.0](https://github.com/braintree/braintree_java/releases/)

* * *

### .NET

* [.NET framework版本要求](#net-requirements)
* [运行PayPal .NET TLSv1.2检查](#to-verify-your-net-and-tls-versions)
* [PayPal .NET SDK版本要求](#supported-sdks-net)

<a id='net-requirements'></a>
#### .NET framework版本要求

为了启用TLSv1.2，您需要升级.Net Framework至4.5或更高版本。

> 如果您的生产环境仍使用Windows Server 2008 R2服务器，还需要在[注册表中开启TLSv1.2](https://support.microsoft.com/zh-cn/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

<a id='to-verify-your-net-and-tls-versions'></a>
#### 运行PayPal .NET TLSv1.2检查

1. 确保您的生产环境 .NET Framework已升级至4.5后，在[SecurityProtocolType枚举](https://msdn.microsoft.com/zh-cn/library/system.net.securityprotocoltype(v=vs.110).aspx)中指定```Tls12```
```System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;```


2. 下载PayPal .Net检查执行文件[TlsCheck](https://github.com/paypal/TLS-update/tree/master/net/TlsCheck)， 并在您的生产环境服务器上运行:

    ```
    > TlsCheck.exe
    ```

    * 检查成功:

        ```
        PayPal_Connection_OK
        ```

<a id="supported-sdks-net"></a>
#### PayPal .NET SDK版本要求

除了满足以上.NET环境版本要求之外，如果您使用了PayPal / Braintree .NET SDK集成，请确保升级至合规版本或更新版本

* PayPal .NET SDK合规版本列表
    
    SDK | TLSv1.2 支持版本
    --- | -------
    [REST NET-SDK](https://github.com/paypal/PayPal-NET-SDK) | [1.7.3](https://github.com/paypal/PayPal-NET-SDK/releases)
    [sdk-core](https://github.com/paypal/sdk-core-dotnet) | [1.7.1](https://github.com/paypal/sdk-core-dotnet/releases)
    [adaptivepayments](https://github.com/paypal/adaptivepayments-sdk-dotnet) | 需要升级至 [sdk-core 1.7.1 或更高版本](https://github.com/paypal/sdk-core-dotnet/releases)
    [adaptiveaccounts](https://github.com/paypal/adaptiveaccounts-sdk-dotnet) | 需要升级至 [sdk-core 1.7.1 或更高版本](https://github.com/paypal/sdk-core-dotnet/releases)
    [invoice](https://github.com/paypal/invoice-sdk-dotnet) | 需要升级至 [sdk-core 1.7.1 或更高版本](https://github.com/paypal/sdk-core-dotnet/releases)
    [buttonmanager](需要升级至://github.com/paypal/buttonmanager-sdk-dotnet) | Requires [sdk-core 1.7.1 或更高版本](https://github.com/paypal/sdk-core-dotnet/releases)
    [permissions](https://github.com/paypal/permissions-sdk-dotnet) | 需要升级至 [sdk-core 1.7.1 或更高版本](https://github.com/paypal/sdk-core-dotnet/releases)
    [merchant (merchant 2.x)](https://github.com/paypal/merchant-sdk-dotnet) | Requires [sdk-core 1.7.1 或更高版本](https://github.com/paypal/sdk-core-dotnet/releases)
    老版本 (merchant 1.x) | 不支持TLSv1.2 - 请升级至 [merchant 2.x版本](https://github.com/paypal/merchant-sdk-dotnet/wiki/Upgrade-Process-from-Legacy-Merchant-SDK)

* Braintree .NET SDK合规版本

    SDK | TLSv1.2 支持版本
    --- | -------
    [braintree_dotnet](https://github.com/braintree/braintree_dotnet) | [3.1.0](https://github.com/braintree/braintree_dotnet/releases/)

* * *

<a id='openssl'></a>
### PHP / Node.js / Python / Ruby

* [OpenSSL 版本要求](#openssl-requirements)
* [自查指导](#openssl-guidelines)

<a id='openssl-requirements'></a>
#### OpenSSL 版本要求

* 大部分PHP / Node.js / Python / Ruby 环境的SSL连接都依赖与系统提供的OpenSSL组件库，请确保您的生产环境服务器上，OpenSSL组件库版本已经更新至1.0.1c或者更高。

    > 此外对于Ruby环境，Ruby版本还需要升级至2.0.0或以上，以支持TLSv1.2 

* (PHP) 即使升级OpenSSL组件完成，部分PHP系统环境仍需要在curl代码中显示声明默认使用TLSv1.2协议，建议在您的PHP代码`curl`参数中设置：
    ```
    curl_setopt($ch, CURLOPT_SSLVERSION, 6);
    ```


* 如果您的服务器环境使用的是[其他SSL/TLS组件库](http://curl.haxx.se/docs/ssl-compared.html)，请确保更新至最新版本或支持TLSv1.2的版本。

<a id='openssl-guidelines'></a>
#### 自查指导

##### 检查OpenSSL版本 / 测试连接

在您的生产环境服务器终端执行以下命令，检查OpenSSL版本以及测试TLSv1.2连接：
> 以下操作需要服务器终端访问权限，如果您使用托管服务，请联系服务器托管商，或服务器管理员执行检查操作

* 检查OpenSSL / 测试PayPal TLS连接

    ```
    openssl version
    ```
    > 检查输出（例），确保版若低于1.0.1c需要升级更新OpenSSL组件库：
    
    ```
    OpenSSL 1.0.2k  26 Jan 2017
    ```

* 使用PayPal专用测试地址测试TLSv1.2连接：
    ```
    openssl s_client -connect tlstest.paypal.com:443
    ```
    > 检查输出（例），确认输出中包含连接成功状态和TLS协议版本：
    
    ```
    SSL-Session:
        Protocol  : TLSv1.2
        Cipher    : AES256-SHA256
        ...
    Verify return code: 0 (ok)
    ---
    ```

##### 各语言对应的TLS连接检查命令


除检查OpenSSL组件库支持性外，您也可以根据使用的开发语言，在服务器的终端执行以下命令测试TLSv1.2支持性：

* Node.js
    ```
    $ node -e "var https = require('https'); https.get('https://tlstest.paypal.com/', function(res){ console.log(res.statusCode) });"
    ```
* Ruby
    ```
    $ ruby -r'net/http' -e 'puts Net::HTTP.get(URI("https://tlstest.paypal.com/"))'
    ```
* Python 2.x:
    ```
    $ python -c "import urllib2; print(urllib2.urlopen('https://tlstest.paypal.com/').read())"
    ```
* Python 3.x:
    ```
    $ python -c "import urllib.request; print(urllib.request.urlopen('https://tlstest.paypal.com/').read())"
    ```

* * *

<a id='native-mobile-apps'></a>
### 原生移动APP

* [Android](#android)
* [iOS](#ios)

#### Android

* [Android版本要求](#android-requirements)
* [PayPal Android SDK版本要求](#supported-sdks-android)

##### Android版本要求

* API 20+版本中TLSv1.2已经被默认打开(设备平台为Android 4.4W 或 `KITKAT` - 可穿戴设备扩展)。

* API 16至20的安卓版本默认关闭对TLSv1.2的支持，您的项目需要显示声明支持TLSv1.2连接协议（设备平台为Android 4.1至4.4） 
    > 您可以在Android项目中通过自定义`SSLSocketFactory`以支持TLSv1.2：
    
    ```
        public TlsSocketFactory() throws KeyManagementException, NoSuchAlgorithmException {
            SSLContext context = SSLContext.getInstance("TLS");
            context.init(null, null, null);
            internalSSLSocketFactory = context.getSocketFactory();
        }
    ```

    完整写法请参考我们提供的 [示例 Android app](https://github.com/paypal/TLS-update/tree/master/android)代码。


* 低于API 16的版本讲无法支持TLSv1.2（设备平台为Android 4.1 `JELLY_BEAN` 版本以下）。[根据2018年4月16日的Google统计数据，仅有0.7%的用户仍在使用API 15或更低版本平台访问Google Play应用商店。](http://developer.android.com/about/dashboards/index.html#Platform)


<a id="supported-sdks-android"></a>

##### PayPal Android SDK版本要求

在安卓API 16 - 19集成中，如果您使用了PayPal / Braintree Android SDK集成，请确保升级以下版本或最新版本

* PayPal Android SDK合规版本

    SDK | TLSv1.2 支持版本
    --- | -------
    [Android SDK](https://github.com/paypal/PayPal-Android-SDK) | [2.13.0](https://github.com/paypal/PayPal-Android-SDK/releases)
    [MPL](https://developer.paypal.com/docs/classic/mobile/ht_mpl-itemPayment-Android/) | [1.5.6_49](https://github.com/paypal/sdk-packages/tree/gh-pages/MPL)
* Braintree Android SDK合规版本

    SDK | TLS v1.2 支持版本
    --- | --------
    [Android 2.x SDK](https://github.com/braintree/braintree_android/) | [2.1.0](https://github.com/braintree/braintree_android/releases/)
    [Android 1.x SDK](https://github.com/braintree/braintree_android/) | [1.7.6](https://github.com/braintree/braintree_android/releases/)


#### iOS

由于苹果从iOS 5开始引入了TLSv1.2支持，[PayPal iOS SDK](https://github.com/paypal/PayPal-iOS-SDK) 和 [Braintree iOS SDK](https://github.com/braintree/braintree_ios) 都基于iOS 7或以上版本，所以2013年后集成的iOS原生APP不受此次TLS升级影响。
