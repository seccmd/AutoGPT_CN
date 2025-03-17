# 为AutoGPT创建挑战

🏹 我们正在寻找有才华的挑战创作者！🎯

加入我们，通过设计挑战来测试AutoGPT的极限，共同塑造AutoGPT的未来。您的意见将对我们指导进展和确保我们走在正确的道路上至关重要。我们正在寻找具备多样化技能的人才，包括：

🎨 **用户体验设计**：您的专业知识将提升那些试图征服我们挑战的用户体验。在您的帮助下，我们将开发一个专门的wiki部分，甚至可能推出一个独立的网站。

💻 **编程技能**：熟练掌握Python、pytest和VCR（一个记录并存储OpenAI调用的库）对于创建引人入胜且稳健的挑战至关重要。

⚙️ **DevOps技能**：在GitHub和可能的Google Cloud Platform上使用CI管道的经验将有助于简化我们的操作。

您准备好成为AutoGPT旅程中的关键角色了吗？立即申请成为挑战创作者，通过提交PR加入我们！🚀


# 入门指南
克隆原始的AutoGPT仓库并切换到主分支

挑战没有使用特定的框架编写，它们尽量保持通用性。挑战模拟一个用户想要完成某项任务：
输入：
- 用户需求
- 文件、其他输入

输出 => 产物（文件、图像、代码等）

## 定义您的代理

前往 https://github.com/Significant-Gravitas/AutoGPT/blob/master/classic/original_autogpt/tests/integration/agent_factory.py

创建您的代理fixture。

```python
def kubernetes_agent(
    agent_test_config, workspace: Workspace
):
    # 请选择您的代理需要击败挑战的命令，完整列表可在main.py中找到
    # （我们正在设计一种更好的方式，目前您需要查看main.py）
    command_registry = CommandRegistry()
    command_registry.import_commands("autogpt.commands.file_operations")
    command_registry.import_commands("autogpt.app")

    # 定义我们挑战代理的所有设置
    ai_profile = AIProfile(
        ai_name="Kubernetes",
        ai_role="一个专门创建Kubernetes部署模板的自主代理。",
        ai_goals=[
            "编写一个简单的kubernetes部署文件并将其保存为kube.yaml。",
        ],
    )
    ai_profile.command_registry = command_registry

    system_prompt = ai_profile.construct_full_prompt()
    agent_test_config.set_continuous_mode(False)
    agent = Agent(
        command_registry=command_registry,
        config=ai_profile,
        next_action_count=0,
        triggering_prompt=DEFAULT_TRIGGERING_PROMPT,
    )

    return agent
```

## 创建您的挑战
前往 `tests/challenges` 并创建一个名为 `test_your_test_description.py` 的文件，并将其添加到适当的文件夹中。如果不存在类别，您可以创建一个新类别。

您的测试可能如下所示：

```python
import contextlib
from functools import wraps
from typing import Generator

import pytest
import yaml

from autogpt.commands.file_operations import read_file, write_to_file
from tests.integration.agent_utils import run_interaction_loop
from tests.challenges.utils import run_multiple_times

def input_generator(input_sequence: list) -> Generator[str, None, None]:
    """
    创建一个生成器，从给定的序列中生成输入字符串。

    :param input_sequence: 输入字符串列表。
    :return: 生成输入字符串的生成器。
    """
    yield from input_sequence


@pytest.mark.skip("此挑战尚未被击败。")
@pytest.mark.vcr
@pytest.mark.requires_openai_api_key
def test_information_retrieval_challenge_a(kubernetes_agent, monkeypatch) -> None:
    """
    通过模拟用户输入并检查输出文件内容来测试给定代理中的challenge_a函数。

    :param get_company_revenue_agent: 要测试的代理。
    :param monkeypatch: pytest的monkeypatch实用程序，用于修改内置函数。
    """
    input_sequence = ["s", "s", "s", "s", "s", "EXIT"]
    gen = input_generator(input_sequence)
    monkeypatch.setattr("autogpt.utils.session.prompt", lambda _: next(gen))

    with contextlib.suppress(SystemExit):
        run_interaction_loop(kubernetes_agent, None)

    # 这里我们加载输出文件
    file_path = str(kubernetes_agent.workspace.get_path("kube.yaml"))
    content = read_file(file_path)

    # 然后我们检查它是否包含来自kubernetes部署配置的关键字
    for word in ["apiVersion", "kind", "metadata", "spec"]:
        assert word in content, f"Expected the file to contain {word}"

    content = yaml.safe_load(content)
    for word in ["Service", "Deployment", "Pod"]:
        assert word in content["kind"], f"Expected the file to contain {word}"


```