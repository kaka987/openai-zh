[内容审核](https://platform.openai.com/docs/guides/moderation/moderation)

[概述](https://platform.openai.com/docs/guides/moderation/overview)

[审核](https://platform.openai.com/docs/api-reference/moderations) 是一个工具，可用于检查内容是否符合 OpenAI 的[使用政策](https://openai.com/policies/usage-policies)。开发者因此可以识别出我们的使用政策所禁止的内容，并采取行动，例如过滤它。

模型分类以下类别：

| 类别                                    | 描述                                                         |
| :-------------------------------------- | :----------------------------------------------------------- |
| `仇恨`                                  | 本类别包括因种族、性别、族裔、宗教、国籍、性取向、残疾状况或种姓而表达、煽动或宣传仇恨的内容。针对非受保护群体（如国际象棋玩家）的仇恨内容*不*包含在此类别中。 |
| `仇恨/威胁`                             | 包含对针对性群体的暴力或严重伤害的仇恨内容。                 |
| `自残`                                  | 宣传、鼓励或描绘自我伤害行为，如自杀、自残和饮食失调的内容。 |
| `性别的/性行为的`                       | 旨在引起性兴奋的内容，如性行为描述或宣传性服务（不包括性教育和健康）的内容。 |
| `未成年人的性行为/涉及未成年人的性行为` | 包含未满18岁个体的性内容。                                   |
| `暴力`                                  | 宣传或美化暴力、庆祝他人痛苦或羞辱的内容。                   |
| `暴力/过于生动或血腥描写的内容`         | 详细描绘死亡、暴力或严重身体伤害的暴力内容。                 |

当监控 OpenAI API 的输入和输出时，审查终端点是免费使用的。我们目前禁止其他使用情况。



我们正在不断努力提高分类器的准确性，特别是在改进对“仇恨(hate)”，“自残(self-harm)”和“暴力/图形(violence/graphic)”内容的分类方面。我们对非英语语言的支持目前还比较有限。

长文本的准确性可能会偏低。为了提高准确性，请尝试将长文本分成小于2,000个字符的较小块。

[Quickstart](https://platform.openai.com/docs/guides/moderation/quickstart)

要获取一段文本的分类，请向 [moderation 端点](https://platform.openai.com/docs/api-reference/moderations) 发送请求，如下面的代码片段所示：

示例：获取审查结果

curl



复制

```shell
curl https://api.openai.com/v1/moderations \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{"input": "Sample text goes here"}'
```

以下是终端节点的示例输出。它返回以下字段：

- `flagged`：如果模型将内容分类为违反 OpenAI 使用政策，则设置为 `true`，否则为 `false`。
- `categories`：包含每个类别的二进制使用政策违规标志的字典。对于每个类别，如果模型将相应的类别标记为违规，则该值为 `true`，否则为 `false`。
- `category_scores`：包含模型输出的每个类别的原始分数字典，表示模型对输入违反 OpenAI 对该类别的政策的信心程度。值介于 0 和 1 之间，其中较高的值表示更高的置信度。这些分数不应被解释为概率。

```python
{
  "id": "modr-XXXXX",
  "model": "text-moderation-001",
  "results": [
    {
      "categories": {
        "hate": false,
        "hate/threatening": false,
        "self-harm": false,
        "sexual": false,
        "sexual/minors": false,
        "violence": false,
        "violence/graphic": false
      },
      "category_scores": {
        "hate": 0.18805529177188873,
        "hate/threatening": 0.0001250059431185946,
        "self-harm": 0.0003706029092427343,
        "sexual": 0.0008735615410842001,
        "sexual/minors": 0.0007470346172340214,
        "violence": 0.0041268812492489815,
        "violence/graphic": 0.00023186142789199948
      },
      "flagged": false
    }
  ]
}
```



OpenAI将不断升级内容审核终端节点的基础模型。因此，依赖于`category_scores`的自定义政策可能需要随着时间的推移进行重新校准。