#MTradeFramework
=
<span id="jump"></span>
##目录
* [前言](#0)  
* [一、简介](#1)  
* [二、功能](#2)  
* [三、A股交易](#3)  
* <font color=white>221</font>	[1. 接入使用](#3.1)  
* <font color=white>221</font>	[2. A股交易代码相关](#3.2)  
* <font color=white>22222</font>	[2.1 API & URL](#3.2.1)
* <font color=white>22222</font>	[2.2 账户系统](#3.2.2)
* <font color=white>22222</font>	[2.3 登录逻辑](#3.2.3)
* <font color=white>22222</font>	[2.4 加密方法](#3.2.4)
* <font color=white>22222</font>	[2.5 第三方SDK](#3.2.5)
* [四、基金交易](#4)
* [五、更新](#5)
* <font color=white>22222</font>	[5.1 路由](#5.1)
* <font color=white>22222</font>	[5.2 见闻->我的页面](#5.2)
* <font color=white>22222</font>	[5.3 立即绑定](#5.3)


<h2 id="0">前言</h2>

MTradeFramework提供A股交易和基金交易。

<h2 id="1">一、简介</h2>


*  作者： 张吉鑫
*  版本： V2.0
*  适用： WSCN V5.0.5 
*  依赖： 
 
 	```
 	1.WSCN工程中的Framework
 	
	MWebViewFramework.framework
	GoldSectionFramework.framework
	GoldRPCFramework.framework
	MRouterFramework.framework
	MAccountFramework.framework
	GoldBaseFramework.framework
	GoldDataFramework.framework
	GoleNetworkFramework.framework
	MAuthorFramework.framework
	MMobileFramework.framework
	MVendorFramework.framework
	LNChart.framework
	
	2.第三方的SDK用于A股交易，接入PAZQOPAVSDK.framework还需要参考平安的文档。
	
	PAZQOPAVSDK.framework
	KwlOpenSDK.framework
	
	3.依赖的系统库
	参考“2016年12月29日华尔街见闻4_4_8接入的金微蓝-平安SDK”中的“金微蓝开户SDK集成文档”
	
 	```
*  提供: MTradeFramework.framework

<h2 id="2">二、功能</h2>

* A股交易
* 基金交易 

<h2 id="3">三、A股交易</h2>

<h3 id="3.1">1.接入使用</h3>

#### 1)通过路由调用
路由表如下：
>
* 闪电开户：	astock.wallstreetcn.com
* 立即绑定：	astock.wallstreetcn.com/paLogin
* 买买买：	 	astock.wallstreetcn.com
* 一键打新：	astock.wallstreetcn.com/more

* 进入A股交易主页面

	**进入A股交易主界面不需要传参数。(见闻登录后在“我的”界面中A股交易下的“买买买”，未登录对应为“闪电开户”)**

	```
	NSString *astock = [NSString stringWithFormat:@"https://astock.wallstreetcn.com"];
   [[MRouter sharedRouter] handleURL:[NSURL URLWithString: astock] userInfo:nil];
	```

* 进入买卖页面    

	**进入买卖入口需要传递两个参数，symbol和transactionType。(由股票详情页下方的“买入／卖出”调用)**

	```
	NSMutableDictionary *parameterDic = [NSMutableDictionary dictionary];
    [parameterDic setObject:prod_code forKey:@"symbol"];
    
    if (quotesTradeType == QuotesTradeType_Buy) {
        [parameterDic setObject:@"buy" forKey:@"transactionType"];
        
    } else if (quotesTradeType == QuotesTradeType_Sell){
        [parameterDic setObject:@"sell" forKey:@"transactionType"];
        
    } else {
        return NO;
    }
    NSString *transaction = [NSString stringWithFormat:@"https://astock.wallstreetcn.com/transaction"];
   [[MRouter sharedRouter] handleURL:[NSURL URLWithString:transaction] userInfo:parameterDic];
   
	```
	
* 一键打新

	**进入一键打新界面。（见闻登录后在“我的”界面中A股交易下的“一键打新”，是A股交易的更多页面）**

	```
	NSString *more = [NSString stringWithFormat:@"https://astock.wallstreetcn.com/more"];
   [[MRouter sharedRouter] handleURL:[NSURL URLWithString: more] userInfo:nil];
   
	```
	
* 立即绑定

	**进入pa-login。(见闻未登录时在“我的”界面中A股交易下的为“立即绑定”)**

	```
	NSString *paLogin = [NSString stringWithFormat:@"https://astock.wallstreetcn.com/paLogin"];
   [[MRouter sharedRouter] handleURL:[NSURL URLWithString: paLogin] userInfo:nil];
	
	```

	
	
####2)调用类方法

A股交易对外暴露了两个接口：

```
/**
 *  进入A股交易主页面，直接调用这个类方法，由类方法来push页面
 */
+ (void)loadZAStockVC;

/**
 *  由NWQuoteDetailViewController的买卖入口进入A股交易的买卖H5
 *
 *  @param type   区分买入和卖出
 *  @param symbol 股票代码，格式为见闻内部的股票代码。例如：SZ000529
 */
+ (void)loadTransactionViewWithType:(TransactionType)type symbol:(NSString *)symbol;

```

* 进入A股交易主页面

	**进入A股交易主界面不需要传参数。**

	```
	[ZAStock loadZAStockVC];
	
	```

* 进入买卖页面

	**进入买卖入口需要传递两个参数，symbol和transactionType**

	```
	[ZAStock loadTransactionViewWithType:TransactionTypeSell symbol:symbol];
   
	```
	
<h3 id="3.2">2.A股交易代码相关</h3>

* API & URL
* 账户系统
* 登录逻辑
* 加密方法等
* 接入的第三方SDK

<h4 id="3.2.1">2.1 API & URL</h4>

#####第一部分：API
**一、HOST**

正式环境：  
测试环境：

**二、功能**

**1.券商登录获取Token（绑定）**

(1) URL  
`/user/reget/token?api_key=APIKEY&app_type=wallstreet&sign=SIGN&_=TIMESTAMP`

(2) 请求方式:`GET`

(3) 请求参数，将URL中对应参数的值替换

|Name|Description|
|------|-------|
| stock-token | 本地存储的stock-token|
|app_type|wallstreet|
| sign|签名|
|_|时间戳|

**2.A股版面数据**

(1) URL  
`/transation/home?stock-token=TOKEN&sign=SIGN&_=TIMESTAMP`

(2) 请求方式:`GET`

(3) 请求参数，将URL中对应参数的值替换

|Name|Description|
|------|-------|
| stock-token | 本地存储的stock-token|
| sign|签名|
|_|时间戳|

**3.A股现价和昨收（用于轮询）**

(1) URL  
`/stock/price/real?code=PRODUCTCODE`

(2) 请求方式:`GET`

(3) 请求参数，将URL中对应参数的值替换

|Name|Description|
|------|-------|
| code |查询的股票代码，可有多个，用`,` (逗号)分隔。|

-----------

#####第二部分：URL
**一、HOST**

正式环境：  
测试环境：

**二、功能**

**1.登录、极速开户**

(1) URL  
`/securities/pre-login?`

(2) 参数 <a>(下面的参数直接拼接在URL后面)</a>

|Name			| Description|
|:------		| :-------|
| api_key 	| 华尔街见闻账户的token|
| app_type 	| wallstreet |
| ter_type 	| MI|
| ter_id 		| UUID|
| iip			| IP地址，如果无法获取可使用`127.0.0.0`|
| osv 			| 操作系统及版本，格式：iOSx.x|
| mac 			| Mac 地址，使用和ter_id一样的UUID|
| nt 			| NT|
| phone_num 	| 手机号码，如果没有则开户时需要绑定手机号码|
| is_login 	| 根据见闻账户下是否有apikey来判断见闻账户是否登录|
| app_version	| 用于识别接入交易Framework的版本，版本号由我们控制。可按照“更新一版加一”的原则赋值|
| wscn\_user_id| 华尔街见闻账户的userId|
| is_pro	 	| 判断是否为pro版，`true` 或者 `false`|

(3) 完整的URL如下

```
/securities/pre-login?api_key=&app_type=wallstreet
&ter_type=MI&ter_id=86922330817C4CE39A105C1F2AF94AD6
&osv=iOS10.3&iip=192.168.11.187&mac=86922330817C4CE39A105C1F2AF94AD6
&nt=NT&phone_num=&is_login=false
&app_version=5.0.2&wscn_user_id=&is_pro=true
```

**2.平安登录**

(1) URL

`/securities/pa-login?`

(2) 参数 <a>(下面的参数直接拼接在URL后面)</a>

在`1.登录、极速开户参数的基础上添加一个参数`

|Name			| Description|
|:------		| :-------|
| page 		| `paLogin`,用于pa-login点击左上角的返回关闭页面|

(3) 完整的URL如下

```
/securities/pre-login?api_key=&app_type=wallstreet
&ter_type=MI&ter_id=86922330817C4CE39A105C1F2AF94AD6
&osv=iOS10.3&iip=192.168.11.187&mac=86922330817C4CE39A105C1F2AF94AD6
&nt=NT&phone_num=&is_login=false
&app_version=5.0.2&wscn_user_id=&is_pro=true&page=paLogin
```


**3.券商管理**
 
`/securities/sec-manage`

**4.下单或买入**
 
`/trade/td-operation?td_type=0`

**5.卖出**

`/trade/td-operation?td_type=1`

**6.撤单**

`/trade/td-operation?td_type=2`

**7.交易查询**

`/query/querypage`

**8.银证转账**

`/account/bank-transfer`

**9.更多**

`/query/more`

-----------

<h4 id="3.3.2">2.2 账户系统</h4>

A股交易的账户对于移动端来说仅仅包含下列三个数据

|Key|类型|Description|
|---|---|---|
|stockToken|字符串|通过pre-login登录成功后由H5会返回|
|phoneNumber|字符串|A股用户的手机号码，绑定成功需要存到本地|
|cookieDic|字典|访问reget接口返回需要存下来的内容|

<h4 id="3.2.3">2.3 登录逻辑</h4>

```


```

<h4 id="3.2.4">2.4 加密方法</h4>

加密方法包括：密码加密算法，签名算法。
两种算法的具体内容由两个算法伪代码展示。
![加密算法](/Users/wscn/Documents/wallstreetcn/AStockA股交易/加密算法.png)
<center>加密算法</center>
![签名算法](/Users/wscn/Documents/wallstreetcn/AStockA股交易/签名方法.png)
<center>签名算法</center>


<h4 id="3.2.5">2.5 第三方SDK</h4>

参考“2016年12月29日华尔街见闻4\_4\_8接入的金微蓝-平安SDK”中的“金微蓝开户SDK集成文档”

<h2 id="4">四、基金交易</h2>

<h3 id="4.1">2017年3月31日活期Plus下线</h3>

<h2 id="5">五、更新</h2>

<h3 id="5.1">2017年4月14日发版，A股交易更新</h3>

<h4 id="5.1">5.1 路由</h4>
* 闪电开户：	astock.wallstreetcn.com
* 立即绑定：	astock.wallstreetcn.com/paLogin
* 买买买：		astock.wallstreetcn.com
* 一键打新：	astock.wallstreetcn.com/more

<h4 id="5.2">5.2 见闻->我的页面</h4>
* 获取A股交易配置的API：`apiv1/user/trade/info`，需要添加一个字段 `app_version`。
* app_version <font color='red'>强制赋值为`5.0.2`</font>，原因`华尔街见闻的版本为5.0.2`，但是`华尔街见闻pro的版本号为4.0.0`，h5区分是否为`pro`的时候非常麻烦，所以一刀切，通过`5.0.2`来区分；
* 版本小于5.0.1，A股交易还按照5.0.0的逻辑；否则，按照新的逻辑（后面说明新的逻辑）;
* 新的逻辑：立即绑定跳往pa-login页面，闪电开户跳往pre-login页面。

<h4 id="5.3">5.3 立即绑定</h4>
1. 直接去pa-login页面，网页的`url`=`/securities/pa-login`
2. 立即绑定添加路由：正式`astock.wallstreetcn.com/paLogin`,测试`astock.wallstcn.com/paLogin`
4. 在打开pa-login页面前需要写cookie，添加一个字段`is_pro=true`内容`"pa-login-app-info" : "api_key=&app_type=wallstreet&ter_type=MI&ter_id=86922330817C4CE39A105C1F2AF94AD6&osv=iOS10.3&iip=192.168.11.187&mac=86922330817C4CE39A105C1F2AF94AD6&nt=NT&phone_num=&is_login=false&app_version=5.0.2&wscn_user_id=&is_pro=true"`  其中<font color='red'>app_version=5.0.2，此版本号固定。</font>
4. 打开pa-login需要在url后面拼接参数，参数的值在上面`pa-login-app-info`的value的基础上再添加一个字段`page=paLogin`，完整的URL如下：`https://pingantest.wallstcn.com/securities/pa-login?api_key=&app_type=wallstreet&ter_type=MI&ter_id=86922330817C4CE39A105C1F2AF94AD6&osv=iOS10.3&iip=192.168.11.187&mac=86922330817C4CE39A105C1F2AF94AD6&nt=NT&phone_num=&is_login=false&app_version=5.0.2&wscn_user_id=&page=paLogin&is_pro=true&page=paLogin`。


<p style="text-align:right">[回到顶部](#jump)



