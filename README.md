# xiezhenstore
xiezhenstore is a fork for sidestore 
蛤蟆助手是一个无限制、社区驱动的替代appstore，适用于非越狱iOS设备，蛤蟆助手是来自于sidestore和altstore的分支
[！[许可证：AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-blue.svg）](https://www.gnu.org/licenses/agpl-3.0)
[！[PR welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://makeapullrequest.com)
[！[生成并上载SideStore](https://github.com/SideStore/SideStore/actions/workflows/build.yml/badge.svg)](https://github.com/SideStore/SideStore/actions/workflows/build.yml)
蛤蟆助手（SideStore）是一款iOS应用程序，允许您仅使用Apple ID将应用程序加载到iOS设备上。SideStore使用您的个人开发证书安装应用程序，然后使用[专门设计的VPN](https://github.com/jkcoxson/Secret-Tunnel)以欺骗iOS安装它们。SideStore将定期在后台“刷新”您的应用程序，以防止其正常的7天开发期到期。
蛤蟆助手（SideStore）的目标是提供无限制的侧加载体验。这是[AltStore]的社区驱动分支(https://github.com/rileytestut/AltStore)，并且已经实现了社区最需要的一些功能。
（欢迎投稿！🙂)
## 编译要求
-Xcode 14
-iOS 14+
-Rustup（“brew install Rustup”）
为什么选择iOS 14？针对这样一个最新版本的iOS，我们可以加快开发速度，尤其是因为没有多少开发人员可以在较旧的设备上进行测试。SwiftUI支持要好得多，这一事实证明了这一点，使我们可以转换为更现代的UI代码库。
## Project Overview

### SideStore
SideStore is a just regular, sandboxed iOS application. The AltStore app target contains the vast majority of SideStore's functionality, including all the logic for downloading and updating apps through SideStore. SideStore makes heavy use of standard iOS frameworks and technologies most iOS developers are familiar with.

### EM Proxy
[EM-Proxy](https://github.com/jkcoxson/em_proxy) powers the defining feature of SideStore: untethered app installation. By levaraging an App Store app with additional entitlements (WireGuard) to create the VPN tunnel for us, it allows SideStore to take advantage of [Jitterbug](https://github.com/osy/Jitterbug)'s loopback method without requiring a paid developer account.

### Minimuxer
[Minimuxer](https://github.com/jkcoxson/minimuxer) is a lockdown muxer that can run inside iOS’s sandbox. It replicates Apple’s usbmuxd protocol on MacOS to “discover” devices to interface with wireguard On-Device.

### Roxas
[Roxas](https://github.com/rileytestut/roxas) is Riley Testut's internal framework from AltStore used across many of their iOS projects, developed to simplify a variety of common tasks used in iOS development.

We're hoping to eventually eliminate our dependency on it, as it increases the amount of unnecessary Objective-C in the project.

## Compilation Instructions
SideStore is fairly straightforward to compile and run if you're already an iOS or macOS developer. Here are some basic instructions to get you started:

1. Clone the repository 
	```
	git clone https://github.com/SideStore/SideStore.git --recurse-submodules
	```
2. After installing Rustup, run `rustup target add aarch64-apple-ios`
12. Within the Dependencies/em_proxy and Dependencies/minimuxer directories, run `cargo build --release --target aarch64-apple-ios`
2. Open `AltStore.xcodeproj` and select the AltStore project in the project navigator. On the `Signing & Capabilities` tab, change the team from to your own account.
3. **(Development only)** Change the value for `ALTDeviceID` in the Info.plist to your device's UDID. Normally, SideServer embeds the device's UDID in SideStore's Info.plist during installation. When running through Xcode you'll need to set the value yourself or else SideStore won't resign (or even install) apps for the proper device. You can achieve this by changing a few things to be able to build and use SideStore.
5. Copy `CodeSigning.xcconfig.sample` to `CodeSigning.xcconfig`
6. Fill out all of the properties in `CodeSigning.xcconfig` to match your account.
7. In `Shared/Extensions/Bundle+AltStore.swift`, replace "group.com.rileytestut.AltStore" with your own App Group ID. 
8. Build + run app! 🎉
##许可
此项目根据AGPLv3许可证获得许可。
