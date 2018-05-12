# TLSv1.2 安全升级

The Payment Card Industry Security Standards Council (PCI SSC) [mandates](http://blog.pcisecuritystandards.org/migrating-from-ssl-and-early-tls) that **all credit card processors must retire early versions of TLS from service by the PCI deadline**. 

As part of this requirement, PayPal and Braintree are making this upgrade alongside the rest of the payments industry. PayPal and Braintree are updating its services to require TLS 1.2 for all HTTPS connections. PayPal and Braintree will also require HTTP/1.1 for all connections.

For more official, relevant information, see the [2017-2018 Merchant Security Roadmap Microsite](https://www.paypal-notice.com/en/):
* [TLS 1.2 and HTTP/1.1 Upgrade Microsite](https://www.paypal-notice.com/en/TLS-1.2-and-HTTP1.1-Upgrade/)
* [SSL Certificate Upgrade Microsite](https://www.paypal-notice.com/en/SSL-Certificate-Upgrade-Microsite/)

See also [Updating Your Production Environment to Support TLSv1.2](https://www.braintreepayments.com/blog/updating-your-production-environment-to-support-tlsv1-2/) on the Braintree blog.

## What does this mean for PayPal and Braintree merchants?

Merchants must verify that their systems can use the TLSv1.2 protocol with a SHA-256 certificate. As a merchant, you must make sure that you are up-to-date with security updates, including current versions of operating systems, encryption libraries, and runtime environments.

请按照以下步骤，选择匹配您的服务器、开发语言了解详细自查步骤：

* [自查前提](#prerequisites)
* [Java](#java)
* [.NET](#net)
* [PHP / Python / Ruby / Node.js](#openssl)
* [原生移动APP](#native-mobile-apps)

* * *

### 自查前提 

* 请在您的生产环境服务器上执行以下检查，如果您的服务器为托管服务，请联系托管商或管理员执行检查

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
#### 运行JAVA TLSv1.2检查

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
* Braintree JAVA SDK合规版本

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

### PHP

* [PHP requirements](#php-requirements)
* [Guidelines](#guidelines)
* [To verify your PHP and TLS versions](#to-verify-your-php-and-tls-versions)

#### PHP requirements

* PHP uses the system-supplied cURL library, which requires OpenSSL 1.0.1c or later. 
* You might need to [update your SSL/TLS libraries](http://curl.haxx.se/docs/ssl-compared.html).

#### Guidelines

Find OpenSSL in these locations:

1. OpenSSL installed in your operating system's `openssl version`.
1. OpenSSL extension installed in your PHP. Find this in your `php.ini`.
1. OpenSSL used by PHP_CURL.`curl_version()`. <a id="option-3"></a> 

These OpenSSL extensions can be different, and you update each one separately.

PayPal and other PHP SDKs use the same OpenSSL extension that [PHP_CURL](#option-3) uses to make HTTP connections. The PHP_CURL OpenSSL extension must support TLSv1.2.

The `php_curl` library uses its own version of the OpenSSL library, which is not the same version that PHP uses, which is the `openssl.so` file in `php.ini`.

#### To verify your PHP and TLS versions

1. To find the `openssl_version` information for cURL, run:

    ```
    php -r 'echo json_encode(curl_version(), JSON_PRETTY_PRINT);'
    ```

    The returned `php_curl` version might be different from the `openssl version` because they are different components.

1. When you update your OpenSSL libraries, you must update the `php_curl` OpenSSL version and not the OS OpenSSL version.

1. Download [cacert.pem](php/cacert.pem) and [TlsCheck.php](php/TlsCheck.php).

1. In a shell on your **production system**, run:

    ```
    php -f TlsCheck.php
    ```

    * On success:
        
        ```
        PayPal_Connection_OK
        ```
        
    * On failure:
            
        ```
        curl_error information
        ```

> **Notes:** <ul><li>Make sure that your command line test uses the same versions of PHP and SSL/TLS libraries that your web server uses.</li><li>If you use MAMP or XAMPP as your development set up, the PHP that is packaged with them uses an earlier version of OpenSSL, which you cannot easily update. For more information about this issue and a temporary workaround, see [Unknown SSL protocol error](https://github.com/paypal/PayPal-PHP-SDK/issues/484#issuecomment-176240130).</li></ul>

* * *

### Python

* [Python requirements](#python-requirements)
* [To verify your Python and TLS versions](#to-verify-your-python-and-tls-versions)

#### Python requirements

* Python uses the system-supplied OpenSSL. 
* TLSv1.2 requires OpenSSL 1.0.1c or later.

#### To verify your Python and TLS versions

1. In a shell on your **production system**, run the command for your environment: 

    * For Python 2.x:

        ```
        $ python -c "import urllib2; print(urllib2.urlopen('https://tlstest.paypal.com/').read())"
        ```

    * For Python 3.x:

        ```
        $ python -c "import urllib.request; print(urllib.request.urlopen('https://tlstest.paypal.com/').read())"
        ```

        * On success:
            
            ```
            PayPal_Connection_OK
            ```
        
        * On failure, an `URLError` is raised:
            
            ```
            urllib2.URLError: <urlopen error EOF occurred in violation of protocol (_ssl.c:590)>
            urllib2.URLError: <urlopen error [Errno 54] Connection reset by peer>
            ```

* * *

### Ruby

* [Ruby requirements](#ruby-requirements)
* [PayPal legacy Ruby SDK update](#paypal-legacy-ruby-sdk-update)
* [To verify your Ruby and TLS versions](#to-verify-your-ruby-and-tls-versions)

#### Ruby requirements

* Ruby 2.0.0 or later and OpenSSL 1.0.1c or later are required:
    
    * Ruby 2.0.0 or later is required to use TLSv1.2 from the system-supplied OpenSSL.
    * TLSv1.2 requires OpenSSL 1.0.1c or later.

* To update your dependencies, you might need to run `bundle update`.

#### PayPal legacy Ruby SDK update

For the PayPal legacy Ruby SDK packaged as `PP_Ruby_NVP_SDK.zip`, download this [PP_Ruby_NVP_SDK.zip](https://github.com/paypal/TLS-update/blob/master/ruby/PP_Ruby_NVP_SDK.zip).

#### To verify your Ruby and TLS versions

1. In a shell on your **production system**, run:

    ```
    $ ruby -r'net/http' -e 'puts Net::HTTP.get(URI("https://tlstest.paypal.com/"))'
    ```

    * On success:
    
        ```
        PayPal_Connection_OK
        ```

    * On failure, a `OpenSSL::SSL::SSLError` or `EOFError` is thrown.

* * *

### Node

* [Node requirements](#node-requirements)
* [To verify your Node and TLS versions](#to-verify-your-node-and-tls-versions)

#### Node requirements

* Node.js uses the system supplied OpenSSL.
* TLSv1.2 requires OpenSSL 1.0.1c or later.

#### To verify your Node and TLS versions

1. In a shell on your **production system**, run:

    ```
    $ node -e "var https = require('https'); https.get('https://tlstest.paypal.com/', function(res){ console.log(res.statusCode) });"
    ```

    * On success:
        
        ```
        200
        ```
    * On failure, a network error occurs.

* * *

## Native Mobile Apps

* [Android](#android)
* [iOS](#ios)
* [Windows](#windows)

### Android

* [Android requirements](#android-requirements)
* [Supported SDKs](#supported-sdks-android)

#### Android requirements

TLSv1.2 is the default for client connections in API 20 (Android 4.4W or `KITKAT` - wearable extensions).

All Android app developers must make sure that their code and PayPal or Braintree SDKs provide explicit support for TLSv1.2. To verify correct implementation, test apps on API 16 through 19 devices (Android 4.1 through 4.4 platforms).

After the TLSv1.2 upgrade, native app support for user devices earlier than API 16 (Android 4.1 or `JELLY_BEAN`) are not available. Fortunately, as of April 16, 2018, [Google reports 0.7% of devices accessing the Play store are API 15 or earlier](http://developer.android.com/about/dashboards/index.html#Platform).

Users of the PayPal or Braintree Android SDKs must update to the latest version. To illustrate how to support TLSv1.2 outside of the SDK, we have provided an [example Android app](android/).

<a id="supported-sdks-android"></a>

#### Supported SDKs

* [PayPal](PayPal/README.md#android)
* [Braintree](Braintree/README.md#android)

### iOS

TLSv1.2 support was introduced in iOS 5. The [PayPal iOS SDK](https://github.com/paypal/PayPal-iOS-SDK) and the [Braintree iOS SDK](https://github.com/braintree/braintree_ios) both require iOS 7 or later. Apps built since 2013 will likely not need any updates.

### Windows

Neither PayPal nor Braintree support any Windows SDKs. For a web browser integration, we recommend [Braintree's JavaScript SDK](https://developers.braintreepayments.com/javascript+dotnet/guides/client-sdk).
