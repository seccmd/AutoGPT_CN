## 🔍 Google API 密钥配置

!!! 注意
    此部分为可选内容。若搜索尝试返回错误429，请使用官方Google API。要使用`google`命令，您需要在环境变量中设置Google API密钥，或通过配置传递给[`WebSearchComponent`](../../forge/components/built-in-components.md)。

创建您的项目：

1. 访问[Google Cloud Console](https://console.cloud.google.com/)。
1. 如果尚未拥有账户，请先创建并登录。
1. 点击页面顶部的*选择项目*下拉菜单，然后点击*新建项目*来创建新项目。
1. 为项目命名后点击*创建*。
1. 设置自定义搜索API并添加到.env文件：
    1. 前往[APIs & Services仪表板](https://console.cloud.google.com/apis/dashboard)。
    1. 点击*启用API和服务*。
    1. 搜索*Custom Search API*并点击它。
    1. 点击*启用*。
    1. 转到[凭据](https://console.cloud.google.com/apis/credentials)页面。
    1. 点击*创建凭据*。
    1. 选择*API密钥*。
    1. 复制API密钥。
    1. 将其设置为`.env`文件中的`GOOGLE_API_KEY`。
1. [启用](https://console.developers.google.com/apis/api/customsearch.googleapis.com)项目上的Custom Search API（可能需要等待几分钟以生效）。设置自定义搜索引擎并添加到.env文件：
    1. 访问[自定义搜索引擎](https://cse.google.com/cse/all)页面。
    1. 点击*添加*。
    1. 按照提示设置您的搜索引擎，可以选择搜索整个网络或特定网站。
    1. 创建搜索引擎后，点击*控制面板*。
    1. 点击*基础信息*。
    1. 复制*搜索引擎ID*。
    1. 将其设置为`.env`文件中的`CUSTOM_SEARCH_ENGINE_ID`。

_请记住，您的免费每日自定义搜索配额仅限100次搜索。要增加此限制，需为项目分配一个计费账户，从而享受每日最多10,000次搜索的权益。_