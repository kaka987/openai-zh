[图像生成 Beta](https://platform.openai.com/docs/guides/images/image-generation-beta)

通过我们的 DALL·E 模型，学习如何生成或操纵图像。

[介绍](https://platform.openai.com/docs/guides/images/introduction)

图像 API 提供了三种与图像进行交互的方法：

1. 根据文本提示从零开始创建图像
2. 根据新的文本提示创建现有图像的编辑
3. 创建现有图像的变体

本指南介绍了如何使用这三个 API 端点的基础知识，提供了有用的代码示例。如果您想看到它们的实际应用，请查看我们的[DALL·E 预览应用程序](https://labs.openai.com/)。

图像 API 处于 beta 版本阶段。在此期间，API 和模型将根据您的反馈不断发展。为了确保所有用户能够舒适地使用原型，API 的默认速率限制为每分钟 50 张图像。您可以在我们的[速率限制指南](https://platform.openai.com/docs/guides/rate-limits)中了解更多速率限制方面的内容。

[用法](https://platform.openai.com/docs/guides/images/usage)

[生成](https://platform.openai.com/docs/guides/images/generations)

[image generations](https://platform.openai.com/docs/api-reference/images/create) 端点允许您根据文本提示创建原始图像。生成的图像可以具有 256x256、512x512 或 1024x1024 像素的大小。较小的尺寸生成速度更快。您可以使用 [n](https://platform.openai.com/docs/api-reference/images/create#images/create-n) 参数请求每次 1-10 张图像。

生成图像：

python



复制

```python
response = openai.Image.create(
  prompt="a white siamese cat",
  n=1,
  size="1024x1024"
)
image_url = response['data'][0]['url']
```

描述得越详细，您或您的最终用户希望得到的结果就越可能实现。您可以在 [DALL·E 预览应用程序](https://labs.openai.com/) 中探索示例，以获取更多提示灵感。这里是一个快速示例：

| 上下文                                                       |                                                         生成 |
| :----------------------------------------------------------- | -----------------------------------------------------------: |
| 一只白色的暹罗猫                                             | <img src="https://cdn.openai.com/API/images/guides/image_generation_simple.webp" alt="img" style="zoom: 25%;" /> |
| 一张特写的、在摄影棚中拍摄的白色暹罗猫的照片，看起来十分好奇，耳朵背后有背光效果 | <img src="https://cdn.openai.com/API/images/guides/image_generation_detailed.webp" alt="img" style="zoom: 25%;" /> |

每个图像都可以使用 [response_format](https://platform.openai.com/docs/api-reference/images/create#images/create-response_format) 参数作为 URL 或 Base64 数据返回。URL 将在一小时后到期。

[编辑](https://platform.openai.com/docs/guides/images/edits)

[image edits](https://platform.openai.com/docs/api-reference/images/create-edit) 端点允许您通过上传蒙版来编辑和扩展图像。蒙版的透明区域指示图像应该在哪里进行编辑，提示应该描述完整的新图像，**不仅仅是被抹掉的区域**。该端点可以实现类似于[我们的 DALL·E 预览应用程序中的编辑器](https://labs.openai.com/editor)的体验。

编辑图像

python



复制

```python
response = openai.Image.create_edit(
  image=open("sunlit_lounge.png", "rb"),
  mask=open("mask.png", "rb"),
  prompt="A sunlit indoor lounge area with a pool containing a flamingo",
  n=1,
  size="1024x1024"
)
image_url = response['data'][0]['url']
```

| 图像                                                         | 掩码                                                         | 输出                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| <img src="https://cdn.openai.com/API/images/guides/image_edit_original.webp" alt="img" style="zoom:25%;" /> | <img src="https://cdn.openai.com/API/images/guides/image_edit_mask.webp" alt="img" style="zoom:25%;" /> | <img src="https://cdn.openai.com/API/images/guides/image_edit_output.webp" alt="img" style="zoom:25%;" /> |

提示：一个阳光普照的室内休息区，里面有一个装有火烈鸟的游泳池

上传的图像和掩码必须都是小于4MB的正方形PNG图像，并且必须具有相同的尺寸。生成输出时，掩码的非透明区域不被使用，因此它们不必像上面的示例那样与原始图像匹配。

[Variations](https://platform.openai.com/docs/guides/images/variations)
[image variations](https://platform.openai.com/docs/api-reference/images/create-variation)终端点可以生成给定图像的变体。

生成图像变体

python



复制

```python
response = openai.Image.create_variation(
  image=open("corgi_and_cat_paw.png", "rb"),
  n=1,
  size="1024x1024"
)
image_url = response['data'][0]['url']
```

| 图像                                                         | 输出                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| <img src="https://cdn.openai.com/API/images/guides/image_variation_original.webp" alt="img" style="zoom:25%;" /> | <img src="https://cdn.openai.com/API/images/guides/image_variation_output.webp" alt="img" style="zoom:25%;" /> |

与编辑终端点类似，输入图像必须是小于4MB的正方形PNG图像。

[内容管理](https://platform.openai.com/docs/guides/images/content-moderation)

基于我们的[内容政策](https://labs.openai.com/policies/content-policy)，我们会对提示和图像进行过滤，如果提示或图像被标记，就会返回一个错误。如果您对误报等问题有任何反馈，请通过我们的[帮助中心](https://help.openai.com/)联系我们。

[语言特定提示](https://platform.openai.com/docs/guides/images/language-specific-tips)

NODE.JS

[使用内存中的图像数据](https://platform.openai.com/docs/guides/images/using-in-memory-image-data)

上面指南中的Node.js示例使用`fs`模块从磁盘读取图像数据。在某些情况下，您可能已经将图像数据存储在内存中。这里有一个使用存储在Node.js `Buffer`对象中的图像数据的API调用示例：

```javascript
// This is the Buffer object that contains your image data
const buffer = [your image data];
// Set a `name` that ends with .png so that the API knows it's a PNG image
buffer.name = "image.png";
const response = await openai.createImageVariation(
  buffer,
  1,
  "1024x1024"
);
```

[使用TypeScript](https://platform.openai.com/docs/guides/images/working-with-typescript)

如果您使用TypeScript，您可能会遇到一些与图像文件参数有关的怪异问题。下面是一个通过显式转换参数来解决类型不匹配的示例：

```javascript
// Cast the ReadStream to `any` to appease the TypeScript compiler
const response = await openai.createImageVariation(
  fs.createReadStream("image.png") as any,
  1,
  "1024x1024"
);
```

这里有一个使用内存中的图像数据的类似示例：

```javascript
// This is the Buffer object that contains your image data
const buffer: Buffer = [your image data];
// Cast the buffer to `any` so that we can set the `name` property
const file: any = buffer;
// Set a `name` that ends with .png so that the API knows it's a PNG image
file.name = "image.png";
const response = await openai.createImageVariation(
  file,
  1,
  "1024x1024"
);
```

[错误处理](https://platform.openai.com/docs/guides/images/error-handling)

由于无效输入、速率限制或其他问题，API请求可能会返回错误。这些错误可以用`try...catch`语句处理，错误细节可以在`error.response`或`error.message`中找到：

```javascript
try {
  const response = await openai.createImageVariation(
    fs.createReadStream("image.png"),
    1,
    "1024x1024"
  );
  console.log(response.data.data[0].url);
} catch (error) {
  if (error.response) {
    console.log(error.response.status);
    console.log(error.response.data);
  } else {
    console.log(error.message);
  }
}
```


PYTHON

[使用内存中的图像数据](https://platform.openai.com/docs/guides/images/using-in-memory-image-data)

上面的指南中的Python示例使用`open`函数从磁盘读取图像数据。在某些情况下，您可能已经将图像数据保存在内存中。以下是一个使用存储在`BytesIO`对象中的图像数据进行API调用的示例：

```python
from io import BytesIO

# This is the BytesIO object that contains your image data
byte_stream: BytesIO = [your image data]
byte_array = byte_stream.getvalue()
response = openai.Image.create_variation(
  image=byte_array,
  n=1,
  size="1024x1024"
)
```

[图像数据操作](https://platform.openai.com/docs/guides/images/operating-on-image-data)

在将图像传递给API之前执行操作可能会很有用。以下是一个使用`PIL`调整图像大小的示例：

```python
from io import BytesIO
from PIL import Image

# Read the image file from disk and resize it
image = Image.open("image.png")
width, height = 256, 256
image = image.resize((width, height))

# Convert the image to a BytesIO object
byte_stream = BytesIO()
image.save(byte_stream, format='PNG')
byte_array = byte_stream.getvalue()

response = openai.Image.create_variation(
  image=byte_array,
  n=1,
  size="1024x1024"
)
```

[错误处理](https://platform.openai.com/docs/guides/images/error-handling)

由于无效输入，速率限制或其他问题，API请求可能会返回错误。可以使用`try ... except`语句处理这些错误，错误详细信息可以在`e.error`中找到：



```python
try:
  openai.Image.create_variation(
    open("image.png", "rb"),
    n=1,
    size="1024x1024"
  )
  print(response['data'][0]['url'])
except openai.error.OpenAIError as e:
  print(e.http_status)
  print(e.error)
```

