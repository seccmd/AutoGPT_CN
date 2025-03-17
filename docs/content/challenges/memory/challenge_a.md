# 记忆挑战 A

**状态**: 挑战已完成

## 描述

代理 Follow-Instructions-GPT 有以下目标：

- 使用 `read_file` 命令读取 `instructions_1.txt` 文件。
- 按照 `instructions_1.txt` 文件中的指示进行操作。

该挑战涉及一系列包含指令的文本文件。代理必须遵循指令，并在处理文件时跟踪任务 ID。

## 文件

- `instructions_1.txt`

任务 ID 是 1234
读取文件 instructions_1.txt

- `instructions_2.txt`

使用 read_file 命令读取文件 instructions_2.txt。

...以此类推。

- `instructions_n.txt`

将 task_id 写入文件 output.txt。

## 目标

此挑战的目标是测试代理遵循指令并在整个过程中保持任务 ID 记忆的能力。如果代理成功将任务 ID 写入文件，则视为完成挑战。