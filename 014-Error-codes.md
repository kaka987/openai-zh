[错误代码](https://platform.openai.com/docs/guides/error-codes/error-codes)

本指南概述了您可能会从[API](https://platform.openai.com/docs/introduction)和我们的[官方Python库](https://platform.openai.com/docs/libraries/python-library)发现的错误码。概述中提到的每个错误码都有一个专门的章节，提供更多的指导信息。

[API错误码](https://platform.openai.com/docs/guides/error-codes/api-errors)

| 代码                                                 | 概述                                                         |
| :--------------------------------------------------- | :----------------------------------------------------------- |
| 401 - 验证无效                                       | **原因：** 验证无效。 **解决方案：** 确保使用了正确的[API密钥](https://platform.openai.com/account/api-keys)和请求组织。 |
| 401 - API密钥不正确                                  | **原因：** 请求API密钥不正确。 **解决方案：** 确保使用的API密钥是正确的，清除浏览器缓存，或者[生成一个新的API密钥](https://platform.openai.com/account/api-keys)。 |
| 401 - 您必须是组织的成员才能使用API                  | **原因：** 您的帐号不属于任何组织。 **解决方案：** 联系我们以加入新的组织，或者要求您的组织管理员[邀请您加入组织](https://platform.openai.com/account/members)。 |
| 429 - 请求的速率限制已达到                           | **原因：** 您发送的请求过于频繁。 **解决方案：** 控制请求速率。阅读[速率限制指南](https://platform.openai.com/docs/guides/rate-limits)。 |
| 429 - 您已超过当前配额，请检查您的计划和帐单详细信息 | **原因：** 您已达到最大月度限额（硬限制），可以在[帐单部分](https://platform.openai.com/account/billing/limits)中查看。 **解决方案：** [申请配额增加](https://platform.openai.com/forms/quota-increase)。 |
| 429 - 引擎当前过载，请稍后重试                       | **原因：** 我们的服务器正在经历高负载。 **解决方案：** 请在短暂等待后重试您的请求。 |
| 500 - 服务器在处理您的请求时发生错误                 | **原因：** 我们的服务器出现问题。 **解决方案：** 在短暂等待后重试您的请求，如果问题仍然存在，请联系我们。请检查[状态页面](https://status.openai.com/)。 |

401 - 无效的认证

该错误消息表示您的身份验证凭证无效。这可能是由于以下几个原因：

- 您正在使用被吊销的API密钥。
- 您正在使用与请求组织分配的API密钥不同的API密钥。
- 您正在使用不具备所调用端点所需权限的API密钥。

要解决此错误，请按照以下步骤操作：

- 检查请求头中是否使用了正确的API密钥和组织ID。您可以在[您的帐户设置](https://platform.openai.com/account/api-keys)中找到您的API密钥和组织ID。
- 如果您不确定自己的API密钥是否有效，可以[生成一个新的](https://platform.openai.com/account/api-keys)。请确保用新的API密钥替换旧的API密钥，并遵循我们的[最佳实践指南](https://help.openai.com/en/articles/5112595-best-practices-for-api-key-safety)。

401 - 提供了不正确的API密钥

该错误消息表示您在请求中使用的API密钥不正确。这可能是由于以下几个原因：

- 您的API密钥中有错别字或多余的空格。
- 您正在使用属于其他组织的API密钥。
- 您正在使用已删除或停用的API密钥。
- 一个旧的，被吊销的API密钥可能在本地缓存中。

要解决此错误，请按照以下步骤操作：

- 尝试清除浏览器缓存和cookie，然后再次尝试。
- 检查您在请求头中是否使用了正确的API密钥。
- 如果您不确定自己的API密钥是否正确，可以[生成一个新的](https://platform.openai.com/account/api-keys)。请确保在代码库中用新的API密钥替换旧的API密钥，并遵循我们的[最佳实践指南](https://help.openai.com/en/articles/5112595-best-practices-for-api-key-safety)。

401 - 您必须是组织的成员才能使用API

该错误消息表示您的帐户不是组织的一部分。这可能是由于以下几个原因：

- 您已经离开或被移除了您之前所属的组织。
- 您所在的组织已被删除。

要解决此错误，请按照以下步骤操作：

- 如果您已经离开或被移除了您之前所在的组织，可以要求加入新的组织或受邀加入现有组织。
- 要请求加入新组织，请通过help.openai.com联系我们。
- 现有组织的所有者可以通过[成员面板](https://platform.openai.com/account/members)邀请您加入他们的组织。

429 - 请求速率限制已达到

该错误消息表示您已经达到API分配的速率限制。这意味着您在短时间内提交了太多的令牌或请求，并超过了允许的请求数量。这可能是由于以下几个原因：

- 您正在使用循环或脚本进行频繁或并发的请求。
- 您正在与其他用户或应用程序共享您的API密钥。
- 您正在使用具有较低速率限制的免费计划。

要解决此错误，请按照以下步骤操作：

- 控制您的请求速度，避免进行不必要或冗余的调用。
- 如果您正在使用循环或脚本，请确保实现一个后退机制或重试逻辑，以考虑速率限制和响应头。您可以在我们的[速率限制指南](https://platform.openai.com/docs/guides/rate-limits)中了解有关我们的速率限制政策和最佳实践的更多信息。
- 如果您正在与其他用户共享组织，请注意限制是按组织而不是按用户应用的，建议检查您团队成员的使用情况，这将有助于了解是否已达到限制。
- 如果您正在使用免费或低级别计划，请考虑升级到付费按需计划，以提供更高的速率限制。您可以在我们的[速率限制指南](https://platform.openai.com/docs/guides/rate-limits)中比较每个计划的限制。

429 - 您已超出当前配额，请检查您的计划和账单详情

该错误消息表示您已经达到 API 的最大月度支出。您可以在您的[账户账单设置](/account/billing/limits)中的“硬限制”下查看您的最大月度限制。这意味着您已经使用完了您计划分配的所有学分，并已达到当前结算周期的限制。这可能是由于以下几个原因：

- 您正在使用大量或复杂的服务，这些服务消耗了大量的学分或令牌。
- 您的限制设置太低，无法满足您组织的使用率。

要解决此错误，请按照以下步骤操作：

- 在您的[账户设置](https://platform.openai.com/account/billing/limits)中检查您当前的配额。您可以在您的账户的[使用情况部分](https://platform.openai.com/account/usage)中查看您的请求已经消耗了多少令牌。
- 如果您正在使用免费计划，请考虑升级到一个付费按需计划，该计划提供更高的配额。
- 如果您需要增加配额，您可以[申请增加配额](https://platform.openai.com/forms/quota-increase)，并提供有关预期使用的相关细节。我们将检查您的请求，并在大约7-10个工作日内回复您。

429 - 引擎当前超载，请稍后再试

该错误消息表示我们的服务器正在经历高流量，并且当前无法处理您的请求。这可能是由于以下几个原因：

- 我们的服务需求突然激增。
- 我们的服务器正在计划或未计划的维护或更新。
- 我们的服务器发生意外或无法避免的故障或事件。

要解决此错误，请按照以下步骤操作：

- 在简短的等待后重试您的请求。我们建议使用指数退避策略或重试逻辑，以考虑响应头和速率限制。您可以在我们的[速率限制最佳实践](https://help.openai.com/en/articles/6891753-rate-limit-advice)中了解更多信息。
- 查看我们的[状态页面](https://status.openai.com/)，了解我们服务和服务器的任何更新或公告。
- 如果经过合理的时间仍然出现此错误，请联系我们进行进一步的帮助。我们对造成的任何不便表示歉意，并感谢您的耐心和理解。

[python库错误类型](https://platform.openai.com/docs/guides/error-codes/python-library-error-types)

| 类型                    | 概述                                                         |
| :---------------------- | :----------------------------------------------------------- |
| APIError                | **原因**：我们侧面有问题。 **解决方案**：稍等片刻后重试您的请求，如果问题仍然存在，请联系我们。 |
| Timeout                 | **原因**：请求超时。 **解决方案**：稍等片刻后重试您的请求，如果问题仍然存在，请联系我们。 |
| RateLimitError          | **原因**：您已达到指定的速率限制。 **解决方案**：安排您的请求。 在我们的[速率限制指南](https://platform.openai.com/docs/guides/rate-limits)中了解更多信息。 |
| APIConnectionError      | **原因**：连接到我们的服务时有问题。 **解决方案**：检查您的网络设置，代理配置，SSL证书或防火墙规则。 |
| InvalidRequestError     | **原因**：您的请求存在错误或缺少一些所需参数，例如令牌或输入等。 **解决方案**：错误消息应告诉您所犯的具体错误。检查您正在调用的特定API方法的[文档](https://platform.openai.com/docs/api-reference/)，并确保您发送有效和完整的参数。您也可能需要检查请求数据的编码，格式或大小。 |
| AuthenticationError     | **原因**：您的API密钥或令牌无效，过期或被撤销。 **解决方案**：检查您的API密钥或令牌并确保其正确和激活。您可能需要从帐户仪表板生成一个新密钥或令牌。 |
| ServiceUnavailableError | **原因**：我们服务器上出现问题。 **解决方案**：稍等片刻后重试您的请求，如果问题仍然存在，请联系我们。检查[状态页面](https://status.openai.com/)。 |

APIError

`APIError`表示在处理您的请求时我们侧面发生错误。这可能是由于临时错误、软件缺陷或系统停机引起的。

我们为给您带来的任何不便道歉，并正在努力尽快解决任何问题。您可以[检查我们的系统状态页面](https://status.openai.com/)以获取更多信息。

如果您遇到`APIError`，请尝试以下步骤:

- 等待几秒钟并重试您的请求。有时，问题可能很快解决，而您的请求可能会在第二次尝试时成功。
- 检查我们的状态页面，以了解可能影响我们服务的任何正在进行的事件或维护。如果有活动事件，请关注更新，并在重新尝试请求之前等待它解决。
- 如果问题仍然存在，请查看我们的“Persistent errors next steps”部分。

我们的支持团队将调查此问题，并尽快回复您。请注意，由于需求量较高，我们的支持队列等待时间可能很长。您也可以[在我们的社区论坛上发帖子](https://community.openai.com/)，但请确保省略任何敏感信息。

Timeout

`Timeout`错误表示您的请求花费的时间太长，因此我们的服务器关闭了连接。这可能是由于网络问题、我们服务的负载过重或需要更多处理时间的复杂请求引起的。

如果您遇到`Timeout`错误，请尝试以下步骤:

- 等待几秒钟并重试您的请求。有时，网络拥塞或我们服务的负载可能会减少，而您的请求可能会在第二次尝试时成功。
- 检查您的网络设置，并确保您拥有稳定且快速的互联网连接。您可能需要切换到不同的网络，使用有线连接或减少使用带宽的设备或应用程序的数量。
- 如果问题仍然存在，请查看我们的“Persistent errors next steps”部分。

RateLimitError

`RateLimitError`表示您已经达到了指定的速率限制。这意味着您在一定时间内发送了太多的令牌或请求，我们的服务暂时阻止您继续发送。

我们实施速率限制以确保对我们的资源进行公平和有效的使用，并防止滥用或过载我们的服务。

如果您遇到`RateLimitError`错误，请尝试以下步骤：

- 发送更少的令牌或请求或放慢速度。您可能需要减少请求的频率或数量，批量处理您的令牌或实现指数回退。您可以阅读我们的[速率限制指南](https://platform.openai.com/docs/guides/rate-limits)以获取更多详细信息。
- 等待您的速率限制重置（一分钟），然后重试您的请求。错误消息应该让您了解您的使用率和允许的使用。
- 您还可以从您的帐户仪表板中检查您的API使用统计信息。

APIConnectionError

`APIConnectionError`表示您的请求无法到达我们的服务器或建立安全连接。这可能是由于网络问题、代理配置、SSL证书或防火墙规则引起的。

如果您遇到`APIConnectionError`错误，请尝试以下步骤：

- 检查您的网络设置，并确保您拥有稳定且快速的互联网连接。您可能需要切换到不同的网络，使用有线连接或减少使用带宽的设备或应用程序的数量。
- 检查您的代理配置，并确保它与我们的服务兼容。您可能需要更新代理设置、使用不同的代理或完全绕过代理。
- 检查您的SSL证书并确保它们是有效且最新的。您可能需要安装或更新您的证书、使用不同的证书颁发机构或禁用SSL验证。
- 检查您的防火墙规则，并确保它们未阻止或过滤我们的服务。您可能需要修改防火墙设置。
- 如果适用，检查您的容器是否具有正确的权限以发送和接收流量。
- 如果问题仍然存在，请查看我们的“Persistent errors next steps”部分。

InvalidRequestError

`InvalidRequestError`表示您的请求格式不正确或缺少一些必需的参数，例如令牌或输入。这可能是由于代码中的拼写错误、格式错误或逻辑错误而引起的。

如果您遇到`InvalidRequestError`错误，请尝试以下步骤：

- 仔细阅读错误消息并确定具体的错误。错误消息应该告诉您哪个参数无效或缺失，以及期望的值或格式是什么。
- 检查您调用的特定API方法的[API参考](https://platform.openai.com/docs/api-reference/)，并确保您发送了有效和完整的参数。您可能需要查看参数名称、类型、值和格式，并确保它们与文档匹配。
- 检查您的请求数据的编码、格式或大小，并确保它们与我们的服务兼容。您可能需要将数据编码为UTF-8、使用JSON格式化数据，或者如果数据太大，则压缩数据。
- 使用类似Postman或curl的工具测试您的请求，并确保它按预期工作。您可能需要调试您的代码并修复请求逻辑中的任何错误或不一致之处。
- 如果问题仍然存在，请查看我们的“Persistent errors next steps”部分。

AuthenticationError

`AuthenticationError`表示您的API密钥或令牌无效、过期或被吊销。这可能是由于拼写错误、格式错误或安全漏洞引起的。

如果您遇到`AuthenticationError`错误，请尝试以下步骤：

- 检查您的API密钥或令牌，并确保其正确且激活。您可能需要从API密钥仪表板生成新密钥，确保没有额外的空格或字符，或者如果您有多个密钥或令牌，则使用不同的密钥或令牌。
- 确保您遵循了正确的格式。

ServiceUnavailableError

`ServiceUnavailableError`表示我们的服务器暂时无法处理您的请求。这可能是由于计划或未经计划的维护、系统升级或服务器故障引起的。在高流量期间，也可能返回这些错误。

我们为任何不便道歉，并努力尽快恢复我们的服务。

如果您遇到`ServiceUnavailableError`错误，请尝试以下步骤：

- 等待几分钟并重试您的请求。有时，问题可能会迅速解决，您的请求可能会在下一次尝试中成功。
- 检查我们的[状态页面](https://status.openai.com/)，了解任何可能影响我们服务的持续事件或维护。如果有活动事件，请关注更新，并等待事件解决后再重试您的请求。
- 如果问题仍然存在，请查看我们的“Persistent errors next steps”部分。

[Persistent errors](https://platform.openai.com/docs/guides/error-codes/persistent-errors)

如果问题仍然存在，请通过[聊天联系我们的支持团队](https://help.openai.com/en/)并向他们提供以下信息：

- 您正在使用的模型
- 您收到的错误消息和代码
- 您发送的请求数据和标题
- 您请求的时间戳和时区
- 可能有助于我们诊断问题的任何其他相关详细信息

我们的支持团队将调查问题，并尽快回复您。请注意，由于需求高峰，我们的支持队列时间可能会很长。您也可以[在我们的社区论坛上发帖](https://community.openai.com/)，但请务必省略任何敏感信息。

[处理错误](https://platform.openai.com/docs/guides/error-codes/handling-errors)

我们建议您以编程方式处理API返回的错误。为此，您可能需要使用以下代码片段：

```python
try:
  #Make your OpenAI API request here
  response = openai.Completion.create(prompt="Hello world",
                                      model="text-davinci-003")
except openai.error.APIError as e:
  #Handle API error here, e.g. retry or log
  print(f"OpenAI API returned an API Error: {e}")
  pass
except openai.error.APIConnectionError as e:
  #Handle connection error here
  print(f"Failed to connect to OpenAI API: {e}")
  pass
except openai.error.RateLimitError as e:
  #Handle rate limit error (we recommend using exponential backoff)
  print(f"OpenAI API request exceeded rate limit: {e}")
  pass
```

