# 插件系统的接入流程

**关于混淆：**
查看目录下的proguard-rules.pro文件，这个是所有插件公用的混淆文件，在出包之前必须添加进项目的混淆配置文件中。
若接入过程中出现方法数超限的错误，请开启dex文件分割
>     multiDexEnabled = true
>     此时一定要提供一个继承自MultiDexApplication的java类并配置在manifest文件中的application属性中，否则在低版本系统会崩溃


在项目的build.gradle文件中添加maven库的地址代码如下：
>     allprojects {
>        repositories {
>            jcenter()
>            maven{
>                url "https://github.com/ZhaoTianze/AdLib/raw/master"
>           }
>        }
>     }


#**基础库Base：**

        

	    

 - 依赖项：

>     compile 'com.talefun.plugins:Base:版本号'

#**组件系统：**

 1.**广告系统：**

 

 - 依赖项

>     compile 'com.talefun.plugins:aD:版本号'

>     若有需求更换applovin的key，请在AndroidManifest中添加如下代码
>     <meta-data android:name="applovin.sdk.key" android:value="请更换成自己的key值" tools:replace="android:value" />

 - 初始化

对于想要监听banner是否加载成功的游戏请使用第二种初始化方法，每当有一个渠道加载成功都会调用一次。参数smallBanner控制的是banner是否横向铺满屏幕。
>     AdControler.init(Activity activity, RelativeLayout layout, boolean smallBanner)
>     AdControler.init(Activity activity, RelativeLayout layout, boolean smallBanner, BannerAdListener listener)

 - 切换中国服

>     AdControler.useCnServer()

 - 启动插件

>     AdControler.start()

 - 生命周期函数

>     AdControler.onStart()
>     AdControler.onStop()
>     AdControler.onResume()
>     AdControler.onPause()
>     AdControler.onDestroy()

 - 使用测试服广告数据（非必须）

>     AdControler.useCNServer()

 - 插屏广告的展示

>     AdControler.showInterstitialAD()

 - banner的展示

>     AdControler.showBottomADBannar(String pos)
>      pos的可选参数为：	
>           ADPOS_BOTTOM（底部居中）
>           ADPOS_BOTTOM_LEFT（底部左对齐）
>           ADPOS_BOTTOM_RIGHT（底部右对齐）
>           ADPOS_TOP（上部居中）
>           ADPOS_TOP_LEFT（上部左对齐）
>           ADPOS_TOP_RIGHT（上部右对齐）

 - banner的隐藏

>     AdControler.hiddenBottomADBannar()

 - nativebanner的展示

>     AdControler.showNativeADBannar(String pos)
>       pos的可选参数为：	
>           ADPOS_BOTTOM（底部居中）
>			ADPOS_BOTTOM_LEFT（底部左对齐）
>			ADPOS_BOTTOM_RIGHT（底部右对齐）
>			ADPOS_TOP（上部居中）
>			ADPOS_TOP_LEFT（上部左对齐）
>			ADPOS_TOP_RIGHT（上部右对齐）

 - nativebanner的隐藏

>     AdControler.hiddenNativeADBannar()

 - 奖励视频广告

>     AdControler.setRewardedAdListener(RewardedVideoListener listener)
>     	   设置奖励视频回调:
>      		rewaredVideoReady加载成功(可以不理会这个回调，可能会废弃)
>      		rewaredVideoCompleted播放成功
>     AdControler.showRewardVideo()

 - 隐藏所有广告(仅限banner和native)

>     AdControler.hiddenAllAds()

 - 广告加载是否成功的判断

>     AdControler.isInterstitialReady()
>     AdControler.isBannerReady()
>     AdControler.isNativeBannerReady()
>     AdControler.isRewardVideoReady()
    
 - 扩展功能：能为游戏提供额外的参数

>     AdControler.getConfigConstant()
>     可直接获取，通过setConfigConstantListener()方法可设置监听，当拉取到服务端数据后会通知游戏



 2.**支付系统**

- 依赖库


>     compile 'com.talefun.plugins:googlepay:版本号'

 - 初始化

>     GoogleWrapper.initPaySDK(Context context)
>     GoogleWrapper.onActivityResult(int requestCode, int resultCode, Intent data)

 - 设置支付回调

>     GoogleWrapper.setPayLisenter(onPayListener lisenter)

 - 调用支付

>     GoogleWrapper.purchaseProductWithIndentifier(final String pInfo)
>     pInfo是组装的订单信息，格式为json字符串（内容如下）
>    		Product_Id：商品的后台id
>			developerPayload：开发者透传参数，在支付成功数据中会原封不动的返回
            
- 支付结果通知数据格式

>     '{
>		   "orderId":"12999763169054705758.1371079406387615",
>		   "packageName":"com.example.app",
>		   "productId":"exampleSku",
>		   "purchaseTime":1345678900000,
>		   "purchaseState":0,
>		   "developerPayload":"bGoa+V7g/yqDXvKRqq+JTFn4uQZbPiQJo4pf9RzJ",
>		   "purchaseToken":"rojeslcdyyiapnqcynkjyyjh"
>	}'


 3.**锁屏广告系统**

- 依赖库

>     compile 'com.talefun.plugins:LockScreen:版本号'
   
 - 切换中国服(最早调用)

>     AdControler.useCnServer()
   
- 初始化

>     LockScreen.startService(Context context)
>     LockScreen.onResume(Context context)
>     LockScreen.onPause(Context context)
    
- 获取锁屏配置

>     LockScreen.getLockScreenCfg(Context context)

- 设置锁屏开关

>     LockScreen.setShowLockScreen(Context context, boolen lock)

- 获取锁屏开关

>     LockScreen.canShowLockScreen(Context context)
		
- 获取第一次打开的时间

>     LockScreen.getFirstOpenTime(Context context)


 4.**交叉推广**

- 依赖库

>     compile 'com.talefun.plugins:CrossPromotion:版本号'

- 初始化

>     CrossPromotion.init(Activity activity)或
>     CrossPromotion.init(Activity activity, CrossPromotionListener listener)

- 使用中国服务器节点

>     CrossPromotion.useCNServer()

- 启动(开始请求数据)

>     CrossPromotion.start(Activity activity)

- 展示推广

>     CrossPromotion.show(Activity activity, String lan, String exit)
>     参数lan(ui语言)，exit(是否显示退出按钮)

 5.**启动推广**

- 依赖库

>     compile 'com.talefun.plugins:nativeinterstitial:版本号'

- 初始化(********必须放在application的onCreate中*********)

>     NativeInterstitial.init(Context context)

- 启动

>     NativeInterstitial.start(Activity activity)

- 展示推广：（不允许在activity的onCreate中调用show）

>     NativeInterstitial.show(Activity activity, boolean isLaunching)
>     参数isLaunching（是否是启动时调用）

 6.**统计（使用的是flurry的统计系统）**

- 依赖库

>     compile 'com.talefun.plugins:StatisticsInterface:版本号'
>       compile 'com.flurry.android:analytics:7.0.0@aar'

- 初始化

>     StatisticsInterface.init(Activity activity, String key)
>		参数（key在flurry申请的唯一key值）
>	  StatisticsInterface.onStart(Activity activity)
>	  StatisticsInterface.onStop(Activity activity)

- 发送统计事件

>     StatisticsInterface.flurryOnEvent(String eventName,String jsonStr)
>     StatisticsInterface.flurryOnEvent(String eventName,String jsonStr)


