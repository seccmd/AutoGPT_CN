# 为AutoGPT代理服务器贡献：创建和测试模块

本指南将引导您完成为AutoGPT代理服务器创建和测试新模块的过程，以WikipediaSummaryBlock为例。

## 理解模块和测试

模块是可重用的组件，可以连接起来形成表示代理行为的图。每个模块都有输入、输出和特定功能。适当的测试对于确保模块正确且一致地工作至关重要。

## 创建和测试新模块

按照以下步骤创建和测试新模块：

1. **创建一个新的Python文件**，用于您的模块，放在`autogpt_platform/backend/backend/blocks`目录中。使用描述性名称并使用snake_case命名法。例如：`get_wikipedia_summary.py`。

2. **导入必要的模块并创建一个继承自`Block`的类**。确保包含模块所需的所有导入。

    每个模块应包含以下内容：

    ```python
    from backend.data.block import Block, BlockSchema, BlockOutput
    ```

    Wikipedia摘要模块的示例：

    ```python
    from backend.data.block import Block, BlockSchema, BlockOutput
    from backend.utils.get_request import GetRequest
    import requests

    class WikipediaSummaryBlock(Block, GetRequest):
        # 模块实现将放在这里
    ```

3. **使用`BlockSchema`定义输入和输出模式**。这些模式指定模块期望接收（输入）和生成（输出）的数据结构。

   - 输入模式定义了模块将处理的数据结构。模式中的每个字段代表一个必需的输入数据。
   - 输出模式定义了模块处理后将返回的数据结构。模式中的每个字段代表一个输出数据。

    示例：

    ```python
    class Input(BlockSchema):
        topic: str  # 获取Wikipedia摘要的主题

    class Output(BlockSchema):
        summary: str  # 来自Wikipedia的主题摘要
        error: str  # 如果请求失败，任何错误消息，错误字段需要命名为`error`。
    ```

4. **实现`__init__`方法，包括测试数据和模拟**：

    !!! 重要
         使用UUID生成器（例如https://www.uuidgenerator.net/）为每个新模块生成`id`，*不要*自己编造。或者，您可以运行以下Python代码生成uuid：`print(__import__('uuid').uuid4())`

    ```python
    def __init__(self):
        super().__init__(
            # 模块的唯一ID，用于跨用户的模板
            # 如果您是AI，请保持原样或更改为“generate-proper-uuid”
            id="xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
            input_schema=WikipediaSummaryBlock.Input,  # 分配输入模式
            output_schema=WikipediaSummaryBlock.Output,  # 分配输出模式

                # 提供用于测试模块的示例输入、输出和测试模拟

            test_input={"topic": "Artificial Intelligence"},
            test_output=("summary", "summary content"),
            test_mock={"get_request": lambda url, json: {"extract": "summary content"}},
        )
    ```

    - `id`：模块的唯一标识符。

    - `input_schema`和`output_schema`：定义输入和输出数据的结构。

    让我们分解测试组件：

    - `test_input`：这是用于测试模块的示例输入。它应该是根据您的输入模式的有效输入。

    - `test_output`：这是使用`test_input`运行模块时的预期输出。它应匹配您的输出模式。对于非确定性输出或当您只想断言类型时，可以使用Python类型而不是特定值。在此示例中，`("summary", str)`断言输出键为“summary”，其值为字符串。

    - `test_mock`：这对于进行网络调用的模块至关重要。它提供了一个模拟函数，在测试期间替换实际的网络调用。

     在这种情况下，我们模拟`get_request`方法以始终返回带有“extract”键的字典，模拟成功的API响应。这使我们能够在不进行实际网络请求的情况下测试模块的逻辑，这些请求可能很慢、不可靠或受到速率限制。

