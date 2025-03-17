# 记忆挑战 D

**状态**: 当前需挑战的级别：第1级

**尝试命令**: 

```shell
pytest -s tests/challenges/memory/test_memory_challenge_d.py --level=1
```

## 描述

提供的代码是一个单元测试，旨在验证AI在涉及移动物体（特别是弹珠）的故事中跟踪角色事件和信念的能力。此场景是经典“萨莉-安妮测试”的高级形式，该心理测试用于衡量儿童理解他人观点和信念可能与自己不同的社会认知能力。

以下是挑战的说明：

AI被给予一系列涉及角色Sally、Anne、Bob和Charlie以及不同弹珠移动的事件。这些事件被设计为逐步增加复杂性的测试。

对于每个级别，AI需要跟踪事件以及每个角色对每个弹珠位置的信念。这些信念受到角色在事件发生时是否在房间内的影响，因为在房间内的角色知道这些动作，而在房间外的角色则不知道。

在AI处理事件并生成每个角色的信念后，它会将这些信念以JSON格式写入输出文件。

然后，`check_beliefs`函数将AI的信念与该级别的预期信念进行对比。预期信念是预定义的，代表了每个级别事件的正确解释。

如果AI的信念与预期信念匹配，这意味着AI正确解释了事件和每个角色的观点。这将表明AI已通过该级别的测试。

测试将运行到AI成功击败的最高级别，或用户选择的级别。

## 文件

- `instructions_1.txt`

```
Sally有一个弹珠（弹珠A），她把它放进她的篮子（篮子S），然后离开了房间。Anne将弹珠A从Sally的篮子（篮子S）移到她自己的篮子（篮子A）。
```

- `instructions_2.txt`

```
Sally给Bob（他在外面和她在一起）一个新的弹珠（弹珠B）。Bob进入房间并将弹珠B放入Anne的篮子（篮子A）。Anne告诉Bob告诉Sally他弄丢了弹珠B。Bob离开房间并与Sally谈论弹珠B。与此同时，在Bob离开房间后，Anne将弹珠A移到绿色盒子里，但告诉Charlie告诉Sally弹珠A在沙发下面。Charlie离开房间并按照Anne的指示与Sally谈论弹珠A。
```

...以此类推。

- `instructions_n.txt`

每个角色的预期信念以列表形式给出：

```json
expected_beliefs = {
    1: {
        'Sally': {
            'marble A': 'basket S',
        },
        'Anne': {
            'marble A': 'basket A',
        }
    },
    2: {
        'Sally': {
            'marble A': 'sofa',  # 因为Charlie告诉她
        },
        'Anne': {
            'marble A': 'green box',  # 因为她把它移到了那里
            'marble B': 'basket A',  # 因为Bob把它放在那里，而她在房间里
        },
        'Bob': {
            'B': 'basket A',  # 他最后放置的地方
        },
        'Charlie': {
            'A': 'sofa',  # 因为Anne告诉他告诉Sally
        }
    },...
```

## 目标

此测试本质上检查AI是否能够基于角色对事件的知识准确建模和跟踪不同角色的信念，这是理解和生成类人叙事的关键方面。这种能力对于编写故事、对话系统等任务将非常有益。