# ⚙️ 协议

协议是由[组件](./components.md)实现的*接口*，用于将相关功能分组。每个协议都需要在执行过程中的某个时刻由代理显式处理。我们提供了一个全面的内置协议列表，这些协议已经在内置的`Agent`中得到处理，因此当你继承基础代理时，所有内置协议都将正常工作！

**协议按默认执行顺序列出。**

## 顺序无关协议

仅实现顺序无关协议的组件可以以任何顺序添加，包括在有序协议之间。

### `DirectiveProvider`

为代理提供约束、资源和最佳实践。这对其他协议没有直接影响；纯粹是信息性的，将在构建提示时传递给LLM。

```py
class DirectiveProvider(AgentComponent):
    def get_constraints(self) -> Iterator[str]:
        return iter([])

    def get_resources(self) -> Iterator[str]:
        return iter([])

    def get_best_practices(self) -> Iterator[str]:
        return iter([])
```

**示例** 一个网络搜索组件可以提供资源信息。请记住，这实际上并不允许代理访问互联网。为此，需要提供相关的`Command`。

```py
class WebSearchComponent(DirectiveProvider):
    def get_resources(self) -> Iterator[str]:
        yield "Internet access for searches and information gathering."
    # 如果不需要，我们可以跳过 "get_constraints" 和 "get_best_practices"
```

### `CommandProvider`

提供代理可以执行的命令。

```py
class CommandProvider(AgentComponent):
    def get_commands(self) -> Iterator[Command]:
        ...
```

提供命令的最简单方法是使用`command`装饰器装饰组件方法，然后生成该方法。每个命令都需要一个名称、描述和使用`JSONSchema`的参数模式。默认情况下，方法名称用作命令名称，文档字符串的第一部分用于描述（在`Args:`或`Returns:`之前），模式可以在装饰器中提供。

**示例** 可以执行乘法的计算器组件。如果与当前任务相关，代理将能够调用此命令，并看到返回的结果。

```py
from forge.agent import CommandProvider, Component
from forge.command import command
from forge.models.json_schema import JSONSchema


class CalculatorComponent(CommandProvider):
    get_commands(self) -> Iterator[Command]:
        yield self.multiply

    @command(parameters={
            "a": JSONSchema(
                type=JSONSchema.Type.INTEGER,
                description="The first number",
                required=True,
            ),
            "b": JSONSchema(
                type=JSONSchema.Type.INTEGER,
                description="The second number",
                required=True,
            )})
    def multiply(self, a: int, b: int) -> str:
        """
        Multiplies two numbers.
        
        Args:
            a: First number
            b: Second number

        Returns:
            Result of multiplication
        """
        return str(a * b)
```

代理将能够调用此命令，名为`multiply`，带有两个参数，并将收到结果。命令描述将是：`Multiplies two numbers.`

要了解更多关于命令的信息，请参阅[🛠️ 命令](./commands.md)。

## 顺序相关协议

实现顺序相关协议的组件的顺序很重要。
某些组件可能依赖于它们之前的组件的结果。

### `MessageProvider`

生成将添加到代理提示中的消息。你可以使用`ChatMessage.user()`：这将被解释为用户发送的消息，或者`ChatMessage.system()`：这将更加重要。

```py
class MessageProvider(AgentComponent):
    def get_messages(self) -> Iterator[ChatMessage]:
        ...
```

**示例** 向代理提示提供消息的组件。

```py
class HelloComponent(MessageProvider):
    def get_messages(self) -> Iterator[ChatMessage]:
        yield ChatMessage.user("Hello World!")
```

### `AfterParse`

在解析响应后调用的协议。

```py
class AfterParse(AgentComponent):
    def after_parse(self, response: ThoughtProcessOutput) -> None:
        ...
```

**示例** 在解析响应后记录响应的组件。

```py
class LoggerComponent(AfterParse):
    def after_parse(self, response: ThoughtProcessOutput) -> None:
        logger.info(f"Response: {response}")
```

### `ExecutionFailure`

在命令执行失败时调用的协议。

```py
class ExecutionFailure(AgentComponent):
    @abstractmethod
    def execution_failure(self, error: Exception) -> None:
        ...
```

**示例** 在命令失败时记录错误的组件。

```py
class LoggerComponent(ExecutionFailure):
    def execution_failure(self, error: Exception) -> None:
        logger.error(f"Command execution failed: {error}")
```

### `AfterExecute`

在代理成功执行命令后调用的协议。

```py
class AfterExecute(AgentComponent):
    def after_execute(self, result: ActionResult) -> None:
        ...
```

**示例** 在命令执行后记录结果的组件。

```py
class LoggerComponent(AfterExecute):
    def after_execute(self, result: ActionResult) -> None:
        logger.info(f"Result: {result}")
```