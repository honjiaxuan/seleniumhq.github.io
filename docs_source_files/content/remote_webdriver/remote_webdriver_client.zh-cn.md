---
title: "远程WebDriver客户端"
weight: 2
---

要运行远程WebDriver客户端, 我们首先需要连接到RemoteWebDriver.
为此, 我们将URL指向运行测试的服务器的地址. 
为了自定义我们的配置, 我们设置了既定的功能. 
下面是一个实例化样例, 
其指向我们的远程Web服务器 _www.example.com_ 的远程WebDriver对象, 
并在Firefox上运行测试.

{{< code-tab >}}
  {{< code-panel language="java" >}}
FirefoxOptions firefoxOptions = new FirefoxOptions();
WebDriver driver = new RemoteWebDriver(new URL("http://www.example.com"), firefoxOptions);
driver.get("http://www.google.com");
driver.quit();
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
from selenium import webdriver

firefox_options = webdriver.FirefoxOptions()
driver = webdriver.Remote(
    command_executor='http://www.example.com',
    options=firefox_options
)
driver.get("http://www.google.com")
driver.quit() 
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
 FirefoxOptions firefoxOptions = new FirefoxOptions();
 IWebDriver driver = new RemoteWebDriver(new Uri("http://www.example.com"), firefoxOptions);
 driver.Navigate().GoToUrl("http://www.google.com");
 driver.Quit();
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
require 'selenium-webdriver'

driver = Selenium::WebDriver.for :remote, url: "http://www.example.com", desired_capabilities: :firefox
driver.get "http://www.google.com"
driver.close
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const { Builder, Capabilities } = require("selenium-webdriver");
var capabilities = Capabilities.firefox();
(async function helloSelenium() {
    let driver = new Builder()        
        .usingServer("http://example.com")   
        .withCapabilities(capabilities)
        .build();
    try {
        await driver.get('http://www.google.com');
    } finally {
        await driver.quit();
    }
})();  
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
firefoxOptions = FirefoxOptions()
driver: WebDriver = new RemoteWebDriver(new URL("http://www.example.com"), firefoxOptions)
driver.get("http://www.google.com")
driver.quit()
  {{< / code-panel >}}
{{< / code-tab >}}


为了进一步自定义测试配置, 我们可以添加其他既定的功能.


## 浏览器选项

例如, 假设您想使用Chrome版本67
在Windows XP上运行Chrome:

{{< code-tab >}}
  {{< code-panel language="java" >}}
ChromeOptions chromeOptions = new ChromeOptions();
chromeOptions.setCapability("browserVersion", "67");
chromeOptions.setCapability("platformName", "Windows XP");
WebDriver driver = new RemoteWebDriver(new URL("http://www.example.com"), chromeOptions);
driver.get("http://www.google.com");
driver.quit();
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
from selenium import webdriver

chrome_options = webdriver.ChromeOptions()
chrome_options.set_capability("browserVersion", "67")
chrome_options.set_capability("platformName", "Windows XP")
driver = webdriver.Remote(
    command_executor='http://www.example.com',
    options=chrome_options
)
driver.get("http://www.google.com")
driver.quit()  
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
var chromeOptions = new ChromeOptions();
chromeOptions.BrowserVersion = "67";
chromeOptions.PlatformName = "Windows XP";
IWebDriver driver = new RemoteWebDriver(new Uri("http://www.example.com"), chromeOptions);
driver.Navigate().GoToUrl("http://www.google.com");
driver.Quit();
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
caps = Selenium::WebDriver::Remote::Capabilities.chrome
caps.platform = Windows XP
caps.version = 67

