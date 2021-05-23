# uni-tencent-captcha
uni-app 腾讯云验证码插件，适配H5、微信小程序。[腾讯云验证码接入指南](https://cloud.tencent.com/document/product/1110/36334)

## 使用插件
**uni-app 插件市场导入**

**源码安装**
> 下载`components`目录下`TencentCaptcha.vue`源文件到你的项目

1. H5平台需自定义模版html，在 Head 标签的最后加入以下代码，引入验证 JS 文件  
```
<script src="https://ssl.captcha.qq.com/TCaptcha.js"></script>
```

[如何自定义uni-app模板](https://uniapp.dcloud.io/collocation/manifest?id=h5-template)

2. 微信小程序接入前置条件  
添加插件:  [添加插件参考](https://cloud.tencent.com/document/product/1110/49319)  

集成插件: 在`pages.json > globalStyle`中添加如下代码:  
```
// #ifdef MP-WEIXIN
"usingComponents": {
    "t-captcha": "plugin://myPlugin/t-captcha"
}
// #endif
```

**使用**  
``` vue
import TencentCaptha from 'uni-tencent-captcha'

<verify-code ref="slider" appId="your app-id" text="滑块验证" locale="zh-cn" eventName="getCode" @close="closeVerify" @verified="codeVerified"></verify-code>


// 1. 作为文本点击时会主动触发
// 2. 手动触发，需给组件添加ref
this.$refs.slider.show()

// 用户主动关闭验证码
closeVerify() {
    // todo
}

// 用户验证成功
codeVerified(res) {
    // 当前处理的事件名称
    console.log(res.eventName)
    // ticket randstr
    // 注 微信小程序下无randstr
    console.log('ticket: ', res.ticket, 'randstr: ', res.randstr)
}
```

#### 属性
|  属性   | 描述  |
|  ----  | ----  |
| appId  | 注册的腾讯滑块验证码app-id |
| locale  | 多语言，支持`zh-cn``zh-hk``en` |
| text | 显示的文本内容 |
| eventName | 当前处理的事件名称，若只有一个滑块验证事件，可不填 |

#### 回调函数
|  方法名   | 描述  |
|  ----  | ----  |
| verified  | 用户前端验证成功，返回ticket和randstr |
| close  | 用户主动关闭验证码时触发 |