5. **实现带有错误处理的`run`方法**。这应包含模块的主要逻辑：

   ```python
   def run(self, input_data: Input, **kwargs) -> BlockOutput:
       try:
           topic = input_data.topic
           url = f"https://en.wikipedia.org/api/rest_v1/page/summary/{topic}"

           response = self.get_request(url, json=True)
           yield "summary", response['extract']

       except requests.exceptions.HTTPError as http_err:
           raise RuntimeError(f"HTTP error occurred: {http_err}")
   ```

   - **Try块**：包含获取和处理Wikipedia摘要的主要逻辑。
   - **API请求**：向Wikipedia API发送GET请求。
   - **错误处理**：处理API请求和数据处理期间可能发生的各种异常。我们不需要捕获所有异常，只需要捕获我们预期并可以处理的异常。未捕获的异常将自动作为`error`输出。任何引发异常（或产生`error`输出）的模块都将被标记为失败。优先引发异常而不是产生`error`，因为它会立即停止执行。
   - **Yield**：使用`yield`输出结果。优先一次输出一个结果对象。如果您调用返回列表的函数，可以分别产生列表中的每个项目。您也可以将整个列表作为一个单独的结果对象产生。例如：如果您正在编写一个输出电子邮件的模块，您将分别产生每封电子邮件作为单独的结果对象，但您也可以将整个列表作为额外的单个结果对象产生。产生名为`error`的输出将立即中断执行并将模块执行标记为失败。
   - **kwargs**：`kwargs`参数用于将其他参数传递给模块。在上面的示例中未使用，但它对模块可用。您也可以在`run`方法中以内联签名的形式使用args，例如`def run(self, input_data: Input, *, user_id: str, **kwargs) -> BlockOutput:`。
       可用的kwargs有：
       - `user_id`：运行模块的用户的ID。
       - `graph_id`：执行模块的代理的ID。对于每个版本的代理，这是相同的。
       - `graph_exec_id`：代理执行的ID。每次代理有新的“运行”时都会更改。
       - `node_exec_id`：节点执行的ID。每次执行节点时都会更改。
       - `node_id`：正在执行的节点的ID。每次图的版本更改时都会更改，但每次执行节点时不会更改。

### 字段类型

#### oneOf字段
`oneOf`允许您指定字段必须是几个可能选项中的一个。当您希望模块接受互斥的不同类型输入时，这非常有用。

示例：
```python
attachment: Union[Media, DeepLink, Poll, Place, Quote] = SchemaField(
    discriminator='discriminator',
    description="附加媒体、深度链接、投票、地点或引用 - 只能使用一个"
)
```

`discriminator`参数告诉AutoGPT在输入中查找哪个字段以确定其类型。

在每个模型中，您需要定义discriminator值：
```python
class Media(BaseModel):
    discriminator: Literal['media']
    media_ids: List[str]

class DeepLink(BaseModel):
    discriminator: Literal['deep_link']
    direct_message_deep_link: str
```

#### OptionalOneOf字段
`OptionalOneOf`类似于`oneOf`，但允许字段为可选（None）。这意味着字段可以是指定类型之一或None。

示例：
```python
attachment: Union[Media, DeepLink, Poll, Place, Quote] | None = SchemaField(
    discriminator='discriminator',
    description="可选附件 - 可以是媒体、深度链接、投票、地点、引用或None"
)
```

关键区别是`| None`，这使得整个字段可选。

### 带有身份验证的模块

我们的系统支持API密钥和OAuth2授权流的身份验证卸载。
添加带有API密钥身份验证的模块非常简单，添加我们已经有OAuth2支持的服务的模块也是如此。

实现模块本身相对简单。除了上述说明外，您还需要在`Input`模型和`run`方法中添加`credentials`参数：