driver = Selenium::WebDriver.for :remote, :url => "http://www.example.com", :desired_capabilities => caps
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
const { Builder } = require("selenium-webdriver");
const chrome = require("selenium-webdriver/chrome");
let opts = new chrome.Options();
opts.setAcceptInsecureCerts(true);
opts.setBrowserVersion('67');
opts.setPlatform('Windows XP');
(async function helloSelenium() {
    let driver = new Builder()
        .usingServer("http://example.com")
        .forBrowser('chrome')
        .setChromeOptions(opts)
        .build();
    try {
        await driver.get('http://www.google.com');
    }
    finally {
        await driver.quit();
    }
})();
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
val chromeOptions = ChromeOptions()
chromeOptions.setCapability("browserVersion", "67")
chromeOptions.setCapability("platformName", "Windows XP")
val driver: WebDriver = new RemoteWebDriver(new URL("http://www.example.com"), chromeOptions)
driver.get("http://www.google.com")
driver.quit()
  {{< / code-panel >}}
{{< / code-tab >}}


## 本地文件检测器

本地文件检测器允许将文件从客户端计算机传输到远程服务器. 
例如, 如果测试需要将文件上传到Web应用程序, 
则远程WebDriver可以在运行时
将文件从本地计算机自动传输到远程Web服务器. 
这允许从运行测试的远程计算机上载文件. 
默认情况下未启用它, 可以通过以下方式启用:

{{< code-tab >}}
  {{< code-panel language="java" >}}
driver.setFileDetector(new LocalFileDetector());
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
from selenium.webdriver.remote.file_detector import LocalFileDetector

driver.file_detector = LocalFileDetector()
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
var allowsDetection = this.driver as IAllowsFileDetection;
if (allowsDetection != null)
{
   allowsDetection.FileDetector = new LocalFileDetector();
}
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
@driver.file_detector = lambda do |args|
  # args => ["/path/to/file"]
  str = args.first.to_s
  str if File.exist?(str)
end
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
var remote = require('selenium-webdriver/remote');
driver.setFileDetector(new remote.FileDetector);   
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
driver.fileDetector = LocalFileDetector()
  {{< / code-panel >}}
{{< / code-tab >}}

定义上述代码后, 
您可以通过以下方式在测试中上传文件:

{{< code-tab >}}
  {{< code-panel language="java" >}}
driver.get("http://sso.dev.saucelabs.com/test/guinea-file-upload");
WebElement upload = driver.findElement(By.id("myfile"));
upload.sendKeys("/Users/sso/the/local/path/to/darkbulb.jpg");
  {{< / code-panel >}}
  {{< code-panel language="python" >}}
driver.get("http://sso.dev.saucelabs.com/test/guinea-file-upload")

driver.find_element(By.ID, "myfile").send_keys("/Users/sso/the/local/path/to/darkbulb.jpg")
  {{< / code-panel >}}
  {{< code-panel language="csharp" >}}
driver.Navigate().GoToUrl("http://sso.dev.saucelabs.com/test/guinea-file-upload");
IWebElement upload = driver.FindElement(By.Id("myfile"));
upload.SendKeys(@"/Users/sso/the/local/path/to/darkbulb.jpg");
  {{< / code-panel >}}
  {{< code-panel language="ruby" >}}
@driver.navigate.to "http://sso.dev.saucelabs.com/test/guinea-file-upload"
    element = @driver.find_element(:id, 'myfile')
    element.send_keys "/Users/sso/SauceLabs/sauce/hostess/maitred/maitred/public/images/darkbulb.jpg"
  {{< / code-panel >}}
  {{< code-panel language="javascript" >}}
driver.get("http://sso.dev.saucelabs.com/test/guinea-file-upload");
var upload = driver.findElement(By.id("myfile"));
upload.sendKeys("/Users/sso/the/local/path/to/darkbulb.jpg");  
  {{< / code-panel >}}
  {{< code-panel language="kotlin" >}}
driver.get("http://sso.dev.saucelabs.com/test/guinea-file-upload")
val upload: WebElement = driver.findElement(By.id("myfile"))
upload.sendKeys("/Users/sso/the/local/path/to/darkbulb.jpg")
  {{< / code-panel >}}
{{< / code-tab >}}

