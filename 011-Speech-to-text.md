[语音转文字 Beta版](https://platform.openai.com/docs/guides/speech-to-text/speech-to-text-beta)

学习如何将音频转换为文本。

[介绍](https://platform.openai.com/docs/guides/speech-to-text/introduction)

语音转文本 API 提供了两个端点，`transcriptions` 和 `translations`，基于我们最先进的开源 large-v2[Whisper 模型](https://openai.com/blog/whisper/)。它们可以用于：

- 将音频转录成音频所在的任何语言。
- 将音频翻译并转录成英语。

文件上传目前仅限于25 MB，并支持以下输入文件类型：`mp3`、`mp4`、`mpeg`、`mpga`、`m4a`、`wav` 和 `webm`。

[快速入门](https://platform.openai.com/docs/guides/speech-to-text/quickstart)

[转录](https://platform.openai.com/docs/guides/speech-to-text/transcriptions)

转录 API 的输入为要转录的音频文件和所需的音频转录输出文件格式。我们目前支持多种输入和输出文件格式。

转录音频

Python

复制

```python
# Note: you need to be using OpenAI Python v0.27.0 for the code below to work
import openai
audio_file= open("/path/to/file/audio.mp3", "rb")
transcript = openai.Audio.transcribe("whisper-1", audio_file)
```

默认情况下，响应类型将为包含原始文本的 json。

{ "text": "想象一下你曾经拥有的最疯狂的想法，你想知道它如何扩展到比原来大100倍，1000倍的规模。...}

要在请求中设置其他参数，您可以添加更多带有相关选项的 `--form` 行。例如，如果您想将输出格式设置为文本，则需要添加以下行：

```bash
...
--form file=@openai.mp3 \
--form model=whisper-1 \
--form response_format=text
```

[翻译](https://platform.openai.com/docs/guides/speech-to-text/translations)

翻译 API 以任何支持的语言作为语音文件的输入，如果必要，将其转录成英语。这与我们的 /Transcriptions 端点不同，因为输出不是原始输入语言，而是被翻译成英文文本。

翻译音频

Python

复制

```python
# Note: you need to be using OpenAI Python v0.27.0 for the code below to work
import openai
audio_file= open("/path/to/file/german.mp3", "rb")
transcript = openai.Audio.translate("whisper-1", audio_file)
```

在这种情况下，输入的音频是德语，输出的文本如下：

你好，我的名字是 Wolfgang，我来自德国。你今天要去哪里？

我们目前仅支持翻译成英语。

[支持的语言](https://platform.openai.com/docs/guides/speech-to-text/supported-languages)

我们目前通过 `transcriptions` 和 `translations` 端点支持以下语言：

南非荷兰语、阿拉伯语、亚美尼亚语、阿塞拜疆语、白俄罗斯语、波斯尼亚语、保加利亚语、加泰罗尼亚语、中文、克罗地亚语、捷克语、丹麦语、荷兰语、英语、爱沙尼亚语、芬兰语、法语、加利西亚语、德语、希腊语、希伯来语、印地语、匈牙利语、冰岛语、印度尼西亚语、意大利语、日语、卡纳达语、哈萨克语、韩语、拉脱维亚语、立陶宛语、马其顿语、马来语、马拉地语、毛利语、尼泊尔语、挪威语、波斯语、波兰语、葡萄牙语、罗马尼亚语、俄语、塞尔维亚语、斯洛伐克语、斯洛文尼亚语、西班牙语、斯瓦希里语、瑞典语、他加禄语、泰米尔语、泰语、土耳其语、乌克兰语、乌尔都语、越南语和威尔士语。

虽然底层模型是在 98 种语言上训练的，但我们只列出了超过 50% [词错误率](https://en.wikipedia.org/wiki/Word_error_rate)（WER）的语言，这是语音到文本模型准确性的行业标准基准。该模型将返回上面未列出的语言的结果，但质量会较低。

[更长的音频输入](https://platform.openai.com/docs/guides/speech-to-text/longer-inputs)

默认情况下，Whisper API 仅支持小于 25 MB 的文件。如果您有一个长于此的音频文件，您需要将其分成小于或等于 25 MB 的块，或使用压缩的音频格式。为了获得最佳性能，我们建议您避免在句子中间分割音频，因为这可能会导致一些上下文丢失。

处理这种情况的一种方法是使用 [PyDub 开源 Python 包](https://github.com/jiaaro/pydub)来分割音频：



```python
from pydub import AudioSegment

song = AudioSegment.from_mp3("good_morning.mp3")

# PyDub handles time in milliseconds
ten_minutes = 10 * 60 * 1000

first_10_minutes = song[:ten_minutes]

first_10_minutes.export("good_morning_10.mp3", format="mp3")
```

1. *OpenAI 对诸如 PyDub 等第三方软件的可用性或安全性不作任何保证。*

   [提示](https://platform.openai.com/docs/guides/speech-to-text/prompting)

   您可以使用 [提示](https://platform.openai.com/docs/api-reference/audio/create#audio/create-prompt) 来改善 Whisper API 生成的转录质量。模型会尝试匹配提示的风格，因此如果提示使用了大写字母和标点符号，它更有可能使用这些来生成转录。然而，当前的提示系统比我们的其他语言模型受限得多，仅提供对生成的音频有限的控制。以下是提示在不同场景下如何有助于改善转录的一些示例：

   1. 对于模型经常在音频中错误识别的特定单词或首字母缩写，提示可能非常有帮助。例如，下面的提示改进了 DALL·E 和 GPT-3 这些词的转录，这些词以前被写作 "GDP 3" 和 "DALI"。

      > 这段转录是关于 OpenAI 的，他们开发了像 DALL·E、GPT-3 和 ChatGPT 这样的技术，希望有朝一日构建一个让全人类受益的 AGI 系统。

   2. 为了保留被分成段落的文件的上下文，您可以使用前一段的转录提示模型。这会使转录更加准确，因为模型会使用先前音频中的相关信息。模型仅考虑提示的最后 224 个 token，并忽略任何早期的内容。

   3. 有时模型会在转录中省略标点符号。您可以通过使用包含标点符号的简单提示来避免这种情况：

      > 你好，欢迎来听我的演讲。

   4. 在音频中，模型还可能忽略常用的填充词。如果您想在转录中保留填充词，您可以使用包含这些词的提示：

      > 嗯，让我想想，像，嗯……好的，这是我在思考的事情。

   5. 有些语言可以用不同的方式书写，例如简体或繁体中文。模型可能不会默认使用您想要的写作风格来进行转录。使用您想要的写作风格的提示可以改善这一点。