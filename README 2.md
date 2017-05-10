Pandora
=

前言
-

[Pandora](https://gitlab.wallstcn.com/mobileios/Pandora)是主站的重构项目主站，基于模块进行开发。

概述
-

* 1、所有子工程，需要提供相应的测试APP，且可以独立运行；
* 2、关于命名，所有模块的命名，均以M开头，以Framework结尾；
* 3、关于注释，请使用VVDocumenter进行描述的添加；


一、代码管理
-

* 1、<b>Master分支</b>
    属于供线上发布的代码，仅从Develop分支合并过来;；
* 2、<b>Develop分支</b>
    此分支为所有模块开发中，较为完整的分支；提供给测试人员的包由此分支出；
    另，此分支亦作为代码review的分支，只有通过review才能合并；
* 3、<b>小组分支、模块分支</b>
    此分支为具体的开发人员的分支，如wscn(主站分支)，quotes(行情分支)，hotfix(线上修复分支)；此分支下的代码，在每次提交之前，需要先合并Develop分支，谨记；
* 4、代码规范[https://www.mediawiki.org/wiki/Wikimedia_Apps/Team/iOS/ObjectiveCStyleGuide](https://www.mediawiki.org/wiki/Wikimedia_Apps/Team/iOS/ObjectiveCStyleGuide)


二、子工程（排名不分先后）
-

| 模块名称		|工程							| 使用中的APP	|    备注	|		作者	|
|---------------|-------------------------------|--------|-----------|----------|
|WSCN			|WSCN			 				|wscn		|主工程		|			|
|Account模块      |MAccountFramework              |wscn       |帐号           |           |
|Author模块      |MAuthorFramework              |wscn       |作者           |           |
|MMCenter模块      |MMCenterFramework              |wscn       |消息中心           |           |
|Launcher模块	|MLauncherFramework				|wscn		|启动页面			|			|
|Home模块		|MHomeFramework					|wscn		|主页			|			|
|News模块     |MNewsFramework                 |wscn       |资讯         |           |
|NewsDetail模块     |MNewsDetailFramework                 |wscn       |资讯详情         |           |
|Live模块     |MLiveFramework                 |wscn       |直播          |           |
|Calendar模块     |MCalendarFramework                 |wscn       |日历         |zhuyi           |
|Vendor模块     |MVendorFramework                 |wscn       |第三方         |           |
|Setting模块			|MSettingFramework				|wscn		|设置			|			|
|Quotes模块       |MQuotesFramework               |wscn       |行情           |           |
|LiveRoom模块       |JLiveRoomFramework        |wscn       |直播间详情           |liumingyuan           |
|GoldQuotes模块       |GoldQuotesFramework        |Gold       |黄金行情           |           |
|AD模块       |MADFramework        |wscn       |广告模块          |           |
|意见反馈模块  |MFeedbackFramework |wscn       |意见反馈 |    |
|文章搜索模块  |YSearchFramework |wscn       |文章搜索 |zhuyi    |
|黑名单模块  |YBlackListFramework |wscn       |黑名单 |zhuyi    |
|聊天详情模块  |YChatFramework |wscn       |聊天详情 |zhuyi    |
|侧边栏模块  |YSlideMenuFramework |wscn       |侧边栏 |zhuyi    |
|通讯录模块  |YAddressBookFramework |wscn       |通讯录、微博好友 |zhuyi    |
|文章评论模块  |MNewsCommentFramework |wscn       |文章评论 |zhangjixing    |
|支付模块  |MPayFramework |wscn       |支付模块|caojinguo    |
|推送模块  |MPushFramework |wscn       |推送模块|caojinguo    |
|第三方登录模块  |MThirdLoginFramework |wscn       |第三方登录模块|caojinguo    |
|第三方分享模块  |MThirdShareFramework |wscn       |第三方分享模块|caojinguo    |
|网页模块  |MWebViewFramework |wscn       |网页，带进度条 |    |
|离线下载模块  |MOffLineFramework |wscn       |离线下载 |    |
|见识模块  |MWitsFramework |wscn       |见识 |    |





三、工程说明
-

* 新建工程中，需要特别注意如下事项
* Deployment Target  == 7.0开始（暂定）
* Linking->Mach-O Type === Static Librayry
* Search Paths -> Framework Search Paths === ${PROJECT_DIR}/../Products
* 依赖的Framework，全部从根目录中的Products中进行添加
* Aggregate中，需要拷贝两份sh脚本，可以从其他Framework中进行拷贝，不需要任何修改
* Framework中所需要的资源（xib,png,storyboard)单独封包为bundle，特别注意，此bundle需要拷贝到WSCN工程中；
* WSCN项目中，修改MobileRuntime.plist中的Launcher标号，用于启动不同的微应用，其中标号需要在Applications中进行注册；

四、IconFont
-

`iconFont`作为图标文字，业务方不必关心其更新及初始化工作，已由主框架完成，此功能后续可以下沉处理；请使用如下方法
```
[UIFont fontWithName:@"iconfont" size:14.0f];
```

五、Monkey测试
-
*   双击`ui-auto-monkey/monkey-wscn.tracetemplate`即可；
*   配置文件在`ui-auto-monkey/wscn/wscn.js`;

六、用户信息
-
获取当前已经登录的用户状态及信息
```
    id<MAccountService> service = [[BGContext sharedInstance] findServiceByName:@"MAccountService"];
    if ([service isLogined]) {
        ...
    }
```
去登录，其中`useDefault`，表示在未匹配到对应路由时，是否使用网页打开
```
[[MRouter sharedRouter] handleURL:[NSURL URLWithString:@"https://wscn/login"]
                                 userInfo:nil
                               useDefault:NO];
```

七、分享使用
-
分享由路由提供`native://share`，请参阅如下代码
```
- (void) goToRecommend {
    __weak typeof(self) weakSelf = self;
    
    UIImage *image = [UIImage imageNamed:@"share_logo"];
    NSString *content = @"我刚安装了【华尔街见闻】移动端。这里有全球市场实时行情，还有影响市场的实时新闻与深度分析。它是迄今为止我用过的最好的财经类app，推荐你也来下载";
    NSString *url = @"http://wallstreetcn.com/";
    NSString *title = @"【华尔街见闻】移动端,与你分享~";
    
    NSDictionary *commonDict = @{@"content":content,
                                 @"url":url,
                                 @"image":image,
                                 @"title":title};
    
    NSDictionary *weiboDict = @{@"content":[NSString stringWithFormat:@"%@ (下载地址: %@ @华尔街见闻APP) ",content,kAPP_WEIBO_DOWNLOAD_URL],
                                 @"url":kAPP_WEIBO_DOWNLOAD_URL,
                                 @"image":image,
                                 @"title":title};
    
    NSDictionary *qqDict = @{@"content":@"致力于第一时间为您解读各个国家金融市场动向，从专业的角度、用易懂的语言分析全球宏观经济面。",
                             @"url":kAPP_IOS_DOWNLOAD_URL,
                             @"title":@"华尔街见闻"};
    
    NSDictionary *weChatDict = ...;
    
    NSDictionary *wechatTimeLine = ...;

    NSDictionary *SMSDict = ...;
    
    NSDictionary *emailDict = ...;
    
    NSDictionary *copyURLDict = @{@"url":kAPP_TUIGUANG_URL};
    
    NSDictionary *safariDict = ...;
    [[MRouter sharedRouter] handleURL:[NSURL URLWithString:@"native://share"]
                             userInfo:@{@"controller":weakSelf,
                                        @"common":commonDict,
                                        @"weibo":weiboDict,
                                        @"qq":qqDict}];
}
```


八、文章是否已经阅读
--

使用如下`MItemReadService`协议的实现类`MItemReadServiceImpl`进行,   
1、查询`[[MItemReadServiceImpl sharedService] checkItem:_targetURL]`   
2、添加`[[MItemReadServiceImpl sharedService] addItem:_targetURL]`   
3、PCH文件中引入`#import <MServicesFramework/MServicesFramework.h>`

```
/**
 判断某条记录是否已经阅读过

 @param itemIdentifier 唯一标识，建议以类名+唯一标识符进行标识
 
 @return 是否已经阅读
 */
- (BOOL) checkItem:(NSString *) itemIdentifier;


/**
 添加某条记录到已经阅读里面
 
 @param itemIdentifier 唯一标识，建议以类名+唯一标识符进行标识
 */
- (void) addItem:(NSString *) itemIdentifier;
```

九、当前目标是否为PRO
---

使用`IsPRO()`方法进行判断，YES为PRO, NO为Normal;  
1、主工程Target的设置，在`Preprocessor Marcros`中,添加`PRO=1`的变量，非0表示是PRO；  
2、PCH文件中引入#import <MServicesFramework/MServicesFramework.h>

十、JSPatch热修复问题
---

1、上传JSPatch必须是main.js文件，上传前压缩为zip文件，格式为：v3.zip  注：v3中的3代表build号
2、七牛上传地址:`https://portal.qiniu.com/bucket/wscn-download/resource`
   选择域名为：`wdl.wallstreetcn.com`
3、普通版为：wscn/版本号/  例如：wscn/6.0.0/
   Pro版本：wscnPro/版本号/  例如：wscnPro/6.0.0/     （该格式为七牛上传格式）
4、上传路径为：`https://wdl.wallstreetcn.com/[wscn|wscnPro]/{version}/v{build}.zip`
最终完整地址例如：
```
https://wdl.wallstreetcn.com/wscnPro/6.0.0/v3.zip              
https://wdl.wallstreetcn.com/wscn/6.0.0/v3.zip
```
5、上传文件到七牛时，如果你做了内容替换而文件名称没变，这样就要去刷新一遍（刷新在七牛界面右上角个人面板-->刷新预取-->输入外链地址-->提交）

十一、关闭FloatView及直播间详情页
---
使用如下：

id<JLiveRoomService> liveRoomService = [JLiveRoom sharedService];
[liveRoomService shutDownLiveRoom];

十二、第三方Fremework
---

1、Framework

|Framework		|来源				|Framework大小(MB)|  打包增加大小|备注|
|---------------	|---------------	|---------------:|---------------:|---------------|
|AlipaySDK		|阿里支付			|	9|533 KB||
|BeaconAPI_Base	|腾讯	|17.6	|547 KB	|  |
|MSDKDns_C11	|腾讯	|1.5	|71 KB	|	|
|TencentOpenAPI	|腾讯	|7.7	|357 KB	|  |
|IJKMediaFramework|哔哩哔哩	|144.3	|6.5 MB |  |
|UMMobClick	|友盟	|7.5	|303 KB	|	|
|UMSocial		|友盟第三方登录	|147.7 |5 MB |`友盟` `有道` `新浪微博` `微信` `短信` `Facebook` `QQ` `twitter` `Email` `Evernote`|
|MVendorFramework|华尔街见闻|17.6|1.53 MB|`Masonry` `MJRefresh` `MJEXtension` `MBProgressHUD` `YYText` `FDFullscreenPopGesture` `UITableView-FDTemplateLayoutCell` `YYModel` `TTTAttributedLabel` `TFHpple`|
|GoldNetworkFramework|华尔街见闻|19.8|1.23 MB|`AFNetworking` `SDWebImage` `JSONKit` `OpenUDID` `SRWebSocket`|
|KwlOpenSDK|金微蓝|9|(KwlOpenSDK和PAZQOPAVSDK)6.2 MB||
|PAZQOPAVSDK|平安证券|99.4|(KwlOpenSDK和PAZQOPAVSDK)6.2 MB||

2、Bundle

| Bundle |来源| Bundle大小|打包增加大小|
|---|---|:---:|---|
|AlipaySDK|阿里支付|5 KB|5 KB|
|KWLStockSDK|金微蓝|676 KB|70 KB|
|KwlVideoSDK|金微蓝|1.2 MB|230 KB|