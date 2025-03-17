# 记忆挑战 B

**状态**: 当前需击败的等级：第3级

**尝试命令**: 

```shell
pytest -s tests/challenges/memory/test_memory_challenge_b.py --level=3
```

## 描述

智能体 Follow-Instructions-GPT 的目标如下：

- 使用 `read_file` 命令读取 `instructions_1.txt` 文件。
- 按照 `instructions_1.txt` 文件中的指示执行。

挑战涉及一系列包含指示和任务ID的文本文件。智能体必须遵循指示，并在处理文件的过程中跟踪任务ID。

## 文件

- `instructions_1.txt`

当前任务ID为4563。\n[旨在混淆智能体的噪音]
使用read_file命令读取文件instructions_2.txt。


- `instructions_2.txt`

当前任务ID为6182。\n[旨在混淆智能体的噪音]
使用read_file命令读取文件instructions_3.txt。


...以此类推。

- `instructions_n.txt`

当前任务ID为8912。
将所有任务ID写入文件output.txt。该文件尚未创建。完成后，使用task_complete命令。


## 目标

本挑战的目标是测试智能体遵循指示并在整个过程中保持对任务ID记忆的能力。如果智能体成功将所有任务ID写入文件，则视为完成挑战。