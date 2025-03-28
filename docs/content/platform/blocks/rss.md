# 读取RSS订阅源

## 功能概述
该模块用于从指定的RSS订阅源URL获取并处理条目。

## 功能描述
此模块连接到提供的RSS订阅源URL，获取订阅内容，并逐一处理每个条目。它会检查条目的发布日期是否在指定的时间范围内，如果是，则格式化并输出条目信息。

## 输入参数
| 输入项 | 描述 |
|-------|-------------|
| RSS URL | 您希望读取的RSS订阅源的网络地址 |
| 时间范围 | 相对于模块开始运行时，回溯查找新条目的分钟数 |
| 轮询频率 | 模块检查新条目的频率（以秒为单位） |
| 持续运行 | 模块是否应无限期地继续检查新条目，还是仅运行一次 |

## 输出结果
| 输出项 | 描述 |
|--------|-------------|
| 条目 | 一个RSS订阅项，包含以下信息： |
| | - 标题：条目的标题或名称 |
| | - 链接：可以找到完整条目的网络地址 |
| | - 描述：条目的简要摘要或摘录 |
| | - 发布日期：条目发布的时间 |
| | - 作者：撰写或创建条目的人 |
| | - 类别：与条目相关的主题或标签 |

## 可能的应用场景
新闻聚合应用程序可以使用此模块持续监控来自不同新闻源的多个RSS订阅源。然后，应用程序可以向用户展示最新的新闻条目，按主题分类并按发布日期排序。