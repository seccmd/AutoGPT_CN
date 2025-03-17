# 文本转语音

输入以下命令以使用AutoGPT的TTS（文本转语音）功能：

```shell
./autogpt.sh --speak
```

Eleven Labs提供语音技术，如语音设计、语音合成和预制语音，AutoGPT可以利用这些技术进行语音输出。

1. 访问[ElevenLabs](https://beta.elevenlabs.io/)，如果还没有账户，请先注册。
2. 选择并设置*Starter*计划。
3. 点击右上角的图标，找到*Profile*以获取您的API密钥。

在`.env`文件中设置：

- `ELEVENLABS_API_KEY`
- `ELEVENLABS_VOICE_1_ID`（例如：_"premade/Adam"_）

### 可用语音列表

!!! 注意
    您可以使用名称或语音ID来配置语音

| 名称   | 语音 ID |
| ------ | -------- |
| Rachel | `21m00Tcm4TlvDq8ikWAM` |
| Domi   | `AZnzlk1XvdvUeBnXmlld` |
| Bella  | `EXAVITQu4vr4xnSDxMaL` |
| Antoni | `ErXwobaYiN019PkySvjV` |
| Elli   | `MF3mGyEYCl7XYWbV9V6O` |
| Josh   | `TxGEqnHWrfWFTfGW9XjX` |
| Arnold | `VR6AewLTigWG4xSOukaG` |
| Adam   | `pNInz6obpgDQGcFmaJgB` |
| Sam    | `yoZ06aMxZJJ28mfd3POQ` |