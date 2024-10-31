## 观察者模式

- Observer 模式是一种易于理解且广泛使用的消息传送设计，其中对象（称为 subject）维护其依赖项（称为 observer）的列表，并自动通知它们任何状态更改。这促进了相关对象之间的松散耦合，并允许它们之间进行高效通信

- 用例：观察者模式通常用于 GUI 开发中，当某个事件（被点击的按钮）发生时，应用程序的一部分（按钮）需要通知应用程序的另一部分（单击事件处理程序）
- 场景：考虑一个 Customer 和一个 Store。客户想要 Microsoft Store 中尚未提供的新 iPhone 型号。他们可以每天检查，但大多数旅行都是不必要的。或者，每当有新产品到货时，商店就可以向所有客户发送垃圾邮件，使他们免于毫无意义的旅行，但会惹恼其他人。这就造成了一个两难境地：要么客户浪费时间，要么商店浪费资源

  - Customer 是观察者，而 Store 是主题。客户可能希望接收有关特定类别产品的更新，也可能不希望接收，从而使其可以选择想要观察的内容
  - 在这种情况下，Store （即主题） 知道谁对其更新感兴趣 （Customer 或观察者），并且可以有许多 Customer。不同的客户可以从同一个 Store 注册更新，并且每当添加新产品时，所有客户都会收到通知
  - 每次在 Microsoft Store 中创建、更新或删除新产品时，都会发生此通信
  - 即使 Store 知道其 Customer 在 Observer 模式中是谁，它也不关心或知道每个 Customer 将如何处理收到的更新。每个客户都可以完全控制如何处理他们收到的更新

- 示例：

  ```js
  // Define a class for the Provider
  class Store {
    constructor() {
      this.observers = [];
    }

    // Add an observer to the list
    add(observer) {
      this.observers.push(observer);
    }

    // Notify all observers about new product
    notify(product) {
      this.observers.forEach((observer) => observer.update(product));
    }
  }

  // Define a class for the Observer
  class Customer {
    update(product) {
      console.log(`New product added: ${product}`);
    }
  }

  // Usage
  const store = new Store();
  const customer = new Customer();
  store.add(customer); // Add a customer to the store's observers
  store.notify("iPhone 15"); // Notify all observers about a new product
  ```
