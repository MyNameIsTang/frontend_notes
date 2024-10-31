## 建造者模式

- Builder 模式是一种创建性设计模式，允许开发人员逐步构建复杂的对象。它将对象的构造与其表示分开，从而使相同的构造过程能够创建不同的表示
- 在 JavaScript 中，当处理需要大量可选参数的对象或构造过程涉及多个步骤时，此模式特别有用。通过使用 Builder Pattern，开发人员可以通过链接方法调用来设置属性，并最终以受控方式构建所需的对象，从而创建更具可读性和可维护性的代码

- 场景：为 Web 应用程序创建一个复杂的仪表板。挑战在于允许用户使用各种小部件（如图表、表格和通知）自定义他们的仪表板。每个小组件都需要不同的设置，例如数据源、显示选项和刷新间隔。通过单个构造函数或多个 setter 管理所有这些选项可能会变得很麻烦并且容易出错

  - Builder 模式通过使用 DashboardBuilder 类提供了一个优雅的解决方案，开发人员可以链接方法调用来设置每个小部件的属性，确保在构建仪表板之前应用所有必要的配置。这种方法不仅使代码更具可读性和可维护性，而且还允许灵活地创建具有不同复杂性的不同类型的仪表板。

- 示例：

  ```js
  class DashboardBuilder {
    constructor() {
      this.widgets = [];
    }

    addChart(type, data) {
      this.widgets.push({ type: "chart", chartType: type, data });
      return this;
    }

    addTable(data) {
      this.widgets.push({ type: "table", data });
      return this;
    }

    addNotification(message) {
      this.widgets.push({ type: "notification", message });
      return this;
    }

    build() {
      return new Dashboard(this);
    }
  }

  class Dashboard {
    constructor(builder) {
      this.widgets = builder.widgets;
    }

    render() {
      this.widgets.forEach((widget) => {
        switch (widget.type) {
          case "chart":
            console.log(
              `Rendering a ${widget.chartType} chart with data:`,
              widget.data
            );
            break;
          case "table":
            console.log("Rendering a table with data:", widget.data);
            break;
          case "notification":
            console.log("Showing notification:", widget.message);
            break;
          default:
            console.log("Unknown widget type");
        }
      });
    }
  }

  // Usage
  const dashboard = new DashboardBuilder()
    .addChart("line", [1, 2, 3])
    .addTable({
      header: ["Name", "Age"],
      rows: [
        ["Alice", 30],
        ["Bob", 25],
      ],
    })
    .addNotification("Welcome to your dashboard!")
    .build();

  dashboard.render();
  ```
