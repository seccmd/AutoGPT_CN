# AutoGPT 模块概览

AutoGPT 采用模块化方法，通过不同的“模块”来处理各种任务。这些模块是 AutoGPT 工作流的基础构建块，允许用户通过组合简单、专门的组件来创建复杂的自动化流程。

以下是所有可用模块的完整列表，按其主要功能分类。点击任何模块名称可查看其详细文档。

## 基本操作
| 模块名称 | 描述 |
|------------|-------------|
| [存储值](basic.md#store-value) | 存储并转发一个值 |
| [打印到控制台](basic.md#print-to-console) | 将文本输出到控制台以进行调试 |
| [在字典中查找](basic.md#find-in-dictionary) | 在字典或列表中查找值 |
| [代理输入](basic.md#agent-input) | 在工作流中接受用户输入 |
| [代理输出](basic.md#agent-output) | 记录并格式化工作流结果 |
| [添加到字典](basic.md#add-to-dictionary) | 向字典中添加新的键值对 |
| [添加到列表](basic.md#add-to-list) | 向列表中添加新条目 |
| [便签](basic.md#note) | 在工作流中显示便签 |

## 数据处理
| 模块名称 | 描述 |
|------------|-------------|
| [读取 CSV](csv.md#read-csv) | 处理并提取 CSV 文件中的数据 |
| [数据采样](sampling.md#data-sampling) | 使用各种采样方法选择数据的子集 |

## 文本处理
| 模块名称 | 描述 |
|------------|-------------|
| [匹配文本模式](text.md#match-text-pattern) | 检查文本是否匹配指定模式 |
| [提取文本信息](text.md#extract-text-information) | 使用模式从文本中提取特定信息 |
| [填充文本模板](text.md#fill-text-template) | 用提供的值填充模板 |
| [合并文本](text.md#combine-texts) | 将多个文本输入合并为一个 |
| [文本解码器](decoder_block.md#text-decoder) | 将编码文本转换为可读格式 |

## AI 和语言模型
| 模块名称 | 描述 |
|------------|-------------|
| [AI 结构化响应生成器](llm.md#ai-structured-response-generator) | 使用 LLM 生成结构化响应 |
| [AI 文本生成器](llm.md#ai-text-generator) | 使用 LLM 生成文本响应 |
| [AI 文本摘要器](llm.md#ai-text-summarizer) | 使用 LLM 对长文本进行摘要 |
| [AI 对话](llm.md#ai-conversation) | 使用 LLM 进行多轮对话 |
| [AI 列表生成器](llm.md#ai-list-generator) | 使用 LLM 根据提示生成列表 |

## 网络和 API 交互
| 模块名称 | 描述 |
|------------|-------------|
| [发送网络请求](http.md#send-web-request) | 向指定网址发送 HTTP 请求 |
| [读取 RSS 订阅](rss.md#read-rss-feed) | 检索并处理 RSS 订阅中的条目 |
| [获取天气信息](search.md#get-weather-information) | 获取指定位置的当前天气数据 |
| [Google 地图搜索](google_maps.md#google-maps-search) | 使用 Google 地图 API 搜索本地商家 |

## 社交媒体和内容
| 模块名称 | 描述 |
|------------|-------------|
| [获取 Reddit 帖子](reddit.md#get-reddit-posts) | 从指定子版块中检索帖子 |
| [发布 Reddit 评论](reddit.md#post-reddit-comment) | 在 Reddit 上发布评论 |
| [发布到 Medium](medium.md#publish-to-medium) | 直接将内容发布到 Medium |
| [读取 Discord 消息](discord.md#read-discord-messages) | 从 Discord 频道中检索消息 |
| [发送 Discord 消息](discord.md#send-discord-message) | 向 Discord 频道发送消息 |

## 搜索和信息检索
| 模块名称 | 描述 |
|------------|-------------|
| [获取维基百科摘要](search.md#get-wikipedia-summary) | 从维基百科获取主题摘要 |
| [搜索网络](search.md#search-the-web) | 执行网络搜索并返回结果 |
| [提取网站内容](search.md#extract-website-content) | 检索并提取网站内容 |

## 时间和日期
| 模块名称 | 描述 |
|------------|-------------|
| [获取当前时间](time_blocks.md#get-current-time) | 提供当前时间 |
| [获取当前日期](time_blocks.md#get-current-date) | 提供当前日期 |
| [获取当前日期和时间](time_blocks.md#get-current-date-and-time) | 提供当前日期和时间 |
| [倒计时器](time_blocks.md#countdown-timer) | 作为倒计时器使用 |

## 数学和计算
| 模块名称 | 描述 |
|------------|-------------|
| [计算器](maths.md#calculator) | 执行基本数学运算 |
| [计数项目](maths.md#count-items) | 计算集合中的项目数量 |

## 媒体生成
| 模块名称 | 描述 |
|------------|-------------|
| [Ideogram 模型](ideogram.md#ideogram-model) | 根据文本提示生成图像 |
| [创建会说话的虚拟形象视频](talking_head.md#create-talking-avatar-video) | 创建带有会说话的虚拟形象的视频 |
| [Unreal 文本转语音](text_to_speech_block.md#unreal-text-to-speech) | 使用 Unreal Speech API 将文本转换为语音 |
| [AI 短视频生成器](ai_shortform_video_block.md#ai-shortform-video-creator) | 使用 AI 生成短视频 |
| [Replicate Flux 高级模型](replicate_flux_advanced.md#replicate-flux-advanced-model) | 使用 Replicate 的 Flux 模型创建图像 |

## 其他
| 模块名称 | 描述 |
|------------|-------------|
| [转录 YouTube 视频](youtube.md#transcribe-youtube-video) | 转录 YouTube 视频中的音频 |
| [发送电子邮件](email_block.md#send-email) | 使用 SMTP 发送电子邮件 |
| [条件模块](branching.md#condition-block) | 评估工作流分支的条件 |
| [逐步遍历项目](iteration.md#step-through-items) | 遍历列表或字典 |

## Google 服务
| 模块名称 | 描述 |
|------------|-------------|
| [Gmail 读取](google/gmail.md#gmail-read) | 从 Gmail 账户中检索并读取电子邮件 |
| [Gmail 发送](google/gmail.md#gmail-send) | 使用 Gmail 账户发送电子邮件 |
| [Gmail 列出标签](google/gmail.md#gmail-list-labels) | 从 Gmail 账户中检索所有标签 |
| [Gmail 添加标签](google/gmail.md#gmail-add-label) | 向 Gmail 账户中的特定电子邮件添加标签 |
| [Gmail 移除标签](google/gmail.md#gmail-remove-label) | 从 Gmail 账户中的特定电子邮件中移除标签 |
| [Google Sheets 读取](google/sheet.md#google-sheets-read) | 从 Google Sheets 电子表格中读取数据 |
| [Google Sheets 写入](google/sheet.md#google-sheets-write) | 向 Google Sheets 电子表格中写入数据 |
| [Google 地图搜索](google_maps.md#google-maps-search) | 使用 Google 地图 API 搜索本地商家 |

## GitHub 集成
| 模块名称 | 描述 |
|------------|-------------|
| [GitHub 评论](github/issues.md#github-comment) | 在 GitHub 问题或拉取请求上发布评论 |
| [GitHub 创建问题](github/issues.md#github-make-issue) | 在 GitHub 仓库中创建新问题 |
| [GitHub 读取问题](github/issues.md#github-read-issue) | 检索特定 GitHub 问题的信息 |
| [GitHub 列出问题](github/issues.md#github-list-issues) | 从 GitHub 仓库中检索问题列表 |
| [GitHub 添加标签](github/issues.md#github-add-label) | 向 GitHub 问题或拉取请求添加标签 |
| [GitHub 移除标签](github/issues.md#github-remove-label) | 从 GitHub 问题或拉取请求中移除标签 |
| [GitHub 分配问题](github/issues.md#github-assign-issue) | 将用户分配给 GitHub 问题 |
| [GitHub 列出标签](github/repo.md#github-list-tags) | 检索并列出指定 GitHub 仓库的所有标签 |
| [GitHub 列出分支](github/repo.md#github-list-branches) | 检索并列出指定 GitHub 仓库的所有分支 |
| [GitHub 列出讨论](github/repo.md#github-list-discussions) | 检索并列出指定 GitHub 仓库的最近讨论 |
| [GitHub 创建分支](github/repo.md#github-make-branch) | 在 GitHub 仓库中创建新分支 |
| [GitHub 删除分支](github/repo.md#github-delete-branch) | 从 GitHub 仓库中删除指定分支 |
| [GitHub 列出拉取请求](github/pull_requests.md#github-list-pull-requests) | 从指定 GitHub 仓库中检索拉取请求列表 |
| [GitHub 创建拉取请求](github/pull_requests.md#github-make-pull-request) | 在指定 GitHub 仓库中创建新拉取请求 |
| [GitHub 读取拉取请求](github/pull_requests.md#github-read-pull-request) | 检索特定 GitHub 拉取请求的详细信息 |
| [GitHub 分配 PR 审查者](github/pull_requests.md#github-assign-pr-reviewer) | 将审查者分配给特定 GitHub 拉取请求 |
| [GitHub 取消分配 PR 审查者](github/pull_requests.md#github-unassign-pr-reviewer) | 从特定 GitHub 拉取请求中移除分配的审查者 |
| [GitHub 列出 PR 审查者](github/pull_requests.md#github-list-pr-reviewers) | 检索特定 GitHub 拉取请求的所有分配审查者列表 |

## Twitter 集成
| 模块名称 | 描述 |
|------------|-------------|
| [Twitter 发布推文](twitter/twitter.md#twitter-post-tweet-block) | 在 Twitter 上创建包含文本内容和可选附件（包括媒体、投票、引用或深层链接）的推文 |
| [Twitter 删除推文](twitter/twitter.md#twitter-delete-tweet-block) | 使用推文 ID 删除指定推文 |
| [Twitter 搜索最近推文](twitter/twitter.md#twitter-search-recent-tweets-block) | 搜索符合指定条件的推文，并提供过滤和分页选项 |
| [Twitter 获取引用推文](twitter/twitter.md#twitter-get-quote-tweets-block) | 获取引用指定推文 ID 的推文，并提供分页和过滤选项 |
| [Twitter 转发](twitter/twitter.md#twitter-retweet-block) | 使用推文 ID 创建指定推文的转发 |
| [Twitter 移除转发](twitter/twitter.md#twitter-remove-retweet-block) | 移除指定推文的现有转发 |
| [Twitter 获取转发者](twitter/twitter.md#twitter-get-retweeters-block) | 获取转发指定推文的用户列表，并提供分页和过滤选项 |
| [Twitter 获取用户提及](twitter/twitter.md#twitter-get-user-mentions-block) | 获取提及特定用户的推文，使用其用户 ID |
| [Twitter 获取主页时间线](twitter/twitter.md#twitter-get-home-timeline-block) | 获取认证用户及其关注账户的最近推文和转发 |
| [Twitter 获取用户](twitter/twitter.md#twitter-get-user-block) | 获取单个 Twitter 用户的详细资料信息 |
| [Twitter 获取多个用户](twitter/twitter.md#twitter-get-users-block) | 获取多个 Twitter 用户的资料信息（最多 100 个） |
| [Twitter 搜索空间](twitter/twitter.md#twitter-search-spaces-block) | 根据标题关键词搜索 Twitter 空间，并提供状态过滤 |
| [Twitter 获取空间](twitter/twitter.md#twitter-get-spaces-block) | 通过空间 ID 或创建者 ID 获取多个 Twitter 空间的信息 |
| [Twitter 获取空间详情](twitter/twitter.md#twitter-get-space-by-id-block) | 获取单个 Twitter 空间的详细信息 |
| [Twitter 获取空间推文](twitter/twitter.md#twitter-get-space-tweets-block) | 获取在 Twitter 空间会话期间分享的推文 |
| [Twitter 关注列表](twitter/twitter.md#twitter-follow-list-block) | 使用列表 ID 关注 Twitter 列表 |
| [Twitter 取消关注列表](twitter/twitter.md#twitter-unfollow-list-block) | 取消关注之前关注的 Twitter 列表 |
| [Twitter 获取列表](twitter/twitter.md#twitter-get-list-block) | 获取特定 Twitter 列表的详细信息 |
| [Twitter 获取拥有的列表](twitter/twitter.md#twitter-get-owned-lists-block) | 获取指定用户拥有的所有 Twitter 列表 |
| [Twitter 获取列表成员](twitter/twitter.md#twitter-get-list-members-block) | 获取指定 Twitter 列表的成员信息 |
| [Twitter 添加列表成员](twitter/twitter.md#twitter-add-list-member-block) | 将指定用户添加为 Twitter 列表的成员 |
| [Twitter 移除列表成员](twitter/twitter.md#twitter-remove-list-member-block) | 从 Twitter 列表中移除指定用户 |
| [Twitter 获取列表推文](twitter/twitter.md#twitter-get-list-tweets-block) | 获取指定 Twitter 列表中发布的推文 |
| [Twitter 创建列表](twitter/twitter.md#twitter-create-list-block) | 使用指定名称和设置创建新的 Twitter 列表 |
| [Twitter 更新列表](twitter/twitter.md#twitter-update-list-block) | 更新现有 Twitter 列表的名称和/或描述 |
| [Twitter 删除列表](twitter/twitter.md#twitter-delete-list-block) | 删除指定的 Twitter 列表 |
| [Twitter 固定列表](twitter/twitter.md#twitter-pin-list-block) | 将 Twitter 列表固定在列表顶部 |
| [Twitter 取消固定列表](twitter/twitter.md#twitter-unpin-list-block) | 从固定列表中移除 Twitter 列表 |
| [Twitter 获取固定列表](twitter/twitter.md#twitter-get-pinned-lists-block) | 获取当前固定的所有 Twitter 列表 |
| Twitter 列表获取关注者 | 正在开发中... 获取指定 Twitter 列表的所有关注者 |
| Twitter 获取关注的列表 | 正在开发中... 获取用户关注的所有列表 |
| Twitter 获取私信事件 | 正在开发中... 检索用户的私信事件 |
| Twitter 发送私信 | 正在开发中... 向指定用户发送私信 |
| Twitter 创建私信会话 | 正在开发中... 创建新的私信会话 |

## Todoist 集成
| 模块名称 | 描述 |
|------------|-------------|
| [Todoist 创建标签](todoist.md#todoist-create-label) | 在 Todoist 中创建新标签 |
| [Todoist 列出标签](todoist.md#todoist-list-labels) | 从 Todoist 中检索所有个人标签 |
| [Todoist 获取标签](todoist.md#todoist-get-label) | 通过 ID 检索特定标签 |
| [Todoist 创建任务](todoist.md#todoist-create-task) | 在 Todoist 中创建新任务 |
| [Todoist 获取任务](todoist.md#todoist-get-tasks) | 从 Todoist 中检索活动任务 |
| [Todoist 更新任务](todoist.md#todoist-update-task) | 更新现有任务 |
| [Todoist 关闭任务](todoist.md#todoist-close-task) | 完成/关闭任务 |
| [Todoist 重新打开任务](todoist.md#todoist-reopen-task) | 重新打开已完成的任务 |
| [Todoist 删除任务](todoist.md#todoist-delete-task) | 永久删除任务 |
| [Todoist 列出项目](todoist.md#todoist-list-projects) | 从 Todoist 中检索所有项目 |
| [Todoist 创建项目](todoist.md#todoist-create-project) | 在 Todoist 中创建新项目 |
| [Todoist 获取项目](todoist.md#todoist-get-project) | 检索特定项目的详细信息 |
| [Todoist 更新项目](todoist.md#todoist-update-project) | 更新现有项目 |
| [Todoist 删除项目](todoist.md#todoist-delete-project) | 删除项目及其内容 |
| [Todoist 列出协作者](todoist.md#todoist-list-collaborators) | 检索项目上的协作者 |
| [Todoist 列出部分](todoist.md#todoist-list-sections) | 从 Todoist 中检索部分 |
| [Todoist 获取部分](todoist.md#todoist-get-section) | 检索特定部分的详细信息 |
| [Todoist 删除部分](todoist.md#todoist-delete-section) | 删除部分及其任务 |
| [Todoist 创建评论](todoist.md#todoist-create-comment) | 在任务或项目上创建新评论 |
| [Todoist 获取评论](todoist.md#todoist-get-comments) | 检索任务或项目的所有评论 |
| [Todoist 获取评论](todoist.md#todoist-get-comment) | 通过 ID 检索特定评论 |
| [Todoist 更新评论](todoist.md#todoist-update-comment) | 更新现有评论 |
| [Todoist 删除评论](todoist.md#todoist-delete-comment) | 删除评论 |

此完整列表涵盖了 AutoGPT 中所有可用的模块。每个模块都设计用于执行特定任务，并且可以组合起来创建强大的自动化工作流。有关每个模块的更多详细信息，请点击其名称查看完整文档。