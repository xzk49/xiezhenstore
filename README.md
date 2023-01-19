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
## 项目概况
### ideStore（SideStore）
SideStore是一个普通的沙盒iOS应用程序。AltStore应用程序目标包含SideStore的绝大多数功能，包括通过SideStore下载和更新应用程序的所有逻辑。SideStore大量使用大多数iOS开发人员熟悉的标准iOS框架和技术。
### EM代理
[EM代理](https://github.com/jkcoxson/em_proxy)支持SideStore的定义功能：无约束应用程序安装。通过提升App Store应用程序的附加权限（WireGuard），为我们创建VPN隧道，SideStore可以利用[Jitterbug](https://github.com/osy/Jitterbug)的环回方法，无需付费开发人员帐户。
### 小型多路复用器
[小型多路复用器](https://github.com/jkcoxson/minimuxer)是一个可以在iOS沙盒中运行的锁定多路复用器。它在MacOS上复制了苹果的usbmuxd协议，以“发现”与设备上的wireguard接口的设备。
### 罗克萨斯
[罗克萨斯](https://github.com/rileytestut/roxas)是Riley Testut的AltStore内部框架，用于他们的许多iOS项目，旨在简化iOS开发中使用的各种常见任务。
我们希望最终消除对它的依赖，因为它增加了项目中不必要的Objective-C的数量。
## 编制说明
如果您已经是iOS或macOS开发人员，SideStore的编译和运行非常简单。以下是一些基本说明，可帮助您入门：
1.克隆存储库
git克隆https://github.com/SideStore/SideStore.git--递归子模块
2.安装Rustup后，运行“Rustup target add aarch64 apple ios”`
12.在Dependencies/em_proxy和Dependencies/minimuxer目录中，运行`cargo build--release--target aarch64 apple ios`
2.打开“AltStore.xcodeproj”并在项目导航器中选择AltStore项目。在“签名和能力”选项卡上，将团队从更改为您自己的帐户。
3.**（仅限开发）**将Info.plist中“ALTDeviceID”的值更改为设备的UDID。通常，SideServer在安装期间将设备的UDID嵌入SideStore的Info.plist中。在运行Xcode时，您需要自己设置值，否则SideStore将不会为正确的设备退出（甚至安装）应用程序。您可以通过更改一些内容来构建和使用SideStore来实现这一点。
5.将`CodeSigning.xcconfig.sample`复制到`CodeSigning_xcconfig`
6.填写“CodeSigning.xcconfig”中的所有属性以匹配您的帐户。
7.在“共享/扩展/捆绑包+AltStore.swift”中，用您自己的应用程序组ID替换“group.com.rileytestut.AltStore”。
8.构建+运行应用程序！🎉
##许可
此项目根据AGPLv3许可证获得许可。