## Tracing client requests

This feature is only available for Java client binding (Beta onwards). The Remote WebDriver client sends requests to the Selenium Grid server, which passes them to the WebDriver. Tracing should be enabled at the server and client-side to trace the HTTP requests end-to-end. Both ends should have a trace exporter setup pointing to the visualization framework. 
By default, tracing is enabled for both client and server. 
To set up the visualization framework Jaeger UI and Selenium Grid 4, please refer to [Tracing Setup](https://github.com/SeleniumHQ/selenium/blob/selenium-4.0.0-beta-1/java/server/src/org/openqa/selenium/grid/commands/tracing.txt) for the desired version.

For client-side setup, follow the steps below.

### Beta 1 

#### Add the required dependencies

Installation of external libraries for tracing exporter can be done using Maven.
Add the _opentelemetry-exporter-jaeger_ and _grpc-netty_ dependency in your project pom.xml:

```xml
  <dependency>
      <groupId>io.opentelemetry</groupId>
      <artifactId>opentelemetry-exporter-jaeger</artifactId>
      <version>0.14.0</version>
    </dependency>
    <dependency>
      <groupId>io.grpc</groupId>
      <artifactId>grpc-netty</artifactId>
      <version>1.34.1</version>
    </dependency>
``` 

 
#### Add/pass the required system properties while running the client

{{< code-tab >}}
  {{< code-panel language="java" >}}

System.setProperty("JAEGER_SERVICE_NAME", "selenium-java-client");
System.setProperty("JAEGER_AGENT_HOST","localhost");
System.setProperty("JAEGER_AGENT_PORT","14250");

ImmutableCapabilities capabilities = new ImmutableCapabilities("browserName", "chrome");

WebDriver driver = new RemoteWebDriver(new URL("http://www.example.com"), capabilities);

driver.get("http://www.google.com");

driver.quit();

  {{< / code-panel >}}
{{< / code-tab >}}

### Beta 2 onwards 

#### Add the required dependencies

Installation of external libraries for tracing exporter can be done using Maven.
Add the _opentelemetry-exporter-jaeger_ and _grpc-netty_ dependency in your project pom.xml:

```xml
  <dependency>
      <groupId>io.opentelemetry</groupId>
      <artifactId>opentelemetry-exporter-jaeger</artifactId>
      <version>1.0.0</version>
    </dependency>
    <dependency>
      <groupId>io.grpc</groupId>
      <artifactId>grpc-netty</artifactId>
      <version>1.35.0</version>
    </dependency>
``` 
 
#### Add/pass the required system properties while running the client

{{< code-tab >}}
  {{< code-panel language="java" >}}
System.setProperty("otel.traces.exporter", "jaeger");
System.setProperty("otel.exporter.jaeger.endpoint", "http://localhost:14250");
System.setProperty("otel.resource.attributes", "service.name=selenium-java-client");

ImmutableCapabilities capabilities = new ImmutableCapabilities("browserName", "chrome");

WebDriver driver = new RemoteWebDriver(new URL("http://www.example.com"), capabilities);

driver.get("http://www.google.com");

driver.quit();

  {{< / code-panel >}}
{{< / code-tab >}}

Please refer to [Tracing Setup](https://github.com/SeleniumHQ/selenium/blob/selenium-4.0.0-beta-1/java/server/src/org/openqa/selenium/grid/commands/tracing.txt) for more information on external dependencies versions required for the desired Selenium version.

More information can be found at:

* OpenTelemetry: https://opentelemetry.io
* Configuring OpenTelemetry:
    https://github.com/open-telemetry/opentelemetry-java/tree/main/sdk-extensions/autoconfigure
* Jaeger: https://www.jaegertracing.io
* [Selenium Grid Observability]({{< ref "grid/grid_4/advanced_features/observability.zh-cn.md" >}}) 

