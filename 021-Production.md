[插件在生产环境中](https://platform.openai.com/docs/plugins/production/plugins-in-production)

[速率限制](https://platform.openai.com/docs/plugins/production/rate-limits)

请考虑在您公开的API端点上实施速率限制。ChatGPT将遵守429响应代码，并在短时间内接收一定数量的429或500之后动态停止向您的插件发送请求。

[超时](https://platform.openai.com/docs/plugins/production/timeouts)

在插件体验期间进行API调用时，如果超过以下阈值，则会发生超时：

- 15秒往返获取`ai-plugin.json`/ `openapi.yaml`
- 45秒往返进行API调用

随着我们将插件体验扩展到更多人员，我们预计超时阈值会降低。

[更新您的插件](https://platform.openai.com/docs/plugins/production/updating-your-plugin)

每次更改`ai-plugin.json`文件时，必须通过插件商店中的“开发自己的插件”流程手动更新清单文件。每次发出请求时，ChatGPT将自动获取最新的OpenAPI规范，因此所做的更改将立即传播到最终用户。如果您的插件在ChatGPT插件商店中可用，并且您通过“开发自己的插件”流程，我们将自动查找文件中的更改并删除插件（如果有更改）。您需要重新提交您的插件才能再次包含在商店中。

[插件条款](https://platform.openai.com/docs/plugins/production/plugin-terms)

为了注册插件，您必须同意[插件条款](http://openai.com/policies/plugin-terms)。

[域验证和安全性](https://platform.openai.com/docs/plugins/production/domain-verification-and-security)

为了确保插件只能在它们控制的资源上执行操作，OpenAI对插件的清单文件和API规范实施要求。

[定义插件的根域](https://platform.openai.com/docs/plugins/production/defining-the-plugin-s-root-domain)

清单文件定义了显示给用户的信息（如标志和联系信息），以及插件的OpenAPI规范托管的URL。当清单被获取时，根据以下规则建立插件的根域：

- 如果域名具有`www.`作为子域，则根域将从托管清单的域中去掉`www.`
- 否则，根域与托管清单的域相同。

关于重定向的说明：如果在解析清单时存在任何重定向，则只允许子域重定向。唯一的例外是从带有www子域的重定向到没有www的子域。

根域看起来像什么的示例：

- ✅

   

  ```
  https://example.com/.well-known/ai-plugin.json
  ```

  - 根域: `example.com`

- ✅

   

  ```
  https://www.example.com/.well-known/ai-plugin.json
  ```

  - 根域: `example.com`

- ✅

   

  ```
  https://www.example.com/.well-known/ai-plugin.json
  ```

   

  → 重定向到

   

  ```
  https://example.com/.well-known/ai-plugin.json
  ```

  - 根域: `example.com`

- ✅

   

  ```
  https://foo.example.com/.well-known/ai-plugin.json
  ```

   

  → 重定向到

   

  ```
  https://bar.foo.example.com/.well-known/ai-plugin.json
  ```

  - 根域: `bar.foo.example.com`

- ✅

   

  ```
  https://foo.example.com/.well-known/ai-plugin.json
  ```

   

  → 重定向到

   

  ```
  https://bar.foo.example.com/baz/ai-plugin.json
  ```

  - 根域: `bar.foo.example.com`

- ❌

   

  ```
  https://foo.example.com/.well-known/ai-plugin.json
  ```

   

  →重定向到

   

  ```
  https://example.com/.well-known/ai-plugin.json
  ```

  - 不允许重定向到父级域名

- ❌

   

  ```
  https://foo.example.com/.well-known/ai-plugin.json
  ```

   

  →重定向到

   

  ```
  https://bar.example.com/.well-known/ai-plugin.json
  ```

  - 不允许重定向到同一级别的子域名

- ❌

   

  ```
  https://example.com/.well-known/ai-plugin.json
  ```

   

  -> 重定向到

   

  ```
  https://example2.com/.well-known/ai-plugin.json
  ```

  - 不允许重定向到另一个域名

[插件清单验证](https://platform.openai.com/docs/plugins/production/manifest-validation)

插件清单中的特定字段必须满足以下要求：

- `api.url` - 提供给 OpenAPI 规范的 URL 必须托管在与根域相同级或更低的子域名。
- `legal_info` - 所提供URL的 [二级域名](https://en.wikipedia.org/wiki/Second-level_domain)必须与根域名的二级域名相同。 
- `contact_info` - 邮箱地址的二级域名应与根域名的二级域名相同。

[解析API规范](https://platform.openai.com/docs/plugins/production/resolving-the-api-spec)

清单中的`api.url`字段提供了一个链接到定义插件可调用的API的OpenAPI规范。OpenAPI允许指定多个[服务器基础URL](https://swagger.io/docs/specification/api-host-and-base-path/)。以下逻辑用于选择服务器URL：

- 遍历服务器URL列表。
- 使用根域的精确匹配或子域中的第一个服务器URL。
- 如果以上情况都不适用，则默认使用API规范托管的域。例如，如果规范托管在`api.example.com`上，则将`api.example.com`用作OpenAPI规范中路由的基础URL。

请注意：请不要使用重定向来托管 API 规范和任何 API 端点，因为不能保证重定向始终会被遵循。

[使用 TLS 和 HTTPS](https://platform.openai.com/docs/plugins/production/use-tls-and-https)

与插件的所有交通（例如获取 `ai-plugin.json` 文件、OpenAPI 规范、API 调用）都必须使用端口 443 上的 TLS 1.2 或更高版本和有效的公共证书。

[IP 出口范围](https://platform.openai.com/docs/plugins/production/ip-egress-ranges)

ChatGPT 将从 [CIDR 块](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) `23.102.140.112/28` 的 IP 地址中调用你的插件。你可能希望显式地将这些 IP 地址列入白名单。

另外，OpenAI的Web浏览插件[访问网站](https://platform.openai.com/docs/plugins/bot)来自其他IP地址块：`23.98.142.176/28`。

[FAQ](https://platform.openai.com/docs/plugins/production/faq)

[我的插件数据是怎样使用的？](https://platform.openai.com/docs/plugins/production/how-is-plugin-data-used)

插件将 ChatGPT 连接到外部应用程序。如果用户启用了插件，则 ChatGPT 可能会将他们的对话部分和所在国家或地区发送到插件。

[如果我的 API 请求失败会发生什么？](https://platform.openai.com/docs/plugins/production/what-happens-if-a-request-to-my-api-fails)

如果 API 请求失败，模型可能会在告知用户无法获取插件的响应之前重试该请求最多 10 次。

[我能邀请人来试用我的插件吗？](https://platform.openai.com/docs/plugins/production/can-i-invite-people-to-try-my-plugin)

可以，所有未经验证的插件都可以被最多 100 名具有插件访问权限的其他开发人员安装。如果你的插件在插件商店中可用，所有 ChatGPT Plus 订阅用户都可以访问它。

[我能为我的插件收费吗？](https://platform.openai.com/docs/plugins/production/can-i-charge-people-money-for-my-plugin)

目前不行。

