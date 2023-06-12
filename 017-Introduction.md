[聊天插件 Beta](https://platform.openai.com/docs/plugins/introduction/chat-plugins-beta)

学习如何构建插件，使 ChatGPT 可以智能调用您的 API。

[简介](https://platform.openai.com/docs/plugins/introduction/introduction)

OpenAI 插件将 ChatGPT 与第三方应用程序连接起来。这些插件使 ChatGPT 能够与开发人员定义的 API 交互，增强 ChatGPT 的功能，使它能够执行各种操作。插件使 ChatGPT 能够做到以下几点：

- 检索实时信息：比如体育比分、股票价格、最新新闻等。
- 检索知识库信息，比如公司文档、个人笔记等。
- 协助用户执行操作，比如预订航班、订购食品等。

插件处于 beta 阶段，开发者可能无法访问。请[加入等待名单](https://openai.com/waitlist/plugins)以获得开发者访问权限。在 beta 阶段，我们将与用户和开发者密切合作，迭代插件系统，该系统可能会有明显的变化。

如果您想在阅读文档并了解插件的更多信息时运行示例，可以从我们的[插件快速入门存储库](https://github.com/openai/plugins-quickstart)开始。

插件开发人员会公开一个或多个 API 端点，并附有一个标准化的清单文件和一个 OpenAPI 规范。这些定义了插件的功能，使 ChatGPT 能够使用这些文件并调用开发人员定义的 API。

AI 模型作为智能的 API 调用方。根据 API 规范和关于何时使用 API 的自然语言描述，模型主动调用 API 执行操作。例如，如果用户问：“我应该在巴黎住几晚？”模型可能会选择调用一个酒店预订插件 API，接收 API 响应，并生成一个用户的答案，结合 API 数据和其自然语言能力。

随着时间的推移，我们预计该系统将发展以适应更先进的用例。

[插件流程](https://platform.openai.com/docs/plugins/introduction/plugin-flow)

要构建一个插件，了解端到端的流程非常重要。

1.创建一个清单文件，并将其托管在 `yourdomain.com/.well-known/ai-plugin.json`

- 文件包括有关插件的元数据（名称、标志等）、需要的身份验证详细信息（身份验证类型、OAuth URL 等）以及要暴露的端点的 OpenAPI 规范。
- 模型将看到 OpenAPI 描述字段，可以用它来提供不同字段的自然语言描述。
- 我们建议在开始时仅公开 1-2 个端点，使用最少量的参数来最小化文本长度。插件描述、API 请求和 API 响应都会插入到与 ChatGPT 的对话中。这会占用模型的上下文限制。

2.在 [ChatGPT UI](https://chat.openai.com/) 中注册您的插件

- 从顶部下拉菜单中选择插件模型，然后选择“插件”、“插件商店”，最后选择“开发自己的插件”。
- 如果需要验证，请提供 OAuth2 的 `client_id` 和 `client_secret` 或 API 密钥。

3.用户激活您的插件

- 用户必须在 ChatGPT UI 中手动激活您的插件。（默认情况下，ChatGPT 不会使用您的插件。）
- 您可以与100个其他用户分享您的插件（只有其他开发人员可以安装未经验证的插件）。
- 如果需要验证，用户将通过 OAuth 被重定向到您的插件以登录。

4.用户开始对话

- OpenAI 会在一条消息中向 ChatGPT 注入一个简洁的插件描述，对最终用户不可见。其中包括插件描述、端点和示例。
- 当用户提出相关问题时，如果似乎有必要，模型可能会选择调用您的插件的 API 访问。对于 POST 请求，我们要求开发人员构建用户确认流程，以避免破坏操作。
- 模型将将 API 调用的结果合并到其对用户的响应中。
- 模型可能会在其响应中包含从 API 调用返回的链接。这些将显示为富预览（使用 [OpenGraph 协议](https://ogp.me/)，我们提取 site_name、title、description、image 和 url 字段）。
- 模型还可以在 markdown 中格式化来自您的 API 的数据，ChatGPT UI 会自动呈现 markdown。

目前，我们将用户的国家和州发送到插件会话标题中（例如，如果您在加利福尼亚州，它将显示为`{"openai-subdivision-1-iso-code": "US-CA"}`）。这对购物、餐厅、天气等非常有用。您可以在我们的[开发者使用条款](http://openai.com/policies/plugin-terms)中了解更多信息。

[下一步](https://platform.openai.com/docs/plugins/introduction/next-steps)

现在您已经了解了插件的基础知识，您可能想要：

- [开始构建插件](https://platform.openai.com/docs/plugins/getting-started)
- 探索[示例插件](https://platform.openai.com/docs/plugins/examples)
- 阅读[将插件投入生产的重要步骤](https://platform.openai.com/docs/plugins/production)
- 了解[插件审核流程](https://platform.openai.com/docs/plugins/review)