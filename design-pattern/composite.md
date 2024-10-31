## 复合模式

- 复合模式是一种结构设计模式，它将对象组织成树结构，以表示整个部分的层次结构。它可以帮助您以相同的方式处理单个对象和对象组。此模式对于使用树状结构（如用户界面或组织层次结构）非常方便
- 在 JavaScript 中，可以通过创建一个基组件类来实现复合模式，该类定义简单对象和复合对象的公共接口。“叶”对象表示合成中的结束对象，而复合对象可以包含其他叶对象或复合对象

- 场景：开发人员面临的一个常见挑战是管理具有嵌套元素的复杂 UI，例如带有子菜单的菜单或带有目录和文件的文件系统。如果没有结构化的方法，代码可能会变得难以管理和扩展，从而导致潜在的错误和效率低下
- 示例：

  ```js
  // Component interface
  class MenuComponent {
    add(component) {}
    remove(component) {}
    getChild(index) {}
    display() {}
  }

  // Leaf object
  class MenuItem extends MenuComponent {
    constructor(name) {
      super();
      this.name = name;
    }

    display() {
      console.log(this.name);
    }
  }

  // Composite object
  class Menu extends MenuComponent {
    constructor(name) {
      super();
      this.name = name;
      this.children = [];
    }

    add(component) {
      this.children.push(component);
    }

    remove(component) {
      this.children = this.children.filter((child) => child !== component);
    }

    getChild(index) {
      return this.children[index];
    }

    display() {
      console.log(this.name);
      this.children.forEach((child) => child.display());
    }
  }

  // Client code
  const mainMenu = new Menu("Main Menu");
  const menuItem1 = new MenuItem("Item 1");
  const menuItem2 = new MenuItem("Item 2");
  const subMenu = new Menu("Sub Menu");
  const subMenuItem1 = new MenuItem("Sub Item 1");

  mainMenu.add(menuItem1);
  mainMenu.add(menuItem2);
  mainMenu.add(subMenu);
  subMenu.add(subMenuItem1);

  // Display the menu structure
  mainMenu.display();
  ```

- 通过应用复合模式，您可以有效地管理复杂的 UI 组件、处理组织层次结构以及构建数据表示形式（如文件系统）。此模式使您的代码库更易于维护和扩展，尤其是在需要统一处理单个对象和对象组合的情况下
