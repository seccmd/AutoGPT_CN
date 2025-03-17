# 🤖 代理

代理由[🧩 组件](./components.md)组成，负责执行管道和一些额外的逻辑。所有代理的基类是`BaseAgent`，它具备收集组件和执行协议的必要逻辑。

## 重要方法

`BaseAgent`提供了任何代理正常工作所需的两个抽象方法：
1. `propose_action`：此方法负责根据代理的当前状态提出一个行动，返回`ThoughtProcessOutput`。
2. `execute`：此方法负责执行提出的行动，返回`ActionResult`。

## AutoGPT 代理

`Agent`是AutoGPT提供的主要代理。它是`BaseAgent`的子类，拥有所有[内置组件](./built-in-components.md)。`Agent`实现了`BaseAgent`中的关键抽象方法：`propose_action`和`execute`。

## 构建你自己的代理

构建自定义代理的最简单方法是扩展`Agent`类并添加额外的组件。通过这种方式，你可以重用现有组件以及执行[⚙️ 协议](./protocols.md)的默认逻辑。

```py
class MyComponent(AgentComponent):
    pass

class MyAgent(Agent):
    def __init__(
        self,
        settings: AgentSettings,
        llm_provider: MultiProvider
        file_storage: FileStorage,
        app_config: AppConfig,
    ):
        # 调用父类构造函数以引入默认组件
        super().__init__(settings, llm_provider, file_storage, app_config)
        # 添加你的自定义组件
        self.my_component = MyComponent()
```

为了更深入的定制，你可以重写`propose_action`和`execute`方法，甚至直接继承`BaseAgent`。这样，你可以完全控制代理的组件和行为。更多细节，请参考[Agent的实现](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/agents/agent.py)。