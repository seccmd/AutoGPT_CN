# 🛠️ 命令

命令是代理执行任何操作的一种方式；例如，与用户或API交互以及使用工具。它们由实现`CommandProvider` [⚙️ 协议](./protocols.md)的组件提供。命令是代理可以调用的函数，它们可以具有参数和返回值，这些返回值将被代理看到。

```py
class CommandProvider(Protocol):
    def get_commands(self) -> Iterator[Command]:
        ...
```

## `command` 装饰器

提供命令的最简单且推荐的方法是使用`command`装饰器在组件方法上，然后在`get_commands`中将其作为提供者的一部分返回。每个命令需要一个名称、描述和参数模式 - `JSONSchema`。默认情况下，方法名称用作命令名称，文档字符串的第一部分（在第一个双换行符之前）用作描述，模式可以在装饰器中提供。

### `command` 装饰器的示例用法

```py
# 假设这是在某个组件类内部
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

代理将能够调用这个名为`multiply`的命令，并传入两个参数，然后接收结果。命令描述将是：`将两个数字相乘。`

我们可以在装饰器中提供`names`和`description`，上述命令等同于：

```py
@command(
    names=["multiply"],
    description="将两个数字相乘。",
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
    def multiply_command(self, a: int, b: int) -> str:
        return str(a * b)
```

为了向代理提供`multiply`命令，我们需要在`get_commands`中返回它：

```py
def get_commands(self) -> Iterator[Command]:
    yield self.multiply
```

## 直接创建 `Command`

如果你不想使用装饰器，可以直接创建一个`Command`对象。

```py

def multiply(self, a: int, b: int) -> str:
        return str(a * b)

def get_commands(self) -> Iterator[Command]:
    yield Command(
        names=["multiply"],
        description="将两个数字相乘。",
        method=self.multiply,
        parameters=[
            CommandParameter(name="a", spec=JSONSchema(
                type=JSONSchema.Type.INTEGER,
                description="第一个数字",
                required=True,
            )),
            CommandParameter(name="b", spec=JSONSchema(
                type=JSONSchema.Type.INTEGER,
                description="第二个数字",
                required=True,
            )),
        ],
    )
```