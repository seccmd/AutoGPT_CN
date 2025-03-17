# 配置

敏感设置（如API凭证）的配置通过环境变量完成。
你可以通过 `.env` 文件设置配置变量。如果没有 `.env` 文件，可以在 `AutoGPT` 文件夹中复制 `.env.template` 并将其命名为 `.env`。

## 环境变量

- `AUTHORISE_COMMAND_KEY`: 授权命令时接受的按键响应。默认值：y
- `ANTHROPIC_API_KEY`: 如果要在AutoGPT中使用Anthropic模型，请设置此变量
- `AZURE_CONFIG_FILE`: Azure配置文件相对于AutoGPT根目录的位置。默认值：azure.yaml
- `COMPONENT_CONFIG_FILE`: 代理的组件配置文件路径（json）。可选
- `DISABLED_COMMANDS`: 要禁用的命令。使用逗号分隔的命令名称。请参阅[此处](../../forge/components/components.md)内置组件的命令列表。默认值：无
- `ELEVENLABS_API_KEY`: ElevenLabs API密钥。可选。
- `ELEVENLABS_VOICE_ID`: ElevenLabs语音ID。可选。
- `EMBEDDING_MODEL`: 用于嵌入任务的LLM模型。默认值：`text-embedding-3-small`
- `EXIT_KEY`: 退出时接受的按键。默认值：n
- `FAST_LLM`: 用于大多数任务的LLM模型。默认值：`gpt-3.5-turbo-0125`
- `GITHUB_API_KEY`: [Github API密钥](https://github.com/settings/tokens)。可选。
- `GITHUB_USERNAME`: GitHub用户名。可选。
- `GOOGLE_API_KEY`: Google API密钥。可选。
- `GOOGLE_CUSTOM_SEARCH_ENGINE_ID`: [Google自定义搜索引擎ID](https://programmablesearchengine.google.com/controlpanel/all)。可选。
- `GROQ_API_KEY`: 如果要在AutoGPT中使用Groq模型，请设置此变量
- `HUGGINGFACE_API_TOKEN`: HuggingFace API，用于图像生成和音频转文本。可选。
- `HUGGINGFACE_IMAGE_MODEL`: 用于图像生成的HuggingFace模型。默认值：CompVis/stable-diffusion-v1-4
- `LLAMAFILE_API_BASE`: Llamafile API基础URL。默认值：`http://localhost:8080/v1`
- `OPENAI_API_KEY`: 如果要在AutoGPT中使用OpenAI模型，请设置此变量；[OpenAI API密钥](https://platform.openai.com/account/api-keys)。
- `OPENAI_ORGANIZATION`: OpenAI中的组织ID。可选。
- `PLAIN_OUTPUT`: 纯文本输出，禁用加载动画。默认值：False
- `RESTRICT_TO_WORKSPACE`: 将文件读写限制在工作区目录内。默认值：True
- `SD_WEBUI_AUTH`: Stable Diffusion Web UI用户名:密码对。可选。
- `SMART_LLM`: 用于“智能”任务的LLM模型。默认值：`gpt-4-turbo-preview`
- `STREAMELEMENTS_VOICE`: 使用的StreamElements语音。默认值：Brian
- `TEMPERATURE`: 提供给OpenAI的温度值。范围从0到2。较低的值更确定，较高的值更随机。参见 https://platform.openai.com/docs/api-reference/completions/create#completions/create-temperature
- `TEXT_TO_SPEECH_PROVIDER`: 文本转语音提供商。选项为 `gtts`、`macos`、`elevenlabs` 和 `streamelements`。默认值：gtts
- `USE_AZURE`: 使用Azure的LLM。默认值：False