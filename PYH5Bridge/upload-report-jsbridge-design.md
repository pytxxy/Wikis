# 天下信用上传报告 jsbridge 设计参考
```js
// AppJSBridge 为例子，B端可以用自己的名称，比如微信就是 WechatJsBridge
// uploadTxxyCreditReport 是对应的事件名或者方法名，具体调用方式可以依据实际情况微调
// params 是对应的上传参数，params.reportJson 是报告内容的 json 对象
// result 是调用后的结果，result.code 是结果字符串，result.message 是错误信息（方便排查错误）
AppJSBridge.uploadTxxyCreditReport(params, (result) => {
  
  // 请注意，如果上传完毕后需要回到报告页面，那么上传完毕指的是服务端已接到报告数据
  // 否则过早的调用上传完毕，上传按钮会过早的关闭，导致本次回话没有上传入口，需退出再进来，影响体验
  if (result.code === '1') {
    console.log('上传成功')
  }
  
  // 需要在用户取消上传或者上传失败调用，不调用则页面会一直存在 loading 框，用户无法触发重新上传
  if (result.code === '0') {
    console.log(result.message || '上传失败')
  }
})

// 附：上传参数定义
params = {

  // reportJson 才是真正的报告内容，请自行解析 JSON 后读取
  reportJson: {
    name: '张三',
    age: 28,
    gender: 1,
    ...more
  }
}
```
