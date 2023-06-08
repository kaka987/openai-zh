[[库](https://platform.openai.com/docs/libraries/libraries)

[Python库](https://platform.openai.com/docs/libraries/python-library)

我们提供了一个Python库，你可以按照以下方式进行安装：

```bash
$ pip install openai
```

安装完成后，你可以使用绑定和你的密钥来运行以下操作：

```python
import os
import openai

# Load your API key from an environment variable or secret management service
openai.api_key = os.getenv("OPENAI_API_KEY")

response = openai.Completion.create(model="text-davinci-003", prompt="Say this is a test", temperature=0, max_tokens=7)
```

绑定还会安装一个命令行工具，你可以按以下方式使用：

```bash
$ openai api completions.create -m text-davinci-003 -p "Say this is a test" -t 0 -M 7 --stream
```

[[Node.js库](https://platform.openai.com/docs/libraries/node-js-library)

我们还有一个Node.js库，可以通过在你的Node.js项目目录中运行以下命令来安装：

```bash
$ npm install openai
```

一旦安装完成，你可以使用该库和你的secret key运行以下操作：

```javascript
const { Configuration, OpenAIApi } = require("openai");
const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);
const response = await openai.createCompletion({
  model: "text-davinci-003",
  prompt: "Say this is a test",
  temperature: 0,
  max_tokens: 7,
});
```

[Azure OpenAI库](https://platform.openai.com/docs/libraries/azure-openai-libraries)

微软的Azure团队维护着与OpenAI API和Azure OpenAI服务兼容的库。阅读下面的库文档以了解如何在OpenAI API中使用它们。

- [.NET的Azure OpenAI客户端库](https://github.com/Azure/azure-sdk-for-net/tree/main/sdk/openai/Azure.AI.OpenAI)
- [JavaScript的Azure OpenAI客户端库](https://github.com/Azure/azure-sdk-for-js/tree/main/sdk/openai/openai)
- [Java的Azure OpenAI客户端库](https://github.com/Azure/azure-sdk-for-java/tree/main/sdk/openai/azure-ai-openai-client)

[社区库](https://platform.openai.com/docs/libraries/community-libraries)

下面的库是由更广泛的开发者社区构建和维护的。如果您想在这里添加新的库，请按照我们在[帮助中心文章](https://help.openai.com/en/articles/6684216-adding-your-api-client-to-the-community-libraries-page)中的说明添加社区库。

请注意，OpenAI不验证这些项目的正确性或安全性。**请自行承担使用风险！****

[c＃/.net](https://platform.openai.com/docs/libraries/c-net)

 -  [betalgo.openai](https://github.com/betalgo/openai)由[betalgo](https://github.com/betalgo)提供
 -  [openai-api-dotnet](https://github.com/okgodoit/openai-api-dotnet)由[okgodoit](https://github.com/okgodoit)提供
 -  [openai-dotnet](https：//github.com/rageagainstthepixel/openai-dotnet)由[rageagainstthepixel](https://github.com/rageagainstthpepixel)提供

[C++](https://platform.openai.com/docs/libraries/c)

- [liboai](https://github.com/D7EAD/liboai) 由 [D7EAD](https://github.com/D7EAD)提供

[Clojure](https://platform.openai.com/docs/libraries/clojure)

- [openai-clojure](https://github.com/wkok/openai-clojure) 由 [wkok](https://github.com/wkok) 提供

[Crystal](https://platform.openai.com/docs/libraries/crystal)

- [openai-crystal](https://github.com/sferik/openai-crystal) 由 [sferik](https://github.com/sferik) 提供

[Dart/Flutter](https://platform.openai.com/docs/libraries/dart-flutter)

- [openai](https://github.com/anasfik/openai) 由 [anasfik](https://github.com/anasfik)提供

[Delphi](https://platform.openai.com/docs/libraries/delphi)

- [DelphiOpenAI](https://github.com/HemulGM/DelphiOpenAI) 由 [HemulGM](https://github.com/HemulGM)提供

[Elixir](https://platform.openai.com/docs/libraries/elixir)

- [openai.ex](https://github.com/mgallo/openai.ex) 由[mgallo](https://github.com/mgallo)提供

[Go](https://platform.openai.com/docs/libraries/go)

- [go-gpt3](https://github.com/sashabaranov/go-gpt3)由 [sashabaranov](https://github.com/sashabaranov)提供

[Java](https://platform.openai.com/docs/libraries/java)

- [openai-java](https://github.com/TheoKanning/openai-java) 由 [Theo Kanning](https://github.com/TheoKanning)提供

[Julia](https://platform.openai.com/docs/libraries/julia)

- [OpenAI.jl](https://github.com/rory-linehan/OpenAI.jl)由[rory-linehan](https://github.com/rory-linehan)提供

[Kotlin](https://platform.openai.com/docs/libraries/kotlin)

- [openai-kotlin](https://github.com/Aallam/openai-kotlin)由 [Mouaad Aallam](https://github.com/Aallam)提供

[Node.js](https://platform.openai.com/docs/libraries/node-js)

- [openai-api](https://www.npmjs.com/package/openai-api)由 [Njerschow](https://github.com/Njerschow)提供
- [openai-api-node](https://www.npmjs.com/package/openai-api-node) 由 [erlapso](https://github.com/erlapso)提供
- [gpt-x](https://www.npmjs.com/package/gpt-x)由 [ceifa](https://github.com/ceifa)提供
- [gpt3](https://www.npmjs.com/package/gpt3) 由 [poteat](https://github.com/poteat)提供
- [gpts](https://www.npmjs.com/package/gpts) 由 [thencc](https://github.com/thencc)提供
- [@dalenguyen/openai](https://www.npmjs.com/package/@dalenguyen/openai)由 [dalenguyen](https://github.com/dalenguyen)提供
- [tectalic/openai](https://github.com/tectalichq/public-openai-client-js)由 [tectalic](https://tectalic.com/)提供

[PHP](https://platform.openai.com/docs/libraries/php)

- [orhanerday/open-ai](https://packagist.org/packages/orhanerday/open-ai)由 [orhanerday](https://github.com/orhanerday)提供
- [tectalic/openai](https://github.com/tectalichq/public-openai-client-php)由 [tectalic](https://tectalic.com/)提供
- [openai-php clinet](https://github.com/openai-php/client) 由 [openai-php](https://github.com/openai-php)提供

[Python](https://platform.openai.com/docs/libraries/python)

- [chronology](https://github.com/OthersideAI/chronology)由[OthersideAI](https://www.othersideai.com/)提供

[R](https://platform.openai.com/docs/libraries/r)

- [rgpt3](https://github.com/ben-aaron188/rgpt3)由[ben-aaron188](https://github.com/ben-aaron188)提供

[Ruby](https://platform.openai.com/docs/libraries/ruby)

- [openai](https://github.com/nileshtrivedi/openai/)由 [nileshtrivedi](https://github.com/nileshtrivedi)提供
- [ruby-openai](https://github.com/alexrudall/ruby-openai)由 [alexrudall](https://github.com/alexrudall)提供

[Rust](https://platform.openai.com/docs/libraries/rust)

- [async-openai](https://github.com/64bit/async-openai)由 [64bit](https://github.com/64bit)提供
- [fieri](https://github.com/lbkolev/fieri) 由 [lbkolev](https://github.com/lbkolev)提供

[Scala](https://platform.openai.com/docs/libraries/scala)

- [openai-scala-client](https://github.com/cequence-io/openai-scala-client) 由 [cequence-io](https://github.com/cequence-io)提供

[Swift](https://platform.openai.com/docs/libraries/swift)

- [OpenAIKit](https://github.com/dylanshine/openai-kit)由 [dylanshine](https://github.com/dylanshine)提供

[Unity](https://platform.openai.com/docs/libraries/unity)

- [OpenAi-Api-Unity](https://github.com/hexthedev/OpenAi-Api-Unity)由 [hexthedev](https://github.com/hexthedev)提供
- [com.openai.unity](https://github.com/RageAgainstThePixel/com.openai.unity)由[RageAgainstThePixel](https://github.com/RageAgainstThePixel)提供

[Unreal Engine](https://platform.openai.com/docs/libraries/unreal-engine)

- [OpenAI-Api-Unreal](https://github.com/KellanM/OpenAI-Api-Unreal) 由 [KellanM](https://github.com/KellanM)提供