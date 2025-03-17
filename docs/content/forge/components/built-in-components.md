# 内置组件

本页面列出了所有原生提供的[🧩 组件](./components.md)及其实现的[⚙️ 协议](./protocols.md)。这些组件由AutoGPT代理使用。
一些组件在表格中列出了额外的配置选项，详情请参见[组件配置](./components.md/#component-configuration)以了解更多。

!!! 注意
    如果配置字段使用环境变量，仍然可以通过配置模型传递。配置中的值优先于环境变量！只有在配置中未设置值时，才会应用环境变量。

## `SystemComponent`

允许代理完成任务的基本组件。

### DirectiveProvider

- 关于API预算的约束

### MessageProvider

- 当前时间和日期
- 剩余的API预算及预算不足时的警告

### CommandProvider

- `finish` 用于任务完成时

## `UserInteractionComponent`

增加在CLI中与用户交互的能力。

### CommandProvider

- `ask_user` 用于向用户请求输入

## `FileManagerComponent`

增加将持久文件读写到本地存储、Google云存储或Amazon S3的能力。对于保存和加载代理状态（保留会话）是必要的。

### `FileManagerConfiguration`

| 配置变量         | 详情                                   | 类型  | 默认值                            |
| ---------------- | -------------------------------------- | ----- | ---------------------------------- |
| `storage_path`   | 代理文件的路径，例如状态文件           | `str` | `agents/{agent_id}/`[^1]           |
| `workspace_path` | 代理可访问文件的路径                   | `str` | `agents/{agent_id}/workspace/`[^1] |

[^1] 此选项在组件构建期间动态设置，而不是在配置模型内部默认设置，`{agent_id}` 被替换为代理的唯一标识符。

### DirectiveProvider

- 可以读写文件的资源信息

### CommandProvider

- `read_file` 用于读取文件
- `write_file` 用于写入文件
- `list_folder` 列出文件夹中的所有文件

## `CodeExecutorComponent`

让代理执行非交互式Shell命令和Python代码。只有在Docker可用时，Python代码才能执行。

### `CodeExecutorConfiguration`

| 配置变量                 | 详情                                              | 类型                        | 默认值           |
| ------------------------ | ------------------------------------------------- | --------------------------- | ----------------- |
| `execute_local_commands` | 启用Shell命令执行                                 | `bool`                      | `False`           |
| `shell_command_control`  | 控制使用哪个列表                                  | `"allowlist" \| "denylist"` | `"allowlist"`     |
| `shell_allowlist`        | 允许的Shell命令列表                               | `List[str]`                 | `[]`              |
| `shell_denylist`         | 禁止的Shell命令列表                               | `List[str]`                 | `[]`              |
| `docker_container_name`  | 用于代码执行的Docker容器名称                      | `str`                       | `"agent_sandbox"` |

所有Shell命令配置仅为方便起见。此组件不安全，不应在生产环境中使用。建议使用更合适的沙箱。

### CommandProvider

- `execute_shell` 执行Shell命令
- `execute_shell_popen` 使用popen执行Shell命令
- `execute_python_code` 执行Python代码
- `execute_python_file` 执行Python文件

## `ActionHistoryComponent`

跟踪代理的操作及其结果。向提示提供其摘要。

### `ActionHistoryConfiguration`

| 配置变量               | 详情                                                 | 类型        | 默认值            |
| ---------------------- | --------------------------------------------------- | ----------- | ------------------ |
| `llm_name`             | 用于压缩历史的LLM模型名称                           | `ModelName` | `"gpt-3.5-turbo"`  |
| `max_tokens`           | 用于历史摘要的最大令牌数                             | `int`       | `1024`             |
| `spacy_language_model` | 用于使用spacy进行摘要分块的语言模型                  | `str`       | `"en_core_web_sm"` |
| `full_message_count`   | 提示中包含的未摘要的周期数                           | `int`       | `4`                |

### MessageProvider

- 代理的进度摘要

### AfterParse

- 注册代理的操作

### ExecutionFailure

- 回滚代理的操作，使其不被保存

### AfterExecute

- 将代理的操作结果保存到历史中

## `GitOperationsComponent`

增加与git仓库和GitHub交互的能力。

### `GitOperationsConfiguration`

| 配置变量        | 详情                                   | 类型  | 默认值 |
| --------------- | -------------------------------------- | ----- | ------ |
| `github_username` | GitHub用户名，*ENV:* `GITHUB_USERNAME` | `str` | `None` |
| `github_api_key`  | GitHub API密钥，*ENV:* `GITHUB_API_KEY` | `str` | `None` |

### CommandProvider

- `clone_repository` 用于克隆git仓库

## `ImageGeneratorComponent`

增加使用各种提供者生成图像的能力。

### Hugging Face

要使用Hugging Face的文本到图像模型，您需要一个Hugging Face API令牌。
链接到相应的设置页面：[Hugging Face > Settings > Tokens](https://huggingface.co/settings/tokens)

### Stable Diffusion WebUI

可以使用自托管的Stable Diffusion WebUI与AutoGPT。确保运行WebUI时启用了`--api`。

### `ImageGeneratorConfiguration`

| 配置变量                | 详情                                                       | 类型                                    | 默认值                           |
| ---------------------- | --------------------------------------------------------- | --------------------------------------- | --------------------------------- |
| `image_provider`        | 图像生成提供者                                             | `"dalle" \| "huggingface" \| "sdwebui"` | `"dalle"`                         |
| `huggingface_image_model` | Hugging Face图像模型，参见[可用模型]                      | `str`                                   | `"CompVis/stable-diffusion-v1-4"` |
| `huggingface_api_token` | Hugging Face API令牌，*ENV:* `HUGGINGFACE_API_TOKEN`      | `str`                                   | `None`                            |
| `sd_webui_url`          | 自托管Stable Diffusion WebUI的URL                          | `str`                                   | `"http://localhost:7860"`         |
| `sd_webui_auth`         | Stable Diffusion WebUI的基本认证，*ENV:* `SD_WEBUI_AUTH`  | `str` 格式为 `{username}:{password}`    | `None`                            |

[可用模型]: https://huggingface.co/models?pipeline_tag=text-to-image

### CommandProvider

- `generate_image` 用于根据提示生成图像

## `WebSearchComponent`

允许代理搜索网络。DuckDuckGo不需要Google凭据。[如何设置Google API密钥的说明](../../classic/configuration/search.md)

### `WebSearchConfiguration`

| 配置变量                     | 详情                                                                 | 类型                        | 默认值 |
| --------------------------- | ------------------------------------------------------------------- | --------------------------- | ------ |
| `google_api_key`            | Google API密钥，*ENV:* `GOOGLE_API_KEY`                             | `str`                       | `None` |
| `google_custom_search_engine_id` | Google自定义搜索引擎ID，*ENV:* `GOOGLE_CUSTOM_SEARCH_ENGINE_ID` | `str`                       | `None` |
| `duckduckgo_max_attempts`   | 使用DuckDuckGo搜索的最大尝试次数                                     | `int`                       | `3`    |
| `duckduckgo_backend`        | 用于DDG SDK的后端                                                   | `"api" \| "html" \| "lite"` | `"api"` |

### DirectiveProvider

- 可以搜索网络的资源信息

### CommandProvider

- `search_web` 用于使用DuckDuckGo搜索网络
- `google` 用于使用Google搜索网络，需要API密钥

## `WebSeleniumComponent`

允许代理使用Selenium读取网站。

### `WebSeleniumConfiguration`

| 配置变量                  | 详情                                     | 类型                                          | 默认值                                                                                                                      |
| ------------------------ | --------------------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `llm_name`               | 用于读取网站的LLM模型名称               | `ModelName`                                   | `"gpt-3.5-turbo"`                                                                                                            |
| `web_browser`            | Selenium使用的Web浏览器                 | `"chrome" \| "firefox" \| "safari" \| "edge"` | `"chrome"`                                                                                                                   |
| `headless`               | 以无头模式运行浏览器                    | `bool`                                        | `True`                                                                                                                       |
| `user_agent`             | 浏览器使用的用户代理                    | `str`                                         | `"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36"` |
| `browse_spacy_language_model` | 用于分块文本的Spacy语言模型           | `str`                                         | `"en_core_web_sm"`                                                                                                           |
| `selenium_proxy`         | 与Selenium一起使用的Http代理            | `str`                                         | `None`                                                                                                                       |

### DirectiveProvider

- 可以读取网站的资源信息

### CommandProvider

- `read_website` 用于读取特定URL并查找特定主题或回答问题

## `ContextComponent`

增加在提示中保持文件和文件夹内容最新的能力。

### MessageProvider

- 上下文中元素的内容

### CommandProvider

- `open_file` 用于将文件打开到上下文中
- `open_folder` 用于将文件夹打开到上下文中
- `close_context_item` 从上下文中移除项目

## `WatchdogComponent`

监视代理是否在循环，并在必要时切换到智能模式。

### AfterParse

- 调查发生了什么并在必要时切换到智能模式