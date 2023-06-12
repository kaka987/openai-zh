[生产最佳实践](https://platform.openai.com/docs/guides/production-best-practices/production-best-practices)

本指南提供一整套最佳实践，以帮助您从原型转向生产。无论您是经验丰富的机器学习工程师还是新近的爱好者，本指南应该为您提供成功将该平台投入生产环境所需的工具：从保护对 API 的访问到设计一个可以处理高流量的稳健架构。使用本指南来帮助制定一个计划，尽可能平稳有效地部署您的应用程序。

[设置您的组织](https://platform.openai.com/docs/guides/production-best-practices/setting-up-your-organization)

一旦您 [登录](https://platform.openai.com/login) 到您的 OpenAI 帐户，您可以在 [组织设置](https://platform.openai.com/account/org-settings) 中找到您的组织名称和 ID。组织名称是您组织的标签，在用户界面中显示。组织 ID 是您组织的唯一标识符，可用于 API 请求。

属于多个组织的用户可以 [传递标头](https://platform.openai.com/docs/api-reference/requesting-organization) 来指定用于 API 请求的组织。这些 API 请求的使用量将计入指定组织的配额。如果未提供标头，则会计费[默认组织](https://platform.openai.com/account/api-keys)。您可以在 [用户设置](https://platform.openai.com/account/api-keys) 中更改您的默认组织。

您可以从 [成员](https://platform.openai.com/account/members) 设置页面邀请新成员加入您的组织。成员可以是**读者**或**所有者**。读者可以进行 API 请求并查看基本组织信息，而所有者可以修改账单信息并管理组织内的成员。

[管理账单限制](https://platform.openai.com/docs/guides/production-best-practices/managing-billing-limits)

新的免费试用用户可以获得$5的初始信用额度，有效期为三个月。使用完信用额度或到期之后，您可以选择输入 [账单信息](https://platform.openai.com/account/billing/overview) 来继续使用 API。如果未输入任何账单信息，则仍然可以登录但无法进行进一步的 API 请求。

一旦输入了您的账单信息，OpenAI 将为您设置每月$120的已批准使用配额上限。如果您想增加配额限制超过$120的月度账单限制，请提交一个 [配额增加请求](https://platform.openai.com/forms/quota-increase)。

如果您想在使用量超出一定数量时收到通知，您可以通过 [使用限制](https://platform.openai.com/account/billing/limits) 页面设置一个软限制。当软限制达到时，组织所有者将收到电子邮件通知。您还可以设置一个硬限制，以便一旦达到硬限制，任何后续的 API 请求都将被拒绝。请注意，这些限制是尽力而为的，使用和限制之间可能会有5到10分钟的延迟。

[API 密钥](https://platform.openai.com/docs/guides/production-best-practices/api-keys)

OpenAI API 使用 API 密钥进行认证。访问您的 [API 密钥](https://platform.openai.com/account/api-keys) 页面以检索在请求中使用的 API 密钥。

这是一种相对简单的方式来控制访问，但必须对保护这些密钥保持警惕。避免在代码或公共资源库中公开 API 密钥；相反，将它们存储在安全位置。应使用环境变量或密钥管理服务向应用程序公开密钥，以便您不需要在代码库中硬编码它们。在我们的 [API 密钥安全最佳实践](https://help.openai.com/en/articles/5112595-best-practices-for-api-key-safety) 中阅读更多相关内容。

[暂存账户](https://platform.openai.com/docs/guides/production-best-practices/staging-accounts)

随着业务规模的扩大，您可能希望为暂存和生产环境创建单独的组织。请注意，您可以通过使用两个不同的电子邮件地址，例如 [bob+prod@widgetcorp.com](mailto:bob+prod@widgetcorp.com) 和 [bob+dev@widgetcorp.com](mailto:bob+dev@widgetcorp.com) 来注册并创建两个组织。这将使您能够隔离开发和测试工作，以免意外干扰生产环境应用程序。这种方式还可以限制对您的生产组织的访问。

[扩展您的解决方案架构](https://platform.openai.com/docs/guides/production-best-practices/scaling-your-solution-architecture)

在为使用我们的 API 的生产应用程序或服务设计时，考虑如何扩展以满足流量需求是非常重要的。无论您选择哪个云服务提供商，都需要考虑几个关键领域：

- **水平扩展**：您可能需要对应用程序进行水平扩展，以容纳来自多个源的应用程序请求。这可能涉及部署其他服务器或容器以分发负载。如果选择此类扩展方式，请确保您的架构设计能够处理多个节点，并且您已经设置了平衡负载的机制。
- **垂直扩展**：另一种选择是对应用程序进行垂直扩展，这意味着您可以增加单个节点可用的资源。这将涉及升级服务器的功能以处理额外的负载。如果选择此类扩展方式，请确保您的应用程序能够利用这些额外的资源。
- **缓存**：通过存储频繁访问的数据，可以改善响应时间，而无需重复调用我们的 API。您的应用程序需要设计成尽可能使用缓存数据，并在添加新信息时使缓存失效。您可以使用几种不同的方式来实现这一点。例如，可以根据应用程序的实际情况将数据存储在数据库、文件系统或内存缓存中。
- **负载均衡**：最后，考虑负载均衡技术，以确保请求平均分配在可用服务器之间。这可能涉及在服务器前使用负载均衡器或使用 DNS 轮询。平衡负载有助于提高性能并减少瓶颈。

[管理速率限制](https://platform.openai.com/docs/guides/production-best-practices/managing-rate-limits)

在使用我们的 API 时，重要的是要理解和计划好[速率限制](https://platform.openai.com/docs/guides/rate-limits)。

[提高延迟性](https://platform.openai.com/docs/guides/production-best-practices/improving-latencies)

延迟是指请求被处理并返回响应所需的时间。在本节中，我们将讨论影响我们的文本生成模型延迟的一些因素，并提供有关如何减少延迟的建议。

完成请求的延迟大多由两个因素影响：模型和生成的令牌数量。完成请求的生命周期如下：

网络

终端用户到 API 延迟

服务器

处理提示令牌的时间

服务器

采样/生成令牌的时间

网络

API 到终端用户延迟



大部分延迟通常来自令牌生成步骤。

> **直觉**: 提示令牌对于完成调用的延迟影响很小。生成完成令牌的时间更长，因为令牌是逐个生成的。由于每个令牌都需要生成，因此更长的生成长度会积累延迟。

[影响延迟的常见因素和可能的缓解技术](https://platform.openai.com/docs/guides/production-best-practices/common-factors-affecting-latency-and-possible-mitigation-techniques)

现在我们已经了解了延迟的基础知识，让我们来看一下各种可能影响延迟的因素，从对延迟影响最大到影响最小进行广泛排序。

[模型](https://platform.openai.com/docs/guides/production-best-practices/model)

我们的 API 提供具有不同复杂性和广度水平的不同模型。最能胜任的模型，例如`gpt-4`，可以生成更复杂和多样化的完成情况，但它们也需要更长时间处理您的查询。像`gpt-3.5-turbo`这样的模型可以生成更快更便宜的聊天完成情况，但它们可能会生成与您的查询不太准确或相关的结果。您可以选择最适合您使用情况和速度与质量之间权衡的模型。

[完成令牌的数量](https://platform.openai.com/docs/guides/production-best-practices/number-of-completion-tokens)

请求大量生成的令牌完成情况可能会导致延迟增加：

- **降低最大令牌数**：对于具有类似令牌生成计数的请求，那些具有较低的`max_tokens`参数会产生较少的延迟。
- **包括停止序列**：要防止生成不需要的令牌，请添加停止序列。 例如，您可以使用停止序列来生成具有特定数量项目的列表。 在这种情况下，通过使用`11。 [阅读我们有关停止序列的帮助文章](https://help.openai.com/en/articles/5072263-how-do-i-i-is-s-stop-sequences)，以获取有关如何执行此操作的更多上下文。
- **生成较少的完成项**：在可能的情况下降低`n`和`best_of`的值，其中`n`表示为每个提示生成多少个完成项，`best_of`用于表示每个令牌的最高对数概率结果。

如果`n`和`best_of`都等于 1（这是默认值），则生成的令牌数量最多等于`max_tokens`。

如果`n`（返回的完成数）或`best_of`（考虑生成的完成数）设置为`>1`，那么每个请求将创建多个输出。在这里，您可以将生成的令牌数视为`[ max_tokens * max(n, best_of) ]`

[流式传输](https://platform.openai.com/docs/guides/production-best-practices/streaming)

在请求中设置`stream:true`可使模型在令牌可用时立即返回它们，而不是等待生成完整的令牌序列。这不会改变获取所有令牌的时间，但它可以减少适用于显示部分进度或将停止生成的应用程序的第一个令牌的时间。这可能会提高用户体验和 UX，因此值得尝试流式处理。

[基础设施](https://platform.openai.com/docs/guides/production-best-practices/infrastructure)

我们的服务器目前位于美国。虽然我们希望未来能够实现全球冗余，但在此期间，您可以考虑将基础设施的相关部分定位在美国以最小化您的服务器和 OpenAI 服务器之间的往返时间。

[批处理](https://platform.openai.com/docs/guides/production-best-practices/batching)

根据您的用例，批处理*可能有所帮助*。如果您向同一个端点发送多个请求，可以[将提示批处理](https://platform.openai.com/docs/guides/rate-limits/batching-requests)到同一个请求中发送。这将减少所需进行的请求次数。prompt 参数最多可以容纳 20 个不同的提示。我们建议您测试这种方法，看看它是否有所帮助。在某些情况下，您可能会增加生成的令牌数量，从而降低响应时间。

[成本管理](https://platform.openai.com/docs/guides/production-best-practices/managing-costs)

为了监控您的成本，您可以在帐户中设置软限制，一旦您超过某个使用阈值就会收到电子邮件警报。您还可以设置硬限制。请注意，硬限制可能会导致应用程序/用户受到干扰。使用[使用跟踪仪表板](https://platform.openai.com/account/usage)监视您在当前和过去的帐单周期中的令牌使用情况。

[文本生成](https://platform.openai.com/docs/guides/production-best-practices/text-generation)

从原型转移到生产的挑战之一是为运行应用程序所需的成本制定预算。OpenAI 提供[按使用付费的定价模型](https://openai.com/api/pricing/)，按每 1,000 个令牌的价格（大致等于 750 个单词）计费。为了估算您的成本，您需要预测令牌利用率。考虑诸如流量水平，用户与应用程序交互的频率以及您将处理的数据量等因素。

**一个有用的框架可以将成本视为令牌数和每个令牌的成本的函数来考虑降低成本的方法。**使用这个框架，有两种降低成本的方法。首先，您可以通过为某些任务切换到较小的模型来减少每个令牌的成本以降低成本。此外，您可以尝试减少所需的令牌数量。您可以通过使用较短的提示，[微调](https://platform.openai.com/docs/guides/fine-tuning)模型或缓存常见的用户查询等方式来做到这一点，以使它们不需要被重复处理。

您可以使用我们的交互式[分词工具](https://platform.openai.com/tokenizer)来帮助您估算成本。 API 和 Playground 还将令牌计数作为响应的一部分返回。一旦您使用我们最强大的模型，您可以看看其他模型是否可以以更低的延迟和成本产生相同的结果。在我们的[令牌使用帮助文章](https://help.openai.com/en/articles/6614209-how-do-i-check-my-token-usage)中了解更多信息。

[MLOps 策略](https://platform.openai.com/docs/guides/production-best-practices/mlops-strategy)

在将原型转移到生产环境时，您可能想考虑开发一个 MLOps 策略。MLOps（机器学习运营）是指管理机器学习模型的端到端生命周期的过程，包括使用我们的 API 进行微调的任何模型。设计 MLOps 策略时需要考虑一些方面。其中包括：

- 数据和模型管理：管理用于训练或微调您的模型的数据，并跟踪版本和更改。
- 模型监控：跟踪您的模型的性能，检测任何潜在的问题或退化。
- 模型再训练：确保您的模型随着数据变化或不断发展的需求保持最新，并根据需要进行再训练或微调。
- 模型部署：自动化将您的模型和相关工件部署到生产环境的过程。

思考应用程序的这些方面有助于确保您的模型随着时间的推移保持相关并具有良好的性能。

[安全合规](https://platform.openai.com/docs/guides/production-best-practices/security-and-compliance)

将原型转移到生产环境时，您需要评估和解决可能适用于您的应用程序的任何安全和合规要求。这将涉及到检查您正在处理的数据，了解我们的API如何处理数据，并确定您必须遵守哪些法规。我们的[安全实践](https://www.openai.com/security)和[信任及合规门户](https://trust.openai.com/)提供我们最全面和最新的文档。供参考，这里是我们的[隐私政策](https://openai.com/privacy/)和[使用条款](https://openai.com/api/policies/terms/)。

一些需要考虑的常见领域包括数据存储、数据传输和数据保留。您可能还需要在可能的情况下实现数据隐私保护，例如加密或匿名化。此外，您应该遵循安全编码的最佳实践，例如输入数据消毒和适当的错误处理。

[安全最佳实践](https://platform.openai.com/docs/guides/production-best-practices/safety-best-practices)

在使用我们的API创建应用程序时，请考虑我们的[安全最佳实践](https://platform.openai.com/docs/guides/safety-best-practices)，以确保您的应用程序安全可靠。这些建议强调了广泛测试产品的重要性，采取积极措施解决潜在问题以及限制滥用机会。