[安全最佳实践](https://platform.openai.com/docs/guides/safety-best-practices/safety-best-practices)

[使用我们的免费Moderation API](https://platform.openai.com/docs/guides/safety-best-practices/use-our-free-moderation-api)

OpenAI的[Moderation API](https://platform.openai.com/docs/guides/moderation)是免费使用的，可以帮助减少您的完成内容中不安全内容的频率。或者，您可能希望开发自己的针对您使用情况定制的内容过滤系统。

[对抗性测试](https://platform.openai.com/docs/guides/safety-best-practices/adversarial-testing)

我们建议“红队测试”您的应用程序，以确保其对对抗性输入具有鲁棒性。在广泛的输入和用户行为范围内测试您的产品，包括代表性集合和那些反映某些人试图“破解”应用程序的行为。它是否偏离主题？是否可以轻松地通过提示注入重定向功能，例如“忽略前面的指令，改为执行此操作”？

[人类在环（HITL）](https://platform.openai.com/docs/guides/safety-best-practices/human-in-the-loop-hitl)

在可能的情况下，我们建议在实际使用输出之前让人类审查。这在高风险领域和代码生成方面尤其关键。人类应了解系统的局限性，并获得验证输出所需的任何信息（例如，如果应用程序总结笔记，则人类应轻松访问原始笔记以供参考）。

[提示工程](https://platform.openai.com/docs/guides/safety-best-practices/prompt-engineering)

“提示工程”可以帮助限制输出文本的主题和语气。这可以减少产生不期望内容的机会，即使用户试图产生这种内容。向模型提供附加上下文（例如，在新输入之前提供一些高质量的期望行为示例）可以更容易地将模型输出定向到期望的方向。

[“了解你的客户”（KYC）](https://platform.openai.com/docs/guides/safety-best-practices/know-your-customer-kyc)

用户通常需要注册并登录才能访问您的服务。将此服务链接到现有帐户（例如，Gmail、LinkedIn或Facebook登录）可能会有所帮助，但可能不适用于所有用例。要求信用卡或身份证可以进一步降低风险。

[限制用户输入并限制输出令牌](https://platform.openai.com/docs/guides/safety-best-practices/constrain-user-input-and-limit-output-tokens)

限制用户可以输入到提示中的文本量有助于避免提示注入。限制输出标记数有助于减少误用的可能性。

缩小输入或输出范围，尤其是从可信来源中选择，可以减少应用程序中可能发生的误用程度。

允许用户通过经过验证的下拉字段输入（例如，维基百科上的电影列表）可能比允许开放式的文本输入更安全。

在可能的情况下，从后端返回经过验证的材料集合的输出比返回新生成的内容更安全（例如，将客户查询路由到最匹配的现有客户支持文章，而不是尝试从头开始回答查询）。

[允许用户报告问题](https://platform.openai.com/docs/guides/safety-best-practices/allow-users-to-report-issues)

用户通常应该有一个方便易用的方法来报告应用程序功能不正确或其他行为方面的顾虑（如列出的电子邮件地址、工单提交方法等）。这种方法应该由人员监控并根据情况作出回应。

[了解和传达模型限制](https://platform.openai.com/docs/guides/safety-best-practices/understand-and-communicate-limitations)

从产生不准确信息、令人不悦输出、偏见等等，语言模型可能需要重大修改才适用于每个用例。请考虑模型是否适合您的目的，并在各种可能的输入上评估API的性能，以识别API性能可能下降的情况。考虑您的客户群体和他们将使用的输入范围，并确保他们的期望值合理。



在 OpenAI，安全和保障对我们非常重要。

如果在开发过程中您注意到 API 或与 OpenAI 相关的任何其他安全问题，请通过我们的[协调漏洞公开计划(Coordinated Vulnerability Disclosure Program)](https://openai.com/security/disclosure/)提交这些问题。

[发送端用户 IDs](https://platform.openai.com/docs/guides/safety-best-practices/end-user-ids)

在您的请求中发送发送端用户 IDs 可以是一个有用的工具，帮助 OpenAI 监控和检测滥用行为。这使得 OpenAI 能够在我们检测到您的应用程序存在任何政策违规行为时，向您的团队提供更具操作性的反馈。

ID 应该是一个字符串，用于唯一标识每个用户。我们建议使用他们的用户名或电子邮件地址进行哈希，以避免向我们发送任何识别信息。如果您向非登录用户提供产品预览，则可以发送会话 ID。

您可以通过 `user` 参数在 API 请求中包含终端用户 ID，如下所示：

例如：提供用户标识符

Python



复制

```python
response = openai.Completion.create(
  model="text-davinci-003",
  prompt="This is a test",
  max_tokens=5,
  user="user123456"
)
```