#MTradeFramework
=

##前言

MTradeFramework提供A股交易和基金交易。
##一、简介


*  作者： 张吉鑫
*  版本： V1.0
*  适用: WSCN
*  依赖: 
 
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

##二、功能
* 基金交易 
* A股交易

##三、A股交易
### 1.接入使用
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
#### 1)通过路由调用
* 进入A股交易主页面

	**进入A股交易主界面不需要传参数。**

	```
	NSString *transaction = [NSString stringWithFormat:@"https://astock.wallstreetcn.com/transaction"];
   [[MRouter sharedRouter] handleURL:[NSURL URLWithString:transaction] userInfo:parameterDic];
	```

* 进入买卖页面

	**进入买卖入口需要传递两个参数，symbol和transactionType**

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
	
###2)调用类方法
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
	
###2.A股交易代码相关
账户系统，登录逻辑，API，接入的第三方SDK（金微蓝和平安两家的SDK），加密方法等
####1)


