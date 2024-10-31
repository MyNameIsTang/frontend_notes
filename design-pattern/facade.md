## 外观模式

- Facade Pattern 是一种结构设计模式，它为更复杂的底层系统、库或框架提供了简化的接口。它有助于抽象出复杂性，并为客户端提供更易于使用和理解的界面
- Facade 是复杂子系统的简化接口，仅提供必要的功能。当使用只需要一小部分特征的复杂库时，它非常有用。

- 场景：考虑一个音乐播放器应用程序。在后台，可能会发生一组复杂的操作，例如加载媒体文件、解码音频、管理音频缓冲区以及将音频流式传输到设备的输出。但是，从用户的角度来看，他们只与一个简单的界面交互 - 播放、暂停或停止

  ```js
  class MusicPlayer {
    constructor() {
      this.audioContext = new AudioContext();
      this.audioBuffer = null;
      // other complex initializations...
    }

    play() {
      // handle complex operations
    }

    pause() {
      // handle complex operations
    }

    stop() {
      // handle complex operations
    }
  }
  ```

  - 有了这个门面，客户端代码可以简单地创建一个新的 MusicPlayer 实例并调用 `play()` 、 `pause()` 或 `stop()` ，而无需担心底层的复杂性
