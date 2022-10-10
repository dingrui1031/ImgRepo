# Android端打包流程以及注意事项：
## 打包前准备工作
### 第一次脚本打包前的准备工作

* 进入项目中的IcreditTools\Reinforcement目录，解压缩Reinforcement.rar（只需要第一次操作）
* 确认是否已安装python环境，在cmd中输入命令 *python -v* 查看，如果没有安装，需要先安装python环境
### 每次打包前的准备工作

* 确保本地代码是最新可发布的节点，清理as中的缓存，如gradle缓存（当前打包使用的gradle版本是4.6），执行*Clean Project*，必要的话执行*Invalidate Caches*
### 打包涉及的主要目录

* 灰度打包脚本目录：IcreditTools\Reinforcement\ReinforceOneTime_JustOneApk
* 全量打包脚本目录：IcreditTools\Reinforcement\ReinforceOneTime
* 渠道文件目录：IcreditTools\Reinforcement\Resource\Channel\Icredit
* apk输出目录：IcreditTools\Reinforcement\Apk_Result\Icredit
* 原始包目录：apk输出目录\origin
* 市场包目录：apk输出目录\market
* service包目录：apk输出目录\service
* sem包目录（许敏）：apk输出目录\sem_deliver
* 信息流包目录（张玮）：apk输出目录\service_deliver
* 客服包目录：apk输出目录\客服包
* strip目录（用来分析native崩溃）：apk输出目录\strip
* 共享目录：\10.10.12.13\androidApp
* 运营所需云盘目录：钉钉-云盘-分享-App发版（需要管理员添加用户权限）
## 大号打包
### 灰度发布

*  进入**灰度打包脚本目录**，执行*ReinforceScriptOneTime.bat*，双击或者cmd中执行脚本（建议通过cmd方式执行脚本，可以看到错误日志）

![](release_flow_images/android/gray_step_1.png)
#### 公司购买360套餐的情况下：

* 在apk输出目录下，可以看到对应的原始包目录和加固后的qichacha_版本号.apk，进入**灰度交付流程**

![](release_flow_images/android/gray_step_2.png)
#### 公司未购买360套餐的情况：

*  由于360加固不支持命令行加固，导致该脚本最终会在加固阶段失败，需要手动加固

![](release_flow_images/android/gray_step_3.png)

* 360手动加固操作，进入**原始包目录**

![](release_flow_images/android/gray_step_4.png)

* 打开360加固软件，设置打包渠道

![](release_flow_images/android/gray_step_5.png)

* 清空上一次的打包渠道

![](release_flow_images/android/gray_step_6.png)

* 选中灰度发布的渠道文件

![](release_flow_images/android/gray_step_7.png)

* 确认加固渠道正确，关闭当前窗口

![](release_flow_images/android/gray_step_8.png)

* 点击添加应用，进入原始包目录，选中灰度包的apk，开始加固

![](release_flow_images/android/gray_step_9.png)

* 加固成功，打开输出目录

![](release_flow_images/android/gray_step_10.png)

* 删除后缀是jiagu_sign.apk的文件，将另一个apk重命名为qichacha_版本号.apk（如qichacha_16.1.0.apk），进入**灰度交付流程**

![](release_flow_images/android/gray_step_11.png)
#### 灰度交付流程

* 将最终包发到安卓测试群，通知测试验收，测试通知可以发布，即可上传oss

![](release_flow_images/android/gray_step_12.png)