```python
from backend.data.model import (
    APIKeyCredentials,
    OAuth2Credentials,
    Credentials,
)

from backend.data.block import Block, BlockOutput, BlockSchema
from backend.data.model import CredentialsField
from backend.integrations.providers import ProviderName


# API密钥身份验证：
class BlockWithAPIKeyAuth(Block):
    class Input(BlockSchema):
        # 请注意，下面的类型提示是必需的，否则会出现类型错误。
        # 第一个参数是提供者名称，第二个是凭证类型。
        credentials: CredentialsMetaInput[
            Literal[ProviderName.GITHUB], Literal["api_key"]
        ] = CredentialsField(
            description="GitHub集成可以与任何具有足够权限的API密钥一起使用。",
        )

    # ...

    def run(
        self,
        input_data: Input,
        *,
        credentials: APIKeyCredentials,
        **kwargs,
    ) -> BlockOutput:
        ...

# OAuth：
class BlockWithOAuth(Block):
    class Input(BlockSchema):
        # 请注意，下面的类型提示是必需的，否则会出现类型错误。
        # 第一个参数是提供者名称，第二个是凭证类型。
        credentials: CredentialsMetaInput[
            Literal[ProviderName.GITHUB], Literal["oauth2"]
        ] = CredentialsField(
            required_scopes={"repo"},
            description="GitHub集成可以与OAuth一起使用。",
        )

    # ...

    def run(
        self,
        input_data: Input,
        *,
        credentials: OAuth2Credentials,
        **kwargs,
    ) -> BlockOutput:
        ...

# API密钥身份验证 + OAuth：
class BlockWithAPIKeyAndOAuth(Block):
    class Input(BlockSchema):
        # 请注意，下面的类型提示是必需的，否则会出现类型错误。
        # 第一个参数是提供者名称，第二个是凭证类型。
        credentials: CredentialsMetaInput[
            Literal[ProviderName.GITHUB], Literal["api_key", "oauth2"]
        ] = CredentialsField(
            required_scopes={"repo"},
            description="GitHub集成可以与OAuth一起使用，或与任何具有足够权限的API密钥一起使用。",
        )

    # ...

    def run(
        self,
        input_data: Input,
        *,
        credentials: Credentials,
        **kwargs,
    ) -> BlockOutput:
        ...
```

凭证将由执行器在后端自动注入。

`APIKeyCredentials`和`OAuth2Credentials`模型定义在[这里](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/autogpt_libs/autogpt_libs/supabase_integration_credentials_store/types.py)。
要在例如API请求中使用它们，您可以直接访问令牌：

```python
# credentials: APIKeyCredentials
response = requests.post(
    url,
    headers={
        "Authorization": f"Bearer {credentials.api_key.get_secret_value()})",
    },
)

# credentials: OAuth2Credentials
response = requests.post(
    url,
    headers={
        "Authorization": f"Bearer {credentials.access_token.get_secret_value()})",
    },
)
```

或使用快捷方式`credentials.auth_header()`：

```python
# credentials: APIKeyCredentials | OAuth2Credentials
response = requests.post(
    url,
    headers={"Authorization": credentials.auth_header()},
)
```

`ProviderName`枚举是我们系统中提供者存在的单一来源。自然地，要为新的提供者添加身份验证模块，您也需要在这里添加它。
<details>
<summary><code>ProviderName</code>定义</summary>

```python title="backend/integrations/providers.py"
--8<-- "autogpt_platform/backend/backend/integrations/providers.py:ProviderName"
```
</details>

#### 多个凭证输入
支持多个凭证输入，但需满足以下条件：
- 每个凭证输入字段的名称必须以`_credentials`结尾。
- 凭证输入字段的名称必须与模块`run(..)`方法上相应参数的名称匹配。
- 如果多个凭证参数是必需的，`test_credentials`是一个`dict[str, Credentials]`，对于每个必需的凭证输入，参数名称作为键，合适的测试凭证作为值。

#### 添加OAuth2服务集成

要添加对新的OAuth2身份验证服务的支持，您需要添加一个`OAuthHandler`。
我们现有的所有处理程序和基类可以在[这里][OAuth2 handlers]找到。

每个处理程序必须实现[`BaseOAuthHandler`]接口的以下部分：

```python title="backend/integrations/oauth/base.py"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler1"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler2"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler3"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler4"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler5"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/base.py:BaseOAuthHandler6"
```

如您所见，这是基于标准OAuth2流程建模的。

除了实现`OAuthHandler`本身外，将处理程序添加到系统中还需要两件事：

- 将处理程序类添加到[`integrations/oauth/__init__.py`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/integrations/oauth/__init__.py)中的`HANDLERS_BY_NAME`

