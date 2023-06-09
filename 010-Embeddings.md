[嵌入式](https://platform.openai.com/docs/guides/embeddings/embeddings)

[什么是嵌入？](https://platform.openai.com/docs/guides/embeddings/what-are-embeddings)

OpenAI的文本嵌入（text embeddings）测量文本字符串之间的关联性。嵌入通常用于以下领域：

- **搜索**（按查询字符串的相关性排序结果）
- **聚类分析**（根据相似性将文本字符串分组）
- **推荐系统**（推荐与相关文本字符串相关联的内容）
- **异常检测**（识别与其他文本字符串关联度低的异常值）
- **多样性度量**（分析相似性分布）
- **分类**（通过最相似的标签将文本字符串分类）

嵌入是由一组浮点数（列表）组成的向量，嵌入向量之间的[距离](https://platform.openai.com/docs/guides/embeddings/which-distance-function-should-i-is)可以度量它们之间的相关性。较小的距离表明相关性较强，而较大的距离则表明相关性较弱。

请访问我们的[定价页面](https://openai.com/api/pricing/)了解关于嵌入的定价信息。请求的计费基于[输入](https://platform.openai.com/docs/api-reference/embeddings/create#embeddings/create-input)中的[token](https://platform.openai.com/tokenizer)数。

**要查看嵌入的实际应用，请查看我们的代码示例：**

- 分类
- 主题聚类
- 搜索
- 推荐系统

[浏览示例](https://platform.openai.com/docs/guides/embeddings/use-cases)

[如何获取嵌入](https://platform.openai.com/docs/guides/embeddings/how-to-get-embeddings)

要获取嵌入，请将文本字符串发送到[嵌入API终端](https://platform.openai.com/docs/api-reference/embeddings)，并选择一个嵌入模型ID（例如，`text-embedding-ada-002`）。响应将包含一个嵌入，您可以提取、保存和使用。

请求示例：

示例：获取嵌入

curl


复制

```shell
curl https://api.openai.com/v1/embeddings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "input": "Your text string goes here",
    "model": "text-embedding-ada-002"
  }'
```

示例响应：



```text
{
  "data": [
    {
      "embedding": [
        -0.006929283495992422,
        -0.005336422007530928,
        ...
        -4.547132266452536e-05,
        -0.024047505110502243
      ],
      "index": 0,
      "object": "embedding"
    }
  ],
  "model": "text-embedding-ada-002",
  "object": "list",
  "usage": {
    "prompt_tokens": 5,
    "total_tokens": 5
  }
}
```

在[OpenAI Cookbook](https://github.com/openai/openai-cookbook/)中可以查看更多Python代码示例。

在使用OpenAI嵌入向量时，请注意其[限制和风险](https://platform.openai.com/docs/guides/embeddings/limitations-risks)。

[嵌入模型](https://platform.openai.com/docs/guides/embeddings/embedding-models)

OpenAI提供了一个第二代嵌入模型（用模型ID中的`-002`表示）和16个第一代模型（用模型ID中的`-001`表示）。

我们建议几乎所有用例都使用text-embedding-ada-002。它更好，更便宜，更容易使用。阅读[博客文章公告](https://openai.com/blog/new-and-improved-embedding-model)。

| 模型生成 | 标记器      | 最大输入标记数量 | 知识截断 |
| :------- | :---------- | :--------------- | :------- |
| V2       | cl100k_base | 8191             | Sep 2021 |
| V1       | GPT-2/GPT-3 | 2046             | Aug 2020 |

使用价格按输入标记计价，标价为每1000个标记0.0004美元，或者约合1美元可以获得大约3000页的量（假设每页大约800个标记）:

| 模型                   | 每美元的大致页数 | 在[beir](https://paperswithcode.com/sota/zero-hot-text-text-search-on-beir)搜索评估上的样例性能表现 |
| :--------------------- | :--------------- | :----------------------------------------------------------- |
| text-embedding-ada-002 | 3000             | 53.9                                                         |
| *-davinci-*-001        | 6                | 52.8                                                         |
| *-curie-*-001          | 60               | 50.9                                                         |
| *-babbage-*-001        | 240              | 50.4                                                         |
| *-ada-*-001            | 300              | 49.0                                                         |

[第二代模型](https://platform.openai.com/docs/guides/embeddings/second-generation-models)

| 模型名称               | 分词器      | 最大输入令牌数 | 输出维度 |
| :--------------------- | :---------- | :------------- | :------- |
| text-embedding-ada-002 | cl100k_base | 8191           | 1536     |

第一代模型（不建议使用）



所有第一代模型（以“-001”结尾的那些）都使用[GPT-3分词器](https://platform.openai.com/tokenizer)，最大输入为2046个标记。

第一代嵌入由五个不同的模型系列生成，针对三个不同的任务进行了调整：文本搜索，文本相似性和代码搜索。搜索模型成对出现：一个用于短查询，一个用于长文档。每个系列包括最多四个模型，速度和质量形成一个谱系。

| 模型    | 输出维度 |
| :------ | :------- |
| Ada     | 1024     |
| Babbage | 2048     |
| Curie   | 4096     |
| Davinci | 12288    |

Davinci是最强大的模型，但比其他模型要慢且更昂贵。Ada是最不强大的模型，但是速度快、价格便宜。

[相似性嵌入](https://platform.openai.com/docs/guides/embeddings/similarity-embeddings)

相似性模型最擅长捕捉文本之间的语义相似性。

| 应用案例                     | 可用模型                                                     |
| :--------------------------- | :----------------------------------------------------------- |
| 聚类、回归、异常检测、可视化 | `text-similarity-ada-001` `text-similarity-babbage-001` `text-similarity-curie-001` `text-similarity-davinci-001` |

[文本搜索嵌入](https://platform.openai.com/docs/guides/embeddings/text-search-embeddings)

文本搜索模型可以帮助衡量哪些长文档最相关于一个短搜索查询。使用两个模型：一个用于为搜索查询生成嵌入向量，另一个用于为要排序的文档生成嵌入向量。与查询嵌入向量最接近的文档嵌入向量应该是最相关的。

| 应用场景                     | 可用模型                                                     |
| :--------------------------- | :----------------------------------------------------------- |
| 搜索、上下文相关性、信息检索 | `text-search-ada-doc-001` `text-search-ada-query-001` `text-search-babbage-doc-001` `text-search-babbage-query-001` `text-search-curie-doc-001` `text-search-curie-query-001` `text-search-davinci-doc-001` `text-search-davinci-query-001` |

[代码搜索嵌入](https://platform.openai.com/docs/guides/embeddings/code-search-embeddings)

与搜索嵌入类似，代码搜索嵌入也有两种类型：一种用于嵌入自然语言搜索查询，另一种用于嵌入要检索的代码片段。

| 应用场景         | 可用模型                                                     |
| :--------------- | :----------------------------------------------------------- |
| 代码搜索和相关性 | `code-search-ada-code-001` `code-search-ada-text-001` `code-search-babbage-code-001` `code-search-babbage-text-001` |



针对“-001”文本嵌入模型（而不是“-002”或代码嵌入模型），我们建议将输入中的换行符 (`\n`) 替换为单个空格，因为我们发现如果存在换行符，结果会更差。

[使用案例](https://platform.openai.com/docs/guides/embeddings/use-cases)

以下是一些典型使用案例。我们将使用 [Amazon fine-food reviews 数据集](https://www.kaggle.com/snap/amazon-fine-food-reviews) 来进行下面的示例。

[获取嵌入](https://platform.openai.com/docs/guides/embeddings/obtaining-the-embeddings)

该数据集包含截至 2012 年 10 月用户在亚马逊留下的共 568454 条食品评论。我们将使用最近的一千条评论的子集作为演示。这些评论是英文的，并且往往是正面或负面的。每个评论都有一个 ProductId、UserId、Score、标题（Summary）和正文（Text）。例如：

| 产品编号   | 用户编号       | 分数 | 概括       | 文本                                                   |
| :--------- | :------------- | :--- | :--------- | :----------------------------------------------------- |
| B001E4KFG0 | A3SGXH7AUHU8GW | 5    | 优质的狗粮 | 我买了几罐Vitality的罐头食品，以作为我狗的一部分饮食。 |
| B00813GRG4 | A1D87F6ZCVE5NK | 1    | 与宣传不符 | 产品到达标有巨型盐的花生...                            |

我们将把评论摘要和评论正文合并为一个单独的文本。模型将对这个合并后的文本进行编码，并输出一个单一的向量嵌入。

[获得_dataset.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Obtain_dataset.ipynb)



```python
def get_embedding(text, model="text-embedding-ada-002"):
   text = text.replace("\n", " ")
   return openai.Embedding.create(input = [text], model=model)['data'][0]['embedding']

df['ada_embedding'] = df.combined.apply(lambda x: get_embedding(x, model='text-embedding-ada-002'))
df.to_csv('output/embedded_1k_reviews.csv', index=False)
```

要从已保存的文件中加载数据，您可以运行以下命令：

```python
import pandas as pd

df = pd.read_csv('output/embedded_1k_reviews.csv')
df['ada_embedding'] = df.ada_embedding.apply(eval).apply(np.array)
```

二维数据可视化



[Visualizing_embeddings_in_2D.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Visualizing_embeddings_in_2D.ipynb)

嵌入向量的大小取决于底层模型的复杂程度。为了将这个高维数据可视化，我们使用t-SNE算法将数据转化为二维坐标。

我们根据评论者给予的星级评分对每个评论进行着色：

- 1星：红色
- 2星：暗橙色
- 3星：金色
- 4星：青绿色
- 5星：深绿色

![Amazon ratings visualized in language using t-SNE](https://cdn.openai.com/API/docs/images/embeddings-tsne.png)

这个可视化似乎生成了大约三个聚类，其中一个主要由负面评论组成。



```python
import pandas as pd
from sklearn.manifold import TSNE
import matplotlib.pyplot as plt
import matplotlib

df = pd.read_csv('output/embedded_1k_reviews.csv')
matrix = df.ada_embedding.apply(eval).to_list()

# Create a t-SNE model and transform the data
tsne = TSNE(n_components=2, perplexity=15, random_state=42, init='random', learning_rate=200)
vis_dims = tsne.fit_transform(matrix)

colors = ["red", "darkorange", "gold", "turquiose", "darkgreen"]
x = [x for x,y in vis_dims]
y = [y for x,y in vis_dims]
color_indices = df.Score.values - 1

colormap = matplotlib.colors.ListedColormap(colors)
plt.scatter(x, y, c=color_indices, cmap=colormap, alpha=0.3)
plt.title("Amazon ratings visualized in language using t-SNE")
```

作为ML算法的文本特征编码器的嵌入

[Regression_using_embeddings.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Regression_using_embeddings.ipynb)

嵌入可以用作机器学习模型中的通用自由文本特征编码器。如果一些相关的输入是自由文本，则将嵌入纳入模型将改善任何机器学习模型的性能。嵌入也可以用作ML模型中的分类特征编码器。如果分类变量的名称有意义且数量众多，例如职称，则可以增加大部分价值。这项任务中，相似度嵌入通常比搜索嵌入表现更好。

我们观察到，一般来说嵌入表示非常丰富和信息密集。例如，使用SVD或PCA将输入的维度降低10％，通常会导致特定任务的下游性能变差。

此代码将数据拆分为训练集和测试集，将由以下两个用例使用，即回归和分类。



```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    list(df.ada_embedding.values),
    df.Score,
    test_size = 0.2,
    random_state=42
)
```

[使用嵌入特征进行回归](https://platform.openai.com/docs/guides/embeddings/regression-using-the-embedding-features)

嵌入提供了一种优雅的方式来预测数值。在这个例子中，我们根据评论的文本预测评价者的星级评分。由于嵌入所包含的语义信息非常高，即使评论非常少，预测结果仍然不错。

我们假设评分是1到5之间的连续变量，并允许算法预测任何浮点值。ML算法最小化预测值与真实分数之间的距离，并实现了0.39的平均绝对误差，这意味着平均预测误差不到半个星级。



```python
from sklearn.ensemble import RandomForestRegressor

rfr = RandomForestRegressor(n_estimators=100)
rfr.fit(X_train, y_train)
preds = rfr.predict(X_test)
```

使用嵌入特征进行分类

[Classification_using_embeddings.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Classification_using_embeddings.ipynb)

这一次，我们不再让算法预测在1到5之间的任何值，而是尝试将评论的星级精确地分类到5个桶中，范围从1到5颗星。

在训练之后，模型学会了更好地预测1颗星和5颗星的评论，而对于更细致的评论（2-4颗星），模型的表现不如前者，这很可能是由于情感表达的更加极端。

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, accuracy_score

clf = RandomForestClassifier(n_estimators=100)
clf.fit(X_train, y_train)
preds = clf.predict(X_test)
```

零样本分类

[Zero-shot_classification_with_embeddings.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Zero-shot_classification_with_embeddings.ipynb)

我们可以使用嵌入进行零样本分类，无需任何有标签的训练数据。对于每个类别，我们嵌入类别名称或类别的简短描述。在零样本的情况下对一些新的文本进行分类时，我们会将其嵌入与所有类别嵌入进行比较，并预测具有最高相似度的类别。

```python
from openai.embeddings_utils import cosine_similarity, get_embedding

df= df[df.Score!=3]
df['sentiment'] = df.Score.replace({1:'negative', 2:'negative', 4:'positive', 5:'positive'})

labels = ['negative', 'positive']
label_embeddings = [get_embedding(label, model=model) for label in labels]

def label_score(review_embedding, label_embeddings):
   return cosine_similarity(review_embedding, label_embeddings[1]) - cosine_similarity(review_embedding, label_embeddings[0])

prediction = 'positive' if label_score('Sample Review', label_embeddings) > 0 else 'negative'
```

获得用户和产品嵌入以进行冷启动推荐

[User_and_product_embeddings.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/User_and_product_embeddings.ipynb)

我们可以通过平均每个用户的所有评论来获得用户嵌入。同样地，我们可以通过平均有关该产品的所有评论来获得产品嵌入。为了展示这种方法的有用性，我们使用50,000条评论的子集来涵盖每个用户和每个产品的更多评论。

我们在一个独立的测试集上评估这些嵌入的有用性，在那里我们绘制用户和产品嵌入相似性作为评分的函数。有趣的是，基于这种方法，即使在用户接收产品之前，我们也可以比随机更好地预测他们是否会喜欢该产品。

![Boxplot grouped by Score](https://cdn.openai.com/API/docs/images/embeddings-boxplot.png)



```python
user_embeddings = df.groupby('UserId').ada_embedding.apply(np.mean)
prod_embeddings = df.groupby('ProductId').ada_embedding.apply(np.mean)
```

聚类

[Clustering.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Clustering.ipynb)

聚类是理解大量文本数据的一种方法。嵌入在此任务中非常有用，因为它们提供每个文本的语义上有意义的向量表示。因此，在无监督的情况下，聚类将揭示数据集中隐藏的分组。

在这个例子中，我们发现了四个不同的类簇：一个聚焦于狗粮，一个聚焦于负面评价，另外两个聚焦于正面评价。

![Clusters identified visualized in language 2d using t-SNE](https://cdn.openai.com/API/docs/images/embeddings-cluster.png)



```python
import numpy as np
from sklearn.cluster import KMeans

matrix = np.vstack(df.ada_embedding.values)
n_clusters = 4

kmeans = KMeans(n_clusters = n_clusters, init='k-means++', random_state=42)
kmeans.fit(matrix)
df['Cluster'] = kmeans.labels_
```

使用嵌入进行文本搜索

[Semantic_text_search_using_embeddings.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Semantic_text_search_using_embeddings.ipynb)

为了检索最相关的文档，我们使用查询向量和每个文档的嵌入向量之间的余弦相似度，并返回得分最高的文档。

```python
from openai.embeddings_utils import get_embedding, cosine_similarity

def search_reviews(df, product_description, n=3, pprint=True):
   embedding = get_embedding(product_description, model='text-embedding-ada-002')
   df['similarities'] = df.ada_embedding.apply(lambda x: cosine_similarity(x, embedding))
   res = df.sort_values('similarities', ascending=False).head(n)
   return res

res = search_reviews(df, 'delicious beans', n=3)
```

使用嵌入进行代码搜索

[Code_search.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Code_search.ipynb)

代码搜索与基于嵌入的文本搜索类似。我们提供一种方法来从给定代码库中的所有 Python 文件中提取 Python 函数。然后，每个函数都由 `text-embedding-ada-002` 模型进行索引。

要执行代码搜索，我们使用相同的模型将查询嵌入自然语言中。然后，我们计算结果查询嵌入和每个函数嵌入之间的余弦相似度。最高余弦相似度的结果最相关。

```python
from openai.embeddings_utils import get_embedding, cosine_similarity

df['code_embedding'] = df['code'].apply(lambda x: get_embedding(x, model='text-embedding-ada-002'))

def search_functions(df, code_query, n=3, pprint=True, n_lines=7):
   embedding = get_embedding(code_query, model='text-embedding-ada-002')
   df['similarities'] = df.code_embedding.apply(lambda x: cosine_similarity(x, embedding))

   res = df.sort_values('similarities', ascending=False).head(n)
   return res
res = search_functions(df, 'Completions API tests', n=3)
```

使用嵌入进行推荐

[Recommendation_using_embeddings.ipynb](https://github.com/openai/openai-cookbook/blob/main/examples/Recommendation_using_embeddings.ipynb)

因为嵌入向量之间的较短距离表示更大的相似性，所以嵌入向量可以用于推荐。

下面，我们演示一个基本的推荐系统。它接受一个字符串列表和一个“源”字符串，计算它们的嵌入，然后返回按相似性从最相似到最不相似的字符串排名。作为一个具体的例子，下面链接的笔记本将该函数的一个版本应用于 [AG 新闻数据集](http://groups.di.unipi.it/~gulli/AG_corpus_of_news_articles.html)（缩小到 2,000 条新闻文章描述），以返回与任何给定源文章最相似的前 5 篇文章。

```python
def recommendations_from_strings(
   strings: List[str],
   index_of_source_string: int,
   model="text-embedding-ada-002",
) -> List[int]:
   """Return nearest neighbors of a given string."""

   # get embeddings for all strings
   embeddings = [embedding_from_string(string, model=model) for string in strings]

   # get the embedding of the source string
   query_embedding = embeddings[index_of_source_string]

   # get distances between the source embedding and other embeddings (function from embeddings_utils.py)
   distances = distances_from_embeddings(query_embedding, embeddings, distance_metric="cosine")

   # get indices of nearest neighbors (function from embeddings_utils.py)
   indices_of_nearest_neighbors = indices_of_nearest_neighbors_from_distances(distances)
   return indices_of_nearest_neighbors
```

[[限制和风险](https://platform.openai.com/docs/guides/embeddings/limitations-risks)

在某些情况下，我们的嵌入模型可能不可靠或存在社会风险，并且在缺乏应对措施的情况下可能会造成伤害。

[社会偏见](https://platform.openai.com/docs/guides/embeddings/social-bias)

> **限制**: 该模型通过刻板印象或负面情绪对某些群体进行编码，从而编码了社会偏见。

通过运行 SEAT（[May et al，2019](https://arxiv.org/abs/1903.10561)）和 Winogender （[Rudinger et al，2018](https://arxiv.org/abs/1804.09301)）基准测试，我们发现我们的模型存在偏见。这些基准测试一起包括了 7 个测试，用于衡量模型在应用于性别化名称、地区名称和一些刻板印象时是否包含隐含偏见。

例如，我们发现相对于非洲裔美国人的名称，我们的模型更强烈地将欧洲裔美国人的名称与积极情绪相关联，同时将负面刻板印象与黑人妇女相关联。

这些基准测试在几个方面存在限制：(a) 它们可能无法推广到您特定的使用情况中，(b) 它们只测试可能的社会偏见中的一小部分。

**这些测试是初步的，我们建议针对您的特定用例运行测试。**这些结果应视为现象存在的证据，而不是针对您的用例的明确描述。有关更多详细信息和指导，请参阅我们的 [使用政策](https://openai.com/policies/usage-policies)。

如果您有任何问题，请通过[聊天联系我们的支持团队](https://help.openai.com/en/)，我们很乐意为您提供建议。

[对最近事件的盲目](https://platform.openai.com/docs/guides/embeddings/blindness-to-recent-events)

> **限制**: 模型缺乏对 2020 年 8 月之后发生的事件的了解。

我们的模型是在包含有关真实世界事件的一些信息的数据集上训练的，这些信息最近可以追溯到 8/2020。如果您依赖于模型对最近事件的表示，则它们可能表现不佳。

[常见问题](https://platform.openai.com/docs/guides/embeddings/frequently-asked-questions)

[在将其嵌入之前，我怎样才能知道一个字符串有多少个标记？](https://platform.openai.com/docs/guides/embeddings/how-can-i-tell-how-many-tokens-a-string-has-before-i-embed-it)

在 Python 中，可以使用 OpenAI 的分词器 [`tiktoken`](https://github.com/openai/tiktoken) 将字符串拆分为标记。

示例代码：



```python
import tiktoken

def num_tokens_from_string(string: str, encoding_name: str) -> int:
    """Returns the number of tokens in a text string."""
    encoding = tiktoken.get_encoding(encoding_name)
    num_tokens = len(encoding.encode(string))
    return num_tokens

num_tokens_from_string("tiktoken is great!", "cl100k_base")
```

对于第二代嵌入模型，例如 `text-embedding-ada-002`，请使用 `cl100k_base` 编码。

更多详细信息和示例代码请参见 OpenAI Cookbook 指南 [如何使用 tiktoken 计算标记数](https://github.com/openai/openai-cookbook/blob/main/examples/How_to_count_tokens_with_tiktoken.ipynb)。

[如何快速检索K个最近的嵌入向量？](https://platform.openai.com/docs/guides/embeddings/how-can-i-retrieve-k-nearest-embedding-vectors-quickly)

为了快速搜索许多向量，我们建议使用向量数据库。您可以在我们的 GitHub  [ 上的 Cookbook](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases) 中找到使用向量数据库和 OpenAI API 的示例。

向量数据库选项包括：

- [Pinecone](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/pinecone)，一个完全托管的向量数据库
- [Weaviate](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/weaviate)，一个开源向量搜索引擎
- [Redis](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/redis) 作为向量数据库
- [Qdrant](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/qdrant)，一个向量搜索引擎
- [Milvus](https://github.com/openai/openai-cookbook/blob/main/examples/vector_databases/Using_vector_databases_for_embeddings_search.ipynb)，为可扩展的相似度搜索而构建的向量数据库
- [Chroma](https://github.com/chroma-core/chroma)，一个开源嵌入式存储器
- [Typesense](https://typesense.org/docs/0.24.0/api/vector-search.html)，快速的开源向量搜索引擎
- [Zilliz](https://github.com/openai/openai-cookbook/tree/main/examples/vector_databases/zilliz)，由 Milvus 提供支持的数据基础架构

[我应该使用哪个距离函数？](https://platform.openai.com/docs/guides/embeddings/which-distance-function-should-i-use)

我们建议使用[余弦相似度](https://en.wikipedia.org/wiki/Cosine_similarity)。距离函数的选择通常并不重要。

OpenAI 的嵌入向量已归一化为长度为1，这意味着：

- 可以仅使用点积计算余弦相似度，从而稍微提高计算速度
- 余弦相似度和欧几里得距离将得到相同的排序

[我可以在线共享我的嵌入向量吗？](https://platform.openai.com/docs/guides/embeddings/can-i-share-my-embeddings-online)

客户拥有我们模型的输入和输出，包括嵌入向量。您负责确保您输入到我们 API 的内容不违反任何适用法律或我们的[使用条款](https://openai.com/policies/terms-of-use)。

