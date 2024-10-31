## 适配器模式

- Adapter Pattern 是一种结构设计模式，它允许具有不兼容接口的对象进行协作。当您想让现有类与其他类一起使用而不修改其源代码时，通常会使用它。当现有类的接口与您需要的接口不匹配时，此模式特别有用

- 场景：将第三方视频播放器集成到您的应用程序中。但是，视频播放器的功能不同，并且具有与应用程序预期的不同的方法接口。在这种情况下，您可以使用 Adapter Pattern 围绕视频播放器创建包装类，使第三方代码与现有应用程序代码兼容
- 示例：

  ```js
  // Adapter class
  class VideoPlayerAdapter {
    constructor() {
      this.externalPlayer = new ThirdPartyVideoPlayer({
        // some configuration
      });
    }

    play() {
      const video = this.externalPlayer.getVideo();
      this.externalPlayer.playVideo(video, {
        // additional parameters
      });
    }
  }

  // Your application code
  class Application {
    constructor() {
      this.videoPlayer = new VideoPlayerAdapter();
    }

    start() {
      // Play video using your application code
      this.videoPlayer.play();
    }
  }
  ```

- 适配器模式和外观模式之间的差异
  - 虽然适配器模式和立面模式可能看起来很相似，但它们的用途不同，并且用于不同的上下文：
  - 目的
    - 适配器模式 ：适配器模式的主要目的是使两个不兼容的接口彼此兼容。它允许使用具有不同接口的现有类，就像它实现了不同的接口一样
    - 外观模式：外观模式的主要目的是为复杂子系统提供简化的接口。它隐藏了子系统的复杂性，并提供了更高级别的接口，使子系统更易于使用
  - 用法
    - 适配器模式 ：当您需要集成与应用程序中的现有类或接口不匹配的新类或库时，会使用它。Adapter 模式是关于使特定接口适应预期的接口
    - 外观模式：当您想要简化与复杂子系统的交互时，会使用它。Facade 提供了一种与系统交互的简单方法，隐藏了其复杂性
  - 设计
    - 适配器模式 ：通常涉及创建一个新类 （Adapter），该类实现客户端所需的接口，并将调用转换为适应的类
    - 外观模式：涉及创建一个 Facade 类，该类为客户端提供简化的方法，通常聚合子系统中的多个功能
  - 虽然这两种模式都提供了一种使用现有代码的方法，但 Adapter 模式侧重于接口兼容性，而 Facade 模式侧重于简化与复杂系统的交互
