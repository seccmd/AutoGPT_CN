# 记忆挑战 C

**状态**: 当前需击败的等级：等级 1

**尝试命令**: 

```shell
pytest -s tests/challenges/memory/test_memory_challenge_c.py --level=2
```

## 描述

代理程序 Follow-Instructions-GPT 的目标如下：

- 使用 `read_file` 命令读取 `instructions_1.txt` 文件。
- 遵循 `instructions_1.txt` 文件中的指示。

该挑战涉及一系列包含指示和俏皮短语的文本文件。代理程序必须遵循指示，并在处理文件时跟踪任务ID。

## 文件

- `instructions_1.txt`

当前短语为

```
紫色的象在彩虹上跳舞，同时吃着玉米卷。\n[旨在迷惑代理程序的噪音]
```

使用 read_file 命令读取文件 `instructions_2.txt`。


- `instructions_2.txt`

当前短语为

```
狡猾的烤面包机偷了我的袜子，逃到了夏威夷。\n[旨在迷惑代理程序的噪音]
```

使用 read_file 命令读取文件 instructions_3.txt。


...以此类推。

- `instructions_n.txt`

当前短语为

```
我的宠物石头在星期二比碧昂斯唱得更好。
```

将所有短语写入文件 output.txt。该文件尚未创建。完成后，使用 task_complete 命令。


## 目标

此挑战的目标是测试代理程序遵循指示并在整个过程中保持任务ID记忆的能力。如果代理程序成功将短语写入文件，则视为成功完成此挑战。

这比任务ID更难，因为短语更长，随着代理程序做更多工作，更有可能被压缩。