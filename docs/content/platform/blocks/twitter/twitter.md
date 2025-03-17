# Twitter API 集成模块

## Twitter 发布推文模块

### 功能概述
该模块用于在 Twitter 上发布带有各种可选附件和设置的推文。

### 功能描述
该模块允许发布带有文本内容和可选附件（如媒体、投票、引用或深度链接）的推文。

### 工作原理
它使用 Twitter API（Tweepy）创建带有指定内容和设置的推文，处理身份验证和错误情况。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| tweet_text | 推文的主要文本内容 |
| attachment | 可选的媒体、深度链接、投票、地点或引用附件 |
| for_super_followers_only | 推文是否仅对超级粉丝可见 |
| exclude_reply_user_ids | 排除在回复线程中的用户 ID |
| in_reply_to_tweet_id | 正在回复的推文 ID |
| reply_settings | 谁可以回复推文 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| tweet_id | 创建的推文 ID |
| tweet_url | 查看推文的 URL |
| error | 如果发布失败，返回错误信息 |

### 可能的使用场景
自动化发布带有丰富内容（如投票、媒体或引用）的推文。

---

## Twitter 删除推文模块

### 功能概述
该模块用于删除 Twitter 上指定的推文。

### 功能描述
该模块使用推文 ID 删除现有的推文。

### 工作原理
它使用 Twitter API（Tweepy）删除指定 ID 的推文，处理身份验证和错误情况。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| tweet_id | 要删除的推文 ID |

### 输出
| 输出 | 描述 |
|--------|-------------|
| success | 删除是否成功 |
| error | 如果删除失败，返回错误信息 |

### 可能的使用场景
自动清理旧或不相关的推文。

---

## Twitter 搜索最近推文模块

### 功能概述
该模块用于搜索 Twitter 上最近的公开推文。

### 功能描述
该模块根据指定的条件搜索推文，并提供过滤和分页选项。

### 工作原理
它使用 Twitter API（Tweepy）的搜索端点，根据提供的参数查询并返回匹配的推文和元数据。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| query | 搜索查询字符串 |
| max_results | 每页的最大结果数 |
| pagination | 获取下一页结果的令牌 |
| expansions | 包含的额外数据字段 |
| start_time | 搜索时间窗口的开始时间 |
| end_time | 搜索时间窗口的结束时间 |
| since_id | 返回此推文 ID 之后的结果 |
| until_id | 返回此推文 ID 之前的结果 |
| sort_order | 返回结果的顺序 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| tweet_ids | 匹配的推文 ID 列表 |
| tweet_texts | 推文文本内容列表 |
| next_token | 获取下一页的令牌 |
| data | 完整的推文数据 |
| included | 请求的额外数据 |
| meta | 分页和结果元数据 |
| error | 如果搜索失败，返回错误信息 |

### 可能的使用场景
监控 Twitter 上特定话题或标签的提及。

---

## Twitter 获取引用推文模块

### 功能概述
该模块用于检索引用特定推文的推文。

### 功能描述
该模块获取引用指定推文 ID 的推文列表，并提供分页和过滤选项。

