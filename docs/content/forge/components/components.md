# 🧩 组件

组件是[🤖 代理](./agents.md)的构建模块。它们是继承自`AgentComponent`或实现一个或多个[⚙️ 协议](./protocols.md)的类，这些协议为代理提供了额外的能力或处理功能。

组件可用于实现各种功能，如向提示提供消息、执行代码或与外部服务交互。它们可以被启用或禁用、排序，并且可以相互依赖。

在代理的`__init__`中通过`self`分配的组件会在代理实例化时自动检测到。例如在`__init__`中：`self.my_component = MyComponent()`。你可以使用任何有效的Python变量名，组件被检测到的关键是它的类型（`AgentComponent`或任何继承自它的协议）。

访问[内置组件](./built-in-components.md)查看开箱即用的组件。

```py
from forge.agent import BaseAgent
from forge.agent.components import AgentComponent

class HelloComponent(AgentComponent):
    pass

class SomeComponent(AgentComponent):
    def __init__(self, hello_component: HelloComponent):
        self.hello_component = hello_component

class MyAgent(BaseAgent):
    def __init__(self):
        # 这些组件将被自动发现并使用
        self.hello_component = HelloComponent()
        # 我们将HelloComponent传递给SomeComponent
        self.some_component = SomeComponent(self.hello_component)
```

## 组件配置

每个组件都可以使用常规的pydantic `BaseModel`定义自己的配置。为了确保配置从文件中正确加载，组件必须继承自`ConfigurableComponent[BM]`，其中`BM`是它使用的配置模型。`ConfigurableComponent`提供了一个`config`属性，该属性保存配置实例。可以直接设置`config`属性或将配置实例传递给组件的构造函数。额外的配置（即不属于代理的组件）可以传递，但会被静默忽略。即使稍后添加了组件，额外的配置也不会应用。要查看内置组件的配置，请访问[内置组件](./built-in-components.md)。

```py
from pydantic import BaseModel
from forge.agent.components import ConfigurableComponent

class MyConfig(BaseModel):
    some_value: str

class MyComponent(AgentComponent, ConfigurableComponent[MyConfig]):
    def __init__(self, config: MyConfig):
        super().__init__(config)
        # 这与上面的效果相同：
        # self.config = config

    def get_some_value(self) -> str:
        # 像访问常规模型一样访问配置
        return self.config.some_value
```

### 敏感信息

虽然可以直接在代码中将敏感数据传递给配置，但建议对API密钥等敏感数据使用`UserConfigurable(from_env="ENV_VAR_NAME", exclude=True)`字段。数据将从环境变量中加载，但请记住，代码中传递的值优先。所有字段，即使是排除的字段（`exclude=True`），在从文件加载配置时也会加载。排除允许你在*序列化*期间跳过它们，未排除的`SecretStr`将序列化为`"**********"`字符串。

```py
from pydantic import BaseModel, SecretStr
from forge.models.config import UserConfigurable

class SensitiveConfig(BaseModel):
    api_key: SecretStr = UserConfigurable(from_env="API_KEY", exclude=True)
```

### 配置序列化

`BaseAgent`提供了两种方法：
1. `dump_component_configs`：将所有组件的配置序列化为json字符串。
1. `load_component_configs`：将json字符串反序列化为配置并应用它。

### JSON配置

在启动代理时，你可以指定一个JSON文件（例如`config.json`）用于配置。该文件包含AutoGPT使用的各个[组件](../components/introduction.md)的设置。要指定文件，请使用`--component-config-file` CLI选项，例如使用`config.json`：

```shell
./autogpt.sh run --component-config-file config.json
```

!!! 注意
    如果你使用Docker运行AutoGPT，你需要将配置文件挂载或复制到容器中。有关更多信息，请参阅[Docker指南](../../classic/setup/docker.md)。

### 示例JSON配置

你可以复制你想要更改的配置，例如到`classic/original_autogpt/config.json`，并根据需要进行修改。*大多数配置都有默认值，最好只设置你想要修改的值。*你可以在[内置组件](./built-in-components.md)中查看可用的配置字段和默认值。你也可以在`.json`文件中设置敏感变量，但建议使用环境变量代替。

