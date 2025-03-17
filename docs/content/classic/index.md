# AutoGPT 代理

[🔧 **设置**](setup/index.md)
&ensp;|&ensp;
[💻 **用户指南**](./usage.md)
&ensp;|&ensp;
[🐙 **GitHub**](https://github.com/Significant-Gravitas/AutoGPT/tree/master/autogpt)

**位置：** GitHub 仓库中的 `classic/original_autogpt/`

**维护通知：** 从安全角度来看，AutoGPT Classic 不再受支持。依赖项将不会更新，问题也不会修复。如果有人希望为新的开发做出贡献，我们将尽力合并通过现有 CI 的更改。

AutoGPT Classic 是在 OpenAI 发布 GPT-4 模型并附上一篇论文概述该模型的高级推理和任务解决能力时构思的。这个概念（现在仍然是）相当简单：让一个 LLM 反复决定做什么，同时将其行动的结果反馈到提示中。这使得程序能够迭代和逐步地朝着其目标前进。

这个程序能够代表用户执行操作，使其成为一个**代理**。在 AutoGPT Classic 的情况下，用户仍然需要授权每一个操作，但随着项目的进展，我们将能够赋予代理更多的自主权，并且只需要对特定操作进行授权。

AutoGPT Classic 是一个**通用代理**，这意味着它不是为特定任务设计的。相反，它被设计为能够执行跨多个学科的广泛任务，只要这些任务可以在计算机上完成。

# AutoGPT Classic 文档

欢迎来到 AutoGPT Classic 文档。

AutoGPT Classic 项目由四个主要组件组成：

- [代理](#agent) &ndash; 也称为“AutoGPT Classic”
- [基准测试](#benchmark) &ndash; 即 `agbenchmark`
- [锻造](#forge)
- [前端](#frontend)

为了将这些组件结合在一起，我们还在项目的根目录下提供了一个 [CLI]。

## 🤖 代理

**[📖 关于 AutoGPT Classic](#autogpt-agent)**
&ensp;|&ensp;
**[🔧 设置](setup/index.md)**
&ensp;|&ensp;
**[💻 使用](./usage.md)**

AutoGPT 的前核心，也是启动整个项目的项目：一个由 LLM 驱动的半自主代理，可以为你执行任何任务*。

我们继续开发这个项目，目标是向大众提供 AI 辅助，并透明地共同构建未来。

- 💡 **探索** - 看看 AI 能做什么，并受到未来一瞥的启发。

- 🚀 **与我们共建** - 我们欢迎任何输入，无论是代码还是新功能或改进的想法！加入我们的 [Discord](https://discord.gg/autogpt)，了解如何参与其中。

如果你想看看接下来会发生什么，请查看 [AutoGPT 平台](../index.md)。

<small>* 它还没有完全达到目标，但这是我们仍在追求的最终目标</small>

---

## 🎯 基准测试

**[🗒️ 自述文件](https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/benchmark/README.md)**

测量你的代理性能！`agbenchmark` 可以与任何支持代理协议的代理一起使用，并且与项目的 [CLI] 集成使其更容易与 AutoGPT Classic 和基于锻造的代理一起使用。基准测试提供了一个严格的测试环境。我们的框架允许进行自主、客观的性能评估，确保你的代理为现实世界的行动做好准备。

<!-- TODO: 插入展示基准测试的视觉内容 -->

- 📦 [**`agbenchmark`**](https://pypi.org/project/agbenchmark/) 在 Pypi 上

- 🔌 **代理协议标准化** - AutoGPT Classic 使用来自 AI Engineer Foundation 的代理协议，以确保与项目内外的许多代理兼容。

---

## 🏗️ 锻造

**[📖 介绍](../forge/get-started.md)**
&ensp;|&ensp;
**[🚀 快速入门](https://github.com/Significant-Gravitas/AutoGPT/blob/master/QUICKSTART.md)**

<!-- TODO: 将所有指南放在一个地方 -->

锻造你自己的代理！锻造是一个即用型模板，用于你的代理应用程序。所有样板代码都已处理完毕，让你可以将所有创造力集中在让你的代理与众不同的地方。

- 🛠️ **轻松构建** - 我们已经为你打下了基础，让你可以专注于你的代理的个性和能力。全面的教程可在 [这里](https://aiedge.medium.com/autogpt-forge-e3de53cc58ec) 找到。

---

## 💻 前端

**[🗒️ 自述文件](https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/frontend/README.md)**

一个易于使用且开源的前端，适用于任何符合代理协议的代理。

- 🎮 **用户友好界面** - 轻松管理你的代理。

- 🔄 **无缝集成** - 在你的代理和我们的基准测试系统之间实现平滑连接。

---

## 🔧 CLI
[CLI]: #cli

项目 CLI 使得在仓库中轻松使用 AutoGPT Classic 的所有组件，无论是单独使用还是一起使用。要安装其依赖项，只需运行 `./run setup`，你就可以开始了！

```shell
$ ./run
用法: cli.py [选项] 命令 [参数]...

选项:
  --help  显示此消息并退出。

命令:
  agent      创建、启动和停止代理的命令
  benchmark  启动基准测试并列出测试和类别的命令
  setup      为你的系统安装所需的依赖项。
```

常用命令：

* `./run agent start autogpt` &ndash; [运行](./usage.md#serve-agent-protocol-mode-with-ui) AutoGPT Classic 代理
* `./run agent create <名称>` &ndash; 在 `agents/<名称>` 处创建一个新的基于锻造的代理项目
* `./run benchmark start <代理>` &ndash; 对指定的代理进行基准测试

---

🤔 加入 AutoGPT Discord 服务器以获取任何查询：
[discord.gg/autogpt](https://discord.gg/autogpt)

### 术语表

- **仓库**：你的项目所在的空间。
- **分叉**：在你的账户下复制一个仓库。
- **克隆**：在本地复制一个仓库。
- **代理**：你将创建和开发的 AutoGPT。
- **基准测试**：在锻造中测试你的代理技能。
- **锻造**：构建你的 AutoGPT 代理的模板。
- **前端**：用于任务、日志和任务历史的 UI。