### 工作原理
它使用 Twitter API（Tweepy）获取指定推文 ID 的引用推文，处理身份验证并返回带有可选扩展的推文数据。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| tweet_id | 要获取引用推文的推文 ID |
| max_results | 返回的最大结果数（最多 100） |
| exclude | 要排除的推文类型 |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 包含的地点相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| ids | 引用推文 ID 列表 |
| texts | 引用推文文本内容列表 |
| next_token | 获取下一页的令牌 [更多信息](twitter.md#common-output)。 |
| data | 完整的推文数据 |
| included | 请求的额外数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 如果请求失败，返回错误信息 |

### 可能的使用场景
通过引用推文监控特定推文的参与度和回应。

---

## Twitter 转发推文模块

### 功能概述
该模块用于转发 Twitter 上现有的推文。

### 功能描述
该模块使用推文 ID 创建指定推文的转发。

### 工作原理
它使用 Twitter API（Tweepy）转发指定 ID 的推文，处理身份验证和错误情况。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| tweet_id | 要转发的推文 ID |

### 输出
| 输出 | 描述 |
|--------|-------------|
| success | 转发是否成功 |
| error | 如果转发失败，返回错误信息 |

### 可能的使用场景
自动转发符合特定标准的内容。

---

## Twitter 移除转发模块

### 功能概述
该模块用于移除 Twitter 上的转发。

### 功能描述
该模块移除指定推文的现有转发。

### 工作原理
它使用 Twitter API（Tweepy）移除指定推文 ID 的转发，处理身份验证和错误情况。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| tweet_id | 要移除转发的推文 ID |

### 输出
| 输出 | 描述 |
|--------|-------------|
| success | 移除转发是否成功 |
| error | 如果移除失败，返回错误信息 |

### 可能的使用场景
根据特定条件自动清理转发。

---

## Twitter 获取转发用户模块

### 功能概述
该模块用于检索转发特定推文的用户信息。

### 功能描述
该模块获取转发指定推文 ID 的用户列表，并提供分页和过滤选项。

### 工作原理
它使用 Twitter API（Tweepy）获取指定推文 ID 的转发用户信息，处理身份验证并返回带有可选扩展的用户数据。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| tweet_id | 要获取转发用户的推文 ID |
| max_results | 每页的最大结果数（1-100） |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 包含的地点相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| ids | 转发用户的用户 ID 列表 |
| names | 转发用户的用户名列表 |
| usernames | 转发用户的用户名列表 |
| next_token | 获取下一页的令牌 |
| data | 完整的用户数据 |
| included | 请求的额外数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 如果请求失败，返回错误信息 |

### 可能的使用场景
通过转发模式监控参与度和分析用户行为。

---

## Twitter 获取用户提及模块

### 功能概述
该模块用于检索提及特定 Twitter 用户的推文。

### 功能描述
该模块获取提及某个用户的推文，使用其用户 ID。

### 工作原理
它使用 Twitter API（Tweepy）根据提供的用户 ID 查询并获取提及该用户的推文，处理分页和过滤器。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| user_id | 要获取提及推文的用户 ID |
| max_results | 每页的结果数（5-100） |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 包含的地点相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| ids | 推文 ID 列表 |
| texts | 推文文本内容列表 |
| userIds | 提及目标用户的用户 ID 列表 |
| userNames | 提及目标用户的用户名列表 |
| next_token | 获取下一页的令牌 |
| data | 完整的推文数据 |
| included | 请求的额外数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 如果请求失败，返回错误信息 |

### 可能的使用场景
监控特定账户的提及以进行社区管理。

---

## Twitter 获取主页时间线模块

### 功能概述
该模块用于检索用户主页时间线的推文。

### 功能描述
该模块返回由认证用户及其关注账户发布的最近推文和转发的集合。

### 工作原理
它使用 Twitter API（Tweepy）获取主页时间线的推文，处理分页并应用过滤器。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| max_results | 每页的结果数（5-100） |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 包含的地点相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| ids | 推文 ID 列表 |
| texts | 推文文本内容列表 |
| userIds | 发布推文的用户 ID 列表 |
| userNames | 发布推文的用户名列表 |
| next_token | 获取下一页的令牌 |
| data | 完整的推文数据 |
| included | 请求的额外数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 如果请求失败，返回错误信息 |

### 可能的使用场景
监控和分析关注账户的内容。

---

## Twitter 获取用户推文模块

### 功能概述
该模块用于检索特定 Twitter 用户发布的推文。

### 功能描述
该模块返回由单个用户发布的推文，使用其用户 ID 进行标识。

### 工作原理
它使用 Twitter API（Tweepy）获取指定用户时间线的推文，处理分页和过滤器。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| user_id | 要获取推文的用户 ID |
| max_results | 每页的结果数（5-100） |
| pagination_token | 获取下一页结果的令牌 |
| expansions | 包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 包含的地点相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| ids | 推文 ID 列表 |
| texts | 推文文本内容列表 |
| userIds | 发布推文的用户 ID 列表 |
| userNames | 发布推文的用户名列表 |
| next_token | 获取下一页的令牌 |
| data | 完整的推文数据 |
| included | 请求的额外数据 [更多信息](twitter.md#common-output)。 |
| meta | 分页和结果元数据 [更多信息](twitter.md#common-output)。 |
| error | 如果请求失败，返回错误信息 |

### 可能的使用场景
分析特定 Twitter 账户的内容和活动模式。

---

## Twitter 获取推文模块

### 功能概述
该模块用于通过推文 ID 检索特定推文的详细信息。

### 功能描述
该模块获取指定推文 ID 的推文信息，包括推文内容、作者详细信息和可选的扩展数据。

### 工作原理
它使用 Twitter API（Tweepy）通过推文 ID 获取单个推文，处理身份验证并返回带有可选扩展的推文数据。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| tweet_id | 要获取的推文 ID |
| expansions | 包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 包含的地点相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| id | 推文 ID |
| text | 推文文本内容 |
| userId | 推文作者 ID |
| userName | 推文作者用户名 |
| data | 完整的推文数据 |
| included | 请求的额外数据 [更多信息](twitter.md#common-output)。 |
| meta | 推文元数据 [更多信息](twitter.md#common-output)。 |
| error | 如果请求失败，返回错误信息 |

### 可能的使用场景
检索特定推文的详细信息以进行分析或监控。

---

## Twitter 获取多条推文模块

### 功能概述
该模块用于通过推文 ID 检索多条推文的信息。

### 功能描述
该模块获取最多 100 条推文的信息，使用推文 ID 进行标识。

### 工作原理
它使用 Twitter API（Tweepy）批量获取推文 ID 的推文数据，处理身份验证并返回带有可选扩展的推文数据。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| tweet_ids | 要获取的推文 ID 列表（最多 100） |
| expansions | 包含的额外数据字段 [更多信息](twitter.md#common-input)。 |
| media_fields | 包含的媒体相关字段 [更多信息](twitter.md#common-input)。 |
| place_fields | 包含的地点相关字段 [更多信息](twitter.md#common-input)。 |
| poll_fields | 包含的投票相关字段 [更多信息](twitter.md#common-input)。 |
| tweet_fields | 包含的推文特定字段 [更多信息](twitter.md#common-input)。 |
| user_fields | 包含的用户相关字段 [更多信息](twitter.md#common-input)。 |

### 输出
| 输出 | 描述 |
|--------|-------------|
| ids | 推文 ID 列表 |
| texts | 推文文本内容列表 |
| userIds | 推文作者 ID 列表 |
| userNames | 推文作者用户名列表 |
| data | 完整的推文数据数组 |
| included | 请求的额外数据 [更多信息](twitter.md#common-output)。 |
| meta | 推文元数据 [更多信息](twitter.md#common-output)。 |
| error | 如果请求失败，返回错误信息 |

### 可能的使用场景
批量获取推文信息以进行分析或存档。

---

## Twitter 点赞推文模块

### 功能概述
该模块用于在 Twitter 上点赞推文。

### 功能描述
该模块使用推文 ID 创建指定推文的点赞。

### 工作原理
它使用 Twitter API（Tweepy）点赞指定 ID 的推文，处理身份验证和错误情况。

### 输入
| 输入 | 描述 |
|-------|-------------|
| credentials | 具有所需范围的 Twitter API 凭证 |
| tweet_id | 要点赞的推文 ID |

### 输出
| 输出 | 描述 |
|--------|-------------|
| success | 点赞是否成功 |
| error | 如果点赞失败，返回错误信息 |

### 可能的使用场景
自动点赞符合特定标准的推文