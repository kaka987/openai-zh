[GPT 最佳实践](https://platform.openai.com/docs/guides/gpt-best-practices/gpt-best-practices)

这个指南分享了从 GPT 中获得更好结果的策略和方法。这里描述的方法有时可以结合使用以达到更好的效果。我们鼓励尝试实验，找到最适合你的方法。

这里展示的一些例子目前只适用于我们最优秀的模型`gpt-4`。如果你还没有拥有`gpt-4`的权限，请考虑加入[等候列表](https://openai.com/waitlist/gpt-4-api)。通常，如果你发现一个 GPT 模型无法完成任务并且存在一个更优秀的模型，那么用更优秀的模型再试一试通常是有帮助的。

[获取更好结果的六种策略](https://platform.openai.com/docs/guides/gpt-best-practices/six-strategies-for-getting-better-results)

[编写清晰明确的指令](https://platform.openai.com/docs/guides/gpt-best-practices/write-clear-instructions)。

GPT 无法读取你的思想。如果输出过长，请要求简短回复。如果输出过于简单，请要求专家级的写作。如果你不喜欢格式，请演示你想要看到的格式。GPT 猜测你想要的越少，你得到它的可能性就越大。

策略：

- [在查询中包含详细信息以获得更相关的答案](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-include-details-in-your-query-to-get-more-relevant-answers)
- [要求模型采用一个人物角色](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-ask-the-model-to-adopt-a-persona)
- [使用分隔符清楚地指示输入的不同部分](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-delimiters-to-clearly-indicate-distinct-parts-of-the-input)
- [指定完成任务所需的步骤](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-specify-the-steps-required-to-complete-a-task)
- [提供示例](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-provide-examples)
- [指定期望的输出长度](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-specify-the-desired-length-of-the-output)

[提供参考文本](https://platform.openai.com/docs/guides/gpt-best-practices/provide-reference-text)

GPTs 可以自信地发明假答案，特别是当被问及深奥的话题或引用文献和网址时。就像一张笔记纸可以帮助学生在考试中表现更好一样，向 GPTs 提供参考文本可以帮助它们在回答时减少虚构。

策略：

- [指示模型使用参考文本回答](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-instruct-the-model-to-answer-using-a-reference-text)
- [指示模型使用参考文本的引用文献进行回答](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-instruct-the-model-to-answer-with-citations-from-a-reference-text)

[将复杂的任务拆分为更简单的子任务](https://platform.openai.com/docs/guides/gpt-best-practices/split-complex-tasks-into-simpler-subtasks)

和软件工程中将复杂系统分解为一组模块化组件一样，将任务分解为一组简单的子任务对于提交给 GPTs 也同样适用。复杂任务的错误率通常比简单任务高。此外，复杂任务往往可以被重新定义为一系列简单任务的工作流，其中早期任务的输出被用于构建后续任务的输入。

策略：

- [使用意图分类来识别用户查询的最相关指令](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-intent-classification-to-identify-the-most-relevant-instructions-for-a-user-query)
- [对于需要非常长时间的对话应用程序，利用摘要或筛选前一个对话](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-for-dialogue-applications-that-require-very-long-conversations-summarize-or-filter-previous-dialogue)
- [将长文档逐段进行总结，递归地构建完整的摘要](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-summarize-long-documents-piecewise-and-construct-a-full-summary-recursively)

[给 GPTs 时间“思考”](https://platform.openai.com/docs/guides/gpt-best-practices/give-gpts-time-to-think)

如果让你计算 17 乘以 28，你可能不会立即知道答案，但是可以用时间来解决。同样地，当试图立即回答时，GPTs 在推理中会出现更多错误，而不是花费时间来得出答案。在得出答案之前要求给出推理链可以帮助 GPTs 更可靠地推理出正确答案。

策略：

- [指示模型先计算出自己的解决方案，再匆忙得出结论](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-instruct-the-model-to-work-out-its-own-solution-before-rushing-to-a-conclusion)
- [使用内心独白或一系列查询来隐藏模型的推理过程](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-inner-monologue-or-a-sequence-of-queries-to-hide-the-model-s-reasoning-process)
- [询问模型在先前的处理中是否漏掉了什么内容](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-ask-the-model-if-it-missed-anything-on-previous-passes)

[使用外部工具](https://platform.openai.com/docs/guides/gpt-best-practices/use-external-tools)

通过将其他工具的输出提供给 GPTs 来弥补它们的弱点。例如，文本检索系统可以告诉 GPTs 相关的文档信息。代码执行引擎可以帮助 GPTs 进行数学计算和运行代码。如果一个任务可以通过一个工具而不是 GPT 更可靠或更高效地完成，那么将其卸载以获得最佳效果。

策略：

- [使用基于嵌入的搜索来实现高效的知识检索](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-embeddings-based-search-to-implement-efficient-knowledge-retrieval)
- [使用代码执行来进行更准确的计算或调用外部 API](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-code-execution-to-perform-more-accurate-calculations-or-call-external-apis)

[系统地测试更改](https://platform.openai.com/docs/guides/gpt-best-practices/test-changes-systematically)

如果能够进行测量，就更容易提高性能。在某些情况下，对提示的修改将在一些孤立的示例上实现更好的性能，但会导致更具代表性的示例集上的整体性能更差。因此，为了确保更改对性能的影响是净积极的，可能需要定义一个全面的测试套件（也称为“评估”）。

策略：

- [将模型输出与标准答案进行比较来评估模型的输出](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-evaluate-model-outputs-with-reference-to-gold-standard-answers)

[策略](https://platform.openai.com/docs/guides/gpt-best-practices/tactics)

上述每个策略都可以通过特定的策略实现。这些策略旨在提供尝试的想法。它们绝不是完全全面的，您应该随意尝试这里未提到的创造性想法。

[策略：编写清晰的说明](https://platform.openai.com/docs/guides/gpt-best-practices/strategy-write-clear-instructions)

[策略：在查询中包含更多细节以获得更相关的答案](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-include-details-in-your-query-to-get-more-relevant-answers)

为了获得高度相关的响应，请确保请求提供任何重要的细节或上下文。否则，您就让模型来猜测您的意思。

|                              |                                                              |
| :--------------------------- | :----------------------------------------------------------- |
| **更差**                     | **更好的**                                                   |
| 我如何在Excel中添加数字？    | 如何在Excel中加一排美元？ 我想对整个行自动执行此操作，所有总数最终都在右侧，称为“ Total”。 |
| 谁是总统？                   | 2021年墨西哥的总统是谁，选举的频率是多少？                   |
| 编写代码以计算斐波那契数列。 | 编写打字稿函数以有效计算斐波那契序列。 自由地对代码发表评论，以解释每件作品的作用以及为何以这种方式编写。 |
| 总结会议笔记。               | 在一段中总结会议笔记。 然后写下扬声器及其每个要点的降价列表。 最后，列出说话者建议的下一步或操作项（如果有）。 |

[策略：要求模型采用角色](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-ask-ask-th-the-model-to-model-to-adopt-a-persona)

系统消息可用于指定模型在其回复中所使用的角色扮演。

系统

当我请求写作帮助时，你会回复一个包含每个段落中至少一个笑话或幽默评论的文档。

用户

写一封感谢信给我的钢栓供应商，感谢他们在短时间内准时交付了订单，这使我们能够按期交付重要的订单。

[在Playground上打开](https://platform.openai.com/playground/p/default-playful-playful-thank-you-note)

[策略：使用分隔符明确指示输入的不同部分。](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-use-dactic-use-delimiters-to-clearly-indicate-indicate-indicate-indicate-disticate-distinctic-distinctinct-parts)

分隔符，如三重引号、XML标记、章节标题等等，可以帮助区分需要以不同方式处理的文本部分。

用户

用俳句概括由三重引号分隔的文本。"""insert text here"""

[在Playground上打开](https://platform.openai.com/playground/p/default-delimiters-1)

系统

您将会被提供一对有关同一主题的文章（由 XML 标签界定）。首先总结每篇文章的论点。然后指出哪篇文章的论点更为有力，以及为什么。

用户

<article> insert first article here </article> <article> insert second article here </article>

[在Playground上打开](https://platform.openai.com/playground/p/default-delimiters-2)

系统

您将会被提供一篇论文摘要和一份建议的标题。论文标题应该让读者了解论文的主题，同时还要引人注目。如果题目不符合这些标准，请提出 5 个替代方案。

用户

摘要：在这里插入摘要 标题：在这里插入标题

[在 Playground 中打开](https://platform.openai.com/playground/p/default-delimiters-3)

对于像这样的简单任务，使用分隔符可能不会对输出质量产生影响。但是，任务越复杂，消除歧义就越重要。不要让 GPT 忙于理解您究竟在要求什么。

[策略：指定完成任务所需的步骤](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-specify-the-steps-required-to-complete-a-task)

有些任务最好被指定为一系列步骤。明确编写这些步骤可以使模型更容易地遵循它们。

系统：
请使用以下逐步说明来回答用户的输入。

步骤 1 - 用户将用三引号提供给您一段文本。用一个前缀“Summary: ”的句子来总结这段文本。

步骤 2 - 将第一步总结翻译成西班牙语，并用一个前缀"Translation: "来表示。

用户：
"""insert text here"""

[在 Playground 中打开](https://platform.openai.com/playground/p/default-step-by-step-summarize-and-translate)

[策略：提供实例](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-provide-examples)

提供适用于所有示例的通用说明通常比通过示例展示所有任务的所有可能分支更有效，但在某些情况下，提供示例可能更容易。例如，如果您打算让模型复制一种难以明确描述的特定响应用户查询的方式。这被称为“少样本”提示。

系统：
请保持一致的风格回答。

用户：
教我关于耐心。

助手：
雕刻最深谷的河流源自一股谦虚的泉水；最壮观的交响乐始于一枚孤独的音符；最复杂的挂毯始自一根孤寂的线。

用户：
教我关于海洋。

[在 Playground 中打开](https://platform.openai.com/playground/p/default-chat-few-shot)

[策略：指定所需输出长度](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-specify-the-desired-length-of-the-output)

您可以让模型生成给定目标长度的输出。目标输出长度可以根据单词数、句子数、段落数、要点符号等来指定。请注意，指示模型生成特定数量的单词并不具有高精度。模型可以更可靠地生成具有特定段落数或要点符号的输出。

用户

请在大约 50 个单词中总结三个引号括起来的文本。"""insert text here"""

[在 Playground 中打开](https://platform.openai.com/playground/p/default-summarize-text-50-words)

用户

在两个段落中总结三个引号括起来的文本。"""insert text here"""

[在 Playground 中打开](https://platform.openai.com/playground/p/default-summarize-text-2-paragraphs)

用户

在三个要点符号中概括三个引号引起来的文本。"""insert text here"""

[在 Playground 中打开](https://platform.openai.com/playground/p/default-summarize-text-3-bullet-points)

[策略：提供参考文本](https://platform.openai.com/docs/guides/gpt-best-practices/strategy-provide-reference-text)

[策略：指示模型使用参考文本回答](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-instruct-the-model-to-answer-using-a-reference-text)

如果我们能够向模型提供与当前查询相关的可信信息，那么我们可以指导模型使用提供的信息来组成其答案。

系统

使用三个引号包含的提供的文章来回答问题。如果在文章中找不到答案，请写“找不到答案”。

用户

<在此处插入由三个引号包含的文章，每篇文章之间用换行符分隔> 问题：<在此处插入问题>

[在 Playground 中打开](https://platform.openai.com/playground/p/default-answer-from-retrieved-documents)

鉴于 GPT 的上下文窗口是有限的，为了应用这种策略，我们需要一种动态查找与所询问的问题相关的信息的方法。[嵌入](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)可以用来实现高效的知识检索。有关如何实现此操作的更多详细信息，请参见策略["使用基于嵌入的搜索来实现高效的知识检索"](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-embeddings-based-search-to-implement-efficient-knowledge-retrieval)。

[策略：指示模型引用参考文本中的引文回答](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-instruct-the-model-to-answer-with-citations-from-a-reference-text)

如果输入已经补充了相关的知识，那么可以直接要求模型通过引用所提供文档中的段落来给其答案加上引用。请注意，输出中的引文可以通过在所提供的文档中进行字符串匹配来进行程序化的验证。

系统

您将会用三个引号括起来的文档以及一个问题。您的任务是仅仅使用所提供的文档来回答问题，并引用用于回答问题的文档段落。如果文档中没有包含回答该问题所需的信息，则简单写上：“信息不足”。如果提供了问题的答案，则必须使用引文进行注释。使用以下格式引用相关段落（{"citation": …}）。

用户

"""<insert document here>"""

 问题：<insert question here>

[在 Playground 中打开](https://platform.openai.com/playground/p/default-answer-with-citation)

[策略：将复杂任务分解为更简单的子任务](https://platform.openai.com/docs/guides/gpt-best-practices/strategy-split-complex-tasks-into-simpler-subtasks)

[策略：使用意图分类来识别最相关的用户查询指令](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-intent-classification-to-identify-the-most-relevant-instructions-for-a-user-query)

对于那些需要处理不同情况的大量独立指令集的任务，首先对查询类型进行分类并使用该分类来确定需要哪些指令可能是有益的。 这可以通过定义固定类别和对于处理给定类别任务相关的指令进行硬编码来实现。 这个过程也可以递归地应用于将一个任务分解为一系列阶段。 这种方法的优点是每个查询仅包含执行任务下一阶段所需的指令，相比使用单个查询执行整个任务，这可能会导致更低的错误率。 这也可能会导致更低的成本，因为较大的提示费用更高（[请参阅定价信息](https://openai.com/pricing)）。

例如，假设针对客户服务应用程序，可将查询有用地分类如下：

系统

您将获得客户服务查询。将每个查询分类为主类别和次要类别。以json格式提供您的输出，其中包含的键为：primary 和 secondary。

主要类别：计费、技术支持、账户管理或一般查询。

计费的次要类别：
- 取消订阅或升级
- 添加付款方式
- 解释收费
- 对费用提出异议

技术支持的次要类别：
- 故障排除
- 设备兼容性
- 软件更新

账户管理的次要类别：
- 重置密码
- 更新个人信息
- 关闭帐户
- 帐户安全

一般查询的次要类别：
- 产品信息
- 定价
- 反馈
- 联系人工客服

用户：
我需要让我的互联网再次工作。

[在 Playground 中打开](https://platform.openai.com/playground/p/default-decomposition-by-intent-classification-1)

根据客户查询的分类，可以向 GPT 模型提供一组更具体的指令，以处理下一步。例如，假设客户需要帮助“故障排除”。

系统：

您将收到需要在技术支持环境中进行故障排除的客户服务查询。帮助客户：

- 要求他们检查所有与路由器相连的电缆是否连接。请注意，电缆随着时间的推移常常会变得松动。
- 如果所有电缆都连接并且问题仍然存在，请询问他们使用的路由器型号。
- 现在，您将向他们建议如何重新启动设备：

  如果型号为MTD-327J，则建议他们按下红色按钮并将其保持5秒钟，然后等待5分钟以测试连接。

  如果型号为MTD-327S，则建议他们将其拔出并重新插入，然后在测试连接之前等待5分钟。
- 如果经过重新启动设备和等待5分钟后客户的问题仍然存在，请输出 {"IT support requested"} 与 IT 支持联系。
- 如果用户开始提出与该主题无关的问题，请确认是否要结束有关故障排除的当前聊天，并按以下方案分类其请求：

<将上面的主要/次要分类方案插入此处>

用户：

我需要让我的互联网再次工作。

[在Playground中](https://platform.openai.com/playground/p/default-decomposition-by-intent-classification-2)

请注意，模型已被指示发出特殊字符串以指示对话状态何时发生变化。这使我们能够将我们的系统转变为状态机，其中状态决定了注入哪些指令。通过跟踪状态、了解在该状态下哪些指令是相关的以及可选地允许从该状态转换到哪些状态，我们可以在用户体验周围设置保护栏，这在采用较少结构化方法时很难实现。

[策略：对于需要非常长的对话应用程序，请总结或过滤以前的对话](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-for-dialogue-applications-that-require-very-long-conversations-summarize-or-filter-previous-dialogue)

由于 GPT 具有固定的上下文长度，因此在用户和助手之间的对话中，整个对话包含在上下文窗口中的情况无法无限期地继续。

有各种解决这个问题的方法，其中之一是总结对话中的先前转换。一旦输入的大小达到预定的阈值长度，这可能会触发查询，总结对话的一部分，并将以前的对话总结作为系统消息的一部分包含在其中。或者，先前的对话可以在整个对话期间异步地在后台进行总结。

另一种替代解决方案是动态地选择与当前查询最相关的先前对话部分。查看策略["使用基于嵌入的搜索实现高效的知识检索"](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-embeddings-based-search-to-implement-efficient-knowledge-retrieval)。

[策略：分段总结长文档并递归构建完整的总结](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-summarize-long-documents-piecewise-and-construct-a-full-summary-recursively)

由于 GPT 具有固定的上下文长度，因此不能在单个查询中用于总结长度超过上下文长度与生成总结长度之差的文本。

为了总结非常长的文档，例如一本书，我们可以使用一系列查询来总结文档的每个部分。可以将部分摘要连接并总结，生成摘要摘要。这个过程可以递归进行，直到整个文档被总结。如果需要使用关于前面部分的信息以便理解后面部分，则可以在摘要内容时包含在书中任何给定点之前的文本的连续摘要。 OpenAI 曾在早期的[研究](https://openai.com/research/summarizing-books)中使用 GPT-3 的变种来研究总结书籍的有效性。

[策略：让 GPT 有时间"思考"](https://platform.openai.com/docs/guides/gpt-best-practices/strategy-give-gpts-time-to-think)

[策略：指示模型先考虑基本原理，然后再得出结论](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-instruct-the-model-to-work-out-its-own-solution-before-rushing-to-a-conclusion)

有时，我们会在让模型得出结论之前明确指导它从基本原理出发进行推理而得到更好的结果。例如，假设我们想让模型评估学生的数学问题答案。最直接的方法是简单地询问模型学生的答案是正确还是错误。
系统：确定学生的解决方案是否正确。

用户：问题陈述：我正在建造一个太阳能发电装置，需要帮助解决财务问题。

- 土地成本为每平方英尺100美元
- 我可以以每平方英尺250美元的价格购买太阳能电池板，
- 我协商了一份维护合同，每年的维护成本为固定的10万美元，额外每平方英尺10美元。
- 作为一个关于平方英尺的函数，首年运营的总成本是多少？

学生的解决方案：让 x 为每平方英尺的安装面积。

1. 土地费用：100x
2. 太阳能电池板费用：250x
3. 维护费用：100,000 + 100x
   总成本：450x + 100,000
   助手：学生的解决方案是正确的。

[在 Playground 中打开](https://platform.openai.com/playground/p/default-rushing-to-a-conclusion)

但是，学生的解决方案实际上是不正确的！我们可以通过提示模型首先生成自己的解决方案来成功地注意到这一点。

系统：首先解决自己的问题。然后将你的解决方案与学生的解决方案进行比较，并评估学生的解决方案是否正确。在你自己尝试问题之前，不要决定学生的解决方案是否正确。

用户：问题陈述：我正在建造一个太阳能发电装置，需要帮助解决财务问题。

- 土地成本为每平方英尺100美元
- 我可以以每平方英尺250美元的价格购买太阳能电池板，
- 我协商了一份维护合同，每年的维护成本为固定的10万美元，额外每平方英尺10美元。
- 作为一个关于平方英尺的函数，首年运营的总成本是多少？

学生的解决方案：让 x 为每平方英尺的安装面积。

1. 土地费用：100x
2. 太阳能电池板费用：250x
3. 维护费用：100,000 + 100x
   总成本：450x + 100,000
   助手：让 x 为每平方英尺的安装面积。
4. 土地费用：100x
5. 太阳能电池板费用：250x
6. 维护费用：100,000 + 10x
   总成本：360x + 100,000

学生的解决方案是不正确的。他们在维护成本计算中犯了一个错误，使用了100x而不是10x。首年运营的正确总成本为360x + 100,000。

[在 Playground 中打开](https://platform.openai.com/playground/p/default-avoid-rushing-to-a-conclusion)

[策略：使用内心独白或一系列查询来隐藏模型的推理过程](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-inner-monologue-or-a-sequence-of-queries-to-hide-the-model-s-reasoning-process)

前面的策略说明了，在回答特定问题之前，模型有时需要详细推理问题。对于某些应用程序，模型用于得出最终答案的推理过程不适合与用户共享。例如，在辅导应用程序中，我们可能希望鼓励学生自己解决问题，但模型关于学生解决方案的推理过程可能会向学生揭示答案。

内心独白是一种可用于减轻这一影响的策略。内心独白的思想是，指示模型将预计将向用户隐藏的部分输出放入一种结构化格式，使对其进行解析变得容易。然后，在向用户呈现输出之前，解析输出并仅使输出的一部分可见。
SYSTEM
按照以下步骤回答用户的询问。

步骤1 - 首先解决自己的问题。不要依赖学生的解决方案，因为它可能是不正确的。将此步骤中的所有工作都封装在三重引号中（"""）中。

步骤2 - 将你的解决方案与学生的解决方案进行比较，并评估学生的解决方案是否正确。将此步骤中的所有工作都封装在三重引号（""""）中。

步骤3 - 如果学生犯了错误，确定你可以给出什么提示而不泄露答案。将此步骤中的所有工作都封装在三重引号中（"""）中。

步骤4 - 如果学生犯了错误，向学生提供来自上一步的提示（不要用三重引号括起来）。而不是写“步骤4 - ...”，写“提示：”。

用户：问题陈述： <<insert problem statement>

学生解决方案： <insert student solution>

[在 Playground 中打开](https://platform.openai.com/playground/p/default-inner-monologue)

或者，我们可以通过一系列查询来实现，其中除最后一个查询外，所有查询的输出都对最终用户隐藏。

首先，我们可以要求模型自行解决问题。由于这个初始查询不需要学生的解决方案，因此它可以省略。这提供了额外的优势，即模型的解决方案不会被学生的尝试解决方案所影响。

用户：

<insert problem statement>

[在 Playground 中打开](https://platform.openai.com/playground/p/default-query-sequence-1)

接下来，我们可以让模型使用所有可用的信息来评估学生解决方案的正确性。
系统：

将你的解决方案与学生的解决方案进行比较，并评估学生的方案是否正确。

用户：

问题陈述："""<insert problem statement>"""

你的解决方案："""<insert model generated solution>"""

学生的解决方案："""<insert student's solution>"""

[在 Playground 中打开](https://platform.openai.com/playground/p/default-query-sequence-2)

最后，我们可以让模型使用自己的分析来构建一个以富有帮助的导师角色的回复。

用户：

问题陈述："""<insert problem statement>"""

你的解决方案："""<insert model generated solution>"""

学生的解决方案："""<insert student's solution>"""

分析： """<insert model generated analysis from previous step>"""

[在 Playground 中打开](https://platform.openai.com/playground/p/default-query-sequence-3)

[策略：询问模型是否在之前的尝试中漏掉了什么](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-ask-the-model-if-it-missed-anything-on-previous-passes)

假设我们使用一个模型来列出与特定问题相关的源代码摘录。在列出每个摘录之后，模型需要确定是否应该开始写另一个摘录还是停止。如果源文件很大，通常会发生模型停止过早而未能列出所有相关的摘录。在这种情况下，通常可以通过提示模型跟进查询来找到之前漏掉的任何摘录，以获得更好的性能。

系统：

你将获得一个由三个引号界定的文档。你的任务是选择与以下问题相关的摘录：“人工智能历史上发生了哪些重大的范式转变。”

确保摘录包含所有需要解释它们的相关上下文——换句话说，不要提取缺少重要上下文的小片段。以以下 JSON 格式提供输出：

[{"excerpt": "..."},
...
{"excerpt": "..."}]

用户：

"""<insert document here>"""

助手：

[{"excerpt": "the model writes an excerpt here"},
...
{"excerpt": "the model writes another excerpt here"}]
用户：
是否有更多相关摘录？请注意不要重复摘录。同时，请确保摘录包含解释它们所需的所有相关上下文 - 换句话说，不要提取缺少重要上下文的小片段。

[在 Playground 中打开](https://platform.openai.com/playground/p/default-2nd-pass)

[策略：使用外部工具](https://platform.openai.com/docs/guides/gpt-best-practices/strategy-use-external-tools)

[策略：使用嵌入式搜索实现有效的知识检索](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-embeddings-based-search-to-implement-efficient-knowledge-retrieval)

如果提供作为输入的外部信息，模型可以利用外部信息。这可以帮助模型生成更丰富和更新的响应。例如，如果用户询问有关特定电影的问题，则将有关电影的高质量信息（例如演员，导演等）添加到模型的输入中可能会有所帮助。嵌入可以用于实现有效的知识检索，因此可能需要在运行时动态地将相关信息添加到模型输入中。

文本嵌入是可以测量文本字符串相关性的向量。相似或相关的字符串将比不相关的字符串更接近。这个事实以及快速的向量搜索算法的存在意味着，嵌入可以用于实现有效的知识检索。特别地，文本语料库可以分成块，并且每个块可以被嵌入和存储。然后，可以对给定的查询进行嵌入和向量搜索，以查找与查询（即在嵌入空间中最接近的）最相关的来自语料库的文本块的嵌入。

示例实现可以在[OpenAI Cookbook](https://github.com/openai/openai-cookbook/blob/main/examples/vector_databases/Using_vector_databases_for_embeddings_search.ipynb)中找到。请参见策略 [“指示模型使用检索出的知识回答查询”](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-instruct-the-model-to-use-retrieved-knowledge-to-answer-queries) 的示例，了解如何使用知识检索来最大程度地减少模型编造错误事实的可能性。

[策略：使用代码执行执行更准确的计算或调用外部API](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-use-use-code-code-execution-to-perform-more-more-  - 准估计或外部 - 外部 - )

这段话的意思是 GPT 模型不能准确地完成算术或长时间计算，因此在需要的情况下，可以指示模型编写和运行代码来代替计算。特别是，可以将要运行的代码放入三个反引号之类的指定格式中。然后，在生成输出后，可以提取和运行该代码。最后，如有必要，可以将代码执行引擎（即 Python 解释器）的输出作为输入提供给模型用于下一个查询。

系统：

您可以使用三个反引号将 Python 代码括起来编写和执行，例如```代码放在这里```。使用它来进行计算。

用户：

找出以下多项式的所有实根：3*x**5 - 5*x**4 - 3*x**3 - 7*x - 10。

[在 Playground 中打开](https://platform.openai.com/playground/p/default-code-execution)

代码执行的另一个好用例是调用外部 API。如果将某个模型指示正确使用 API，那么它可以编写可以使用该 API 的代码。可以通过提供文件说明或代码示例来指示模型如何使用 API。
您可以使用三个反引号将 Python 代码括起来编写和执行。此外，请注意，您可以访问以下模块来帮助用户向他们的朋友发送消息：

```python
import message
message.write(to="John", message="Hey, want to meetup after work?")
```

[在 Playground 中打开](https://platform.openai.com/playground/p/default-code-api)

**警告：执行模型生成的代码本质上并不安全，应在寻求此功能的任何应用程序中采取预防措施。特别是需要沙盒代码执行环境来限制不受信任的代码可能造成的危害。**

[策略：系统化地测试更改](https://platform.openai.com/docs/guides/gpt-best-practices/strategy-test-changes-systematically)

有时很难判断一个更改（例如新的指令或新的设计）是否使您的系统更好或更差。查看一些示例可能暗示哪个更好，但对于小样本大小，很难区分真正的改善还是随机幸运。也许此更改有助于某些输入的性能，但对其他输入的性能有所损害。

评估程序（或“评估”）对于优化系统设计非常有用。好的评估应满足以下条件：

- 代表真实世界的使用情况（或至少是多样化的）
- 包含许多测试用例，以具有更大的统计能力（有关准则，请参见下表）
- 易于自动化或重复执行

| 检测差异 | 95%置信度所需的样本量 |
| :------- | :-------------------- |
| 30%      | ~10                   |
| 10%      | ~100                  |
| 3%       | ~1,000                |
| 1%       | ~10,000               |

输出结果的评估可以由计算机、人类或二者混合完成。在一些客观的标准下（如只有一个正确答案的问题），计算机可以自动化评估，而在一些主观或模糊的标准下（模型输出需要通过其他模型查询进行评估），计算机也可以完成自动化评估。 [Openai Evals](https://github.com/openai/evals)是一个开源软件框架，提供了创建自动化评估工具的工具。

当存在一系列可能被认为是同样高质量的输出时（例如需要长回答的问题），基于模型的评估可以很有用。使用基于模型的评估和需要人类评估之间的分界线模糊，并且随着模型变得更加能力强大，这个界限不断变化。我们鼓励进行实验，以确定基于模型的评估在您的用例中可以工作的效果。

[策略：参考金标准答案评估模型输出](https://platform.openai.com/docs/guides/gpt-best-practices/tactic-evaluate-model-model----------------------  - 标准奖励)

假设已知问题的正确答案应该参考一组特定的已知事实。然后我们可以使用一个模型查询来计算回答中包含了多少所需事实。

例如，使用以下系统消息：

系统
你将获得由三个引号分隔的文本，该文本应该是回答一个问题的答案。请检查以下信息是否直接包含在答案中：

- 尼尔·阿姆斯特朗是第一个在月球上行走的人。
- 尼尔·阿姆斯特朗首次行走在月球上的日期是1969年7月21日。

对于每个点执行以下步骤：

1- 重述该点。
2- 从答案中提供最接近此点的引用。
3- 考虑是否存在不知道该主题的人可以直接推断该点的阅读引用。在做出决定之前解释为什么或者为什么不。
4- 如果回答3是"yes"，则写下"yes"，否则写下"no"。

最后，提供有多少个"yes"回答的计数。以这种格式提供：{"count":<插入计数> }。

这是一个满足两点的示例输入:

系统

<insert system message above>

用户

"""尼尔·阿姆斯特朗因为成为第一位踏上月球的人而闻名。这一历史性事件发生在阿波罗11号任务期间的1969年7月21日。"""

[在 Playground 中打开](https://platform.openai.com/playground/p/default-model-based-eval-1)

这是只有一个点符合要求的示例输入：

系统

<insert system message above>

用户

"""尼尔·阿姆斯特朗当他走出登月舱时，创造了历史，成为第一个在月球上行走的人。"""

[在 Playground 中打开](https://platform.openai.com/playground/p/default-model-based-eval-2)

这是没有任何一点符合要求的示例输入：

系统

<insert system message above>

用户

"""在'69年的夏天，一场宏大的航行，
阿波罗11号，勇敢如传说。
阿姆斯特朗迈出一步，历史展开了，
"一个小步骤，"他说，为了一个新世界。"""

[在 Playground 中打开](https://platform.openai.com/playground/p/default-model-based-eval-3)

这种模型评估类型有很多可能的变体。考虑以下变体，它跟踪候选答案与标准答案之间的重叠程度，并跟踪候选答案是否与标准答案的任何部分矛盾。
系统：

按照以下步骤回应用户输入。在进行下一步之前，完全重述每个步骤。例如“步骤1：逐步推理...”。

步骤1：逐步推理提交答案中的信息与专家答案相比是以下哪一种情况：不交叉、相等、子集、超集或重叠（即某些交集但不是子集/超集）。

步骤2：逐步推理提交的答案是否与专家答案相矛盾。

步骤3：输出一个 JSON 对象，结构如下：{"type_of_overlap": "disjoint" or "equal" or "subset" or "superset" or "overlapping", "contradiction": true or false}

这是一个包含不合格答案的示例输入，尽管如此，它并没有与专家答案相矛盾：
<insert system message above>
用户：

问题：“Neil Armstrong最著名的事件是什么，它发生在什么日期？假设使用的是 UTC 时间。”

提交的答案：“他不是在月球上行走了什么的吗？”

专家回答：“Neil Armstrong最著名的事件是成为第一个踏上月球的人。这一历史性事件发生在1969年7月21日。”

[在 Playground 中打开](https://platform.openai.com/playground/p/default-model-based-eval-4)

这是一个直接与专家答案相矛盾的答案的示例输入：
系统：
<insert system message above>
用户：

问题：“Neil Armstrong最著名的事件是什么，它发生在什么日期？假设使用的是 UTC 时间。”

提交的答案：“在1969年7月21日，Neil Armstrong成为第二个踏上月球的人，在Buzz Aldrin之后。”

专家回答：“Neil Armstrong最著名的事件是成为第一个踏上月球的人。这一历史性事件发生在1969年7月21日。”

[在 Playground 中打开](https://platform.openai.com/playground/p/default-model-based-eval-5)

这是一个正确的答案的示例输入，其中提供了比必要的更多细节：
系统:
<insert system message above>
用户：

问题：“Neil Armstrong最著名的事件是什么，它发生在什么日期？假设使用的是 UTC 时间。”

提交的答案：“在1969年7月21日UTC时间02:56左右，Neil Armstrong成为第一个在月球表面留下脚印的人，标志着人类历史上的重大成就。”

专家回答：“Neil Armstrong最著名的事件是成为第一个踏上月球的人。这一历史性事件发生在1969年7月21日。”

[在 Playground 中打开](https://platform.openai.com/playground/p/default-model-based-eval-6)

[其他资源](https://platform.openai.com/docs/guides/gpt-best-practices/other-resources)

如需更多灵感，请访问[OpenAI Cookbook](https://github.com/openai/openai-cookbook)，其中包含示例代码，并链接到第三方资源，例如：

- [提示库和工具](https://github.com/openai/openai-cookbook#prompting-libraries--tools)
- [提示指南](https://github.com/openai/openai-cookbook#prompting-guides)
- [视频课程](https://github.com/openai/openai-cookbook#video-courses)
- [有关改进推理的高级提示的论文](https://github.com/openai/openai-cookbook#papers-on-advanced-prompting-to-improve-reasoning)

