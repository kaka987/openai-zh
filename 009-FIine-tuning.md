1. [微调](https://platform.openai.com/docs/guides/fine-tuning/fine-tuning)

   学习如何为您的应用程序自定义模型。

   [介绍](https://platform.openai.com/docs/guides/fine-tuning/introduction)

   微调通过提供以下内容，可以使您从通过API获得的模型中获得更多效果：

   1. 比prompt设计更高质量的结果
   2. 能够在prompt中无法放入的更多示例上进行训练
   3. 由于缩短了prompt而节省的令牌
   4. 低延迟请求

   GPT-3已经在公开互联网的大量文本上进行了预训练。当提供一些示例的提示时，它通常可以直觉地知道您正在尝试执行的任务并生成一个合理的完成。这通常称为“少样本学习”。

   微调通过训练可以比prompt中包含的更多示例让您在许多任务上获得更好的结果，从而改进了少样本学习。**一旦模型被微调，您就不再需要在prompt中提供示例了。**这样可以节省成本并实现更低延迟的请求。

   在高层次上，微调包括以下步骤：

   1. 准备并上传训练数据
   2. 训练新的微调模型
   3. 使用您的微调模型

请访问我们的[定价页面](https://openai.com/api/pricing)了解更多有关微调模型的培训和使用的计费方式。

[哪些模型可以被微调？](https://platform.openai.com/docs/guides/fine-tuning/what-models-can-be-fine-tuned)

目前，微调仅适用于以下基础模型：`davinci`，`curie`，`babbage`和`ada`。这些是最初的模型，训练后没有任何指令（例如`text-davinci-003`）。您还可以[继续微调微调模型](https://platform.openai.com/docs/guides/fine-tuning/continue-fine-tuning-from-a-fine-tuned-model)，向其中添加其他数据而无需从头开始。

[安装](https://platform.openai.com/docs/guides/fine-tuning/installation)

我们建议使用OpenAI命令行界面（CLI）。要安装它，请运行以下命令：



```bash
pip install --upgrade openai
```

以下说明适用于版本 **0.9.4** 和更高版本。另外，OpenAI CLI需要Python 3。

通过将以下行添加到您的Shell初始化脚本（例如.bashrc、zshrc等）或在微调命令之前在命令行中运行它来设置您的 `OPENAI_API_KEY` 环境变量：

```bash
export OPENAI_API_KEY="<OPENAI_API_KEY>"
```

[准备训练数据](https://platform.openai.com/docs/guides/fine-tuning/prepare-training-data)

训练数据是教授GPT-3你希望它说的内容的方式。

您的数据必须是[JSONL](https://jsonlines.org/)文档，其中每行是一个提示完成对应于一个训练示例。您可以使用我们的[CLI数据准备工具](https://platform.openai.com/docs/guides/fine-tuning/cli-data-preparation-tool)轻松将您的数据转换为此文件格式。

```json
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
...
```

设计用于Fine-tuning的提示和完成方式不同于设计用于我们基础模型（Davinci、Curie、Babbage、Ada）的提示方式。特别是，虽然基础模型的提示通常由多个示例组成（"Few-shot learning"），但对于Fine-tuning，每个训练示例通常由单个输入示例及其关联的输出组成，无需给出详细说明或包含多个示例在同一个提示中。

有关如何为各种任务准备训练数据的详细指导，请参阅我们的[准备数据集](https://platform.openai.com/docs/guides/fine-tuning/preparing-your-dataset)最佳实践。

您拥有的训练示例越多，越好。我们建议至少有几百个示例。总的来说，我们发现数据集大小每翻倍，模型质量会线性增加。

[CLI数据准备工具](https://platform.openai.com/docs/guides/fine-tuning/cli-data-preparation-tool)

我们开发了一个工具，可以验证、给出建议并重新格式化您的数据：



```bash
openai tools fine_tunes.prepare_data -f <LOCAL_FILE>
```

该工具接受不同的格式，唯一的要求是它们包含一个提示和一个完成列/键。您可以传递**CSV，TSV，XLSX，JSON**或**JSONL**文件，并在引导您进行建议更改的过程之后将输出保存到准备好进行Fine-tuning的JSONL文件中。

[创建Fine-tuned模型](https://platform.openai.com/docs/guides/fine-tuning/create-a-fine-tuned-model)

以下假设您已经按照[上述说明](https://platform.openai.com/docs/guides/fine-tuning/prepare-training-data)准备好训练数据。

使用OpenAI CLI启动Fine-Tuning作业：

```bash
openai api fine_tunes.create -t <TRAIN_FILE_ID_OR_PATH> -m <BASE_MODEL>
```

其中`BASE_MODEL`是您所选的基础模型的名称（ada、babbage、curie或davinci）。您可以使用[suffix参数](https://platform.openai.com/docs/guides/fine-tuning/customize-your-model-name)自定义Fine-tuned模型的名称。

运行上述命令会执行以下几个步骤：

1. 使用[文件API](https://platform.openai.com/docs/api-reference/files)上传文件（或使用已上载的文件）
2. 创建一个Fine-tune作业
3. 流式传输事件，直到作业完成（这通常需要几分钟，但如果队列中有许多作业或数据集很大，则可能需要数小时）

每个Fine-tuning作业都从一个基础模型开始，默认为curie。模型的选择会影响模型的性能和运行Fine-tuned模型的成本。您可以选择的模型包括：`ada`，`babbage`，`curie`和`davinci`。有关Fine-tune费率的详细信息，请访问我们的[定价页面](https://openai.com/api/pricing/#faq-fine-tuning-pricing-calculation)。

在启动Fine-tune作业后，可能需要一些时间才能完成。您的作业可能排在我们系统中的其他作业后面，根据模型和数据集大小，训练我们的模型可能需要几分钟或数小时。如果任何原因导致事件流中断，您可以通过运行以下命令恢复：

```bash
openai api fine_tunes.follow -i <YOUR_FINE_TUNE_JOB_ID>
```

当作业完成时，它应该显示Fine-tuned模型的名称。

除了创建Fine-tune作业之外，您还可以列出现有作业，检索作业的状态或取消作业。

```bash
# List all created fine-tunes
openai api fine_tunes.list

# Retrieve the state of a fine-tune. The resulting object includes
# job status (which can be one of pending, running, succeeded, or failed)
# and other information
openai api fine_tunes.get -i <YOUR_FINE_TUNE_JOB_ID>

# Cancel a job
openai api fine_tunes.cancel -i <YOUR_FINE_TUNE_JOB_ID>
```

[使用 Fine-tuned 模型](https://platform.openai.com/docs/guides/fine-tuning/use-a-fine-tuned-model)

当作业成功完成后，`fine_tuned_model`字段将被填充上模型名称。现在，您可以将此模型作为 [Completions API](https://platform.openai.com/docs/api-reference/completions) 的一个参数，并使用 [Playground](https://platform.openai.com/playground) 向其发送请求。

在作业完成后，您的模型可能需要几分钟才能准备好处理请求。如果发送至模型的完成请求超时，则很可能是因为模型仍在加载中。如果出现此问题，请稍等几分钟后再重试。

您可以通过在完成请求中将模型名称作为 `model` 参数来开始发出请求：

OpenAI CLI：

```bash
openai api completions.create -m <FINE_TUNED_MODEL> -p <YOUR_PROMPT>
```

cURL:



```bash
curl https://api.openai.com/v1/completions \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"prompt": YOUR_PROMPT, "model": FINE_TUNED_MODEL}'
```

Python:



```python
import openai
openai.Completion.create(
    model=FINE_TUNED_MODEL,
    prompt=YOUR_PROMPT)
```

Node.js:



```javascript
const response = await openai.createCompletion({
  model: FINE_TUNED_MODEL
  prompt: YOUR_PROMPT,
});
```

您可以在发送到 fine-tuned 模型的请求中继续使用所有其他 [Completions](https://platform.openai.com/docs/api-reference/completions) 参数，例如 `temperature`、`frequency_penalty`、`presence_penalty` 等等。

[删除 Fine-tuned 模型](https://platform.openai.com/docs/guides/fine-tuning/delete-a-fine-tuned-model)

要删除 Fine-tuned 模型，您必须是您的组织内的“所有者”。

OpenAI CLI:



```bash
openai api models.delete -i <FINE_TUNED_MODEL>
```

cURL:



```bash
curl -X "DELETE" https://api.openai.com/v1/models/<FINE_TUNED_MODEL> \
  -H "Authorization: Bearer $OPENAI_API_KEY"
```

Python:



```python
import openai
openai.Model.delete(FINE_TUNED_MODEL)
```

[准备数据集](https://platform.openai.com/docs/guides/fine-tuning/preparing-your-dataset)

Fine-tuning 是一种强大的技术，用于创建一个特定于您的用例的新模型。**在对您的模型进行 Fine-tuning 之前，我们强烈建议您阅读下面的最佳实践和[特定指南](https://platform.openai.com/docs/guides/fine-tuning/specific-guidelines)以适应您的用例。**

[Data formatting](https://platform.openai.com/docs/guides/fine-tuning/data-formatting)

为了 Fine-tune 模型，您需要一组训练示例，每个示例由一个单独的输入（“提示”）和其相关的输出（“完成”）组成。这与使用我们的基础模型有明显不同，对于基础模型，您可能会在单个提示中输入详细的说明或多个示例。

- 每个提示应以固定的分隔符结尾，以通知模型提示何时结束并开始完成。一个通常很好的简单分隔符是`\n\n###\n\n`。分隔符不应出现在任何提示中的其他位置。
- 由于我们的[分词](https://platform.openai.com/tokenizer)，每个完成应以一个空格开头，因为分词将大多数单词与前面的空格分开。
- 每个完成应以固定的停止序列结尾，以通知模型完成何时结束。停止序列可以是`\n`、`###`或任何不出现在任何完成中的标记。
- 在推断时，您应以与创建训练数据集时相同的方式格式化提示，包括相同的分隔符。还要指定相同的停止序列以正确截断完成。

[通用最佳实践](https://platform.openai.com/docs/guides/fine-tuning/general-best-practices)

Fine-tuning 需要更多高质量的示例以表现更好。为了 Fine-tune 一个比使用高质量提示与我们的基础模型更好的模型，您应该提供至少几百个高质量示例，最好由人类专家审查。从那里开始，性能往往会随着示例数量的每倍增加而线性增加。增加示例数量通常是提高性能的最佳和最可靠的方法。

分类器是最容易入手的模型。对于分类问题，建议使用ada，一旦 Fine-tune，它通常只会略微表现不如更强大的模型，同时速度和价格更快更便宜。

如果您是在现有数据集上进行 Fine-tune 而不是从头开始编写提示，请务必手动查看您的数据，以排除不当或不准确的内容，如果可能的话，或者尽可能多地查看数据集的随机样本，如果数据集很大。

- [具体指南](https://platform.openai.com/docs/guides/fine-tuning/specific-guidelines)

  Fine-tuning 可以解决各种问题，最佳使用方式可能取决于您的具体用例。在下面，我们列出了 Fine-tuning 的最常见用例和相应的指南。

  分类
  - [模型是否作出了不真实的陈述？](https://platform.openai.com/docs/guides/fine-tuning/case-study-is-the-model-making-untrue-statements)
  - [情感分析](https://platform.openai.com/docs/guides/fine-tuning/case-study-sentiment-analysis)
  - [邮件分类](https://platform.openai.com/docs/guides/fine-tuning/case-study-categorization-for-email-triage)

  条件生成
  - [基于维基百科文章编写吸引人的广告](https://platform.openai.com/docs/guides/fine-tuning/case-study-write-an-engaging-ad-based-on-a-wikipedia-article)
  - [实体提取](https://platform.openai.com/docs/guides/fine-tuning/case-study-entity-extraction)
  - [客户支持聊天机器人](https://platform.openai.com/docs/guides/fine-tuning/case-study-customer-support-chatbot)
  - [基于技术属性列表的产品描述](https://platform.openai.com/docs/guides/fine-tuning/case-study-product-description-based-on-a-technical-list-of-properties)

[分类](https://platform.openai.com/docs/guides/fine-tuning/classification)

在分类问题中，提示中的每个输入应被归类为预定义的类之一。针对这种类型的问题，我们建议：

- 在提示结尾处使用分隔符，例如 `\n\n###\n\n`。使用模型时记得追加这个分隔符。
- 选择与单个 [token](https://platform.openai.com/tokenizer) 相对应的类。在推理时，指定 `max_tokens=1`，因为你只需要第一个 token 来进行分类。
- 确保提示 + 完成不超过 2048 个 token，包括分隔符在内。
- 每个类别至少要有 100 个示例。
- 当使用模型时，可以指定 `logprobs=5`（用于 5 个类）以获取类的对数概率。
- 确保用于 Finetuning 的数据集在结构和任务类型上与模型将要用于的任务非常相似。

[案例研究：模型是否作出了不真实的陈述？](https://platform.openai.com/docs/guides/fine-tuning/case-study-is-the-model-making-untrue-statements)

比方说，您想要确保您网站上的广告文本正确地提到了产品和公司。换句话说，您希望确保该模型不会胡编乱造。您可能希望微调一个分类器，以过滤掉不正确的广告。

该数据集可能如下所示：

```json
{"prompt":"Company: BHFF insurance\nProduct: allround insurance\nAd:One stop shop for all your insurance needs!\nSupported:", "completion":" yes"}
{"prompt":"Company: Loft conversion specialists\nProduct: -\nAd:Straight teeth in weeks!\nSupported:", "completion":" no"}
```

在上面的示例中，我们使用了一个结构化的输入，其中包含公司名称、产品和相关广告。我们使用 `\nSupported:` 作为分隔符来清晰地分隔提示和完成部分。有足够的示例时，分隔符不会有太大的影响（通常不超过 0.4%），只要它不出现在提示或完成部分中就可以。

针对这种用例，我们微调了 ada 模型，因为它将更快、更便宜，而且性能将与大型模型相当，因为这是一个分类任务。

现在，我们可以通过进行完成请求来查询我们的模型。

```bash
curl https://api.openai.com/v1/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "prompt": "Company: Reliable accountants Ltd\nProduct: Personal Tax help\nAd:Best advice in town!\nSupported:",
    "max_tokens": 1,
    "model": "YOUR_FINE_TUNED_MODEL_NAME"
  }'
```

这将返回“是”或“否”。

[案例研究：情感分析](https://platform.openai.com/docs/guides/fine-tuning/case-study-sentiment-analysis)

比方说，您想要了解一条特定的推文是积极的还是消极的程度。该数据集可能如下所示：

```json
{"prompt":"Overjoyed with the new iPhone! ->", "completion":" positive"}
{"prompt":"@lakers disappoint for a third straight night https://t.co/38EFe43 ->", "completion":" negative"}
```

一旦模型进行了微调，您可以通过在完成请求上设置 `logprobs=2` 来获取第一个完成标记的对数概率。正面类别的概率越高，相对情感越高。

现在，我们可以通过进行完成请求来查询我们的模型。

```bash
curl https://api.openai.com/v1/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "prompt": "https://t.co/f93xEd2 Excited to share my latest blog post! ->",
    "max_tokens": 1,
    "model": "YOUR_FINE_TUNED_MODEL_NAME"
  }'
```

这将返回：



```json
{
  "id": "cmpl-COMPLETION_ID",
  "object": "text_completion",
  "created": 1589498378,
  "model": "YOUR_FINE_TUNED_MODEL_NAME",
  "choices": [
    {
      "logprobs": {
        "text_offset": [
          19
        ],
        "token_logprobs": [
          -0.03597255
        ],
        "tokens": [
          " positive"
        ],
        "top_logprobs": [
          {
            " negative": -4.9785037,
            " positive": -0.03597255
          }
        ]
      },

      "text": " positive",
      "index": 0,
      "finish_reason": "length"
    }
  ]
}
```

[案例研究：电子邮件的分类](https://platform.openai.com/docs/guides/guides/fine-tuning/case-study-categorization-for-email-triage)

假设您想将收到的电子邮件分类为一组大量预定义的类别之一。对于大量类别的分类，我们建议您将这些类别转换为数字，这在 ~500 个类别以下的情况下效果良好。我们观察到，在数字之前添加一个空格有时会稍微有助于性能，这是由于标记化造成的。您可能想按照以下方式构建您的训练数据：



```json
{"prompt":"Subject: <email_subject>\nFrom:<customer_name>\nDate:<date>\nContent:<email_body>\n\n###\n\n", "completion":" <numerical_category>"}
```

例如：



```json
{"prompt":"Subject: Update my address\nFrom:Joe Doe\nTo:support@ourcompany.com\nDate:2021-06-03\nContent:Hi,\nI would like to update my billing address to match my delivery address.\n\nPlease let me know once done.\n\nThanks,\nJoe\n\n###\n\n", "completion":" 4"}
```

在上面的示例中，我们使用了一个最多包含2043个标记的输入电子邮件。（这允许使用4个标记分隔符和一个标记完成符，总共2048个）。作为分隔符，我们使用了`\n\n###\n\n`，并删除了电子邮件中的任何`###`出现。

[条件生成](https://platform.openai.com/docs/guides/fine-tuning/conditional-generation)

条件生成是一个需要根据某种输入生成内容的问题。这包括改写、摘要、实体提取、根据规格说明编写产品描述、聊天机器人等等。对于这种类型的问题，我们建议：

- 在提示的结尾使用分隔符，例如`\n\n###\n\n`。请记住，在最终向模型发出请求时，也要附加此分隔符。
- 在完成的结尾处使用结束标记，例如`END`
- 在推理过程中将结束标记添加为停止序列，例如`stop=[" END"]`
- 至少拥有约500个样本
- 确保提示+完成符不超过2048个标记，包括分隔符
- 确保示例具有高质量并遵循相同的期望格式
- 确保用于 Fine-Tuning 的数据集在结构和任务类型上与模型将要用于的相似
- 对于这些用例，使用较低的学习率和只有1-2个 epochs 通常效果更好

[案例研究：根据维基百科文章编写引人入胜的广告](https://platform.openai.com/docs/guides/fine-tuning/case-study-write-an-engaging-ad-based-on-a-wikipedia-article)

这是一个生成性的用例，因此您需要确保提供的样本质量最高，因为精细调整的模型将尝试模仿给定示例的风格（和错误）。一个良好的起点是大约500个示例。样本数据集可能如下所示：



```json
{"prompt":"<Product Name>\n<Wikipedia description>\n\n###\n\n", "completion":" <engaging ad> END"}
```

例如：

```json
{"prompt":"Samsung Galaxy Feel\nThe Samsung Galaxy Feel is an Android smartphone developed by Samsung Electronics exclusively for the Japanese market. The phone was released in June 2017 and was sold by NTT Docomo. It runs on Android 7.0 (Nougat), has a 4.7 inch display, and a 3000 mAh battery.\nSoftware\nSamsung Galaxy Feel runs on Android 7.0 (Nougat), but can be later updated to Android 8.0 (Oreo).\nHardware\nSamsung Galaxy Feel has a 4.7 inch Super AMOLED HD display, 16 MP back facing and 5 MP front facing cameras. It has a 3000 mAh battery, a 1.6 GHz Octa-Core ARM Cortex-A53 CPU, and an ARM Mali-T830 MP1 700 MHz GPU. It comes with 32GB of internal storage, expandable to 256GB via microSD. Aside from its software and hardware specifications, Samsung also introduced a unique a hole in the phone's shell to accommodate the Japanese perceived penchant for personalizing their mobile phones. The Galaxy Feel's battery was also touted as a major selling point since the market favors handsets with longer battery life. The device is also waterproof and supports 1seg digital broadcasts using an antenna that is sold separately.\n\n###\n\n", "completion":"Looking for a smartphone that can do it all? Look no further than Samsung Galaxy Feel! With a slim and sleek design, our latest smartphone features high-quality picture and video capabilities, as well as an award winning battery life. END"}
```

这里我们使用了一个多行分隔符，因为维基百科文章包含多个段落和标题。我们还使用了一个简单的结束标记，以确保模型知道何时完成预测。

[案例研究：实体提取](https://platform.openai.com/docs/guides/fine-tuning/case-study-entity-extraction)

这类似于语言转换任务。为了提高性能，最好按字母顺序或按原始文本中出现的顺序对不同的提取实体进行排序。这将有助于模型跟踪需要按顺序生成的所有实体。该数据集可能如下所示：

```json
{"prompt":"<any text, for example news article>\n\n###\n\n", "completion":" <list of entities, separated by a newline> END"}
```

例如：



```json
{"prompt":"Portugal will be removed from the UK's green travel list from Tuesday, amid rising coronavirus cases and concern over a \"Nepal mutation of the so-called Indian variant\". It will join the amber list, meaning holidaymakers should not visit and returnees must isolate for 10 days...\n\n###\n\n", "completion":" Portugal\nUK\nNepal mutation\nIndian variant END"}
```

最好使用一个多行分隔符，因为文本通常包含多行。最好能具有各种不同类型的输入提示（新闻文章、维基百科页面、推文、法律文件），以反映提取实体时可能遇到的文本类型多样性。

[案例研究：客户支持聊天机器人](https://platform.openai.com/docs/guides/fine-tuning/case-study-customer-support-chatbot)

聊天机器人通常包含有关对话的相关上下文（订单详细信息），迄今为止的对话摘要以及最新的消息。对于这个用例，相同的过去对话可以生成数据集中的多个行，每次都带有稍微不同的上下文，作为每个代理生成的补全。 由于可能涉及不同类型的请求和客户问题，这个用例将需要几千个示例。为确保高质量的性能，我们建议审核对话样本，以确保代理的消息质量。摘要可以通过单独的文本转换微调模型生成。该数据集可能如下所示：



```json
{"prompt":"Summary: <summary of the interaction so far>\n\nSpecific information:<for example order details in natural language>\n\n###\n\nCustomer: <message1>\nAgent: <response1>\nCustomer: <message2>\nAgent:", "completion":" <response2>\n"}
{"prompt":"Summary: <summary of the interaction so far>\n\nSpecific information:<for example order details in natural language>\n\n###\n\nCustomer: <message1>\nAgent: <response1>\nCustomer: <message2>\nAgent: <response2>\nCustomer: <message3>\nAgent:", "completion":" <response3>\n"}
```

这里我们特意将不同类型的输入信息分开，但在提示和完成之间保持了客户代理对话的相同格式。所有的完成只应该由代理完成，我们可以在推理时使用 `\n` 作为停止序列。

[案例研究：基于技术属性清单的产品描述](https://platform.openai.com/docs/guides/fine-tuning/case-study-product-description-based-on-a-technical-list-of-properties)

在这种情况下，将输入数据转换成自然语言非常重要，这可能会导致更好的性能。例如，以下格式：



```json
{"prompt":"Item=handbag, Color=army_green, price=$99, size=S->", "completion":" This stylish small green handbag will add a unique touch to your look, without costing you a fortune."}
```

不如以下格式效果好：



```json
{"prompt":"Item is a handbag. Colour is army green. Price is midrange. Size is small.->", "completion":" This stylish small green handbag will add a unique touch to your look, without costing you a fortune."}
```

为了获得高性能，确保完成是基于提供的描述的。如果经常查询外部内容，那么自动添加此类内容将提高性能。如果描述是基于图像的，可以使用算法提取图像的文本描述。由于完成只有一句话长，我们在推理期间可以使用 `.` 作为停止序列。

[高级用法](https://platform.openai.com/docs/guides/fine-tuning/advanced-usage)

[自定义模型名称](https://platform.openai.com/docs/guides/fine-tuning/customize-your-model-name)

您可以使用 [suffix](https://platform.openai.com/docs/api-reference/fine-tunes/create#fine-tunes/create-suffix) 参数将最多 40 个字符的后缀添加到您的微调模型名称中。

OpenAI CLI:



```bash
openai api fine_tunes.create -t test.jsonl -m ada --suffix "custom model name"
```

生成的名称将会是：



```bash
ada:ft-your-org:custom-model-name-2022-02-15-04-21-04
```

[分析您的微调模型](https://platform.openai.com/docs/guides/fine-tuning/analyzing-your-fine-tuned-model)

一旦一项工作完成，我们将结果文件附加到该工作中。当您检索微调时，将列出此结果文件 ID，以及在查看微调事件时也会列出。您可以下载这些文件：

OpenAI CLI:



```bash
openai api fine_tunes.results -i <YOUR_FINE_TUNE_JOB_ID>
```

CURL:



```text
curl https://api.openai.com/v1/files/$RESULTS_FILE_ID/content \
  -H "Authorization: Bearer $OPENAI_API_KEY" > results.csv
```

`_results.csv` 文件包含每个训练步骤的一行数据，其中一步骤指的是对一批数据进行前向和反向传递。除了步骤数，每行还包含以下与该步骤对应的字段：

- **elapsed_tokens**：模型目前已经看到的标记数（包括重复）
- **elapsed_examples**：模型目前已经看到的例子数（包括重复），每个例子指的是您批处理中的一个元素。例如，如果 `batch_size = 4`，每个步骤将使 `elapsed_examples` 增加 4。
- **training_loss**：训练批次的损失
- **training_sequence_accuracy**：训练批次中“完成”的百分比，其预测的标记与真实的完成标记完全吻合。例如，对于 `batch_size` 为 3 的情况，如果您的数据包含完成标记 [[1,2], [0,5], [4,2]]，而模型预测的是 [[1,1], [0,5], [4,2]]，则准确度为 2/3 = 0.67。
- **training_token_accuracy**：模型正确预测训练批次中**标记**的百分比。例如，对于 `batch_size` 为 3 的情况，如果您的数据包含完成标记 [[1,2], [0,5], [4,2]]，而模型预测的是 [[1,1], [0,5], [4,2]]，则正确率为 5/6 = 0.83。

[分类特定度量标准](https://platform.openai.com/docs/guides/fine-tuning/classification-specific-metrics)

我们还提供了在结果文件中生成额外分类特定度量标准的选项，如准确性和加权 F1 得分。这些度量标准会在全面验证集上定期计算，并在微调结束时计算。您会在结果文件中看到它们作为额外的列。

要启用此功能，请将参数 `--compute_classification_metrics` 设置为开启状态。此外，您还必须提供一个验证文件，并设置 `classification_n_classes` 参数（多类分类）或 `classification_positive_class` 参数（二类分类）。

OpenAI CLI ：

```bash
# For multiclass classification
openai api fine_tunes.create \
  -t <TRAIN_FILE_ID_OR_PATH> \
  -v <VALIDATION_FILE_OR_PATH> \
  -m <MODEL> \
  --compute_classification_metrics \
  --classification_n_classes <N_CLASSES>

# For binary classification
openai api fine_tunes.create \
  -t <TRAIN_FILE_ID_OR_PATH> \
  -v <VALIDATION_FILE_OR_PATH> \
  -m <MODEL> \
  --compute_classification_metrics \
  --classification_n_classes 2 \
  --classification_positive_class <POSITIVE_CLASS_FROM_DATASET>
```

如果您设置 `--compute_classification_metrics`，则以下度量标准将显示在您的[结果文件](https://platform.openai.com/docs/guides/fine-tuning/analyzing-your-fine-tuned-model)中：

##### 用于多类分类

- **classification/accuracy**：准确性度量标准
- **classification/weighted_f1_score**：加权 F1 得分

##### 对于二分类

以下度量标准基于分类阈值为 0.5（即当概率> 0.5 时，将一个示例分类为属于阳性类）：

- **classification/accuracy**：准确性度量标准
- **classification/precision**：查准率
- **classification/recall**：召回率
- **classification/f{beta}**：F-beta 分数
- **classification/auroc**：ROC 曲线下面积（AUROC）
- **classification/auprc**：PR 曲线下面积（AUPRC）：

请注意，这些评估假定您正在使用文本标签作为词汇仅有一个标记的类，如上所述。如果不符合这些条件，您得到的数字可能会有误。

[验证](https://platform.openai.com/docs/guides/fine-tuning/validation)

您可以保留一些数据进行验证。验证文件与训练文件具有完全相同的格式，您的训练和验证数据应该彼此互斥。

如果在创建微调作业时包含验证文件，则生成的结果文件将包含定期在训练过程中评估微调模型在验证数据上表现如何的评估。

OpenAI CLI:



```bash
openai api fine_tunes.create -t <TRAIN_FILE_ID_OR_PATH> \
  -v <VALIDATION_FILE_ID_OR_PATH> \
  -m <MODEL>
```

如果您提供了验证文件，我们会在训练期间定期计算验证数据批次上的度量标准。您将在结果文件中看到以下额外的度量标准：

- **validation_loss**：验证批次上的损失
- **validation_sequence_accuracy**：验证批次中完成的完成情况，其中模型预测的令牌完全匹配真实的完成令牌的百分比。例如，使用批次大小为 3，如果数据包含完成 [[1, 2], [0, 5], [4, 2]]，而模型预测 [[1, 1], [0, 5], [4, 2]]，则准确率为 2/3 = 0.67
- **validation_token_accuracy**：模型正确预测验证批次中的令牌的百分比。例如，使用批次大小为 3，如果数据包含完成 [[1, 2], [0, 5], [4, 2]]，而模型预测 [[1, 1], [0, 5], [4, 2]]，则准确率为 5/6 = 0.83

[超参数](https://platform.openai.com/docs/guides/fine-tuning/hyperparameters)

我们选择了在各种用途下工作良好的默认超参数。唯一必需的参数是训练文件。

也就是说，调整用于微调的超参数通常可以导致生成更高质量的模型。具体而言，您可能想要配置以下内容：

- `model`：要微调的基础模型的名称。您可以选择其中一个："ada"、"babbage"、"curie" 或 "davinci"。有关这些模型的更多信息，请参见[模型](https://platform.openai.com/docs/models)文档。
- `n_epochs` - 默认为 4。要训练模型的时期数。一个时期是指对训练数据集进行完整的循环。
- `batch_size` - 默认为训练集中示例的 ~0.2%，上限为 256。批处理大小是用于训练单个前向和后向传递的训练示例的数量。一般来说，我们发现对于较大的数据集，较大的批处理大小通常效果更好。
- `learning_rate_multiplier` - 默认为 0.05、0.1 或 0.2，具体取决于最终的 `batch_size`。微调学习速率是用于预训练的原始学习速率，乘以该乘数。我们建议试验取值范围在 0.02 到 0.2 之间，以查看哪些会产生最好的结果。根据经验，我们发现，学习率越大，与较大的批处理大小一起使用时性能越好。
- `compute_classification_metrics` - 默认为 False。如果设置为 True，对于用于分类任务的微调，会在每个时期结束时对验证集计算分类特定的度量标准（准确度、F-1 分数等）。

要配置这些额外的超参数，请通过 OpenAI CLI 的命令行标志传递它们，例如：



```bash
openai api fine_tunes.create \
  -t file-JD89ePi5KMsB3Tayeli5ovfW \
  -m ada \
  --n_epochs 1
```

[从微调模型继续进行微调](https://platform.openai.com/docs/guides/fine-tuning/continue-fine-tuning-from-a-fine-tuned-model)

如果您已经针对您的任务微调了模型，并且现在有额外的训练数据需要整合，您可以从该模型继续进行微调。这将创建一个从所有训练数据中学习的模型，而无需从头开始重新训练。

要做到这一点，在创建新的微调作业时传入微调模型名称（例如 `-m curie:ft-<org>-<date>`）。其他训练参数无需更改，但是如果您的新训练数据比以前的训练数据小得多，您可能会发现将 `learning_rate_multiplier` 降低 2 到 4 倍会非常有用。

[权重和偏差](https://platform.openai.com/docs/guides/fine-tuning/weights-biases)

您可以将微调模型与[权重和偏差](https://wandb.me/openai-docs)同步，以跟踪实验、模型和数据集。

要开始使用，请您需要一个[权重和偏差](https://wandb.me/openai-docs)账户和付费的 OpenAI 计划。为确保您正在使用最新版本的 `openai` 和 `wandb`，请运行：

```bash
pip install --upgrade openai wandb
```

要将您的微调模型与权重和偏差同步，请运行：



```bash
openai wandb sync
```

您可以阅读[权重和偏差文档](https://wandb.me/openai-docs)以获取有关此集成的更多信息。

[示例笔记本](https://platform.openai.com/docs/guides/fine-tuning/example-notebooks)

分类

[finetuning-classification.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Fine-tuned_classification.ipynb)

本笔记本将演示如何微调模型，以对输入文本是否与棒球或曲棍球相关进行分类。在[笔记本](https://github.com/openai/openai-cookbook/blob/main/examples/Fine-tuned_classification.ipynb)中，我们将通过四个步骤执行此任务：

1. **数据探索**将介绍数据来源和一个示例长什么样子
2. **数据准备**将把我们的数据源转换为可用于微调的 jsonl 文件
3. **微调**将启动微调作业并解释生成模型的性能
4. **使用模型**将演示对微调模型进行请求以获取预测。

问答

[olympics-1-collect-data.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/fine-tuned_qa/olympics-1-collect-data.ipynb)[olympics-2-create-qa.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/fine-tuned_qa/olympics-2-create-qa.ipynb)[olympics-3-train-qa.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/fine-tuned_qa/olympics-3-train-qa.ipynb)

这个项目的思路是创建一个问答模型，基于几段提供的文本。基本的 GPT-3 模型在答案包含在段落中时回答问题很好，但是如果答案不包含在内，则基本模型往往仍试图回答问题，往往导致编造的答案。

为了创建一个模型只有在存在足够的上下文时才回答问题，我们首先创建了一个基于文本段落的问题和答案的数据集。为了训练模型只在答案存在时回答，我们还添加了对抗性样例，其中问题不与上下文匹配。在这些情况下，我们要求模型输出“没有足够的上下文来回答这个问题”。

我们将在三个笔记本中执行此任务：

1. [第一个笔记本](https://github.com/openai/openai-cookbook/blob/main/examples/fine-tuned_qa/olympics-1-collect-data.ipynb)专注于收集 GPT-3 预训练过程中没有看到的最近数据。我们选择了 2020 年奥运会的主题（实际上是在 2021 年夏天举行的），并下载了 713 个唯一的页面。我们通过单独的章节组织了数据集，这些章节将用作提出问题和回答问题的上下文。
2. [第二个笔记本](https://github.com/openai/openai-cookbook/blob/main/examples/fine-tuned_qa/olympics-2-create-qa.ipynb)将利用 Davinci-instruct 基于维基百科的一个章节提出一些问题，并根据该章节回答这些问题。
3. [第三个笔记本](https://github.com/openai/openai-cookbook/blob/main/examples/fine-tuned_qa/olympics-3-train-qa.ipynb)将利用上下文、问题和答案对的数据集，另外创建对抗性问题和上下文对，在这些情况下，问题并未基于该上下文生成。在这些情况下，模型将被提示回答“没有足够的上下文来回答这个问题”。我们还将训练一个鉴别器模型，该模型预测问题是否可以基于上下文进行回答。