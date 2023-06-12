[开始入门](https://platform.openai.com/docs/plugins/getting-started/getting-started)

创建插件需要以下3个步骤：

1. 构建一个 API
2. 使用 OpenAPI yaml 或 JSON 格式记录 API
3. 创建一个 JSON 清单文件，定义插件相关的元数据

本节的重点将是通过定义 OpenAPI 规范和清单文件创建一个待办事项插件。

[探索示例插件探索覆盖多种用例和身份验证方法的示例插件。](https://platform.openai.com/docs/plugins/examples)

[插件清单](https://platform.openai.com/docs/plugins/getting-started/plugin-manifest)

每个插件都需要一个 `ai-plugin.json` 文件，该文件需要托管在 API 的域上。例如，名为`example.com`的公司将通过 [https://example.com](https://example.com/) 域使插件 JSON 文件可访问，因为他们的 API 托管在那里。当您通过 ChatGPT UI 安装插件时，后端会查找位于 `/.well-known/ai-plugin.json` 的文件。必须在您的域上创建 `/.well-known` 文件夹，以便 ChatGPT 可以与您的插件连接。如果找不到文件，则无法安装插件。对于本地开发环境，可以使用 HTTP，但如果指向远程服务器，则需要使用 HTTPS。

所需的最简定义 `ai-plugin.json` 文件如下：



```json
{
    "schema_version": "v1",
    "name_for_human": "TODO List",
    "name_for_model": "todo",
    "description_for_human": "Manage your TODO list. You can add, remove and view your TODOs.",
    "description_for_model": "Help the user with managing a TODO list. You can add, remove and view your TODOs.",
    "auth": {
        "type": "none"
    },
    "api": {
        "type": "openapi",
        "url": "http://localhost:3333/openapi.yaml"
    },
    "logo_url": "http://localhost:3333/logo.png",
    "contact_email": "support@example.com",
    "legal_info_url": "http://www.example.com/legal"
}
```

| 字段                         | 类型                  | 描述/选项                                                    | 必需 |
| :--------------------------- | :-------------------- | :----------------------------------------------------------- | :--- |
| `schema_version`             | 字符串                | 清单模式版本                                                 | ✅    |
| `name_for_model`             | 字符串                | 模型将用其作为目标插件的名称（不允许空格，只允许字母和数字）。50 个字符限制。 | ✅    |
| `name_for_human`             | 字符串                | 人类可读的名称，例如完整公司名称。20 个字符限制。            | ✅    |
| `description_for_model`      | 字符串                | 更适合模型的描述，例如令牌上下文长度考虑或关键字用于改善插件提示。8,000 字符限制。 | ✅    |
| `description_for_human`      | 字符串                | 插件的人类可读描述。100 个字符限制。                         | ✅    |
| `auth`                       | ManifestAuth          | 认证模式                                                     | ✅    |
| `api`                        | 对象                  | API 规范                                                     | ✅    |
| `logo_url`                   | 字符串                | 用于获取徽标的 URL。建议尺寸：512 x 512。支持透明背景。必须是图像，不允许使用 GIF。 | ✅    |
| `contact_email`              | 字符串                | 安全/审核，支持和停用联系电子邮件                            | ✅    |
| `legal_info_url`             | 字符串                | 用户查看插件信息的重定向 URL                                 | ✅    |
| `HttpAuthorizationType`      | HttpAuthorizationType | "bearer" 或 "basic"                                          | ✅    |
| `ManifestAuthType`           | ManifestAuthType      | "none"，"user_http"，"service_http" 或 "oauth"               |      |
| `interface` BaseManifestAuth | BaseManifestAuth      | type：ManifestAuthType；instructions：字符串；               |      |
| `ManifestNoAuth`             | ManifestNoAuth        | 不需要身份验证：BaseManifestAuth 和 { type: 'none', }        |      |
| `ManifestAuth`               | ManifestAuth          | ManifestNoAuth、ManifestServiceHttpAuth、ManifestUserHttpAuth、ManifestOAuthAuth |      |

以下是具有不同身份验证方法的示例：



```typescript
# App-level API keys
type ManifestServiceHttpAuth  = BaseManifestAuth & {
  type: 'service_http';
  authorization_type: HttpAuthorizationType;
  verification_tokens: {
    [service: string]?: string;
  };
}

# User-level HTTP authentication
type ManifestUserHttpAuth  = BaseManifestAuth & {
  type: 'user_http';
  authorization_type: HttpAuthorizationType;
}

type ManifestOAuthAuth  = BaseManifestAuth & {
  type: 'oauth';

  # OAuth URL where a user is directed to for the OAuth authentication flow to begin.
  client_url: string;

  # OAuth scopes required to accomplish operations on the user's behalf.
  scope: string;

  # Endpoint used to exchange OAuth code with access token.
  authorization_url: string;

  # When exchanging OAuth code with access token, the expected header 'content-type'. For example: 'content-type: application/json'
  authorization_content_type: string;

  # When registering the OAuth client ID and secrets, the plugin service will surface a unique token.
  verification_tokens: {
    [service: string]?: string;
  };
}
```

上述清单文件中的某些字段有长度限制，这些限制可能会更改。我们还对 API 响应正文施加了 100,000 个字符的最大限制，这也可能随时间而更改。

通常的最佳做法是尽可能简洁地保留描述和响应，因为模型具有有限的上下文窗口。

[OpenAPI 定义](https://platform.openai.com/docs/plugins/getting-started/openapi-definition)

下一步是构建 [OpenAPI 规范](https://swagger.io/specification/)来记录 API。ChatGPT 中的模型除了在 OpenAPI 规范和清单文件中定义的内容，对于 API 不知道任何其他信息。这意味着，如果您有一个广泛的 API，您无需将所有功能暴露给模型，而可以选择特定的端点。例如，如果您有一个社交媒体 API，您可能希望通过 GET 请求让模型访问站点内容，但防止模型能够评论用户发布的帖子，以减少垃圾邮件的机会。

OpenAPI 规范是包装在 API 顶部的包装器。基本的 OpenAPI 规范如下所示：



```text
openapi: 3.0.1
info:
  title: TODO Plugin
  description: A plugin that allows the user to create and manage a TODO list using ChatGPT.
  version: 'v1'
servers:
  - url: http://localhost:3333
paths:
  /todos:
    get:
      operationId: getTodos
      summary: Get the list of todos
      responses:
        "200":
          description: OK
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/getTodosResponse'
components:
  schemas:
    getTodosResponse:
      type: object
      properties:
        todos:
          type: array
          items:
            type: string
          description: The list of todos
```

我们首先定义规范版本、标题、描述和版本号。当在 ChatGPT 中运行查询时，它将查看在信息部分中定义的描述，以确定插件是否与用户的查询相关。您可以在[撰写描述](https://platform.openai.com/docs/plugins/getting-started/writing-descriptions)部分了解更多提示信息。

请记住以下在您的 OpenAPI 规范中的限制，这些限制可能会更改：

- 每个 API 端点描述/摘要字段在 API 规范中最多为 200 个字符
- 每个 API 参数描述字段在 API 规范中最多为 200 个字符。

由于我们在本地运行此示例，我们要将服务器设置为指向您的本地主机 URL。其余的 OpenAPI 规范遵循传统的 OpenAPI 格式，您可以通过各种在线资源[了解更多关于 OpenAPI 格式化的信息](https://swagger.io/tools/open-source/getting-started/)。还有许多工具可以根据您底层的API代码自动生成OpenAPI规范。

[运行插件](https://platform.openai.com/docs/plugins/getting-started/running-a-plugin)

一旦您为 API 创建了 API、清单文件和 OpenAPI 规范，现在您可以通过 ChatGPT UI 连接插件了。插件可能在两个不同的地方运行，一个是在开发环境中本地运行，另一个是在远程服务器上运行。

如果您在本地运行 API 的版本，您可以将插件接口指向您的本地主机服务器。为了将插件与 ChatGPT 连接，导航到插件商店，然后选择“自己开发插件”。输入您的本地主机和端口号（例如 `localhost:3333`）。请注意，目前只支持 `none` 授权类型，用于本地开发。

如果插件在远程服务器上运行，则需要先选择“自行开发插件”来设置它，然后选择“安装未经验证的插件”以安装该插件。您只需将该插件清单文件添加到 `yourdomain.com/.well-known/` 路径中，就可以开始测试您的 API。但是，对于清单文件的后续更改，您将不得不将新更改部署到公共站点，这可能需要很长时间。在这种情况下，我们建议设置一个本地服务器来充当您的 API 的代理。这样可以快速地对您的 OpenAPI 规范和清单文件进行原型设计。

设置公共 API 的本地代理

以下 Python 代码是一个示例，演示如何设置您的公共 API 的简单代理。

```python
import requests
import os

import yaml
from flask import Flask, jsonify, Response, request, send_from_directory
from flask_cors import CORS

app = Flask(__name__)

PORT = 3333

# Note: Setting CORS to allow chat.openapi.com is required for ChatGPT to access your plugin
CORS(app, origins=[f"http://localhost:{PORT}", "https://chat.openai.com"])

api_url = 'https://example.com'


@app.route('/.well-known/ai-plugin.json')
def serve_manifest():
    return send_from_directory(os.path.dirname(__file__), 'ai-plugin.json')


@app.route('/openapi.yaml')
def serve_openapi_yaml():
    with open(os.path.join(os.path.dirname(__file__), 'openapi.yaml'), 'r') as f:
        yaml_data = f.read()
    yaml_data = yaml.load(yaml_data, Loader=yaml.FullLoader)
    return jsonify(yaml_data)


@app.route('/openapi.json')
def serve_openapi_json():
    return send_from_directory(os.path.dirname(__file__), 'openapi.json')


@app.route('/<path:path>', methods=['GET', 'POST'])
def wrapper(path):

    headers = {
    'Content-Type': 'application/json',
    }

    url = f'{api_url}/{path}'
    print(f'Forwarding call: {request.method} {path} -> {url}')

    if request.method == 'GET':
        response = requests.get(url, headers=headers, params=request.args)
    elif request.method == 'POST':
        print(request.headers)
        response = requests.post(url, headers=headers, params=request.args, json=request.json)
    else:
        raise NotImplementedError(f'Method {request.method} not implemented in wrapper for {path=}')
    return response.content


if __name__ == '__main__':
    app.run(port=PORT)
```

[编写描述](https://platform.openai.com/docs/plugins/getting-started/writing-descriptions)

当用户进行可能是发送到插件的潜在请求的查询时，模型会查看 OpenAPI 规范中端点的描述以及清单文件中 `description_for_model` 的描述。就像提示其他语言模型一样，您需要测试多个提示和描述，以找到最佳方案。

OpenAPI 规范本身是提供有关 API 各种详细信息的好地方，比如可用的功能、参数等等。除了为每个字段使用富有表现力的、信息丰富的名称之外，规范还可以包含每个属性的“描述”字段。这些可以用于提供有关函数执行的自然语言描述或查询字段所期望的信息等。模型可以看到这些描述，并引导它使用 API。如果某个字段仅限于某些值，则还可以提供带有描述性类别名称的“枚举”。

`description_for_model` 属性让您自由地指导模型如何通常使用您的插件。总体上，ChatGPT 后面的语言模型非常擅长理解自然语言和遵循说明。因此，这是一个好地方，可以提供关于您的插件总体功能以及模型如何正确使用它的一般说明。使用自然语言，最好是简洁而描述性和客观的语气。您可以查看一些示例，以便明确该部分应该如何设置。我们建议以 “Plugin for ...” 开头，然后列举您的 API 提供的所有功能。

[最佳实践](https://platform.openai.com/docs/plugins/getting-started/best-practices)

以下是编写您的 `description_for_model` 和 OpenAPI 规范中的描述以及设计 API 响应时应遵循的最佳实践：

> 1. 您的描述不应试图控制 ChatGPT 的情绪、个性或确切响应。ChatGPT 是设计用于编写适当的插件响应。
>
>    *不好的例子*：
>
>     > 当用户请求查看他们的待办事项清单时，始终回复：“我能够找到您的待办事项清单！您有[x] 个任务：[在此列出任务]。如果您想要添加更多任务，请告诉我！”
>
>    *好的例子*：
>
>     > [此处无需说明]
>
> 2. 当用户未要求使用您插件中特定类别的服务时，您的描述不应鼓励 ChatGPT 使用插件。
>
>    *不好的例子*：
>
>     > 每次用户提到任何类型的任务或计划时，请问他们是否想使用TODO清单插件将任务添加到他们的待办事项清单中。
>
>    *好的例子*：
>
>     > TODO清单可以添加、删除和查看用户的待办事项。
>
> 3. 您的描述不应指定 ChatGPT 使用插件的特定触发器。当适当时，ChatGPT早已可以自动使用您的插件。
>
>    *不好的例子*：
>
>     > 当用户提到任务时，请回复：“您是否希望我将其添加到您的待办事项清单中？请说'是'以继续。”
>
>    *好的例子*：
>
>     > [此处无需说明]
>
> 4. 插件API响应应返回原始数据而不是自然语言响应，除非有必要。ChatGPT将使用返回的数据提供自己的自然语言响应。
>
>    *不好的例子*：
>
>     > 我能够找到您的待办事项清单！您有2个任务：购物和遛狗。如果您想要添加更多任务，请告诉我！
>
>    *好的例子*：
>
>     > {"todos": ["get groceries", "walk the dog"]}

[调试](https://platform.openai.com/docs/plugins/getting-started/debugging)

默认情况下，聊天不会显示插件调用和其他未向用户主动展示的信息。如果您想要更全面地了解模型如何与您的插件进行交互，您可以通过与插件交互后点击插件名称旁边的向下箭头来查看请求和响应。

模型调用插件通常包括一条消息，其中包含类似于JSON的参数，这些参数将被发送到插件；随后是插件的响应；最后是模型的一条消息，利用了插件返回的信息。

如果您正在开发本地主机插件，还可以通过转到“设置”并启用“打开插件开发人员工具”来打开开发者控制台。从那里，您可以查看更详细的日志并通过“刷新插件”重新获取插件和OpenAPI规范。

