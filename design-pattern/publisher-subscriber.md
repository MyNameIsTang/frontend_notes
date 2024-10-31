## 发布者/订阅者模式

- PubSub 模式是一种消息传递模式，可促进松散耦合和可伸缩性。此模式围绕将消息从发布者分派给未指定数量的订阅者或侦听器，从而促进对象之间的多对多依赖关系

- 用例：在前端开发的上下文中，发布者订阅者模式可以发挥作用。例如，在复杂的 Web 应用程序中，不同的组件可能需要实时交换数据，例如在聊天应用程序中，跟踪出租车的 GPS，甚至实时更新股票市场数据。在这些情况下，不同的组件需要保持更新，而彼此之间没有直接关系
- 场景：考虑一个聊天组应用程序。Pub/Sub 系统可以有效地处理消息的发送和接收：

  - 订阅事件
    - 打开聊天窗口时，它会订阅一个事件，如 newMessage ，使用` pubsub.subscribe("newMessage", callback)` 完成的。回调函数是在将新消息发布到 newMessage 事件时将执行的函数。在这种情况下，回调会记录新消息并更新聊天 UI
  - 发布到事件
    - 当用户发送消息时，使用 `pubsub.publish("newMessage", messageData)` 将其发布到 newMessage 事件。所有订阅 newMessage 主题的聊天窗口都将以新消息作为参数执行其回调函数
  - 通过这种方式，Pub/Sub 系统允许将聊天窗口（订阅者）和消息发送者（发布者）分离。聊天窗口不需要知道谁发送消息（就像我们在之前的帖子中看到的关于观察者模式一样），它们只需要知道收到消息时该怎么做
  - 同样，消息发件人不需要知道谁将接收消息;他们只需要向正确的主题发送消息

- 示例

  ```js
  class PubSub {
    static events = {}; // It has the an empty list of events

    // The subscribe method takes an event name and a callback function
    subscribe(eventName, callback) {
      if (!this.events[eventName]) {
        // If the event doesn't exist yet, initialize it as an empty array
        this.events[eventName] = [];
      }

      // Push the callback function into the array of callbacks for the given event
      this.events[eventName].push(callback);
    }

    // The publish method takes an event name and data
    publish(event, data) {
      // If the event doesn't exist, or there's no subscribers for this event, return
      if (!this.events[event]) {
        return;
      }

      // For each subscriber of this event, call the callback function with the provided data
      this.events[event].forEach((callback) => {
        callback(data);
      });
    }
  }
  ```
