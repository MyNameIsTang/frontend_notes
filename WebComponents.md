## Web Components

### Web Components 是一种浏览器原生支持的 Web 组件化技术，它允许开发者创建可重用的「自定义元素」，并且可以在任何支持 Web Components 的浏览器中使用

1. 「Custom Elements」（自定义元素）：允许开发者创建新的 HTML 元素，并且可以定义它的行为和样式

   - 创建自定义元素：

     ```
       class MyButton extends HTMLElement {
         constructor() {
           super();
           const shadowRoot = this.attachShadow({ mode: "open" });
           <!-- 封装UI样式 -->
           shadowRoot.innerHTML = `
             <style>
               button {
                 background-color: #4CAF50;
                 border: none;
                 color: white;
                 text-align: center;
                 text-decoration: none;
                 display: inline-block;
                 font-size: 16px;
                 margin: 4px 2px;
                 cursor: pointer;
                 padding: 10px 24px;
                 border-radius: 4px;
               }
             </style>
             <button>Click Me!</button>
           `;
           <!-- 封装UI交互 -->
           shadowRoot.querySelector("button").addEventListener("click", () => {
             alert("按钮被点击了！");
           });
         }

          connectedCallback() {
            console.log("connectedCallback");
          }
          disconnectedCallback() {
            console.log("disconnectedCallback");
          }

          attributeChangedCallback(name, oldValue, newValue) {
            console.log(name, oldValue, newValue);
          }
       }
       customElements.define("my-button", MyButton);
     ```

   - 生命周期回调方法
     - 「`constructor()`」：构造函数，在创建元素实例时调用。适用于执行初始化操作，例如设置初始属性、添加事件监听器或创建 Shadow DOM
     - 「`connectedCallback()`」：当自定义元素被插入到上下文时调用。适用于元素被插入到 DOM 时执行的操作，例如获取数据、渲染内容或启动定时器
     - 「`disconnectedCallback()`」：当自定义元素从文档中移除时调用。适用于元素从 DOM 中移除时执行的操作，例如移除事件监听器或停止定时器
     - 「`attributeChangedCallback(attributeName, oldValue, newValue)`」：当自定义元素的属性被添加、移除或更改时调用。要使用这个回调，需要在类中定义一个静态的 observedAttributes 属性，列出你想要监听的属性

- 「Shadow DOM」（影子 DOM）：允许开发者封装组件的内部结构和样式，避免全局命名空间的污染

  - Shadow DOM 允许开发者创建一个封闭的 DOM 子树，这个子树与主文档的 DOM 分离，这意味着 Shadow DOM 内部的样式和结构不会受到外部的影响，也不会影响到外部

    - 使用 innerHTML：通过设置 Shadow DOM 的 innerHTML 属性，可以直接添加一个或多个元素。这种方式适用于从字符串模板快速填充 Shadow DOM

      ```
        class MyElement extends HTMLElement {
          constructor (){
            super()
            const shadowRoot = this.attachShadow({mode: "open"})
            shadowRoot.innerHTML = `<div>123</div>`
          }
        }
        customElements.define('my-element', MyElement)
      ```

    - 使用 createElement 和 appendChild：可以使用 `document.createElement` 创建一个新元素，然后使用 appendChild 将其添加到 Shadow DOM 中

      ```
        const wrapper = document.createElement("p");
        wrapper.textContent = "使用 createElement 和 appendChild";
        const style = document.createElement("style");
        style.textContent = `
          p { color: gray; }
        `;

        class MyText extends HTMLElement {
          constructor() {
            super();
            const shadowRoot = this.attachShadow({ mode: "open" });
            shadowRoot.appendChild(wrapper);
            shadowRoot.appendChild(style);
          }
        }
        customElements.define("my-text", MyText);
      ```

    - template：使用模板元素（<template>）添加，如下

  - Shadow Mode
    - open：允许外部访问 Shadow DOM 的 API
      - 通过 `Element.shadowRoot` 属性访问 Shadow DOm 的根节点
      - 意味着可以从外部查询、修改 Shadow DOM 内部的元素和样式
      - `const element = document.quertSelector('my-button')`
    - closed：不允许外部访问 Shadow DOM 的 API
      - 外部脚本无法通过 `Element.shadowRoot` 属性访问 Shadow DOM 的根节点。
      - 意味着 Shadow DOM 内部的元素和样式对外部是完全隐藏的，无法从外部直接访问或修改

- 「Slots」（插槽）：允许开发者创建一个可插入内容的占位符，以便在不同的组件中使用

  - Slots 是一种特殊类型的元素，允许将内容从组件的一个部分传递到另一个部分，增加了组件的灵活性。使得 Web Component 自定义元素，更加的灵活
    ```
      class MyButton extends HTMLElement {
        constructor() {
          super();
          const shadowRoot = this.attachShadow({ mode: 'open' });
          shadowRoot.innerHTML = `
            <style>
              /* ...样式代码保持不变... */
            </style>
            <button>
                <slot>Click Me!</slot>
            </button>
          `;
        }
      }
      customElements.define('my-button', MyButton);
    ```
  - 命名插槽
    ```
      class MyButtonName extends HTMLElement {
        constructor() {
          super();
          const shadowRoot = this.attachShadow({ mode: 'open' });
          shadowRoot.innerHTML = `
            <style>
              /* ...样式代码保持不变... */
            </style>
            <button>
              <slot name="element-name"></slot>
              <slot name="element-age"></slot>
              <slot name="element-email"></slot>
            </button>
          `;
        }
      }
      customElements.define('my-button-name', MyButtonName);
    ```

- 「Templates」（模板）：允许开发者定义一个可以在多个组件中重用的 HTML 结构

  - 允许定义一个可以在多个组件中重用的 HTML 结构。将模板放在 HTML 文件中的任何位置，并通过 JavaScript 动态的实例化

    ```
      <!-- template -->
      <template id="my-button-template">
        <style>
          /* ...样式代码保持不变... */
        </style>
        <button>
            <slot>Click Me!</slot>
        </button>
      </template>

      <script>
        class MyButton extends HTMLElement {
          constructor() {
            super();
            const shadowRoot = this.attachShadow({ mode: 'open' });
            const template = document.getElementById('my-button-template');
            // 使用`cloneNode()` 方法添加了拷贝到 Shadow root 根节点上。
            shadowRoot.appendChild(template.content.cloneNode(true));
          }
        }
        customElements.define('my-button', MyButton);
      </script>
    ```

- Polyfills
  - 对于旧版浏览器不支持的兼容性情况，可以考虑使用 polyfill 来实现兼容性。Polyfills 是一种代码注入技术，使得浏览器可以支持新的标准 API。
  - 使用 webcomponents.js