```json
{
    "CodeExecutorConfiguration": {
        "execute_local_commands": false,
        "shell_command_control": "allowlist",
        "shell_allowlist": ["cat", "echo"],
        "shell_denylist": [],
        "docker_container_name": "agent_sandbox"
    },
    "FileManagerConfiguration": {
        "storage_path": "agents/AutoGPT/",
        "workspace_path": "agents/AutoGPT/workspace"
    },
    "GitOperationsConfiguration": {
        "github_username": null
    },
    "ActionHistoryConfiguration": {
        "llm_name": "gpt-3.5-turbo",
        "max_tokens": 1024,
        "spacy_language_model": "en_core_web_sm"
    },
    "ImageGeneratorConfiguration": {
        "image_provider": "dalle",
        "huggingface_image_model": "CompVis/stable-diffusion-v1-4",
        "sd_webui_url": "http://localhost:7860"
    },
    "WebSearchConfiguration": {
        "duckduckgo_max_attempts": 3
    },
    "WebSeleniumConfiguration": {
        "llm_name": "gpt-3.5-turbo",
        "web_browser": "chrome",
        "headless": true,
        "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36",
        "browse_spacy_language_model": "en_core_web_sm"
    }
}
```

## 组件排序

组件的执行顺序很重要，因为某些组件可能依赖于前一个组件的结果。**默认情况下，组件按字母顺序排序。**

### 单个组件排序

你可以通过将其他组件（或它们的类型）传递给`run_after`方法来对单个组件进行排序。这样可以确保组件在指定的组件之后执行。`run_after`方法返回组件本身，因此你可以在将组件分配给变量时调用它：

```py
class MyAgent(Agent):
    def __init__(self):
        self.hello_component = HelloComponent()
        self.calculator_component = CalculatorComponent().run_after(self.hello_component)
        # 这与传递类型等效：
        # self.calculator_component = CalculatorComponent().run_after(HelloComponent)
```

!!! 警告
    在排序组件时，请确保不要形成循环依赖！

### 所有组件排序

你也可以通过在代理的`__init__`方法中设置`self.components`列表来对所有组件进行排序。这种方式确保没有循环依赖，并且忽略任何`run_after`调用。

!!! 警告
    确保包含所有组件 - 通过设置`self.components`列表，你正在覆盖自动发现组件的默认行为。由于这通常不是预期的行为，代理会在终端中通知你是否有组件被跳过。

```py
class MyAgent(Agent):
    def __init__(self):
        self.hello_component = HelloComponent()
        self.calculator_component = CalculatorComponent()
        # 显式设置组件列表
        self.components = [self.hello_component, self.calculator_component]
```

## 禁用组件

你可以通过设置它们的`_enabled`属性来控制哪些组件被启用。组件默认是*启用的*。可以提供一个`bool`值或一个`Callable[[], bool]`，每次组件即将执行时都会检查。这样你可以根据某些条件动态启用或禁用组件。你还可以通过设置`_disabled_reason`来提供禁用组件的原因。该原因将在调试信息中可见。

```py
class DisabledComponent(MessageProvider):
    def __init__(self):
        # 禁用此组件
        self._enabled = False
        self._disabled_reason = "此组件因某些原因被禁用。"

        # 或者根据某些条件禁用，静态地...：
        self._enabled = self.some_property is not None
        # ... 或动态地：
        self._enabled = lambda: self.some_property is not None

    # 此方法永远不会被调用
    def get_messages(self) -> Iterator[ChatMessage]:
        yield ChatMessage.user("此消息将不会被看到！")

    def some_condition(self) -> bool:
        return False
```

如果你根本不想要该组件，你可以直接从代理的`__init__`方法中删除它。如果你想删除从父类继承的组件，你可以将相关属性设置为`None`：

!!! 警告
    在删除其他组件所需的组件时要小心。这可能导致错误和意外行为。

```py
class MyAgent(Agent):
    def __init__(self):
        super().__init__(...)
        # 禁用父类中的WatchdogComponent
        self.watchdog = None
```

## 异常

提供了自定义错误，可用于在出现问题时控制执行流程。所有这些错误都可以在协议方法中引发，并由代理捕获。默认情况下，代理将重试三次，如果仍未解决，则重新引发异常。所有传递的参数都会自动处理，并在需要时恢复值。所有错误都接受可选的`str`消息。以下是按范围递增排序的错误：

1. `ComponentEndpointError`：单个端点方法执行失败。代理将重试该组件的端点执行。
2. `EndpointPipelineError`：管道执行失败。代理将重试所有组件的端点执行。
3. `ComponentSystemError`：多个管道失败。

**示例**

```py
from forge.agent.components import ComponentEndpointError
from forge.agent.protocols import MessageProvider

# 引发错误的示例
class MyComponent(MessageProvider):
    def get_messages(self) -> Iterator[ChatMessage]:
        # 这将导致组件始终失败
        # 并在重新引发异常之前重试3次
        raise ComponentEndpointError("端点错误！")
```