* 通知张全排配置版本升级，监测升级用户，控制在5千左右，达到量级后通知张全排关闭版本升级。观测友盟和我们自己的异常监测后台，即时发现崩溃和异常。
* 升级用户以及异常报表统计地址：[http://grafana.office.qichacha.com/d/ox4umLR4k/da-ping-bao-biao?orgId=1&refresh=5s&from=now-7d&to=now](http://grafana.office.qichacha.com/d/ox4umLR4k/da-ping-bao-biao?orgId=1&refresh=5s&from=now-7d&to=now)
* 异常日志线上环境地址：[http://grafana.office.qichacha.com/explore?orgId=1&left=%5B%22now-7d%22,%22now%22,%22qcc-log-native%22,%7B%22queryText%22:%22*%20and%20fields.eventType:%20crashInfo%22%7D%5D](http://grafana.office.qichacha.com/explore?orgId=1&left=%5B%22now-7d%22,%22now%22,%22qcc-log-native%22,%7B%22queryText%22:%22*%20and%20fields.eventType:%20crashInfo%22%7D%5D)

### 全量发布
#### 公司购买360套餐的情况下：

* 进入**全量打包脚本目录，**执行加固脚本*ReinforceScriptOneTime.bat*，脚本结束后（大约1个半小时），会在**apk输出目录**下看到所有目录，进入**全量包交付流程**

![](release_flow_images/android/release_step_1.png)

![](release_flow_images/android/release_step_2.png)
#### 公司未购买360套餐的情况：

* 进入**全量打包脚本目录，**执行加固前的脚本*ReinforceScriptOneTime_before.bat*，脚本结束后（大约1小时），会在**apk输出目录**下看到原始包等目录

![](release_flow_images/android/release_step_3.png)

* 打开360加固软件，用**原始包目录**中的apk和**渠道文件目录**中的渠道一一对应，手动加固，具体的加固操作参考**灰度打包流程，icredit_qcc_one_apk.txt是灰度渠道文件，全量加固忽略此渠道文件**

![](release_flow_images/android/release_step_4.png)

![](release_flow_images/android/release_step_5.png)

* 加固完成后，进入**全量打包脚本目录**，执行加固后的脚本*ReinforceScriptOneTime_after.bat*，脚本结束后（大约半小时），在**apk输出目录**下会看到所有新生成的目录，进入**全量包交付流程**

![](release_flow_images/android/release_step_6.png)
#### 全量包交付流程

* 在**共享目录**下新建文件夹（以当前版本号命名），将**service包目录**和**市场包目录**中的所有apk复制到**共享目录**，如果提示空间不足，那就需要删掉最早版本的文件夹，直到不再提示空间不足，然后通知自动化测试（苏强强）跑脚本测试

![](release_flow_images/android/release_step_7.png)

* 将**市场包目录**的压缩文件发到安卓测试群，通知测试（林亚萍）抽验市场包目录，测试告知没问题，将**市场包目录**、**service包目录**、**sem包目录（许敏）**、**信息流包目录（张玮）**、**客服包目录** 对应的压缩文件上传至**运营所需云盘目录**，发布群通知运营下载所需的压缩包，后续上传oss等流程交给运营

![](release_flow_images/android/release_step_8.png)

![](release_flow_images/android/release_step_9.png)
## 小号打包

* 小号需要在AS中手动打包，然后根据对应的渠道文件（**见小号对应的分支和打包渠道目录表格**），在360加固软件中手动加固，后续通知测试验收，没问题转交给运营（**高秀云**）
* 相同包名的小号需要一起更新发布，避免只更新其中某一个小号发生抓包。例如，运营要求更新白底小号（华为联运）的同时也需要更新白底小号（其他）

## 应用商店目前在线应用对应的仓库分支和打包渠道目录（appType为smalle和smallf的小号，经排查已弃用）
| | 企查查 | 白底小号（华为联运） | 白底小号（其他） | 工商马甲 | 征信马甲（小米联运） | 征信马甲（其他） | 企查查启航版 | 
| --- | --- | --- | --- | --- | --- | --- | --- |
| 应用类型 | 大号 | 小号 | 小号 | 小号 | 小号 | 小号 | 小号 |
| 包名 | com.android.icredit | com.android.qichacha | com.android.qichacha | com.greatid.business | com.greatid.qcccredit | com.greatid.qcccredit | com.greatld.eyeofgod |
| 签名 | 4D:7B:A9:A8:AC:44:A3:B4:14:5D:8C:DB:9B:10:72:7C或4d7ba9a8ac44a3b4145d8cdb9b10727c |  4D:7B:A9:A8:AC:44:A3:B4:14:5D:8C:DB:9B:10:72:7C或4d7ba9a8ac44a3b4145d8cdb9b10727c |  4D:7B:A9:A8:AC:44:A3:B4:14:5D:8C:DB:9B:10:72:7C或4d7ba9a8ac44a3b4145d8cdb9b10727c |  4D:7B:A9:A8:AC:44:A3:B4:14:5D:8C:DB:9B:10:72:7C或4d7ba9a8ac44a3b4145d8cdb9b10727c |  4D:7B:A9:A8:AC:44:A3:B4:14:5D:8C:DB:9B:10:72:7C或4d7ba9a8ac44a3b4145d8cdb9b10727c |  4D:7B:A9:A8:AC:44:A3:B4:14:5D:8C:DB:9B:10:72:7C或4d7ba9a8ac44a3b4145d8cdb9b10727c |  4D:7B:A9:A8:AC:44:A3:B4:14:5D:8C:DB:9B:10:72:7C或4d7ba9a8ac44a3b4145d8cdb9b10727c |
| 目前线上最新版本 | 16.2.0 | 14.3.2 | 14.3.2 | 15.4.0 | 16.1.0 | 16.1.0 | 15.9.0 |
| 仓库分支名 | origin/master | origin/qichacha_huawei | origin/qichacha_other | origin/ICredit_businnes_small | origin/qcccredit_investigation_xiaomi | origin/qcccredit | origin/EyeOfGod12.2.0 |
| 对应渠道文件目录 | IcreditTools\Reinforcement\Resource\Channel\icredit | IcreditTools\Reinforcement\Resource\Channel\qichacha | IcreditTools\Reinforcement\Resource\Channel\qichacha | IcreditTools\Reinforcement\Resource\Channel\business | IcreditTools\Reinforcement\Resource\Channel\qcccredit | IcreditTools\Reinforcement\Resource\Channel\qcccredit | IcreditTools\Reinforcement\Resource\Channel\eyeofgod |
| apptype（用来区分支付） | - | small | small | smallb | smallc | smallc | smalld |
| branchName（用来区分版本升级） | - | qichacha_huawei | qichacha_other | business | qcccredit | qcccredit | EyeOfGod12.2.0 |
| 闪验一键登录 | APPID:IyNrxqFx | APPID:u2Oylbv1 | APPID:u2Oylbv1 | APPID:IyNrxqFx（未申请，使用的大号APPID） | APPID:IyNrxqFx（未申请，使用的大号APPID） | APPID:IyNrxqFx（未申请，使用的大号APPID） | APPID:IyNrxqFx（未申请，使用的大号APPID） | 
| 微信支付 | APPID:wx1907a2c53c2d6524 | APPID:wx8bd43d0660e86342 | APPID:wx8bd43d0660e86342 | APPID:wxe61d84e8d1120f98 | APPID:wx11acf5ca890099e6 | APPID:wx11acf5ca890099e6 | APPID:wx1347e91b4625f42e |
| 友盟SDK | APPKEY:5459d603fd98c5a9cb003655 APPSECRET:0028ac07f723b22eae712a7a53594bc4 | APPKEY:5796c70b67e58e3b48003cef APPSECRET:52a3f6cb095bfb3fdbe116fe2b3a83de | APPKEY:5796c70b67e58e3b48003cef APPSECRET:52a3f6cb095bfb3fdbe116fe2b3a83de | APPKEY:5950b9ca734be402260007d6 APPSECRET:0028ac07f723b22eae712a7a53594bc4 | APPKEY:5950b9b3f43e483159001909 APPSECRET:0028ac07f723b22eae712a7a53594bc4 | APPKEY:5950b9b3f43e483159001909 APPSECRET:0028ac07f723b22eae712a7a53594bc4 | APPKEY:5b232a028f4a9d138a000073 APPSECRET:b0b7609bdf242afdd1039a15e8ad8f89 |
| 微信登录分享（友盟） | APPID:wx1907a2c53c2d6524 APPSECRET:109ecc4fda0ff43eec167b3aaa379088 | APPID:wx8bd43d0660e86342 APPSECRET:f472f9e7dea53568b558b7b8ab6c66b4 | APPID:wx8bd43d0660e86342 APPSECRET:f472f9e7dea53568b558b7b8ab6c66b4 | APPID:wxe61d84e8d1120f98 APPSECRET:f10868eca17560a975671624638418ac | APPID:wx11acf5ca890099e6 APPSECRET:f8197facce04bc54d20a132728643d83 | APPID:wx11acf5ca890099e6 APPSECRET:f8197facce04bc54d20a132728643d83 | APPID:wx1347e91b4625f42e APPSECRET:2160af1302e9e404698bd9073f687446 |
| QQ登录分享（友盟） | APPID:1103443672 APPSECRET:9LA68EUXG3Mr3Pl6 | APPID:1105102610 APPSECRET:d9u672TbAchWdCbK | APPID:1105102610 APPSECRET:d9u672TbAchWdCbK | APPID:1106164150 APPSECRET:draY2NV8HzAdXqj9 | APPID:1106164144 APPSECRET:Q5IJHN0M9OBi3tC7 | APPID:1106164144 APPSECRET:Q5IJHN0M9OBi3tC7 | APPID:1107873610 APPSECRET:ngHBeVrm7diEPUdM |
| 新浪微博分享（友盟） | APPID:3643419138 APPSECRET:6b09c0b9485a11d60cce5de32a9472e1 | APPID:3643419138 APPSECRET:6b09c0b9485a11d60cce5de32a9472e1 | APPID:3643419138 APPSECRET:6b09c0b9485a11d60cce5de32a9472e1 | APPID:3643419138 APPSECRET:6b09c0b9485a11d60cce5de32a9472e1 | APPID:3643419138 APPSECRET:6b09c0b9485a11d60cce5de32a9472e1 | APPID:3643419138 APPSECRET:6b09c0b9485a11d60cce5de32a9472e1 | APPID:3643419138 APPSECRET:6b09c0b9485a11d60cce5de32a9472e1 |
| 钉钉分享（友盟） | APPID:dingoah8purfjnyguhxkju | APPID:dingoayx7ikzc13xvchew7 | APPID:dingoayx7ikzc13xvchew7 | APPID:dingoah8purfjnyguhxkju（未申请，使用的大号APPID） | APPID:dingoa5dylq2sizb2hwbhf | APPID:dingoa5dylq2sizb2hwbhf | APPID:dingoah8purfjnyguhxkju（未申请，使用的大号APPID） |
| 百度地图 | APPKEY:mlTplV340an11qNEh8icTkGL | APPKEY:bQBhabHqPAYy4eZaUGBj6515Hc4rBu0C | APPKEY:bQBhabHqPAYy4eZaUGBj6515Hc4rBu0C | APPKEY:OftlRNms6mUrbPBlBa9aEzEFgQ2PWrnT | APPKEY:hF4XInICPVQHtHTGZO467wq1Bsy0RBUi | APPKEY:hF4XInICPVQHtHTGZO467wq1Bsy0RBUi | APPKEY:vrfYaUaKGGslKNaGbVGnpDSCUFDpxPZF |
| 诸葛IO | APPKEY:de1d1a35bfa24ce29bbf2c7eb17e6c4f | APPKEY:9a859d139b1d4aafbd65a33d59286b82 | APPKEY:9a859d139b1d4aafbd65a33d59286b82 | APPKEY:9a859d139b1d4aafbd65a33d59286b82（未申请，使用的qichacha_other的APPKEY） | APPKEY:9a859d139b1d4aafbd65a33d59286b82（未申请，使用的qichacha_other的APPKEY） | APPKEY:9a859d139b1d4aafbd65a33d59286b82（未申请，使用的qichacha_other的APPKEY） | APPKEY:de1d1a35bfa24ce29bbf2c7eb17e6c4f |
| 华为支付 | - | APPID:10622460 | APPID:10622460 | - | - | - | - |
| 小米支付 | - | - | - | - | APPID:2882303761517579318 APPKEY:5951757956318 | - | - |
| Bugly | - | APPID:aba401e1c8 APPKEY:fa933b53-5259-4ef0-9f59-b46c629bdbd6 | APPID:aba401e1c8 APPKEY:fa933b53-5259-4ef0-9f59-b46c629bdbd6 | - | - | - | - |
| 科大讯飞语音 | APPKEY:56d79889 | APPKEY:56d79889 | APPKEY:56d79889 | APPKEY:56d79889 | APPKEY:56d79889 | APPKEY:56d79889 | APPKEY:56d79889 |

## 注意事项

* 以上脚本打包只针对windows系统下，主包使用脚本打包的流程，小号一般都是as中手动打包加固
* oss上的下载地址，需要提供对外下载功能时，域名需要改为https://co-image.qichacha.com
* 目前公司未购买360套餐，使用360加固一天有20次限额（已认证），且无法使用命令行打包，目前原始渠道共11个，避免同一天打两次全量包（超过限额需要购买套餐）。
## 账密

* 360加固：
  账号：services[@gelawa.com ](/gelawa.com )
  密码：Pass@778800#
* 打包电脑向日葵：
  识别码：483010625
  验证码：4F0bcN
