# AutoGPT 代理设置

[🐋 **使用 Docker 进行设置与运行**](./docker.md)
&ensp;|&ensp;
[👷🏼 **开发者指南**](./for-developers.md)

## 📋 要求

### Linux / macOS

- Python 3.10 或更高版本
- Poetry（[安装说明](https://python-poetry.org/docs/#installation)）

### Windows (WSL)

- WSL 2
  - 另请参阅 [Linux 的要求](#linux-macos)
- [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/)

### Windows

!!! 注意
    我们建议使用 WSL 设置 AutoGPT。某些功能在 Windows 上运行方式不同，我们目前无法为所有情况提供专门的说明。

- Python 3.10 或更高版本（[安装说明](https://www.tutorialspoint.com/how-to-install-python-in-windows)）
- Poetry（[安装说明](https://python-poetry.org/docs/#installation)）
- [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/)

## 设置 AutoGPT

### 获取 AutoGPT

由于我们不将 AutoGPT 作为桌面应用程序分发，您需要从 GitHub 下载[项目]并在您的计算机上为其指定一个位置。

![克隆或下载仓库的对话框截图](get-repo-dialog.png)

- 要获取最新的前沿版本，请使用 `master`。
- 如果您需要更稳定的版本，请查看最新的 AutoGPT [发布][releases]。

[项目]: https://github.com/Significant-Gravitas/AutoGPT
[发布]: https://github.com/Significant-Gravitas/AutoGPT/releases

!!! 注意
    如果您打算将 AutoGPT 作为 Docker 镜像运行，这些说明不适用。请查看 [Docker 设置](./docker.md) 指南。

### 完成设置

克隆或下载项目后，您可以在 `original_autogpt/` 文件夹中找到 AutoGPT 代理。
在此文件夹中，您可以使用 `.env` 文件和（可选）JSON 配置文件来配置 AutoGPT 应用程序：

- `.env` 用于环境变量，主要用于敏感数据如 API 密钥
- JSON 配置文件用于自定义 AutoGPT 的某些功能 [组件](../../forge/components/introduction.md)

有关可用环境变量的列表，请参阅 [配置](../configuration/options.md) 参考。

1. 找到名为 `.env.template` 的文件。在某些操作系统中，由于点前缀，此文件可能默认隐藏。要显示隐藏文件，请按照您的特定操作系统的说明操作：
   [Windows][显示隐藏文件/Windows] 和 [macOS][显示隐藏文件/macOS]。
2. 创建 `.env.template` 的副本并命名为 `.env`；如果您已经在命令提示符/终端窗口中：
   ```shell
   cp .env.template .env
   ```
3. 在文本编辑器中打开 `.env` 文件。
4. 设置您想要使用的 LLM 提供商的 API 密钥：请参阅 [下文](#设置-llm-提供商)。
5. 输入您想要使用的任何其他 API 密钥或令牌。

    !!! 注意
        要激活和调整设置，请删除 `# ` 前缀。

6. 保存并关闭 `.env` 文件。
7. _可选：运行 `poetry install` 以安装所有必需的依赖项。_ 应用程序在启动时也会检查并安装任何必需的依赖项。
8. _可选：使用您所需的设置配置 JSON 文件（例如 `config.json`）。_ 如果您不提供 JSON 配置文件，应用程序将使用默认设置。
   了解如何 [设置 JSON 配置文件](../../forge/components/components.md#json-配置)

您现在应该能够探索 CLI（`./autogpt.sh --help`）并运行应用程序。

有关进一步说明，请参阅 [用户指南](../usage.md)。

[显示隐藏文件/Windows]: https://support.microsoft.com/en-us/windows/view-hidden-files-and-folders-in-windows-97fbc472-c603-9d90-91d0-1166d1d9f4b5
[显示隐藏文件/macOS]: https://www.pcmag.com/how-to/how-to-access-your-macs-hidden-files

## 设置 LLM 提供商

您可以将 AutoGPT 与以下任何 LLM 提供商一起使用。
每个提供商都有其自己的设置说明。

AutoGPT 最初是基于 OpenAI 的 GPT-4 构建的，但现在您也可以使用其他模型/提供商获得类似且有趣的结果。
如果您不知道选择哪个，可以安全地选择 OpenAI*。

<small>* 可能会发生变化</small>

### OpenAI

!!! 注意
    要将 AutoGPT 与 GPT-4（推荐）一起使用，您需要设置一个付费的 OpenAI 账户并存入一些资金。请参考 OpenAI 的进一步说明（[链接][openai/help-gpt-4-access]）。
    免费账户仅限于 GPT-3.5，每分钟只能发出 3 个请求。

1. 确保您已设置了一个有信用的付费账户：[设置 > 组织 > 账单][openai/billing]
1. 从以下位置获取您的 OpenAI API 密钥：[API 密钥][openai/api-keys]
2. 打开 `.env`
3. 找到显示 `OPENAI_API_KEY=` 的行
4. 在 = 后直接插入您的 OpenAI API 密钥，不带引号或空格：
    ```ini
    OPENAI_API_KEY=sk-proj-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```

    !!! 信息 "使用 GPT Azure 实例"
        如果您想在 Azure 实例上使用 GPT，请将 `USE_AZURE` 设置为 `True` 并
        创建一个 Azure 配置文件。

        将 `azure.yaml.template` 重命名为 `azure.yaml` 并提供相关的
        `azure_api_base`、`azure_api_version` 和您想要使用的模型的部署 ID。

        例如，如果您想使用 `gpt-3.5-turbo` 和 `gpt-4-turbo`：

        ```yaml
        # 请将所有值指定为双引号字符串
        # 将尖括号 (<>) 中的字符串替换为您自己的部署名称
        azure_model_map:
            gpt-3.5-turbo: "<gpt-35-turbo-deployment-id>"
            gpt-4-turbo: "<gpt-4-turbo-deployment-id>"
            ...
        ```

        详细信息可以在 [openai/python-sdk/azure] 和 [Azure OpenAI 文档] 中找到，用于嵌入模型。
        如果您在 Windows 上，可能需要安装 [MSVC 库](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170)。

!!! 重要
    请密切关注 [使用页面][openai/usage] 上的 API 成本。

[openai/api-keys]: https://platform.openai.com/account/api-keys
[openai/billing]: https://platform.openai.com/account/billing/overview
[openai/usage]: https://platform.openai.com/account/usage
[openai/api-limits]: https://platform.openai.com/docs/guides/rate-limits/free-tier-rate-limits
[openai/help-gpt-4-access]: https://help.openai.com/en/articles/7102672-how-can-i-access-gpt-4-gpt-4-turbo-and-gpt-4o#h_9bddcd317c
[openai/python-sdk/azure]: https://github.com/openai/openai-python?tab=readme-ov-file#microsoft-azure-openai

### Anthropic

1. 确保您的账户中有信用：[设置 > 计划与账单][anthropic/billing]
2. 从 [设置 > API 密钥][anthropic/api-keys] 获取您的 Anthropic API 密钥
3. 打开 `.env`
4. 找到显示 `ANTHROPIC_API_KEY=` 的行
5. 在 = 后直接插入您的 Anthropic API 密钥，不带引号或空格：
    ```ini
    ANTHROPIC_API_KEY=sk-ant-api03-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```
6. 将 `SMART_LLM` 和/或 `FAST_LLM` 设置为您想要使用的 Claude 3 模型。
   有关可用模型的信息，请参阅 Anthropic 的 [模型概览][anthropic/models]。
   示例：
    ```ini
    SMART_LLM=claude-3-opus-20240229
    ```

!!! 重要
    请密切关注 [使用页面][anthropic/usage] 上的 API 成本。

[anthropic/api-keys]: https://console.anthropic.com/settings/keys
[anthropic/billing]: https://console.anthropic.com/settings/plans
[anthropic/usage]: https://console.anthropic.com/settings/usage
[anthropic/models]: https://docs.anthropic.com/en/docs/models-overview

### Groq

!!! 注意
    尽管 Groq 受支持，但其内置的函数调用 API 尚不成熟。
    使用此 API 的任何功能可能会遇到性能下降的问题。
    请告诉我们您的体验！

1. 从 [设置 > API 密钥][groq/api-keys] 获取您的 Groq API 密钥
2. 打开 `.env`
3. 找到显示 `GROQ_API_KEY=` 的行
4. 在 = 后直接插入您的 Groq API 密钥，不带引号或空格：
    ```ini
    GROQ_API_KEY=gsk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
    ```
5. 将 `SMART_LLM` 和/或 `FAST_LLM` 设置为您想要使用的 Groq 模型。
   有关可用模型的信息，请参阅 Groq 的 [模型概览][groq/models]。
   示例：
    ```ini
    SMART_LLM=llama3-70b-8192
    ```

[groq/api-keys]: https://console.groq.com/keys
[groq/models]: https://console.groq.com/docs/models

### Llamafile

使用 llamafile 您可以在本地运行模型，这意味着无需设置账单，并且保证数据隐私。

有关更多信息和深入文档，请查看 [llamafile 文档]。

!!! 警告
    目前，llamafile 一次只能服务一个模型。这意味着您不能
    将 `SMART_LLM` 和 `FAST_LLM` 设置为两个不同的 llamafile 模型。

!!! 警告
    由于以下链接的问题，llamafiles 在 WSL 上无法工作。要在 WSL 中使用 llamafile
    与 AutoGPT，您必须在 Windows 中运行 llamafile（在 WSL 之外）。

    <details>
    <summary>说明</summary>

    1. 通过以下两种方式之一获取 `llamafile/serve.py` 脚本：
        1. 在您的 Windows 环境中克隆 AutoGPT 仓库，
           脚本位于 `classic/original_autogpt/scripts/llamafile/serve.py`
        2. 在您的 Windows 环境中仅下载 [serve.py] 脚本
    2. 确保已安装 `click`：`pip install click`
    3. 在 WSL 中运行 `ip route | grep default | awk '{print $3}'` 以获取
       WSL 主机的地址
    4. 运行 `python3 serve.py --host {WSL_HOST_ADDR}`，其中 `{WSL_HOST_ADDR}`
       是您在步骤 3 中找到的地址。
       如果端口 8080 被占用，请使用 `--port {PORT}` 指定不同的端口。
    5. 在 WSL 中，在 `.env` 中设置 `LLAMAFILE_API_BASE=http://{WSL_HOST_ADDR}:8080/v1`。
    6. 按照下面的常规说明继续操作。

    [serve.py]: https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/original_autogpt/scripts/llamafile/serve.py
    </details>

    * [Mozilla-Ocho/llamafile#356](https://github.com/Mozilla-Ocho/llamafile/issues/356)
    * [Mozilla-Ocho/llamafile#100](https://github.com/Mozilla-Ocho/llamafile/issues/100)

!!! 注意
    这些说明将下载并使用 `mistral-7b-instruct-v0.2.Q5_K_M.llamafile`。
    `mistral-7b-instruct-v0.2` 是目前唯一经过测试和受支持的模型。
    如果您想尝试其他模型，您必须将它们添加到 [`llamafile.py`][classic/forge/llamafile.py] 中的 `LlamafileModelName`。
    为了获得最佳结果，您可能还需要添加一些逻辑来调整消息格式，
    如 `LlamafileProvider._adapt_chat_messages_for_mistral_instruct(..)` 所做的那样。

1. 运行 llamafile 服务脚本：
   ```shell
   python3 ./scripts/llamafile/serve.py
   ```
   第一次运行时，它将下载包含模型 + 运行时的文件，这可能需要一些时间和几 GB 的磁盘空间。

   要强制使用 GPU 加速，请在命令中添加 `--use-gpu`。

3. 在 `.env` 中，将 `SMART_LLM`/`FAST_LLM` 或两者设置为 `mistral-7b-instruct-v0.2`

4. 如果服务器运行在不同于 `http://localhost:8080/v1` 的地址上，
   请在 `.env` 中将 `LLAMAFILE_API_BASE` 设置为正确的基础 URL

[llamafile 文档]: https://github.com/Mozilla-Ocho/llamafile#readme
[classic/forge/llamafile.py]: https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/forge/llm/providers/llamafile/llamafile.py