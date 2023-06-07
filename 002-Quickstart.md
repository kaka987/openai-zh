[快速开始](https://platform.openai.com/docs/quickstart/quickstart)



寻找chatgpt？ 前往[chat.openai.com](https://chat.openai.com/)。

OpenAI 训练了先进的语言模型，这些模型在理解和生成文本方面非常出色。我们的 API 提供了对这些模型的访问，并可用于解决几乎涉及处理语言的任何任务。

在本快速入门教程中，您将构建一个简单的示例应用程序。在此过程中，您将学习到 API 的关键概念和技术，这些概念和技术对于使用 API 解决任何任务都是基础的，包括：

- 内容产生
- 摘要
- 分类，分类和情感分析
- 数据提取
- 翻译
- 还有很多！

[简介](https://platform.openai.com/docs/quickstart/introduction)

[Completions](https://platform.openai.com/docs/api-reference/completions)端点是我们API的核心，提供了一个非常灵活和功能强大的简单接口。您将一些文本作为**提示**输入，API将返回一个文本**完成**，尝试匹配您提供的任何指令或上下文。

提示：    写一个冰淇淋的标语              

完成：    我们用每一勺冰淇淋都带来微笑！

您可以将其视为非常高级的自动完成 - 该模型会处理您的文本提示，并试图预测接下来最有可能发生的事情。

[1 从指令开始](https://platform.openai.com/docs/quickstart/start-with-an-an-instruction)

想象一下，您想创建一个宠物名称生成器。 从头开始提出名字很难！

首先，您需要一个提示，以清楚您想要什么。 让我们从指导开始。 **提交此提示**以生成您的第一个完成。

建议一匹马的名字。

不错！ 现在，尝试使您的指导更具体。

建议一个黑马的名字。

如您所见，在我们的提示中添加一个简单的形容词会更改结果完成。 设计提示本质上是您如何“编程”模型。

[2 添加一些示例](https://platform.openai.com/docs/quickstart/add-some-examples)

制定良好的说明对于获得良好的结果很重要，但有时还不够。 让我们尝试使您的指导更加复杂。

为一匹超级英雄的马建议三个名字。

这个结果并不是我们想要的。 这些名称非常通用，似乎模型没有在我们的指示的马部分上拾取。 让我们看看是否可以提出一些更相关的建议。

在许多情况下，显示 *和 *告诉模型您想要什么都是有帮助的。 在提示中添加示例可以帮助传达模式或细微差别。 尝试提交此提示，其中包括几个示例。

为一个超级英雄的动物建议三个名字。

动物：猫
姓名：夏普爪上尉，特工fluffball，令人难以置信的猫科动物
动物：狗
姓名：ruff保护者，奇迹犬，barks-a-lot爵士
动物：马
名称：

好的！ 添加我们期望给定输入的输出的示例帮助该模型提供了我们正在寻找的名称类型。

[3 调整您的设置](https://platform.openai.com/docs/quickstart/adjust-your-settings)

及时设计并不是您可以使用的唯一工具。 您还可以通过调整设置来控制结果。 最重要的设置之一称为**温度**。

您可能已经注意到，如果您在上面的示例中多次提交了相同的提示，则该模型将始终返回相同或非常相似的完成。 这是因为您的温度设置为**0**。

尝试将相同的提示重新提示几次，温度设置为**1**。

为一个超级英雄的动物建议三个名字。 温度1

动物：猫
姓名：夏普爪上尉，特工fluffball，令人难以置信的猫科动物

动物：狗
姓名：ruff保护者，奇迹犬，barks-a-lot爵士
动物：马
名称：

看看发生了什么？ 当温度高于0时，每次提交相同的提示会导致不同的结果。

请记住，该模型可以预测哪种文本最有可能遵循其先前的文本。 温度是0到1之间的值，从本质上讲，您可以控制模型在做出这些预测时应具有多大的信心。 降低温度意味着风险更少，结果将更加准确和确定性。 温度升高将导致更多样化的结果。

   深层挖掘，深入探究。

理解标记和概率

我们的模型通过将文本分解为称为“标记”的较小单位来处理文本。标记可以是单词，单词块或单个字符。编辑以下文本以查看如何进行标记化。

我有一只名叫Butterscotch的橙色猫。

我有一只名叫Butterscotch的橙色猫。

常见的单词如“猫”是一个单一的标记，而不常见的单词通常被分解成多个标记。例如，“Butterscotch”翻译成四个标记：“But”，“ters”，“cot”和“ch”。许多标记以空格开头，例如“ hello”和“ bye”。

给定一些文本，模型确定下一个最可能出现的标记是哪个。例如，文本“马是我最喜欢的”最可能跟随标记“动物”。

马是我最喜欢的。                        动物                        49.65%      

​                                                       动物                        42.58%

​                                                        \n                           3.49%

​                                                       ！                            0.91%

这就是**温度**发挥作用的地方。如果您使用温度为0四次提交这个提示，模型将始终返回“动物”下一个词，因为它的概率最高。如果您增加温度，模型将冒更多的风险，并考虑概率较低的标记。

**如果温度为0 **                                              **如果温度为1 **

马是我最喜欢的动物                                  马是我最喜欢的动物

马是我最喜欢的动物                                  马是我最喜欢的动物

马是我最喜欢的动物                                  马是我最喜欢的！

马是我最喜欢的动物                                  马是我最喜欢的动物l

通常情况下，对于那些希望得到明确定义输出的任务，设定较低的温度是最好的选择。如果需要多样性或创造性，则较高的温度可能会有帮助，或者当你想生成几个变体供最终用户或专家选择时。

对于你的宠物名字生成器，你可能希望能够生成很多的名字创意。温度设置在0.6左右可能效果最好。

[4 建立你的应用](https://platform.openai.com/docs/quickstart/build-your-application)

**NODE.JS**

现在你已经找到了一个好的提示和设置，你已经准备好构建你的爱称生成器了！ 我们已经编写了一些代码来帮助您入门——按照下面的说明下载代码并运行应用程序。

[设置](https://platform.openai.com/docs/quickstart/setup)

如果您没有安装 Node.js，[从此处安装](https://nodejs.org/en/)。 然后通过克隆 [this repository](https://github.com/openai/openai-quickstart-node) 下载代码。


```nodejs
git 克隆 https://github.com/openai/openai-quickstart-node.git
```

如果您不想使用 git，也可以使用 [此 zip 文件](https://github.com/openai/openai-quickstart-node/archive/refs/heads/master.zip) 下载代码。

[添加您的 API 密钥](https://platform.openai.com/docs/quickstart/add-your-api-key)

要使应用程序正常运行，您需要一个 API 密钥。 您可以通过 [注册](https://platform.openai.com/signup) 帐户并返回此页面来获得一个。

[运行应用](https://platform.openai.com/docs/quickstart/run-the-app)

在项目目录下运行以下命令安装依赖并运行应用程序。


```nodejs
npm 安装
npm 运行开发
```

在浏览器中打开 [http://localhost:3000](http://localhost:3000/)，您应该会看到昵称生成器！

[理解代码](https://platform.openai.com/docs/quickstart/understand-the-code)

在 `openai-quickstart-node/pages/api` 文件夹中打开 `generate.js`。 在底部，您会看到生成我们在上面使用的提示的函数。 由于用户将输入他们宠物的动物类型，因此它会动态换出指定动物的提示部分。



```nodejs
函数 generatePrompt（动物）{
   const capitalizedAnimal = animal[0].toUpperCase() + animal.slice(1).toLowerCase();
   return `为超级英雄的动物建议三个名字。

动物：猫
姓名：尖爪船长、绒毛球特工、不可思议的猫科动物
动物：狗
名称：Ruff the Protector、Wonder Canine、Sir Barks-a-Lot
动物：${capitalizedAnimal}
姓名：`;
}
```

在 `generate.js` 的第 9 行，您将看到发送实际 API 请求的代码。 如上所述，它使用温度为 0.6 的 [completions](https://platform.openai.com/docs/api-reference/completions) 端点。



```nodejs
const completion = await openai.createCompletion({
   模型：“text-davinci-003”，
   提示：generatePrompt(req.body.animal),
   温度：0.6，
});
```

就是这样！ 您现在应该完全了解您的（超级英雄）宠物名称生成器如何使用 OpenAI API！

---
**PYTHON(FLASK)**

现在你已经找到了一个好的提示和设置，你已经准备好构建你的爱称生成器了！ 我们已经编写了一些代码来帮助您入门——按照下面的说明下载代码并运行应用程序。

[设置](https://platform.openai.com/docs/quickstart/setup)

如果您没有安装 Python，[从此处安装](https://www.python.org/downloads/)。 然后通过克隆 [this repository](https://github.com/openai/openai-quickstart-python) 下载代码。

```python
git 克隆 https://github.com/openai/openai-quickstart-python.git
```

如果您不想使用 git，也可以使用 [此 zip 文件](https://github.com/openai/openai-quickstart-python/archive/refs/heads/master.zip) 下载代码。

[添加您的 API 密钥](https://platform.openai.com/docs/quickstart/add-your-api-key)

要使应用程序正常运行，您需要一个 API 密钥。 您可以通过 [注册](https://platform.openai.com/signup) 帐户并返回此页面来获得一个。

[运行应用](https://platform.openai.com/docs/quickstart/run-the-app)

运行以下命令以安装依赖并运行应用程序。运行命令时，您可能需要根据您的设置键入`python3`/`pip3`而不是`python`/`pip`。

```python
1 python -m venv venv
2 . venv/bin/activate
3 pip install -r requirements.txt
4 flask run
```

在浏览器中打开[http://localhost:5000](http://localhost:5000/)，您应该会看到宠物名字生成器！

[了解代码](https://platform.openai.com/docs/quickstart/understand-the-code)

在`openai-quickstart-python`文件夹中打开`app.py`文件。在底部，您将看到生成我们之前使用的提示的功能。由于用户将输入他们的宠物是哪种动物，它会动态更换指定动物部分的提示。

```python
1 def generate_prompt(animal):
    return """Suggest three names for an animal that is a superhero.

2 Animal: Cat
3 Names: Captain Sharpclaw, Agent Fluffball, The Incredible Feline
4 Animal: Dog
5 Names: Ruff the Protector, Wonder Canine, Sir Barks-a-Lot
6 Animal: {}
7 Names:""".format(animal.capitalize())
```

在`app.py`的第14行，您将看到发送实际API请求的代码。如上所述，它使用[completions](https://platform.openai.com/docs/api-reference/completions)端点，温度设为0.6。

```python
response = openai.Completion.create(
  model="text-davinci-003",
  prompt=generate_prompt(animal),
  temperature=0.6
)
```

好了！现在您应该完全了解您的(超级英雄)宠物名称生成器如何使用OpenAI API！


[定价](https://platform.openai.com/docs/quickstart/pricing)

我们提供一系列具有不同能力和[价格点](https://openai.com/pricing/)的[模型](https://platform.openai.com/docs/models)。在本教程中，我们使用了`text-davinci-003`。我们建议在实验中使用此模型或`gpt-3.5-turbo`，因为它们会产生最佳结果。一旦您开始运作，您可以尝试看看其他模型是否可以以更低的延迟和成本生成相同的结果。或者，如果您需要升级到更强大的模型，如`gpt-4`。

一次请求中处理的令牌总数（提示和完成）不能超过模型的最大上下文长度。对于大多数模型，这是4,096个令牌或大约3,000个单词。作为粗略的经验法则，对于英文文本，1个令牌大约是4个字符或0.75个单词。

定价是按每1,000个令牌付费的，前3个月可以使用价值5美元的免费信用额度。[了解更多](https://openai.com/pricing/)。

[关闭](https://platform.openai.com/docs/quickstart/closing)

这些概念和技术将帮助您构建自己的应用程序，但是这个简单的示例只展示了一小部分可能性！完成端点足够灵活，可以解决几乎任何语言处理任务，包括内容生成、总结、语义搜索、主题标记、情感分析等等。

需要注意的一个限制是，对于大多数模型，单个API请求只能在您的提示和完成之间处理最多4,096个标记。

对于更高级的任务，您可能会希望提供更多的示例或上下文，而这是单个提示所容不下的。  [微调API](https://platform.openai.com/docs/guides/fine-tuning) 是更高级任务的一个很好的选择。微调允许您提供数百甚至数千个示例来自定义模型，以适应您特定的用例。

[下一步](https://platform.openai.com/docs/quickstart/next-steps)

要获得灵感并了解如何为不同任务设计提示，请尝试以下方法：

- 阅读我们的[GPT指南](https://platform.openai.com/docs/guides/gpt)。
- 浏览我们的[示例提示](https://platform.openai.com/examples)库。
- 开始在[Playground](https://platform.openai.com/playground)中实验。
- 开始构建时，请牢记我们的[使用政策](https://openai.com/policies/usage-policies)。

寻找ChatGPT？请前往[chat.openai.com](https://chat.openai.com/)。



