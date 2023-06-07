[简介](https://platform.openai.com/docs/introduction/introduction)

[概述](https://platform.openai.com/docs/introduction/overview)

寻找 ChatGPT？请前往[chat.openai.com](https://chat.openai.com/)。

OpenAI API 可应用于几乎任何需要理解或生成自然语言和代码的任务。OpenAI API 也可用于生成和编辑图像或将语音转换为文本。我们提供不同能力和价格的[模型](https://platform.openai.com/docs/models)，以及调整[微调](https://platform.openai.com/docs/guides/fine-tuning)自定义模型的能力。

[资源](https://platform.openai.com/docs/introduction/resources)

- 在[playground](https://platform.openai.com/playground?mode=chat)里进行实验
- 阅读[API 参考文档](https://platform.openai.com/docs/api-reference)
- 访问[帮助中心](https://help.openai.com/en/)
- 查看当前 API 的[状态](https://status.openai.com/)
- 浏览[OpenAI 开发者论坛](https://community.openai.com/)
- 了解我们的[使用政策](https://openai.com/policies/usage-policies)

[关键概念](https://platform.openai.com/docs/introduction/key-concepts)

[GPTs](https://platform.openai.com/docs/introduction/gpts)

OpenAI 的 GPT(生成型预训练 transformer) 模型已经被训练好去理解自然语言和代码。 GPTs会在输入后提供文本输出。 GPTs的输入也被称为 "prompt"(提示)。设计一个 prompt 本质上就是如何“编程”一个 GPT 模型，通常是提供说明或一些关于成功完成任务的例子。GPTs可用于各种任务，包括内容或代码生成，摘要，对话，创意写作等等。阅读我们的入门 [GPT 指南](https://platform.openai.com/docs/guides/gpt) 和 [GPT 最佳实践指南](https://platform.openai.com/docs/guides/gpt-best-practices) 以了解更多。

[嵌入式](https://platform.openai.com/docs/introduction/embeddings)

嵌入(embedding)是数据(如一些文本)的向量表示，它旨在保留其内容和/或意义的方面。在某种程度上相似的数据块将有比不相干的数据更接近的嵌入向量。 OpenAI 提供文本嵌入模型，这些模型以文本字符串作为输入，并生成嵌入向量作为输出。嵌入向量可用于搜索、聚类、推荐、异常检测、分类等领域。在我们的[嵌入指南](https://platform.openai.com/docs/guides/embeddings)中了解更多关于嵌入的内容。

[标记](https://platform.openai.com/docs/introduction/tokens)

GPT和嵌入模型处理文本的段落被称为标记(tokens)。Tokens代表常见的字符序列。例如，字符串“tokenization”被分解为“token”和“ization”，而像“the”这样的短单词被表示为单个 token。请注意，在一个句子中，每个单词的第一个标记通常以一个空格字符开始。查看我们的[分词器工具](https://platform.openai.com/tokenizer)来测试特定的字符串，并查看它们如何被转换为 tokens。粗略的经验法则是，对于英文文本，1个标记约为4个字符或0.75个单词。

需要记住的一个限制是，对于GPT模型，提示和生成的输出的总长度不能超过模型的最大上下文长度。对于嵌入模型（不输出标记），输入长度必须小于模型的最大上下文长度。每个GPT和嵌入模型的最大上下文长度可以在[模型索引](https://platform.openai.com/docs/models/overview)中找到。



[指南](https://platform.openai.com/docs/introduction/guides)

跳转到我们的指南之一以了解更多信息。

[快速入门教程](https://platform.openai.com/docs/quickstart)
通过构建快速示例应用程序进行学习

[GPT](https://platform.openai.com/docs/guides/gpt)
学习如何生成文本

[GPT最佳实践](https://platform.openai.com/docs/guides/gpt-best-practices)
了解使用 GPT 模型构建的最佳实践

[嵌入](https://platform.openai.com/docs/guides/guides/embeddings)
了解如何搜索、分类和比较文本

[语音转文本Beta](https://platform.openai.com/docs/guides/guides/speech-to-to-toxt)
了解如何将语音转换为文本

[图像生成Beta](https://platform.openai.com/docs/guides/images)
了解如何生成或编辑图像

[微调](https://platform.openai.com/docs/guides/guides/fine-tuning)
了解如何为您的用例训练模型













