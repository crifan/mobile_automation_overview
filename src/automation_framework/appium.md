# Appium

移动端测试框架中，流行度最广的是：`Appium`

## Appium依赖于底层框架

* iOS
    * `iOS >= 9.3`: Apple's [XCUITest](https://developer.apple.com/reference/xctest)
    * `iOS <= 9.3`: Apple's [UIAutomation](https://web.archive.org/web/20160904214108/https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/UIAutomationRef/)
* `Android >= 4.3`: Google's [UiAutomator / UiAutomator2](https://developer.android.com/training/testing/ui-automator)
* `Windows`: Microsoft's [WinAppDriver](http://github.com/microsoft/winappdriver)

## iOS自动化

对于iOS的真机，需要预先配置好才可以：

关于Appium的真机的设置：

* 官网 英文
  * [XCUITest Real Devices (iOS) - Appium](https://appium.io/docs/en/drivers/ios-xcuitest-real-devices/)
* readthedocs 中文
  * [Real devices ios - appium](https://appium.readthedocs.io/en/latest/cn/appium-setup/real-devices-ios/)
* GitHub
  * [appium-xcuitest-driver/real-device-config.md at master · appium/appium-xcuitest-driver](https://github.com/appium/appium-xcuitest-driver/blob/master/docs/real-device-config.md)
