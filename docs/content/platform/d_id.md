# 查找D-ID可用的语音

1. **ElevenLabs**
   - 从语音列表中选择任意语音：https://api.elevenlabs.io/v1/voices
   - 复制voice_id
   - 将其作为字符串用于CreateTalkingAvatarClip模块中的voice_id字段

2. **Microsoft Azure语音**
   - 从语音库中选择任意语音：https://speech.microsoft.com/portal/voicegallery
   - 点击右侧的“示例代码”标签
   - 复制语音名称，例如：config.SpeechSynthesisVoiceName ="en-GB-AbbiNeural"
   - 在CreateTalkingAvatarClip模块的voice_id字段中使用此字符串en-GB-AbbiNeural

3. **Amazon Polly语音**
   - 从语音列表中选择任意语音：https://docs.aws.amazon.com/polly/latest/dg/available-voices.html
   - 复制语音名称/ID
   - 将其作为字符串用于CreateTalkingAvatarClip模块中的voice_id字段