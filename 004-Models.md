[模型](https://platform.openai.com/docs/models/models)

[概述](https://platform.openai.com/docs/models/overview)

OpenAI API 由一组能力和价格不同的多样化模型支持。您还可以通过 [微调](https://platform.openai.com/docs/guides/fine-tuning) 对我们的原始基础模型进行有限的定制，以适应您特定的用例。

| 模型                                                         | 模型                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [GPT-4](https://platform.openai.com/docs/models/gpt-4) 有限的测试版 | 一组模型，可以在 GPT-3.5 基础上进行改进，能够理解并生成自然语言或代码。 |
| [GPT-3.5](https://platform.openai.com/docs/models/gpt-3-5)   | OpenAI正在开发一组名为 Codex 的新模型，它们是在 GPT-3 基础上进行改进的。Codex 模型可以生成代码，并具有编码和自然语言理解功能。这些模型正在不断优化中，OpenAI 希望这些模型能够进一步改善自然语言和代码生成方面的性能表现。 |
| [DALL·E](https://platform.openai.com/docs/models/dall-e)Beta | 一个模型，可以根据自然语言提示生成和编辑图像。               |
| [Whisper](https://platform.openai.com/docs/models/whisper)Beta | 一个可以将音频转换为文本的模型。                             |
| [Embeddings](https://platform.openai.com/docs/models/embeddings)（嵌入） | 一组能够将文本转换成数字形式的模型                           |
| [Moderation](https://platform.openai.com/docs/models/moderation)（审查） | 一种经过调整的模型，能够检测文本中可能存在敏感或不安全内容。 |
| [GPT-3](https://platform.openai.com/docs/models/gpt-3)       | 一组可以理解和生成自然语言的模型。                           |
| [Codex](https://platform.openai.com/docs/models/codex)       | 一组可以理解和生成代码，包括将自然语言翻译成代码的模型。     |

我们还发布了一些开源模型，包括[Point-E](https://github.com/openai/point-e)、[Whisper](https://github.com/openai/whisper)、[Jukebox](https://github.com/openai/jukebox)和[CLIP](https://github.com/openai/CLIP)。

请访问我们的[研究人员模型索引](https://platform.openai.com/docs/model-index-for-researchers)以了解更多信息，其中包括我们的研究论文中推荐的模型以及InstructGPT和GPT-3.5等模型系列之间的区别。

[模型端点兼容性](https://platform.openai.com/docs/models/model-endpoint-compatibility)

| 端点          | 型号名称                                                     |
| :------------ | :----------------------------------------------------------- |
| /v1/聊天/完成 | gpt-4, gpt-4-0314, gpt-4-32k, gpt-4-32k-0314, gpt-3.5-turbo, gpt-3.5-turbo-0301 |
| /V1/完成      | text-davinci-003, text-davinci-002, text-curie-001, text-babbage-001, text-ada-001 |
| /v1/编辑      | text-davinci-edit-001, code-davinci-edit-001                 |
| /v1/音频/转录 | 耳语1                                                        |
| /v1/音频/翻译 | 耳语1                                                        |
| /v1/微型      | davinci, curie, babbage, ada                                 |
| /v1/嵌入      | text-embedding-ada-002，文本搜索-ADA-DOC-001                 |
| /V1/适中      | 文本调整稳定，文本 - 修改胶                                  |

这个列表不包含我们的第一代嵌入模型和 DALL·E 模型。关于嵌入模型的更多信息，请参考[这里](https://platform.openai.com/docs/guides/embeddings/similarity-embeddings)；有关 DALL·E 模型的更多信息，请参考[这里](https://platform.openai.com/docs/guides/images/image-generation-beta)。

[连续模型升级](https://platform.openai.com/docs/models/continuul-model-upgrades)

随着“gpt-3.5-turbo”的发布，我们的部分模型正在持续更新。我们还提供静态模型版本，开发者可以在更新模型推出后至少三个月继续使用旧版本。随着模型更新周期的加快，我们还提供了人们可以贡献评估数据的机会，以帮助我们改进模型以适应各种不同的应用场景。如果您感兴趣，请查看[OpenAI Evals](https://github.com/openai/evals)存储库。

以下模型是临时快照，一旦更新版本可用，我们将宣布它们的弃用日期。如果您想使用最新的模型版本，请使用标准模型名称，如“gpt-4”或“gpt-3.5-turbo”。

| 型号名称           | 贬值日期 |
| :----------------- | :------- |
| gpt-3.5-turbo-0301 | TBD      |
| gpt-4-0314         | TBD      |
| gpt-4-32k-0314     | TBD      |



[GPT-4 有限 Beta 版](https://platform.openai.com/docs/models/gpt-4)

GPT-4 是一个大型多模态模型（今天接受文本输入和输出文本，未来将支持图像输入），由于它更广泛的通用知识和先进的推理能力，可以比我们以前的任何模型更准确地解决困难问题。像 `gpt-3.5-turbo` 一样，GPT-4 也针对聊天进行了优化，但是对于使用[聊天完成 API](https://platform.openai.com/docs/api-reference/chat)进行传统的自动完成任务也很有效。在我们的[GPT 指南](https://platform.openai.com/docs/guides/gpt)中了解如何使用 GPT-4。

目前，GPT-4处于有限的 Beta 阶段，只有获得授权的人才能访问。请[加入候补名单](https://openai.com/waitlist/gpt-4)，在有容量可用时获得访问权限。

| 最新模型       | 描述                                                         | 最大标记数             | 训练数据      |
| :------------- | :----------------------------------------------------------- | :--------------------- | :------------ |
| gpt-4          | 比任何 GPT-3.5 模型更具能力，能够执行更复杂的任务，并针对聊天进行优化。将随着我们最新的模型迭代进行更新。 | 8,192个标记（tokens）  | 截至2021年9月 |
| gpt-4-0314     | “GPT-4” 于2023年3月14日的快照。与“GPT-4”不同的是，该模型将不会接收更新，在新版本发布后三个月将被弃用。 | 8,192个标记（tokens）  | 截至2021年9月 |
| gpt-4-32k      | 具有基础“GPT-4”模型相同的能力，但具有4倍的上下文长度。将随着我们最新的模型迭代进行更新。 | 32,768个标记（tokens） | 截至2021年9月 |
| gpt-4-32k-0314 | “GPT-4-32”于2023年3月14日的快照。与“GPT-4-32k”不同的是，该模型将不会接收更新，在新版本发布后三个月将被弃用。 | 32,768 tokens          | 截至2021年9月 |

对于许多基础任务来说，GPT-4和GPT-3.5模型之间的差异不太明显。然而，在更复杂的推理任务中，GPT-4比我们之前的任何模型都要强大得多。

[GPT-3.5](https://platform.openai.com/docs/models/gpt-3-5)

GPT-3.5模型能够理解和生成自然语言或代码。在GPT-3.5系列中，我们最强大、最具成本效益的模型是 `gpt-3.5-turbo`，该模型已经针对聊天进行了优化，但在传统的自动补全任务中也表现良好。

| 最新模型           | 描述                                                         | 最大标记数             | 训练数据      |
| :----------------- | :----------------------------------------------------------- | :--------------------- | :------------ |
| gpt-3.5-turbo      | 最强大的 GPT-3.5 模型，专为聊天进行优化，成本仅为“text-davinci-003”的1/10。将使用我们的最新模型迭代进行更新。 | 4,096 个标记(tokens)   | 截至2021年9月 |
| gpt-3.5-turbo-0301 | 2023年3月1日 `gpt-3.5-turbo` 的快照。与 `gpt-3.5-turbo` 不同的是，这个模型将不会接收更新，而且在新版本发布后的3个月内将被弃用。 | 4,096个标记（tokens）  | 截至2021年9月 |
| text-davinci-003   | 它可以比Curie、Babbage或Ada模型更好地完成任何语言任务，输出质量更高、长度更长，并且具有一致的指令跟踪。此外，它还支持一些额外功能，如[插入文本](https://platform.openai.com/docs/guides/gpt/inserting-text)。 | 4,097 个标记（tokens） | 截至2021年9月 |
| text-davinci-002   | 这个模型和`text-davinci-003`具有类似的功能，但是是通过有监督的微调方式进行训练，而不是强化学习。 | 4,097个标记（tokens）  | 截至2021年9月 |
| code-davinci-002   | 该模型优化了代码补全任务。                                   | 8,001 个标记（tokens） | 截至2021年9月 |

我们建议使用`gpt-3.5-turbo`而不是其他GPT-3.5模型，因为它的成本更低。

OpenAI模型是非确定性的，这意味着相同的输入可能会产生不同的输出。将[temperature](https://platform.openai.com/docs/api-reference/completions/create#completions/create-temperature)设置为0将使输出大多数是确定性的，但可能仍保留小部分的可变性。



[DALL·E Beta](https://platform.openai.com/docs/models/dall-e)

DALL·E是一个能够根据自然语言的描述创造出逼真图像和艺术品的AI系统。我们目前支持根据提示创建指定大小的新图像，编辑现有图像或创建用户提供图像的变体。

我们API中提供的当前DALL·E模型是第二代DALL·E模型，比原始模型具有更逼真、准确和4倍分辨率更高的图像。您可以通过我们的[Labs界面](https://labs.openai.com/)或[API](https://platform.openai.com/docs/guides/images/introduction)进行尝试。



[Whisper Beta](https://platform.openai.com/docs/models/whisper)

Whisper是一个通用的语音识别模型。它在大量的多样性音频数据集上进行训练，并且还是一个多任务模型，可以进行多语言语音识别、语音翻译和语言识别。目前，Whisper v2-large模型可以通过我们的API使用，使用的模型名称是 `whisper-1`。

目前，Whisper的[开源版本](https://github.com/openai/whisper)和我们API可用的版本没有区别。但是，通过我们的API，我们提供了优化的推理过程，使得通过我们的API运行Whisper要比通过其他方式更快。如果想要了解Whisper的更多技术细节，可以[阅读论文](https://arxiv.org/abs/2212.04356)。

[嵌入式](https://platform.openai.com/docs/models/embeddings)

Embeddings是文本的数字表示形式，可以用来衡量两个文本之间的相关性。我们的第二代嵌入模型`text-embedding-ada-002`旨在以大幅降低成本的方式取代之前的16个第一代嵌入模型。Embeddings对于搜索、聚类、推荐、异常检测和分类任务非常有用。您可以在[公告博客文章](https://openai.com/blog/new-and-improved-embedding-model)中了解更多关于我们最新的嵌入模型的详细信息。

[Moderation](https://platform.openai.com/docs/models/moderation)

Moderation模型旨在检查内容是否符合OpenAI的[使用政策](https://openai.com/policies/usage-policies)。该模型提供分类功能，可查找以下类别的内容：仇恨、仇恨/威胁、自残、性别、未成年人涉及的性别问题、暴力和暴力/血腥。您可以在我们的[审查指南](https://platform.openai.com/docs/guides/moderation/overview)中了解更多信息。

Moderation模型接受任意大小的输入，该输入会自动分解以适应模型特定的上下文窗口。

| 模型         | 描述                                           |
| :----------- | :--------------------------------------------- |
| 文本模式最佳 | 最有能力的审核模型。其准确率将略高于稳定模型。 |
| 文本调整稳定 | 几乎和最新模型一样具备能力，但略旧一些。       |



[GPT-3](https://platform.openai.com/docs/models/gpt-3)

GPT-3模型可以理解和生成自然语言。这些模型被更强大的GPT-3.5生成模型所取代。然而，最初的GPT-3基础模型（`davinci`，`curie`，`ada`和`babbage`）目前是唯一可用于微调的模型。

| 最新模型         | 描述                                                         | 最大标记数           | 训练数据       |
| :--------------- | :----------------------------------------------------------- | :------------------- | :------------- |
| text-curie-001   | 非常有能力，比Davinci更快、成本更低。                        | 2,049个标记(tokens)  | 截至2019年10月 |
| text-babbage-001 | 能够处理简单任务，速度非常快，成本更低。                     | 2,049 个标记(tokens) | 截至2019年10月 |
| text-ada-001     | 能够处理非常简单的任务，通常是 GPT-3 系列中速度最快、成本最低的模型。 | 2,049 个标记(tokens) | 截至2019年10月 |
| davinci          | 最具能力的 GPT-3 模型。可以完成其他模型能做的任何任务，通常质量更高。 | 2,049个标记(tokens)  | 截至2019年10月 |
| curie            | 非常具备能力，但比 Davinci 更快速、成本更低。                | 2,049 个标记(tokens) | 截至2019年10月 |
| babbage          | 能够完成简单任务，速度很快，成本较低。                       | 2,049 个标记(tokens) | 截至2019年10月 |
| ada              | 能够完成非常简单的任务，通常是 GPT-3 系列中速度最快、成本最低的模型。 | 2,049个标记(tokens)  | 截至2019年10月 |



[Codex Deprecated](https://platform.openai.com/docs/models/codex)

Codex模型现已被弃用。它们是我们GPT-3模型的后代，可以理解和生成代码。它们的训练数据包含自然语言和来自GitHub的数十亿行公共代码。[了解更多](https://help.openai.com/en/articles/5480054)。

它们在Python方面具有最强的能力，并熟练掌握JavaScript、Go、Perl、PHP、Ruby、Swift、TypeScript、SQL甚至Shell等十多种语言。

以下Codex模型现已被弃用：

| 最新模型         | 描述                                                         | 最大标记数           | 训练数据      |
| :--------------- | :----------------------------------------------------------- | :------------------- | :------------ |
| 代码davinci-002  | 最有能力的法典模型。 特别擅长将自然语言转换为代码。 除了完成代码外，还支持[inserting](https://platform.openai.com/docs/guides/code/inserting-code-code-code-code-code)。 | 8,001个标记(tokens)  | 截至2021年6月 |
| 代码davinci-001  | `code-davinci-002` 的早期版本。                              | 8,001 个标记(tokens) | 截至2021年6月 |
| code-cushman-002 | 几乎与 Davinci Codex 一样强大，但速度稍快。这种速度优势可能使它在实时应用中更受青睐。 | 最多 2,048 个标记    |               |
| code-cushman-001 | `code-cushman-002` 的早期版本。                              | 最多 2,048 个标记    |               |

了解更多信息，请访问我们关于[使用 Codex 的指南](https://platform.openai.com/docs/guides/code)。

[我们如何使用您的数据](https://platform.openai.com/docs/models/how-we-we-use-your-data)

您的数据就是您的数据。

自 2023 年 3 月 1 日起，发送到 OpenAI API 的数据将不会用于训练或改善 OpenAI 模型（除非您明确表示同意 [选择加入该计划](https://docs.google.com/forms/d/e/1FAIpQLSevgtKyiSWIOj6CV6XWBHl1daPZSOcIWzcUYUXQ1xttjBgDpA/viewform)）。

为了帮助识别滥用行为，API 数据可能会保留最多 30 天，之后将被删除（除非法律有其他要求）。对于有敏感应用的可信客户，可能提供零数据保留。零数据保留表示，请求和响应体不会持久化到任何日志机制中，仅存在于内存中以处理请求。

请注意，此数据政策不适用于 OpenAI 的非-API消费者服务，例如 [ChatGPT](https://chat.openai.com/) 或 [DALL·E Labs](https://labs.openai.com/)。

[按终端的默认使用政策](https://platform.openai.com/docs/models/default-usage-policies-by-endpoint)

| 终端            | 用于训练的数据 | 默认存储期限   | 符合零数据保留资格 |
| :-------------- | :------------- | :------------- | :----------------- |
| /V1/完成        | 否             | 30天           | 是                 |
| /v1/聊天/完成   | 否             | 30天           | 是                 |
| /v1/编辑        | 否             | 30天           | 是                 |
| /v1/图片/几代人 | 否             | 30天           | 否                 |
| /v1/图片/编辑   | 否             | 30天           | 否                 |
| /v1/图像/变化   | 否             | 30天           | 否                 |
| /v1/嵌入        | 否             | 30天           | 是                 |
| /v1/音频/转录   | 否             | 零数据保留     | -                  |
| /v1/音频/翻译   | 否             | 零数据保留     | -                  |
| /v1/文件        | 否             | 直到被客户删除 | 否                 |
| /v1/微调        | 否             | 直到被客户删除 | 否                 |
| /v1/适中        | 否             | 零数据保留     | -                  |

请参阅我们的[API数据使用政策](https://openai.com/policies/api-data-usage-policies)了解详情。