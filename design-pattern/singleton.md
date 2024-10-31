## 单例模式

- Singleton Pattern 是一种设计模式，它将类的创建限制为仅一个实例。这在需要单点控制或协调的情况下非常有用。换句话说，它确保一个类只有一个实例，并提供对它的全局访问点
- 此模式通常用于配置数据、缓存、连接池或日志记录，其中运行一个可供应用程序中的其他进程使用的实例会更有效。当您需要维护状态、初始化字段或管理调用和回调队列时，它也很有用

- 场景：如果应用程序有一个从不同位置访问的项目下拉列表，则 Singleton 可以管理此共享资源。这可确保在一个位置修改列表时，更改将反映在整个应用程序中
- ES2015

  ```js
  let dropdownListItems;
  class DropdownList {
    constructor(items = []) {
      if (dropdownListItems) {
        return dropdownListItems;
      }

      dropdownListItems = items;
    }

    addItem(item) {
      dropdownListItems.push(item);
    }

    removeItem(item) {
      dropdownListItems = dropdownListItems.filter((i) => i !== item);
    }
  }

  const dropdownList = new DropdownList();
  export default dropdownList;
  ```

- ES2016

  ```js
  class DropdownList {
    static dropdownListItems = [];

    constructor(items = []) {
      if (DropdownList.dropdownListItems.length) {
        return DropdownList.dropdownListItems;
      }
      DropdownList.dropdownListItems = items;
    }

    addItem(item) {
      DropdownList.dropdownListItems.push(item);
    }

    removeItem(item) {
      DropdownList.dropdownListItems = DropdownList.dropdownListItems.filter(
        (i) => i !== item
      );
    }
  }
  const dropdownList = new DropdownList();
  export default dropdownList;
  ```

  - 在此 ES2016 版本中，实例是类本身的静态属性，而不是单独的变量。这清楚地表明实例与类相关联，而不仅仅是顶部的某个随机变量
