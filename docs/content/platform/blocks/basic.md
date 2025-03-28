# 基本操作模块

## 存储值

### 功能概述
一个用于存储并转发值的基本模块。

### 作用
该模块接收一个输入值并存储它，使得该值可以在不改变的情况下被重复使用。

### 工作原理
它接受一个输入值和一个可选的数据值。如果提供了数据值，则将其作为输出；否则，使用输入值作为输出。

### 输入
| 输入 | 描述 |
|-------|-------------|
| 输入 | 需要存储或转发的值 |
| 数据 | 可选的常量值，用于替代输入值进行存储 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| 输出 | 存储的值（输入值或数据值） |

### 应用场景
在工作流开始时存储用户的姓名，以便在后续多个模块中使用，而无需再次询问。

---

## 打印到控制台

### 功能概述
一个用于调试目的，将文本打印到控制台的基本模块。

### 作用
该模块接收一个文本输入并将其打印到控制台，然后输出一个状态消息。

### 工作原理
它接收一个文本输入，将其以“打印：”为前缀打印到控制台，然后生成一个“已打印”状态。

### 输入
| 输入 | 描述 |
|-------|-------------|
| 文本 | 需要打印到控制台的文本 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| 状态 | 表示文本已打印的消息（“已打印”） |

### 应用场景
通过在工作流的不同阶段打印中间结果或消息来进行调试。

---

## 在字典中查找

### 功能概述
一个使用给定键在字典、对象或列表中查找值的基本模块。

### 作用
该模块在输入的数据结构中搜索指定的键，并返回找到的对应值。

### 工作原理
它接受一个输入（字典、对象或列表）和一个键。然后尝试在输入中查找该键并返回对应的值。如果未找到键，则返回整个输入作为“缺失”。

### 输入
| 输入 | 描述 |
|-------|-------------|
| 输入 | 需要搜索的字典、对象或列表 |
| 键 | 在输入中查找的键 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| 输出 | 找到的对应键的值 |
| 缺失 | 如果未找到键，则返回整个输入 |

### 应用场景
从复杂的数据结构中提取特定信息，例如在用户资料字典中查找用户的电子邮件地址。

---

## 代理输入

### 功能概述
一个在工作流中接受用户输入的输入模块。

### 作用
该模块允许用户在工作流中输入值，并提供命名、描述和设置占位符值的选项。

### 工作原理
它接受用户提供的值，以及名称、描述和可选的占位符值等元数据。然后输出提供的值。

### 输入
| 输入 | 描述 |
|-------|-------------|
| 值 | 用户提供的实际输入值 |
| 名称 | 输入字段的名称 |
| 描述 | 输入的描述（可选） |
| 占位符值 | 建议值的可选列表 |
| 限制为占位符值 | 是否将输入限制为仅占位符值 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| 结果 | 提供的输入值 |

### 应用场景
在个性化推荐工作流的开始阶段收集用户偏好。

---

## 代理输出

### 功能概述
一个记录并格式化工作流最终结果的输出模块。

### 作用
该模块接收一个值及其相关元数据，可选地对其进行格式化，并将其作为工作流的输出呈现。

### 工作原理
它接受一个输入值以及名称、描述和可选的格式字符串。如果提供了格式字符串，则尝试在输出之前对输入值应用格式化。

### 输入
| 输入 | 描述 |
|-------|-------------|
| 值 | 需要记录为输出的值 |
| 名称 | 输出的名称 |
| 描述 | 输出的描述（可选） |
| 格式 | 应用于值的可选格式字符串 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| 输出 | 格式化后的输出值（如果适用） |

### 应用场景
以特定格式呈现数据分析工作流的最终结果。

---

## 添加到字典

### 功能概述
一个向字典中添加新键值对的基本模块。

### 作用
该模块接收一个现有字典（或创建一个新字典）、一个键和一个值，并将键值对添加到字典中。

### 工作原理
它接受一个可选的输入字典、一个键和一个值。如果未提供字典，则创建一个新字典。然后将键值对添加到字典中并返回更新后的字典。

### 输入
| 输入 | 描述 |
|-------|-------------|
| 字典 | 需要添加到的现有字典（可选） |
| 键 | 新条目的键 |
| 值 | 新条目的值 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| 更新后的字典 | 添加了新条目的字典 |
| 错误 | 如果操作失败，则返回错误消息 |

### 应用场景
通过在工作流中逐步收集新信息来构建用户资料。

---

## 添加到列表

### 功能概述
一个向列表中添加新条目的基本模块。

### 作用
该模块接收一个现有列表（或创建一个新列表）并向其中添加一个新条目，可选地指定插入位置。

### 工作原理
它接受一个可选的输入列表、一个要添加的条目和一个可选的位置。如果未提供列表，则创建一个新列表。然后将条目添加到列表中的指定位置（如果未指定位置，则添加到末尾）并返回更新后的列表。

### 输入
| 输入 | 描述 |
|-------|-------------|
| 列表 | 需要添加到的现有列表（可选） |
| 条目 | 要添加到列表中的新项 |
| 位置 | 插入新条目的可选位置 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| 更新后的列表 | 添加了新条目的列表 |
| 错误 | 如果操作失败，则返回错误消息 |

### 应用场景
在任务管理工作流中维护待办事项列表，可以在特定优先级（位置）添加新任务。

---

## 便签

### 功能概述
一个显示自定义文本的便签的基本模块。

### 作用
该模块接收一个文本输入并将其显示为工作流界面中的便签。

### 工作原理
它简单地接受一个文本输入并将其作为输出传递，以显示为便签。

### 输入
| 输入 | 描述 |
|-------|-------------|
| 文本 | 显示在便签中的文本 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| 输出 | 显示在便签中的文本 |

### 应用场景
在复杂的工作流中添加解释性注释或提醒，以帮助用户理解不同阶段或提供额外上下文。