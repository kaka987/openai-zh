[示例插件](https://platform.openai.com/docs/plugins/examples/example-plugins)

为了开始构建插件，我们提供了一组简单的插件，涵盖不同的身份验证模式和用例。从我们的简单无身份验证的待办事项列表插件到[更强大的检索插件](https://github.com/openai/chatgpt-retrieval-plugin/)，这些示例提供了我们希望使用插件实现的可能性的一瞥。

在开发过程中，您可以 [在本地计算机上运行插件](https://platform.openai.com/docs/plugins/getting-started/running-a-plugin) 或者通过云开发环境，如[GitHub Codespaces](https://github.com/features/codespaces)，[Replit](https://replit.com/)或[CodeSandbox](https://codesandbox.io/)等工具来运行插件。

插件快速入门

我们创建了插件快速入门作为起点，供开发人员在不到5分钟的时间内启动并运行插件。如果您还没有运行过插件，并想了解运行插件所需的最小步骤，建议从[插件快速入门仓库](https://github.com/openai/plugins-quickstart)开始。

学习如何构建一个简单的无需身份验证的待办事项列表插件

首先，请查看[无身份验证](https://platform.openai.com/docs/plugins/authentication/no-authentication)页面，然后使用以下字段定义`ai-plugin.json`文件：

```json
{
    "schema_version": "v1",
    "name_for_human": "TODO List (No Auth)",
    "name_for_model": "todo",
    "description_for_human": "Manage your TODO list. You can add, remove and view your TODOs.",
    "description_for_model": "Plugin for managing a TODO list, you can add, remove and view your TODOs.",
    "auth": {
        "type": "none"
    },
    "api": {
        "type": "openapi",
        "url": "PLUGIN_HOSTNAME/openapi.yaml"
    },
    "logo_url": "PLUGIN_HOSTNAME/logo.png",
    "contact_email": "support@example.com",
    "legal_info_url": "https://example.com/legal"
}
```

请注意，`PLUGIN_HOSTNAME`应该是您插件服务器的实际主机名。

接下来，我们可以定义 API 端点来创建、删除和获取特定用户的待办事项列表项。

```python
import json

import quart
import quart_cors
from quart import request

# Note: Setting CORS to allow chat.openapi.com is only required when running a localhost plugin
app = quart_cors.cors(quart.Quart(__name__), allow_origin="https://chat.openai.com")

_TODOS = {}


@app.post("/todos/<string:username>")
async def add_todo(username):
    request = await quart.request.get_json(force=True)
    if username not in _TODOS:
        _TODOS[username] = []
    _TODOS[username].append(request["todo"])
    return quart.Response(response='OK', status=200)


@app.get("/todos/<string:username>")
async def get_todos(username):
    return quart.Response(response=json.dumps(_TODOS.get(username, [])), status=200)


@app.delete("/todos/<string:username>")
async def delete_todo(username):
    request = await quart.request.get_json(force=True)
    todo_idx = request["todo_idx"]
    if 0 <= todo_idx < len(_TODOS[username]):
        _TODOS[username].pop(todo_idx)
    return quart.Response(response='OK', status=200)


@app.get("/logo.png")
async def plugin_logo():
    filename = 'logo.png'
    return await quart.send_file(filename, mimetype='image/png')


@app.get("/.well-known/ai-plugin.json")
async def plugin_manifest():
    host = request.headers['Host']
    with open("ai-plugin.json") as f:
        text = f.read()
        # This is a trick we do to populate the PLUGIN_HOSTNAME constant in the manifest
        text = text.replace("PLUGIN_HOSTNAME", f"https://{host}")
        return quart.Response(text, mimetype="text/json")


@app.get("/openapi.yaml")
async def openapi_spec():
    host = request.headers['Host']
    with open("openapi.yaml") as f:
        text = f.read()
        # This is a trick we do to populate the PLUGIN_HOSTNAME constant in the OpenAPI spec
        text = text.replace("PLUGIN_HOSTNAME", f"https://{host}")
        return quart.Response(text, mimetype="text/yaml")


def main():
    app.run(debug=True, host="0.0.0.0", port=5002)


if __name__ == "__main__":
    main()
```

最后，我们需要设置并定义一个 OpenAPI 规范，以匹配在我们的本地或远程服务器上定义的端点。您不需要通过规范公开API的全部功能，而可以选择让 ChatGPT 仅访问特定功能。

还有许多工具可以自动将您的服务器定义代码转换为 OpenAPI 规范，因此您无需手动完成。对于上面的 Python 代码，OpenAPI 规范将如下所示：

```yaml
openapi: 3.0.1
info:
    title: TODO Plugin
    description: A plugin that allows the user to create and manage a TODO list using ChatGPT. If you do not know the user's username, ask them first before making queries to the plugin. Otherwise, use the username "global".
    version: "v1"
servers:
    - url: PLUGIN_HOSTNAME
paths:
    /todos/{username}:
        get:
            operationId: getTodos
            summary: Get the list of todos
            parameters:
                - in: path
                  name: username
                  schema:
                      type: string
                  required: true
                  description: The name of the user.
            responses:
                "200":
                    description: OK
                    content:
                        application/json:
                            schema:
                                $ref: "#/components/schemas/getTodosResponse"
        post:
            operationId: addTodo
            summary: Add a todo to the list
            parameters:
                - in: path
                  name: username
                  schema:
                      type: string
                  required: true
                  description: The name of the user.
            requestBody:
                required: true
                content:
                    application/json:
                        schema:
                            $ref: "#/components/schemas/addTodoRequest"
            responses:
                "200":
                    description: OK
        delete:
            operationId: deleteTodo
            summary: Delete a todo from the list
            parameters:
                - in: path
                  name: username
                  schema:
                      type: string
                  required: true
                  description: The name of the user.
            requestBody:
                required: true
                content:
                    application/json:
                        schema:
                            $ref: "#/components/schemas/deleteTodoRequest"
            responses:
                "200":
                    description: OK

components:
    schemas:
        getTodosResponse:
            type: object
            properties:
                todos:
                    type: array
                    items:
                        type: string
                    description: The list of todos.
        addTodoRequest:
            type: object
            required:
                - todo
            properties:
                todo:
                    type: string
                    description: The todo to add to the list.
                    required: true
        deleteTodoRequest:
            type: object
            required:
                - todo_idx
            properties:
                todo_idx:
                    type: integer
                    description: The index of the todo to delete.
                    required: true
```

学习如何使用服务级身份验证构建简单的待办事项列表插件

首先，请查看[服务级身份验证](https://platform.openai.com/docs/plugins/authentication/service-level)页面，然后使用以下字段定义 `ai-plugin.json` 文件：

```json
{
    "schema_version": "v1",
    "name_for_human": "TODO List (service auth)",
    "name_for_model": "todo",
    "description_for_human": "Manage your TODO list. You can add, remove and view your TODOs.",
    "description_for_model": "Plugin for managing a TODO list, you can add, remove and view your TODOs.",
    "auth": {
        "type": "service_http",
        "authorization_type": "bearer",
        "verification_tokens": {
            "openai": "Replace_this_string_with_the_verification_token_generated_in_the_ChatGPT_UI"
        }
    },
    "api": {
        "type": "openapi",
        "url": "https://example.com/openapi.yaml"
    },
    "logo_url": "https://example.com/logo.png",
    "contact_email": "support@example.com",
    "legal_info_url": "https://example.com/legal"
}
```

请注意，服务级身份验证插件需要验证令牌。在设置服务访问令牌后，在 ChatGPT Web UI 中安装插件过程中会生成令牌。

接下来，您需要将“Example.com”更改为您的远程服务器的名称。

接下来，我们可以定义 API 端点，以便为特定用户创建、删除和获取待办事项列表项。这些端点还会检查用户是否已通过身份验证。

```python
import json

import quart
import quart_cors
from quart import request

app = quart_cors.cors(quart.Quart(__name__))

# This key can be anything, though you will likely want a randomly generated sequence.
_SERVICE_AUTH_KEY = "REPLACE_ME"
_TODOS = {}

def assert_auth_header(req):
    assert req.headers.get(
        "Authorization", None) == f"Bearer {_SERVICE_AUTH_KEY}"

@app.post("/todos/<string:username>")
async def add_todo(username):
    assert_auth_header(quart.request)
    request = await quart.request.get_json(force=True)
    if username not in _TODOS:
        _TODOS[username] = []
    _TODOS[username].append(request["todo"])
    return quart.Response(response='OK', status=200)

@app.get("/todos/<string:username>")
async def get_todos(username):
    assert_auth_header(quart.request)
    return quart.Response(response=json.dumps(_TODOS.get(username, [])), status=200)

@app.delete("/todos/<string:username>")
async def delete_todo(username):
    assert_auth_header(quart.request)
    request = await quart.request.get_json(force=True)
    todo_idx = request["todo_idx"]
    if 0 <= todo_idx < len(_TODOS[username]):
        _TODOS[username].pop(todo_idx)
    return quart.Response(response='OK', status=200)

@app.get("/logo.png")
async def plugin_logo():
    filename = 'logo.png'
    return await quart.send_file(filename, mimetype='image/png')

@app.get("/.well-known/ai-plugin.json")
async def plugin_manifest():
    host = request.headers['Host']
    with open("ai-plugin.json") as f:
        text = f.read()
        return quart.Response(text, mimetype="text/json")

@app.get("/openapi.yaml")
async def openapi_spec():
    host = request.headers['Host']
    with open("openapi.yaml") as f:
        text = f.read()
        return quart.Response(text, mimetype="text/yaml")

def main():
    app.run(debug=True, host="0.0.0.0", port=5002)

if __name__ == "__main__":
    main()
```

最后，我们需要设置和定义一个符合远程服务器定义的端点的 OpenAPI 规范。通常，在不同的身份验证方法下，OpenAPI 规范看起来是一样的。使用自动 OpenAPI 生成器将减少创建 OpenAPI 规范时出现错误的可能性。

```yaml
openapi: 3.0.1
info:
    title: TODO Plugin
    description: A plugin that allows the user to create and manage a TODO list using ChatGPT. If you do not know the user's username, ask them first before making queries to the plugin. Otherwise, use the username "global".
    version: "v1"
servers:
    - url: https://example.com
paths:
    /todos/{username}:
        get:
            operationId: getTodos
            summary: Get the list of todos
            parameters:
                - in: path
                  name: username
                  schema:
                      type: string
                  required: true
                  description: The name of the user.
            responses:
                "200":
                    description: OK
                    content:
                        application/json:
                            schema:
                                $ref: "#/components/schemas/getTodosResponse"
        post:
            operationId: addTodo
            summary: Add a todo to the list
            parameters:
                - in: path
                  name: username
                  schema:
                      type: string
                  required: true
                  description: The name of the user.
            requestBody:
                required: true
                content:
                    application/json:
                        schema:
                            $ref: "#/components/schemas/addTodoRequest"
            responses:
                "200":
                    description: OK
        delete:
            operationId: deleteTodo
            summary: Delete a todo from the list
            parameters:
                - in: path
                  name: username
                  schema:
                      type: string
                  required: true
                  description: The name of the user.
            requestBody:
                required: true
                content:
                    application/json:
                        schema:
                            $ref: "#/components/schemas/deleteTodoRequest"
            responses:
                "200":
                    description: OK

components:
    schemas:
        getTodosResponse:
            type: object
            properties:
                todos:
                    type: array
                    items:
                        type: string
                    description: The list of todos.
        addTodoRequest:
            type: object
            required:
                - todo
            properties:
                todo:
                    type: string
                    description: The todo to add to the list.
                    required: true
        deleteTodoRequest:
            type: object
            required:
                - todo_idx
            properties:
                todo_idx:
                    type: integer
                    description: The index of the todo to delete.
                    required: true
```

学习如何构建一个简单的体育统计插件

这个插件是一个简单的体育统计 API 的例子。在考虑要构建什么时，请牢记我们的域名政策和使用政策。

首先，定义一个包含以下字段的 `ai-plugin.json` 文件：

```json
{
    "schema_version": "v1",
    "name_for_human": "Sport Stats",
    "name_for_model": "sportStats",
    "description_for_human": "Get current and historical stats for sport players and games.",
    "description_for_model": "Get current and historical stats for sport players and games. Always display results using markdown tables.",
    "auth": {
        "type": "none"
    },
    "api": {
        "type": "openapi",
        "url": "PLUGIN_HOSTNAME/openapi.yaml"
    },
    "logo_url": "PLUGIN_HOSTNAME/logo.png",
    "contact_email": "support@example.com",
    "legal_info_url": "https://example.com/legal"
}
```

请注意，`PLUGIN_HOSTNAME` 应该是你插件服务器的实际主机名。

接下来，我们定义一个简单的体育服务插件的模拟 API。

```python
import json
import requests
import urllib.parse

import quart
import quart_cors
from quart import request

# Note: Setting CORS to allow chat.openapi.com is only required when running a localhost plugin
app = quart_cors.cors(quart.Quart(__name__), allow_origin="https://chat.openai.com")
HOST_URL = "https://example.com"

@app.get("/players")
async def get_players():
    query = request.args.get("query")
    res = requests.get(
        f"{HOST_URL}/api/v1/players?search={query}&page=0&per_page=100")
    body = res.json()
    return quart.Response(response=json.dumps(body), status=200)


@app.get("/teams")
async def get_teams():
    res = requests.get(
        "{HOST_URL}/api/v1/teams?page=0&per_page=100")
    body = res.json()
    return quart.Response(response=json.dumps(body), status=200)


@app.get("/games")
async def get_games():
    query_params = [("page", "0")]
    limit = request.args.get("limit")
    query_params.append(("per_page", limit or "100"))
    start_date = request.args.get("start_date")
    if start_date:
        query_params.append(("start_date", start_date))
    end_date = request.args.get("end_date")

    if end_date:
        query_params.append(("end_date", end_date))
    seasons = request.args.getlist("seasons")

    for season in seasons:
        query_params.append(("seasons[]", str(season)))
    team_ids = request.args.getlist("team_ids")

    for team_id in team_ids:
        query_params.append(("team_ids[]", str(team_id)))

    res = requests.get(
        f"{HOST_URL}/api/v1/games?{urllib.parse.urlencode(query_params)}")
    body = res.json()
    return quart.Response(response=json.dumps(body), status=200)


@app.get("/stats")
async def get_stats():
    query_params = [("page", "0")]
    limit = request.args.get("limit")
    query_params.append(("per_page", limit or "100"))
    start_date = request.args.get("start_date")
    if start_date:
        query_params.append(("start_date", start_date))
    end_date = request.args.get("end_date")

    if end_date:
        query_params.append(("end_date", end_date))
    player_ids = request.args.getlist("player_ids")

    for player_id in player_ids:
        query_params.append(("player_ids[]", str(player_id)))
    game_ids = request.args.getlist("game_ids")

    for game_id in game_ids:
        query_params.append(("game_ids[]", str(game_id)))
    res = requests.get(
        f"{HOST_URL}/api/v1/stats?{urllib.parse.urlencode(query_params)}")
    body = res.json()
    return quart.Response(response=json.dumps(body), status=200)


@app.get("/season_averages")
async def get_season_averages():
    query_params = []
    season = request.args.get("season")
    if season:
        query_params.append(("season", str(season)))
    player_ids = request.args.getlist("player_ids")

    for player_id in player_ids:
        query_params.append(("player_ids[]", str(player_id)))
    res = requests.get(
        f"{HOST_URL}/api/v1/season_averages?{urllib.parse.urlencode(query_params)}")
    body = res.json()
    return quart.Response(response=json.dumps(body), status=200)


@app.get("/logo.png")
async def plugin_logo():
    filename = 'logo.png'
    return await quart.send_file(filename, mimetype='image/png')


@app.get("/.well-known/ai-plugin.json")
async def plugin_manifest():
    host = request.headers['Host']
    with open("ai-plugin.json") as f:
        text = f.read()
        # This is a trick we do to populate the PLUGIN_HOSTNAME constant in the manifest
        text = text.replace("PLUGIN_HOSTNAME", f"https://{host}")
        return quart.Response(text, mimetype="text/json")


@app.get("/openapi.yaml")
async def openapi_spec():
    host = request.headers['Host']
    with open("openapi.yaml") as f:
        text = f.read()
        # This is a trick we do to populate the PLUGIN_HOSTNAME constant in the OpenAPI spec
        text = text.replace("PLUGIN_HOSTNAME", f"https://{host}")
        return quart.Response(text, mimetype="text/yaml")


def main():
    app.run(debug=True, host="0.0.0.0", port=5001)


if __name__ == "__main__":
    main()
```

最后，我们定义我们的 OpenAPI 规范：

```yaml
openapi: 3.0.1
info:
    title: Sport Stats
    description: Get current and historical stats for sport players and games.
    version: "v1"
servers:
    - url: PLUGIN_HOSTNAME
paths:
    /players:
        get:
            operationId: getPlayers
            summary: Retrieves all players from all seasons whose names match the query string.
            parameters:
                - in: query
                  name: query
                  schema:
                      type: string
                  description: Used to filter players based on their name. For example, ?query=davis will return players that have 'davis' in their first or last name.
            responses:
                "200":
                    description: OK
    /teams:
        get:
            operationId: getTeams
            summary: Retrieves all teams for the current season.
            responses:
                "200":
                    description: OK
    /games:
        get:
            operationId: getGames
            summary: Retrieves all games that match the filters specified by the args. Display results using markdown tables.
            parameters:
                - in: query
                  name: limit
                  schema:
                      type: string
                  description: The max number of results to return.
                - in: query
                  name: seasons
                  schema:
                      type: array
                      items:
                          type: string
                  description: Filter by seasons. Seasons are represented by the year they began. For example, 2018 represents season 2018-2019.
                - in: query
                  name: team_ids
                  schema:
                      type: array
                      items:
                          type: string
                  description: Filter by team ids. Team ids can be determined using the getTeams function.
                - in: query
                  name: start_date
                  schema:
                      type: string
                  description: A single date in 'YYYY-MM-DD' format. This is used to select games that occur on or after this date.
                - in: query
                  name: end_date
                  schema:
                      type: string
                  description: A single date in 'YYYY-MM-DD' format. This is used to select games that occur on or before this date.
            responses:
                "200":
                    description: OK
    /stats:
        get:
            operationId: getStats
            summary: Retrieves stats that match the filters specified by the args. Display results using markdown tables.
            parameters:
                - in: query
                  name: limit
                  schema:
                      type: string
                  description: The max number of results to return.
                - in: query
                  name: player_ids
                  schema:
                      type: array
                      items:
                          type: string
                  description: Filter by player ids. Player ids can be determined using the getPlayers function.
                - in: query
                  name: game_ids
                  schema:
                      type: array
                      items:
                          type: string
                  description: Filter by game ids. Game ids can be determined using the getGames function.
                - in: query
                  name: start_date
                  schema:
                      type: string
                  description: A single date in 'YYYY-MM-DD' format. This is used to select games that occur on or after this date.
                - in: query
                  name: end_date
                  schema:
                      type: string
                  description: A single date in 'YYYY-MM-DD' format. This is used to select games that occur on or before this date.
            responses:
                "200":
                    description: OK
    /season_averages:
        get:
            operationId: getSeasonAverages
            summary: Retrieves regular season averages for the given players. Display results using markdown tables.
            parameters:
                - in: query
                  name: season
                  schema:
                      type: string
                  description: Defaults to the current season. A season is represented by the year it began. For example, 2018 represents season 2018-2019.
                - in: query
                  name: player_ids
                  schema:
                      type: array
                      items:
                          type: string
                  description: Filter by player ids. Player ids can be determined using the getPlayers function.
            responses:
                "200":
                    description: OK
```

学习如何构建一个简单的OAuth待办事项插件



为了创建一个OAuth插件，我们先定义一个 `ai-plugin.json` 文件，其中认证类型设置为 `oauth` ：

```json
{
    "schema_version": "v1",
    "name_for_human": "TODO List (OAuth)",
    "name_for_model": "todo_oauth",
    "description_for_human": "Manage your TODO list. You can add, remove and view your TODOs.",
    "description_for_model": "Plugin for managing a TODO list, you can add, remove and view your TODOs.",
    "auth": {
        "type": "oauth",
        "client_url": "PLUGIN_HOSTNAME/oauth",
        "scope": "",
        "authorization_url": "PLUGIN_HOSTNAME/auth/oauth_exchange",
        "authorization_content_type": "application/json",
        "verification_tokens": {
            "openai": "Replace_this_string_with_the_verification_token_generated_in_the_ChatGPT_UI"
        }
    },
    "api": {
        "type": "openapi",
        "url": "PLUGIN_HOSTNAME/openapi.yaml"
    },
    "logo_url": "PLUGIN_HOSTNAME/logo.png",
    "contact_email": "contact@example.com",
    "legal_info_url": "http://www.example.com/legal"
}
```

接下来，我们需要定义我们的OAuth服务。这个OAuth示例不适用于生产环境下的场景，它的目的是为了突出一个简单的OAuth流程的样子，以便开发人员能够获得构建面向生产解决方案的经验。

```python
import json

import quart
import quart_cors
from quart import request

app = quart_cors.cors(quart.Quart(__name__), allow_origin="*")

_TODOS = {}

@app.post("/todos/<string:username>")
async def add_todo(username):
    request = await quart.request.get_json(force=True)
    if username not in _TODOS:
        _TODOS[username] = []
    _TODOS[username].append(request["todo"])
    return quart.Response(response='OK', status=200)

@app.get("/todos/<string:username>")
async def get_todos(username):
    print(request.headers)
    return quart.Response(response=json.dumps(_TODOS.get(username, [])), status=200)

@app.delete("/todos/<string:username>")
async def delete_todo(username):
    request = await quart.request.get_json(force=True)
    todo_idx = request["todo_idx"]
    # fail silently, it's a simple plugin
    if 0 <= todo_idx < len(_TODOS[username]):
        _TODOS[username].pop(todo_idx)
    return quart.Response(response='OK', status=200)

@app.get("/logo.png")
async def plugin_logo():
    filename = 'logo.png'
    return await quart.send_file(filename, mimetype='image/png')

@app.get("/.well-known/ai-plugin.json")
async def plugin_manifest():
    host = request.headers['Host']
    with open("manifest.json") as f:
        text = f.read()
        text = text.replace("PLUGIN_HOSTNAME", f"https://{host}")
        return quart.Response(text, mimetype="text/json")

@app.get("/openapi.yaml")
async def openapi_spec():
    host = request.headers['Host']
    with open("openapi.yaml") as f:
        text = f.read()
        text = text.replace("PLUGIN_HOSTNAME", f"https://{host}")
        return quart.Response(text, mimetype="text/yaml")

@app.get("/oauth")
async def oauth():
    query_string = request.query_string.decode('utf-8')
    parts = query_string.split('&')
    kvps = {}
    for part in parts:
        k, v = part.split('=')
        v = v.replace("%2F", "/").replace("%3A", ":")
        kvps[k] = v
    print("OAuth key value pairs from the ChatGPT Request: ", kvps)
    url = kvps["redirect_uri"] + f"?code={OPENAI_CODE}"
    print("URL: ", url)
    return quart.Response(
        f'<a href="{url}">Click to authorize</a>'
    )

# Sample names
OPENAI_CLIENT_ID = "id"
OPENAI_CLIENT_SECRET = "secret"
OPENAI_CODE = "abc123"
OPENAI_TOKEN = "def456"

@app.post("/auth/oauth_exchange")
async def oauth_exchange():
    request = await quart.request.get_json(force=True)
    print(f"oauth_exchange {request=}")

    if request["client_id"] != OPENAI_CLIENT_ID:
        raise RuntimeError("bad client ID")
    if request["client_secret"] != OPENAI_CLIENT_SECRET:
        raise RuntimeError("bad client secret")
    if request["code"] != OPENAI_CODE:
        raise RuntimeError("bad code")

    return {
        "access_token": OPENAI_TOKEN,
        "token_type": "bearer"
    }

def main():
    app.run(debug=True, host="0.0.0.0", port=5002)

if __name__ == "__main__":
    main()
```

最后，像我们的其他示例一样，我们根据端点定义一个简单的OpenAPI文件：

```yaml
openapi: 3.0.1
info:
    title: TODO Plugin
    description: A plugin that allows the user to create and manage a TODO list using ChatGPT. If you do not know the user's username, ask them first before making queries to the plugin. Otherwise, use the username "global".
    version: "v1"
servers:
    - url: PLUGIN_HOSTNAME
paths:
    /todos/{username}:
        get:
            operationId: getTodos
            summary: Get the list of todos
            parameters:
                - in: path
                  name: username
                  schema:
                      type: string
                  required: true
                  description: The name of the user.
            responses:
                "200":
                    description: OK
                    content:
                        application/json:
                            schema:
                                $ref: "#/components/schemas/getTodosResponse"
        post:
            operationId: addTodo
            summary: Add a todo to the list
            parameters:
                - in: path
                  name: username
                  schema:
                      type: string
                  required: true
                  description: The name of the user.
            requestBody:
                required: true
                content:
                    application/json:
                        schema:
                            $ref: "#/components/schemas/addTodoRequest"
            responses:
                "200":
                    description: OK
        delete:
            operationId: deleteTodo
            summary: Delete a todo from the list
            parameters:
                - in: path
                  name: username
                  schema:
                      type: string
                  required: true
                  description: The name of the user.
            requestBody:
                required: true
                content:
                    application/json:
                        schema:
                            $ref: "#/components/schemas/deleteTodoRequest"
            responses:
                "200":
                    description: OK

components:
    schemas:
        getTodosResponse:
            type: object
            properties:
                todos:
                    type: array
                    items:
                        type: string
                    description: The list of todos.
        addTodoRequest:
            type: object
            required:
                - todo
            properties:
                todo:
                    type: string
                    description: The todo to add to the list.
                    required: true
        deleteTodoRequest:
            type: object
            required:
                - todo_idx
            properties:
                todo_idx:
                    type: integer
                    description: The index of the todo to delete.
                    required: true
```

学习如何构建语义搜索和检索插件

[ChatGPT检索插件](https://github.com/openai/chatgpt-retrieval-plugin)是一个更完整的代码示例。插件的范围很大，因此我们鼓励您阅读代码，以查看更高级的插件是什么样子。

检索插件包括：

- 支持多个矢量数据库提供程序
- 所有四种不同的身份验证方法
- 多个不同的API功能