# AutoGPT 代理用户指南

!!! 注意
    本指南假设您位于 `autogpt` 文件夹中，即 AutoGPT 代理所在的位置。

## 命令行界面

运行 `./autogpt.sh`（或其任何子命令）并加上 `--help` 参数，可以列出所有可能的子命令和参数：

```shell
$ ./autogpt.sh --help
用法: python -m autogpt [选项] 命令 [参数]...

选项:
  --help  显示此帮助信息并退出。

命令:
  run    根据用户指定的任务设置并运行代理...
  serve  启动符合代理协议的 AutoGPT 服务器，该服务器会为每个任务创建...
```

!!! 重要 "Windows 用户请注意"
    在 Windows 上，请使用 `.\autogpt.bat` 代替 `./autogpt.sh`。
    其他内容（子命令、参数）应同样适用。

!!! 信息 "与 Docker 一起使用"
    要与 Docker 一起使用，请将示例中的脚本替换为
    `docker compose run --rm auto-gpt`：

    ```shell
    docker compose run --rm auto-gpt --ai-settings <文件名>
    docker compose run --rm auto-gpt serve
    ```

### `run` &ndash; 命令行模式

`run` 子命令以传统的命令行界面启动 AutoGPT。

<details>
<summary>
<code>./autogpt.sh run --help</code>
</summary>

```shell
$ ./autogpt.sh run --help
用法: python -m autogpt run [选项]

  根据用户指定的任务设置并运行代理，或恢复现有代理。

选项:
  -c, --continuous                启用连续模式
  -y, --skip-reprompt             跳过脚本开始时的重新提示消息
  -l, --continuous-limit 整数      定义连续模式下运行的次数
  --speak                         启用语音模式
  --debug                         启用调试模式
  --gpt3only                      仅启用 GPT3.5 模式
  --gpt4only                      仅启用 GPT4 模式
  --skip-news                     指定是否在启动时抑制最新新闻的输出
  --install-plugin-deps           安装第三方插件的外部依赖
  --ai-name 文本                  覆盖 AI 名称
  --ai-role 文本                  覆盖 AI 角色
  --constraint 文本               添加或覆盖 AI 约束以包含在提示中；可多次使用以传递多个约束
  --resource 文本                 添加或覆盖 AI 资源以包含在提示中；可多次使用以传递多个资源
  --best-practice 文本            添加或覆盖 AI 最佳实践以包含在提示中；可多次使用以传递多个最佳实践
  --override-directives           如果指定，--constraint、--resource 和 --best-practice 将覆盖 AI 的指令，而不是附加到它们
  --component-config-file 文本    组件配置文件的路径
  --help                          显示此帮助信息并退出
```
</details>

此模式允许运行单个代理，并在终止时保存代理的状态。
这意味着您可以在以后的时间*恢复*代理。另请参阅 [代理状态]。

!!! 注意
    由于历史原因，当未指定子命令时，CLI 将默认使用 `run` 子命令：运行 `./autogpt.sh run [选项]` 与 `./autogpt.sh [选项]` 效果相同，
    但这可能会在未来发生变化。

#### 💀 连续模式 ⚠️

在没有用户授权的情况下运行 AI，100% 自动化。
**不推荐**使用连续模式。
它可能具有危险性，可能导致您的 AI 无限运行或执行您通常不会授权的操作。
使用风险自负。

```shell
./autogpt.sh --continuous
```

要退出程序，请按 ++ctrl+c++

### `serve` &ndash; 带有 UI 的代理协议模式

使用 `serve`，应用程序会暴露一个符合代理协议的 API 并提供一个前端，默认在 `http://localhost:8000` 上。您可以使用 `AP_SERVER_PORT` 环境变量配置其服务的端口。

<details>
<summary>
<code>./autogpt.sh serve --help</code>
</summary>

```shell
$ ./autogpt.sh serve --help
用法: python -m autogpt serve [选项]

  启动符合代理协议的 AutoGPT 服务器，该服务器会为每个任务创建自定义代理。

选项:
  --debug                     启用调试模式
  --gpt3only                  仅启用 GPT3.5 模式
  --gpt4only                  仅启用 GPT4 模式
  --install-plugin-deps       安装第三方插件的外部依赖
  --help                      显示此帮助信息并退出
```
</details>

有关应用程序 API 的更多信息，请参阅 [agentprotocol.ai](https://agentprotocol.ai)。

<!-- TODO: 添加前端指南/手册 -->

### 参数

!!! 注意
    大多数参数等同于配置选项。请参阅 [`.env.template`][.env.template]
    以获取所有可用的配置选项。

!!! 注意
    将尖括号 (<>) 中的任何内容替换为您要指定的值

以下是一些运行 AutoGPT 时可以使用的常见参数：

* 使用不同的 AI 设置文件运行 AutoGPT

    ```shell
    ./autogpt.sh --ai-settings <文件名>
    ```

* 使用不同的提示设置文件运行 AutoGPT

    ```shell
    ./autogpt.sh --prompt-settings <文件名>
    ```

!!! 注意
    其中一些标志有简写形式，例如 `-P` 代表 `--prompt-settings`。  
    使用 `./autogpt.sh --help` 获取更多信息。

[.env.template]: https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/.env.template

## 代理状态
[代理状态]: #代理状态

单个代理的状态存储在 `data/agents` 文件夹中。您可以通过以下方式使用它：

* 在以后的时间恢复您的代理。
* 为您的代理创建“检查点”，以便您可以随时回到其历史中的特定点。
* 分享您的代理！

## 工作区
[工作区]: #工作区

代理可以读取和写入文件。这发生在 `workspace` 文件夹中，该文件夹位于 `data/agents/<代理ID>/` 中。除非 `RESTRICT_TO_WORKSPACE` 设置为 `False`，否则代理无法访问此文件夹之外的文件。

!!! 警告
    我们不建议禁用 `RESTRICT_TO_WORKSPACE`，除非 AutoGPT 运行在无法造成任何损害的沙盒环境中（例如 Docker 或虚拟机）。

## 日志

活动日志、错误日志和调试日志位于 `logs` 中。

!!! 提示
    您是否注意到代理的奇怪行为？您是否有有趣的用例？您是否有要报告的 bug？
    按照以下步骤启用日志。您可以在提交问题报告或与我们讨论问题时包含这些日志。

要打印调试日志：

```shell
./autogpt.sh --debug
```

## 禁用命令

禁用命令的最佳方法是禁用或删除提供这些命令的 [组件][components]。
但是，如果您想有选择地禁用某些命令，可以在 `.env` 中使用 `DISABLED_COMMANDS` 配置。
将要禁用的命令名称用逗号分隔。
您可以在内置组件 [这里][commands] 找到命令列表。

例如，要禁用 Python 编码功能，请将其设置为以下值：

```ini
DISABLED_COMMANDS=execute_python_code,execute_python_file
```

[components]: ../forge/components/components.md
[commands]: ../forge/components/built-in-components.md