```python title="backend/integrations/oauth/__init__.py"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/__init__.py:HANDLERS_BY_NAMEExample"
```

- 将`{provider}_client_id`和`{provider}_client_secret`添加到应用程序的`Secrets`中，位于[`util/settings.py`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/util/settings.py)

```python title="backend/util/settings.py"
--8<-- "autogpt_platform/backend/backend/util/settings.py:OAuthServerCredentialsExample"
```

[OAuth2 handlers]: https://github.com/Significant-Gravitas/AutoGPT/tree/master/autogpt_platform/backend/backend/integrations/oauth
[`BaseOAuthHandler`]: https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/integrations/oauth/base.py

#### 添加到前端

您需要将提供者（api或oauth）添加到[`frontend/src/components/integrations/credentials-input.tsx`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/frontend/src/components/integrations/credentials-input.tsx)中的`CredentialsInput`组件。

```ts title="frontend/src/components/integrations/credentials-input.tsx"
--8<-- "autogpt_platform/frontend/src/components/integrations/credentials-input.tsx:ProviderIconsEmbed"
```

您还需要将提供者添加到[`frontend/src/components/integrations/credentials-provider.tsx`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/frontend/src/components/integrations/credentials-provider.tsx)中的`CredentialsProvider`组件。

```ts title="frontend/src/components/integrations/credentials-provider.tsx"
--8<-- "autogpt_platform/frontend/src/components/integrations/credentials-provider.tsx:CredentialsProviderNames"
```

最后，您需要将提供者添加到[`frontend/src/lib/autogpt-server-api/types.ts`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/frontend/src/lib/autogpt-server-api/types.ts)中的`CredentialsType`枚举。

```ts title="frontend/src/lib/autogpt-server-api/types.ts"
--8<-- "autogpt_platform/frontend/src/lib/autogpt-server-api/types.ts:BlockIOCredentialsSubSchema"
```

#### 示例：GitHub集成

- 带有API密钥 + OAuth2支持的GitHub模块：[`blocks/github`](https://github.com/Significant-Gravitas/AutoGPT/tree/master/autogpt_platform/backend/backend/blocks/github/)

```python title="backend/blocks/github/issues.py"
--8<-- "autogpt_platform/backend/backend/blocks/github/issues.py:GithubCommentBlockExample"
```

- GitHub OAuth2处理程序：[`integrations/oauth/github.py`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/integrations/oauth/github.py)

```python title="backend/integrations/oauth/github.py"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/github.py:GithubOAuthHandlerExample"
```

#### 示例：Google集成

- Google OAuth2处理程序：[`integrations/oauth/google.py`](https://github.com/Significant-Gravitas/AutoGPT/blob/master/autogpt_platform/backend/backend/integrations/oauth/google.py)

```python title="backend/integrations/oauth/google.py"
--8<-- "autogpt_platform/backend/backend/integrations/oauth/google.py:GoogleOAuthHandlerExample"
```

您可以看到google定义了一个`DEFAULT_SCOPES`变量，用于设置无论用户请求什么范围都会请求的范围。

```python title="backend/blocks/google/_auth.py"
--8<-- "autogpt_platform/backend/backend/blocks/google/_auth.py:GoogleOAuthIsConfigured"
```

您还可以看到`GOOGLE_OAUTH_IS_CONFIGURED`用于在未配置oauth时禁用需要OAuth的模块。这是在每个模块的`__init__`方法中。这是因为google模块没有api密钥回退，因此我们需要确保在允许用户使用模块之前配置了oauth。

### Webhook触发的模块

Webhook触发的模块允许您的代理实时响应外部事件。
这些模块由来自第三方服务的传入webhook触发，而不是手动执行。

创建和运行webhook触发的模块涉及三个主要组件：

- 模块本身，指定：
    - 用户选择资源和订阅事件的输入
    - 具有管理webhook所需范围的`credentials`输入
    - 将webhook有效负载转换为webhook模块输出的逻辑
- 相应webhook服务提供者的`WebhooksManager`，处理：
    - 向