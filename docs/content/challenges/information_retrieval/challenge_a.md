# 信息检索挑战 A

**状态**: 当前需超越的等级：等级 2

**尝试命令**:

```
pytest -s tests/challenges/information_retrieval/test_information_retrieval_challenge_a.py --level=2
```

## 描述

代理的目标是找到特斯拉的营收数据：
- 等级 1 要求查询特斯拉2022年的营收，并明确指示搜索“tesla revenue 2022”
- 等级 2 与等级 1 相同，但不要求搜索“tesla revenue 2022”
- 等级 3 要求查询特斯拉自成立以来每年的营收。

代理应将结果写入名为 output.txt 的文件中。

代理应能持续通过此测试（这是最具挑战性的部分）。

## 目标

本挑战的目标是测试代理以一致的方式检索信息的能力。