## WebView

- 模块：`@ohos.web.webview`，提供 web 控制能力
- Web 组件提供具有网页显示能力
- 注意：访问在线网页时需添加网络权限：`ohos.permission.INTERNET`

1. Web

   - 文档：https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-basic-components-web-V5
   - 同一个页面的多个 web 组件，必须绑定不同的 WebviewController
   - 用例：`Web(options: { src: ResourceStr, controller: WebviewController | WebController, renderMode? : RenderMode, incognitoMode? : boolean})`
     - src：网页资源地址。如果加载应用包外沙箱路径的本地资源文件，请使用 `file://沙箱文件路径`
     - controller：控制器
     - renderMode 表示当前 Web 组件的渲染方式
       - ASYNC_RENDER 0 Web 组件自渲染模式。
       - SYNC_RENDER 1 Web 组件统一渲染模式。
     - incognitoMode 表示当前创建的 webview 是否是隐私模式

   ```
    import web_webview from '@ohos.web.webview'
    @Entry
    @Component
    struct WebComponent {
      controller: web_webview.WebviewController = new web_webview.WebviewController()
      build() {
        Column() {
          <!-- 加载线上页面 -->
          Web({ src: 'https://www.baidu.com', controller: this.controller })
          <!-- 加载本地页面 -->
          Web({ src: $rawfile("index.html"), controller: this.controller })
          <!-- 加载沙箱路径下 -->
          Web({ src: 'file://' + globalThis.filesDir + '/xxx.html', controller: this.controller })
        }
      }
    }
   ```

   - 常用属性

     - javaScriptProxy 参与注册的对象。只能声明方法，不能声明属性。
        - 注入 JavaScript 对象到 window 对象中，并在 window 对象中调用该对象的方法。
        - 所有参数不支持更新
        - 


   - ## 常用事件

2. `@ohos.web.webview`
