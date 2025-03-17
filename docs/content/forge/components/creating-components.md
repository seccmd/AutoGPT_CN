# 创建组件

## 最小化组件

组件可用于实现各种功能，如向提示提供消息、执行代码或与外部服务交互。

*组件*是一个继承自`AgentComponent`或实现一个或多个*协议*的类。每个*协议*都继承自`AgentComponent`，因此一旦你继承了任何*协议*，你的类就会自动成为一个*组件*。

```py
class MyComponent(AgentComponent):
    pass
```

这已经是一个有效的组件，但它还没有任何功能。要为其添加一些功能，你需要实现一个或多个*协议*。

让我们创建一个简单的组件，向代理的提示中添加“Hello World!”消息。为此，我们需要在我们的组件中实现`MessageProvider`*协议*。`MessageProvider`是一个带有`get_messages`方法的接口：

```py
# 不再需要继承AgentComponent，因为MessageProvider已经继承了
class HelloComponent(MessageProvider):
    def get_messages(self) -> Iterator[ChatMessage]:
        yield ChatMessage.user("Hello World!")
```

现在我们可以将我们的组件添加到现有的代理中，或者创建一个新的Agent类并将其添加到其中：

```py
class MyAgent(Agent):
    self.hello_component = HelloComponent()
```

每次代理需要构建新提示时，`get_messages`都会被调用，并且生成的消息将相应地被添加。

## 向组件传递数据及组件间传递数据

由于组件是常规类，你可以通过`__init__`方法向它们传递数据（包括其他组件）。
例如，我们可以传递一个配置对象，然后在需要时从中检索API密钥：

```py
class DataComponent(MessageProvider):
    def __init__(self, config: Config):
        self.config = config

    def get_messages(self) -> Iterator[ChatMessage]:
        if self.config.openai_credentials.api_key:
            yield ChatMessage.system("API key found!")
        else:
            yield ChatMessage.system("API key not found!")
```

!!! 注意
    组件特定的配置处理尚未实现。

## 配置组件

