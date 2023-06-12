[插件认证](https://platform.openai.com/docs/plugins/authentication/plugin-authentication)

插件提供了多种认证机制来适应不同的用例。要为您的插件指定认证模式，请使用清单文件。我们的[插件域策略](https://platform.openai.com/docs/plugins/production/domain-verification-and-security)概述了我们处理域安全问题的策略。有关可用认证选项的示例，请参阅[示例部分](https://platform.openai.com/docs/plugins/examples)，其中展示了所有不同的选择。

`ai-plugin.json`文件需要设置一个`auth`模式。即使您决定不使用任何认证，仍需要指定`"auth": { "type": "none" }`。

我们仅支持使用本地主机进行开发而无需进行认证，如果要使用服务、用户或OAuth认证，则需要设置远程服务器。

[服务级别](https://platform.openai.com/docs/plugins/authentication/service-level)



我们建议使用服务级别认证，因为它可以使开发人员控制其插件的使用方式，同时也不会给用户增加负担。

如果您想特别允许OpenAI插件与您的API一起工作，可以在插件安装流程中提供一个客户端密钥。这意味着来自OpenAI插件的所有流量都将得到认证，但不是在用户级别上。这种流程有助于简化终端用户体验，但在API层面上却具有较少的控制权。

- 首先，在ChatGPT插件商店中选择“开发您自己的插件”，并输入您的插件托管的域名。
- 在`ai-plugin.json`中，将`auth.type`设置为`"service_http"`，如我们的[服务级别认证示例](https://platform.openai.com/docs/plugins/examples/example-plugins)所示。
- 您将被要求提供服务访问令牌，这是在您的代码中指定的字符串。
  - 我们安全地存储了您的服务访问令牌的加密副本，以实现插件安装而无需进行额外的身份验证。
  - 服务访问令牌在插件请求中以`Authorization`头部的形式发送。
- 一旦您将服务访问令牌添加到ChatGPT UI中，将会呈现一个验证令牌。
- 将验证令牌添加到您的`ai-plugin.json`文件的auth部分中，如下所示。

```json
"auth": {
  "type": "service_http",
  "authorization_type": "bearer",
  "verification_tokens": {
    "openai": "Replace_this_string_with_the_verification_token_generated_in_the_ChatGPT_UI"
  }
},
```

验证令牌设计用于支持多个应用程序。您可以简单地添加您希望插件支持的其他应用程序：

```json
"verification_tokens": {
    "openai": "Replace_this_string_with_the_verification_token_generated_in_the_ChatGPT_UI",
    "other_service": "abc123"
  }
```

插件协议支持OAuth。我们期望的OAuth流程的简单示例应该与以下内容类似：

- 首先，在ChatGPT插件商店中选择“开发您自己的插件”，并输入您的插件托管的域名（不能是localhost）。
- 在`ai-plugin.json`中，将`auth.type`设置为`"oauth"`，如我们的[OAuth示例](https://platform.openai.com/docs/plugins/examples/example-plugins)所示。
- 然后，您将被提示输入OAuth客户端ID和客户端密钥。
  - 客户端ID和密钥可以是简单的文本字符串，但应该[遵循OAuth最佳实践](https://www.oauth.com/oauth2-servers/client-registration/client-id-secret/)。
  - 我们存储客户端密钥的加密版本，而客户端ID可供终端用户使用。
- 一旦您将客户端ID和客户端密钥添加到ChatGPT UI中，将呈现一个验证令牌。
- 将验证令牌添加到您的`ai-plugin.json`文件的auth部分中，如下所示。
- OAuth请求将包括以下信息：`request={'grant_type': 'authorization_code', 'client_id': 'id_set_by_developer', 'client_secret': 'secret_set_by_developer', 'code': 'abc123', 'redirect_uri': 'https://chat.openai.com/aip/plugin-some_plugin_id/oauth/callback'}`
- 为了让其他人使用带有OAuth的插件，他们需要安装插件，然后在ChatGPT UI中呈现“与...登录”按钮。
- `authorization_url`端点应返回一个响应，看起来像：`{ "access_token": "example_token", "token_type": "bearer", "refresh_token": "example_token", "expires_in": 59, }`
- 在用户登录过程中，ChatGPT使用指定的`authorization_content_type`向您的`authorization_url`发出请求，我们期望得到访问令牌，以及可选的[刷新令牌](https://auth0.com/learn/refresh-tokens)，我们用来定期获取新的访问令牌。
- 每次用户对插件发出请求时，将传递用户的令牌Authorization header中：（“Authorization”：“[Bearer/Basic][user’s token]”）。

下面是`ai-plugin.json`文件中OAuth配置的示例：

```json
"auth": {
  "type": "oauth",
  "client_url": "https://example.com/authorize",
  "scope": "",
  "authorization_url": "https://example.com/auth/",
  "authorization_content_type": "application/json",
  "verification_tokens": {
    "openai": "Replace_this_string_with_the_verification_token_generated_in_the_ChatGPT_UI"
  }
},
```

为了更好地理解OAuth的URL结构，以下是一些字段的简短描述：

- 当您使用ChatGPT设置插件时，您将被要求提供OAuth的`client_id`和`client_secret`。
- 当用户登录插件时，ChatGPT会将用户的浏览器重定向到`"[client_url]?response_type=code&client_id=[client_id]&scope=[scope]&redirect_uri=https%3A%2F%2Fchat.openai.com%2Faip%2F[plugin_id]%2Foauth%2Fcallback"`
- `plugin_id`通过向您的OAuth端点发出的请求传递（请注意，它今天在ChatGPT UI中不可见，但将来可能可见）。您可以在那里检查请求以查看`plugin_id`。
- 当您的插件重定向回给定的`redirect_uri`后，ChatGPT会使用授权类型`authorization_content_type`和参数`{"grant_type": "authorization_code", "client_id": [client_id], "client_secret": [client_secret], "code": [通过重定向返回的代码], "redirect_uri": [与之前相同的重定向URI]} `向`authorization_url`发出POST请求，并完成OAuth流程。

[无身份验证](https://platform.openai.com/docs/plugins/authentication/no-authentication)

我们不建议使用“无身份验证”，建议使用“服务身份验证”。

我们支持不需要身份验证的应用程序的"无认证流程"，其中用户能够直接向您的API发送请求而不受任何限制。如果您拥有一个开放的API，希望让所有人都能使用，这将非常有用，因为它允许来自OpenAI插件请求以外的来源的流量。

```json
"auth": {
  "type": "none"
},
```

[用户级别](https://platform.openai.com/docs/plugins/authentication/user-level)

由于当前的UI限制，我们不允许具有“用户身份验证”的插件进入插件商店。我们希望这种情况在未来会有所改变。

与用户可能已经使用您的API一样，我们通过允许最终用户在插件安装期间将其密钥复制粘贴到ChatGPT UI中来允许用户级别的身份验证。虽然我们在存储时对密钥进行加密，但由于用户体验不佳，我们不建议采用这种方法。

- 首先，用户在安装插件时粘贴其访问令牌。
- 我们存储令牌的加密版本。
- 然后，我们在向插件发出请求时将它传递到Authorization标头中（“Authorization”：“[Bearer / Basic] [用户令牌]”）。

```json
"auth": {
  "type": "user_http",
  "authorization_type": "bearer",
},
```