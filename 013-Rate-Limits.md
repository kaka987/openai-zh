[速率限制](https://platform.openai.com/docs/guides/rate-limits/rate-limits)

[概述](https://platform.openai.com/docs/guides/rate-limits/overview)

[什么是速率限制？](https://platform.openai.com/docs/guides/rate-limits/what-are-rate-limits)

速率限制是API对在指定时间段内用户或客户端访问服务器次数所施加的限制。

[为什么我们需要速率限制？](https://platform.openai.com/docs/guides/rate-limits/why-do-we-have-rate-limits)

速率限制是API的通用惯例，它们有几个不同的原因：

- **它们有助于保护API免受滥用或误用。**例如，恶意行为者可能会试图向API发送大量请求，以试图使其过载或造成服务中断。通过设置速率限制，OpenAI可以防止这种活动。
- **速率限制有助于确保每个人对API有平等的访问权。**如果一个人或组织发出过多的请求，可能会使API变得拥挤，影响其他所有人。通过限制单个用户可以发出的请求数量，OpenAI确保最多的人有机会使用API而不会遇到减速。
- **速率限制可以帮助OpenAI管理其基础设施的总体负载。**如果对API的请求数量大幅增加，可能会使服务器负担过重并引起性能问题。通过设置速率限制，OpenAI可以帮助所有用户保持平稳和一致的体验。

请完整阅读本文档，以更好地了解OpenAI的速率限制系统如何工作。我们包括代码示例和处理常见问题的可能解决方案。建议在填写[速率限制增加请求表格](https://docs.google.com/forms/d/e/1FAIpQLSc6gSL3zfHFlL6gNIyUcjkEv29jModHGxg5_XGyr-PrE2LaHw/viewform)之前遵循此指南，并在最后一节中了解有关如何填写表格的详细信息。

[我们的API的速率限制是多少？](https://platform.openai.com/docs/guides/rate-limits/what-are-the-rate-limits-for-our-api)

您可以在账户管理页面的[速率限制](https://platform.openai.com/account/rate-limits)部分查看您的组织的速率限制。

我们在[组织级别](https://platform.openai.com/docs/guides/production-best-practices)，而非用户级别上执行速率限制，依据使用的特定端点和您所拥有的帐户类型进行。速率限制以两种方式进行衡量：**RPM**（每分钟请求次数）和**TPM**（每分钟令牌数）。下表突出了我们API的默认速率限制，但这些限制可以在填写“速率限制增加请求”表单后根据您的用例进行增加。

**TPM**（每分钟令牌数）单位根据模型的不同而有所不同：

| 类型    | 1 TPM 相当于      |
| :------ | :---------------- |
| davinci | 每分钟 1 个令牌   |
| curie   | 每分钟 25 个令牌  |
| babbage | 每分钟 100 个令牌 |
| ada     | 每分钟 200 个令牌 |

从实际意义上讲，这意味着您可以向`ada`模型发送的令牌数量，相对于`davinci`模型而言，每分钟多200倍左右。

|                            | 文本和嵌入向量                             | 聊天                                      | CODEX                                  | 编辑                                    | 图像             | 音频             |
| :------------------------- | :----------------------------------------- | :---------------------------------------- | :------------------------------------- | :-------------------------------------- | :--------------- | :--------------- |
| 免费试用用户               | 每分钟 3 次请求，每分钟 150,000 个令牌     | 每分钟 3 次请求，每分钟 40,000 个令牌     | 每分钟 3 次请求，每分钟 40,000 个令牌  | 每分钟 3 次请求，每分钟 150,000 个令牌  | 每分钟 5 张图像  | 每分钟 3 次请求  |
| 按量计费用户（前 48 小时） | 每分钟60个请求，每分钟250,000个令牌        | 每分钟 60 次请求，每分钟 60,000 个令牌    | 每分钟 20 次请求，每分钟 40,000 个令牌 | 每分钟 20 次请求，每分钟 150,000 个令牌 | 每分钟 50 张图像 | 每分钟 50 次请求 |
| 按量计费用户（48 小时后）  | 每分钟 3,500 次请求，每分钟 350,000 个令牌 | 每分钟 3,500 次请求，每分钟 90,000 个令牌 | 每分钟 20 次请求，每分钟 40,000 个令牌 | 每分钟 20 次请求，每分钟 150,000 个令牌 | 每分钟 50 张图像 | 每分钟 50 次请求 |

需要注意的是，限制速率的因素取决于哪个会先达到上限。例如，您可能会向 Codex 端点发送 20 个请求，但只包含 100 个令牌，即使在这 20 个请求中没有发送 40,000 个令牌，您的限制也将被填满。

[GPT-4 速率限制](https://platform.openai.com/docs/guides/rate-limits/gpt-4-rate-limits)

在 GPT-4 有限的测试期间，模型将具有更为激进的速率限制以满足需求。`gpt-4`/`gpt-4-0314` 的默认速率限制为每分钟 40,000 个令牌和每分钟 200 个请求。`gpt-4-32k`/`gpt-4-32k-0314` 的默认速率限制为每分钟 150,000 个令牌和每分钟 20 个请求。**由于产能限制，我们无法满足提高速率限制的请求。** 在当前状态下，该模型旨在进行实验和原型设计，而非用于高容量生产用例。

[速率限制如何起作用？](https://platform.openai.com/docs/guides/rate-limits/how-do-rate-limits-work)

如果您的速率限制是每分钟 60 个请求和每分钟 150,000 个`davinci` 令牌，则您将受到两者中先达到上限的限制影响。例如，如果您的最大请求/分钟为 60，则您应该能够每秒钟发送 1 个请求。如果您每 800ms 发送 1 个请求，一旦达到速率限制，您只需要将程序休眠 200ms，就可以发送另一个请求，否则后续的请求将失败。默认值为每分钟 3,000 个请求，客户可以有效地每 20ms 或每 0.02 秒发送 1 个请求。

[What happens if I hit a rate limit error?](https://platform.openai.com/docs/guides/rate-limits/what-happens-if-i-hit-a-rate-limit-error)

速率限制错误的样式如下：

> 针对组织 org-{id} 的 default-text-davinci-002 的请求每分钟速率已达到限制。限制：每分钟 20.000000 个请求。当前：每分钟 24.000000 个请求。

如果您达到速率限制，这意味着您在短时间内发送了太多请求，API 将拒绝进一步的请求直到特定时间已经过去。

[速率限制与 `max_tokens`](https://platform.openai.com/docs/guides/rate-limits/rate-limits-vs-max_tokens)

我们提供的每个[模型](https://platform.openai.com/docs/models/overview)在进行请求时都有一个有限的嵌入向量数，可以作为输入。您无法增加模型接受的嵌入向量数的最大数量。例如，如果您正在使用`text-ada-001`，则每个请求发送给该模型的最大嵌入向量数为 2,048。

[错误缓解](https://platform.openai.com/docs/guides/rate-limits/error-mitigation)

[我可以采取哪些措施来缓解这个问题？](https://platform.openai.com/docs/guides/rate-limits/what-are-some-steps-i-can-take-to-mitigate-this)

OpenAI Cookbook 提供了一个[Python笔记本](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_handle_rate_limits.ipynb)，它详细解释了如何避免速率限制错误，还提供了一个[Python脚本](https://github.com/openai/openai-cookbook/blob/main/examples/api_request_parallel_processor.py)，用于在批处理API请求时遵守速率限制。

当提供编程访问、批处理功能和自动化社交媒体发布时，应谨慎行事，只为受信任的客户开启。

为了防止滥用和过度使用，可以在特定时间范围内（每日、每周或每月）为单个用户设置使用限制。对于超出限制的用户，可以考虑实施硬限制或手动审核流程。

[指数退避重试](https://platform.openai.com/docs/guides/rate-limits/retrying-with-exponential-backoff)

避免速率限制错误的一种简单方法是自动使用随机指数退避重试。重新尝试指数退避意味着在遇到速率限制错误时进行短暂休眠，然后重试未成功的请求。如果请求仍然失败，则增加休眠时间并重复该过程，直到请求成功或达到最大重试次数。这种方法有许多好处：

- 自动重试意味着您可以在没有崩溃或丢失数据的情况下从速率限制错误中恢复
- 指数退避意味着您的第一次重试可以很快尝试，同时如果您的前几次重试失败，则仍能从更长的延迟中受益。
- 向延迟添加随机抖动有助于避免多个重试同时发送请求。

请注意，不成功的请求会对您的每分钟限制产生贡献，因此不断重新发送请求是不起作用的。

下面是一些**Python**示例解决方案，使用指数退避重试的方法。

示例#1：使用Tenacity库

Tenacity是一种Apache 2.0许可证的通用重试库，用于简化将重试行为添加到几乎任何内容的任务。要将指数退避添加到您的请求中，您可以使用`tenacity.retry`装饰器。下面的示例使用`tenacity.wait_random_exponential`函数将随机指数退避添加到一个请求中。

使用Tenacity库

python

复制

```python
import openai
from tenacity import (
    retry,
    stop_after_attempt,
    wait_random_exponential,
)  # for exponential backoff
 
@retry(wait=wait_random_exponential(min=1, max=60), stop=stop_after_attempt(6))
def completion_with_backoff(**kwargs):
    return openai.Completion.create(**kwargs)
 
completion_with_backoff(model="text-davinci-003", prompt="Once upon a time,")
```

请注意，Tenacity库是第三方工具，OpenAI不对其可靠性或安全性作出任何保证。

示例#2：使用backoff库

另一个提供回退和重试函数装饰器的Python库是[backoff](https://pypi.org/project/backoff/):

使用Tenacity库

python

复制

```python
import backoff 
import openai 
@backoff.on_exception(backoff.expo, openai.error.RateLimitError)
def completions_with_backoff(**kwargs):
    return openai.Completion.create(**kwargs)
 
completions_with_backoff(model="text-davinci-003", prompt="Once upon a time,")
```

与Tenacity一样，backoff库是第三方工具，OpenAI不对其可靠性或安全性作出任何保证。

示例3：手动回退实现

如果您不想使用第三方库，您可以按照以下示例实现自己的回退逻辑：

使用手动回退实现

python

复制

```python
# imports
import random
import time
 
import openai
 
# define a retry decorator
def retry_with_exponential_backoff(
    func,
    initial_delay: float = 1,
    exponential_base: float = 2,
    jitter: bool = True,
    max_retries: int = 10,
    errors: tuple = (openai.error.RateLimitError,),
):
    """Retry a function with exponential backoff."""
 
    def wrapper(*args, **kwargs):
        # Initialize variables
        num_retries = 0
        delay = initial_delay
 
        # Loop until a successful response or max_retries is hit or an exception is raised
        while True:
            try:
                return func(*args, **kwargs)
 
            # Retry on specific errors
            except errors as e:
                # Increment retries
                num_retries += 1
 
                # Check if max retries has been reached
                if num_retries > max_retries:
                    raise Exception(
                        f"Maximum number of retries ({max_retries}) exceeded."
                    )
 
                # Increment the delay
                delay *= exponential_base * (1 + jitter * random.random())
 
                # Sleep for the delay
                time.sleep(delay)
 
            # Raise exceptions for any errors not specified
            except Exception as e:
                raise e
 
    return wrapper
    
@retry_with_exponential_backoff
def completions_with_backoff(**kwargs):
    return openai.Completion.create(**kwargs)
```

同样，OpenAI不对此解决方案的安全性或效率作出任何保证，但它可以成为您自己解决方案的好起点。

[减少“max_tokens”以匹配您的完成量大小](https://platform.openai.com/docs/guides/rate-limits/reduce-the-max_tokens-to-match-the-size-of-your-completions)

您的速率限制是根据请求的字符数估算的令牌数量和“max_tokens”的最大值计算的。请尽量将“max_tokens”值设置为期望响应大小的最接近值。

[批处理请求](https://platform.openai.com/docs/guides/rate-limits/batching-requests)

OpenAI API有每分钟请求和每分钟令牌的分别限制。

如果您的每分钟请求限制已达到上限，但每分钟令牌仍有可用容量，则可以将多个任务合并到每个请求中以增加吞吐量。这将使您能够处理更多的令牌，尤其是对于我们更小的模型。

发送批处理的提示与普通API调用完全相同，只是将字符串列表传递给prompt参数，而不是单个字符串。

没有批处理示例

没有批处理

python

复制

```python
import openai
 
num_stories = 10
prompt = "Once upon a time,"
 
# serial example, with one story completion per request
for _ in range(num_stories):
    response = openai.Completion.create(
        model="curie",
        prompt=prompt,
        max_tokens=20,
    )
    # print story
    print(prompt + response.choices[0].text)
```

带有批处理的示例

批处理

python

复制

```python
import openai  # for making OpenAI API requests
 
 
num_stories = 10
prompts = ["Once upon a time,"] * num_stories
 
# batched example, with 10 story completions per request
response = openai.Completion.create(
    model="curie",
    prompt=prompts,
    max_tokens=20,
)
 
# match completions to prompts by index
stories = [""] * len(prompts)
for choice in response.choices:
    stories[choice.index] = prompts[choice.index] + choice.text
 
# print stories
for story in stories:
    print(story)
```

警告：响应对象可能不按照提示的顺序返回完成结果，因此请始终记得使用索引字段将响应匹配回提示。

[请求限制增加](https://platform.openai.com/docs/guides/rate-limits/request-increase)

[什么时候应考虑申请限制速率增加？](https://platform.openai.com/docs/guides/rate-limits/when-should-i-consider-applying-for-a-rate-limit-increase)

我们的默认速率限制帮助我们最大化稳定性并防止滥用我们的API。我们增加限制以实现高流量应用程序，因此申请限制速率增加的最佳时间是当您觉得具有必要的流量数据支持增加速率限制的强有力的理由。大量的限制速率增加请求如果没有支持数据，不太可能被批准。如果您正在为产品推出做准备，请通过10天的分阶段发布获得相关数据。

请记住，限制速率增加有时可能需要7-10天，因此如果有数据支持您将达到当前增长数字的速率限制，请尝试提前计划并及早提交。

[我的限制速率增加请求会被拒绝吗？](https://platform.openai.com/docs/guides/rate-limits/will-my-rate-limit-increase-request-be-rejected)

限制速率增加请求往往被拒绝是因为缺乏证明增加的数据。我们在下面提供了数值示例，展示如何最好地支持限制速率增加请求，我们会尽最大努力批准与我们的安全策略相符合并展示支持数据的所有请求。我们致力于让开发者通过我们的API扩展和成功。

[我已经为我的文本代码API实施了指数退避，但仍然遇到此错误。如何增加我的速率限制？](https://platform.openai.com/docs/guides/rate-limits/i-ve-implemented-exponential-backoff-for-my-text-code-apis-but-i-m-still-hitting-this-error-how-do-i-increase-my-rate-limit)

我们理解有限速率限制可能会带来的挫败感，我们很想为每个人提高默认限制。但是，由于共享的容量限制，我们只能为已经通过我们的 [限制速率增加请求表单](https://docs.google.com/forms/d/e/1FAIpQLSc6gSL3zfHFlL6gNIyUcjkEv29jModHGxg5_XGyr-PrE2LaHw/viewform) 展示了需求的付费客户批准限制速率增加。为了帮助我们正确评估您的需求，我们请您在表单的“分享需求的证据”部分提供您当前使用情况或基于历史用户活动的预测统计数据。如果没有这些信息，我们建议采取分段发布的方法。从以当前限制速率发布服务的一部分用户开始，收集10个工作日的使用数据，然后基于这些数据提交正式的限制速率增加请求，等待我们的审批。

我们将审核您的请求，如果被批准，我们将在7-10个工作日内通知您。

以下是一些您可能如何填写此表单的示例：

DALL-E API 示例

| 模型       | 预估每分钟令牌数 | 预估每分钟请求数 | 用户数量 | 需求证明                                                     | 1小时最大吞吐成本 |
| :--------- | :--------------- | :--------------- | :------- | :----------------------------------------------------------- | :---------------- |
| DALL-E API | N/A              | 50               | 1000     | 我们的应用目前已经在生产环境中，根据我们过去的流量计算，我们大约每分钟发起10个请求。 | $60               |
| DALL-E API | N/A              | 150              | 10000    | 我们的应用在App Store中受到关注，我们开始遇到速率限制。我们能否将默认限制的50张/分钟增加到150张/分钟？如果需要更多，我们会提交另一个表单。谢谢！ | $180              |

语言模型的示例



| 模型             | 预估每分钟令牌数 | 预估每分钟请求数 | 用户数量 | 需求证明                                                     | 1小时最大吞吐成本 |
| :--------------- | :--------------- | :--------------- | :------- | :----------------------------------------------------------- | :---------------- |
| text-davinci-003 | 325,000          | 4,000            | 50       | 我们将向一组初始的Alpha测试人员发布应用程序，需要更高的限制来适应他们的初始使用。我们在此提供了我们的google drive链接，显示分析和API使用情况。 | $390              |
| text-davinci-002 | 750,000          | 10,000           | 10,000   | 我们的应用程序受到了很多关注；我们有50,000人在我们的等待列表上。我们想先推出给每天1,000人的人群，直到达到50,000用户。请查看我们过去30天的当前令牌/分钟的交通量链接。这是面向500个用户的，基于他们的使用情况，我们认为750,000令牌/分钟和10,000请求/分钟将成为一个很好的起点。 | $900              |

代码模型的示例



| 模型             | 预估每分钟令牌数 | 预估每分钟请求数 | 用户数量 | 需求证明                                                     | 1小时最大吞吐成本                                            |
| :--------------- | :--------------- | :--------------- | :------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| code-davinci-002 | 150,000          | 1,000            | 15       | 我们是一组正在研究论文的研究人员。我们估计我们需要更高的速率限制来完成我们的研究，以便在月底之前完成。这些估计是基于以下的计算[...] | Codex模型目前处于免费beta阶段，因此我们可能无法立即为这些模型提供速率限制的增加。 |

请注意，这些示例只是一般的使用情况，实际的使用率将根据具体的实现和用法而有所不同。