组件可以使用pydantic模型进行配置。
要使组件可配置，它必须继承自`ConfigurableComponent[BM]`，其中`BM`是继承自pydantic的`BaseModel`的配置类。
你应该将配置实例传递给`ConfigurableComponent`的`__init__`，或者直接设置其`config`属性。
使用配置允许你从文件加载配置，并且可以轻松地为任何代理序列化和反序列化配置。
要了解更多关于配置的信息，包括存储敏感信息和序列化，请参阅[组件配置](./components.md#component-configuration)。

```py
# 示例组件配置
class UserGreeterConfiguration(BaseModel):
    user_name: str

class UserGreeterComponent(MessageProvider, ConfigurableComponent[UserGreeterConfiguration]):
    def __init__(self):
        # 创建配置实例
        # 你也可以将其传递给组件构造函数
        # 例如 `def __init__(self, config: UserGreeterConfiguration):`
        config = UserGreeterConfiguration(user_name="World")
        # 将配置实例传递给父类
        UserGreeterComponent.__init__(self, config)
        # 这行代码与上面的代码效果相同：
        # self.config = UserGreeterConfiguration(user_name="World")

    def get_messages(self) -> Iterator[ChatMessage]:
        # 你可以像使用常规模型一样使用配置
        yield ChatMessage.system(f"Hello, {self.config.user_name}!")
```

## 提供命令

要扩展代理的功能，你需要使用`CommandProvider`协议提供命令。例如，为了允许代理将两个数字相乘，你可以创建一个如下所示的组件：

```py
class MultiplicatorComponent(CommandProvider):
    def get_commands(self) -> Iterator[Command]:
        # 生成命令以便代理可以使用它
        yield self.multiply

    @command(
    parameters={
        "a": JSONSchema(
            type=JSONSchema.Type.INTEGER,
            description="第一个数字",
            required=True,
        ),
        "b": JSONSchema(
            type=JSONSchema.Type.INTEGER,
            description="第二个数字",
            required=True,
        )})
    def multiply(self, a: int, b: int) -> str:
        """
        将两个数字相乘。
        
        参数:
            a: 第一个数字
            b: 第二个数字

        返回:
            相乘的结果
        """
        return str(a * b)
```

要了解更多关于命令的信息，请参阅[🛠️ 命令](./commands.md)。

## 提示结构

在组件提供了所有必要的数据后，代理需要构建最终将发送给llm的提示。
目前，`PromptStrategy`（*不是*协议）负责构建最终的提示。

如果你想改变提示的构建方式，你需要创建一个新的`PromptStrategy`类，然后在你的代理类中调用相关方法。
你可以查看AutoGPT代理使用的默认策略：[OneShotAgentPromptStrategy](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/agents/prompt_strategies/one_shot.py)，以及它在[Agent](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/agents/agent.py)中的使用方式（搜索`self.prompt_strategy`）。

## 示例`UserInteractionComponent`

让我们创建一个内置代理使用的组件的简化版本。
它使代理能够在终端中向用户请求输入。

1. 创建一个继承自`CommandProvider`的组件类。

    ```py
    class MyUserInteractionComponent(CommandProvider):
        """提供与用户交互的命令。"""
        pass
    ```

2. 实现一个命令方法，该方法将向用户请求输入并返回输入内容。

    ```py
    def ask_user(self, question: str) -> str:
        """如果你需要更多关于给定目标的详细信息或信息，
        你可以向用户请求输入。"""
        print(f"\nQ: {question}")
        resp = input("A:")
        return f"用户的回答: '{resp}'"
    ```

3. 命令需要用`@command`装饰。

    ```py
    @command(
        parameters={
            "question": JSONSchema(
                type=JSONSchema.Type.STRING,
                description="向用户提出的问题或提示",
                required=True,
            )
        },
    )
    def ask_user(self, question: str) -> str:
        """如果你需要更多关于给定目标的详细信息或信息，
        你可以向用户请求输入。"""
        print(f"\nQ: {question}")
        resp = input("A:")
        return f"用户的回答: '{resp}'"
    ```

4. 我们需要实现`CommandProvider`的`get_commands`方法来生成命令。

    ```py
    def get_commands(self) -> Iterator[Command]:
        yield self.ask_user
    ```

5. 由于代理并不总是在终端或交互模式下运行，我们需要在无法请求用户输入时通过设置`self._enabled=False`来禁用此组件。

    ```py
    def __init__(self, interactive_mode: bool):
        self.config = config
        self._enabled = interactive_mode
    ```

最终的组件应该如下所示：

```py
# 1.
class MyUserInteractionComponent(CommandProvider):
    """提供与用户交互的命令。"""

    # 我们传递config以检查是否处于非交互模式
    def __init__(self, interactive_mode: bool):
        self.config = config
        # 5.
        self._enabled = interactive_mode

    # 4.
    def get_commands(self) -> Iterator[Command]:
        # 生成命令以便代理可以使用它
        # 如果组件被禁用，则不会生成此命令
        yield self.ask_user

    # 3.
    @command(
        # 我们需要为所有命令参数提供模式
        parameters={
            "question": JSONSchema(
                type=JSONSchema.Type.STRING,
                description="向用户提出的问题或提示",
                required=True,
            )
        },
    )
    # 2.
    # 命令名称将是其方法名称，描述将是其文档字符串
    def ask_user(self, question: str) -> str:
        """如果你需要更多关于给定目标的详细信息或信息，
        你可以向用户请求输入。"""
        print(f"\nQ: {question}")
        resp = input("A:")
        return f"用户的回答: '{resp}'"
```

现在，如果我们想使用我们的用户交互*代替*默认的用户交互，我们需要以某种方式移除默认的用户交互（如果我们的代理继承自`Agent`，则默认的用户交互会被继承）并添加我们自己的。我们可以简单地在`__init__`方法中覆盖`user_interaction`：

```py
class MyAgent(Agent):
    def __init__(
        self,
        settings: AgentSettings,
        llm_provider: MultiProvider,
        file_storage: FileStorage,
        app_config: Config,
    ):
        # 调用父构造函数以引入默认组件
        super().__init__(settings, llm_provider, file_storage, app_config)
        # 通过覆盖默认的用户交互组件来禁用它
        self.user_interaction = MyUserInteractionComponent()
```

或者，我们可以通过将其设置为`None`来禁用默认组件：

```py
class MyAgent(Agent):
    def __init__(
        self,
        settings: AgentSettings,
        llm_provider: MultiProvider,
        file_storage: FileStorage,
        app_config: Config,
    ):
        # 调用父构造函数以引入默认组件
        super().__init__(settings, llm_provider, file_storage, app_config)
        # 禁用默认的用户交互组件
        self.user_interaction = None
        # 添加我们自己的组件
        self.my_user_interaction = MyUserInteractionComponent(app_config)
```

## 了解更多

查看更多示例的最佳位置是查看[classic/original_autogpt/components](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/components/)和[classic/original_autogpt/commands](https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/commands/)目录中的内置组件。

关于如何扩展内置代理并构建自己的代理的指南：[🤖 代理](./agents.md)  
某些组件的顺序很重要，请参阅[🧩 组件](./components.md)以了解更多关于组件及其自定义方式的信息。  
要查看内置协议及其附带示例，请访问[⚙️ 协议](./protocols.md)。