# 天下信用上传报告 jsbridge 设计参考
```js
// AppJSBridge 为例子，B端可以用自己的名称，比如微信就是 WechatJsBridge
// uploadTxxyCreditReport 是对应的事件名或者方法名，具体调用方式可以依据实际情况微调
// params 是对应的上传参数，params.reportJson 是报告内容的 json 对象
// result 是调用后的结果，result.code 是结果字符串，result.message 是错误信息（方便排查错误）
AppJSBridge.uploadTxxyCreditReport(params, (result) => {
  if (result.code === '1') {
    console.log('上传成功')
  }
  if (result.code === '0') {
    console.log(result.message || '上传失败')
  }
})

// 附：上传参数定义
params = {
  reportJson: {
    name: '张三',
    age: 28,
    gender: 1,
    ...more
  }
}
```
