# App拉起小程序对接指南

> 暂时只支持微信小程序。

- 功能介绍
    考虑到部分场景下 `APP` 需要通过小程序来承载服务，为此 OpenSDK 提供了移动应用（APP）拉起小程序功能。移动应用（APP）接入此功能后，用户可以在 `APP` 中跳转至微信某一小程序的指定页面，完成服务后再跳回至原 `APP` 。  

    移动应用拉起小程序功能已向全体开发者开放，开发者在微信开放平台账号下申请移动应用并通过审核后，即可获得移动应用拉起小程序功能权限。

- 跳转规则
    对于已通过认证的开放平台账号，其移动应用可以跳转至任何合法的小程序，且不限制跳转的小程序数量。  
    对于未通过认证的开放平台账号，其移动应用仅可以跳转至同一开放平台账号下小程序。

- 若移动应用未上架，则最多只能跳转小程序100次/天，用于满足调试需求。

[微信官方链接](https://developers.weixin.qq.com/doc/oplatform/Mobile_App/Launching_a_Mini_Program/Launching_a_Mini_Program.html)

## 1.1 移动应用拉起小程序

### 1.1.1 iOS

[官方开发示例](https://developers.weixin.qq.com/doc/oplatform/Mobile_App/Launching_a_Mini_Program/iOS_Development_example.html)  

```objc
#import <WechatOpenSDK/WXApi.h>

//调起小程序
- (void)openMiniProgram {
 WXLaunchMiniProgramReq *launch = [WXLaunchMiniProgramReq object];
 // 小程序原始ID，可在微信开放平台的小程序详情中查看
 launch.userName = @"xxxxxxxxxxx"; 
 // 小程序页面路径，不填默认拉起小程序首页
 launch.path = @"pages/setting/about/about";
 // 拉起小程序的类型，如开发版、预览版、正式版
 launch.miniProgramType = WXMiniProgramTypePreview;//WXMiniProgramTypeTest;
 [WXApi sendReq:launch];
}

//小程序返回移动应用回调
-(void)onResp:(BaseResp *)resp 
{
     if ([resp isKindOfClass:[WXLaunchMiniProgramResp class]])
     {
          NSString *string = resp.extMsg;
          // 对应JsApi navigateBackApplication中的extraData字段数据
     }
}
```

### 1.1.2 Android

[官方开发示例](https://developers.weixin.qq.com/doc/oplatform/Mobile_App/Launching_a_Mini_Program/Android_Development_example.html)  

```java
//调起小程序
String appId = "wxd930ea5d5a258f4f"; // 填应用AppId
IWXAPI api = WXAPIFactory.createWXAPI(context, appId);

WXLaunchMiniProgram.Req req = new WXLaunchMiniProgram.Req();
req.userName = "gh_d43f693ca31f"; // 填小程序原始id
req.path = path;                  //拉起小程序页面的可带参路径，不填默认拉起小程序首页
req.miniprogramType = WXLaunchMiniProgram.Req.MINIPTOGRAM_TYPE_RELEASE;// 可选打开 开发版，体验版和正式版
api.sendReq(req);

//小程序返回移动应用回调
//WXEntryActivity.java
public void onResp(BaseResp resp) {
    if (resp.getType() == ConstantsAPI.COMMAND_LAUNCH_WX_MINIPROGRAM) {
        WXLaunchMiniProgram.Resp launchMiniProResp = (WXLaunchMiniProgram.Resp) resp;
        String extraData =launchMiniProResp.extMsg; // 对应JsApi navigateBackApplication中的extraData字段数据
    }
}
```

### 1.1.3 鸿蒙

[官方开发示例](https://developers.weixin.qq.com/doc/oplatform/Mobile_App/Launching_a_Mini_Program/OHOS_Development_example.html)

```typescript
import * as wxopensdk from '@tencent/wechat_open_sdk'; // 导入微信 SDK

let context = getContext(this) as common.UIAbilityContext; // 假定我们在组件环境内调用

let launchMiniProgramReq = new wxopensdk.LaunchMiniProgramReq;
launchMiniProgramReq.userName = userName;  //拉起的小程序的原始id
launchMiniProgramReq.path = path;    ////拉起小程序页面的可带参路径，不填默认拉起小程序首页，对于小游戏，可以只传入 query 部分，来实现传参效果，如：传入 "?foo=bar"。
launchMiniProgramReq.miniProgramType = miniProgramType; //拉起小程序的类型 0-正式版 1-开发版 2-体验版
let success = WXApi.sendReq(context, launchMiniProgramReq);

// 回调说明
onResp(resp: wxopensdk.BaseResp): void {
    if (resp instanceof wxopensdk.LaunchMiniProgramResp) {
        let extMsg = resp.extMsg // 对应小程序组件 <button open-type="launchApp"> 中的 app-parameter 属性
    }
}
```

## 1.2 小程序返回移动应用

[官方文档](https://developers.weixin.qq.com/miniprogram/dev/framework/open-ability/launchApp.html)  
当小程序从 APP 打开的场景打开时（场景值 1069），小程序会获得返回 APP 的能力，此时用户点击按钮可以打开拉起该小程序的 APP。即小程序不能打开任意 APP，只能 `跳回` APP。
在小程序提供服务完成的界面添加"返回App"按钮，用户点击按钮即可返回原来的App。

```html
<button open-type="launchApp" app-parameter="wechat" binderror="launchAppError">返回App</button>
```

```javascript
  launchAppError(e) { 
    console.log(e.detail.errMsg) 
  } 
```
