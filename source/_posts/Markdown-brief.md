---
title: Markdown 简述
date: 2024-05-20 08:53:30
tags: 语法
---
# 目录
[前言](#1)
[Markdown 基本语法](#2)
[Markdown 扩展语法](#3)

<h1 id="1">前言</h1>

本篇教程旨在方便自己使用 **Markdown** 语法。<br>
本篇参考 [Markdown 官网](https://markdown.com.cn)上的资料编写而成。感兴趣的亦可直接移步官网查阅。<br>
对于编写Markdown文本时使用的工具有很多可选，官网上也推荐了一些比较好用的工具。笔者自己则是使用的 **VSCode** 编辑器+ **Markdown Preview Enhanced** 扩展。对于该搭配的选择，是因为如下几个原因：<br>
- VSCode 既可以作为一个代码编辑器，也可以作为一个文本编辑器<br>
- MPE 扩展可以帮助我们实时预览 Markdown 文本的效果，同时在预览页面上支持右键菜单，提供了很多功能，比如导出 PDF 等<br>

<h1 id="2">Markdown 基本语法</h1>

### 1. 标题语法
要创建标题，请在标题前添加井号(`#`)。`#` 的数量代表了标题的级别。<br>
需要注意的是，标准的标题语法要求在 `#` 和标题文字之间使用一个空格进行分隔<br>

|Markdown语法|HTML|预览效果|
|:---:|:---:|:---|
|`# Heading Level 1`|`<h1>Heading level 1</h1>`|<h1>Heading level 1</h1>|
|`## Heading Level 2`|`<h2>Heading level 2</h2>`|<h2>Heading level 1</h2>|
|`### Heading Level 3`|`<h3>Heading level 3</h3>`|<h3>Heading level 1</h3>|
|`#### Heading Level 4`|`<h4>Heading level 4</h4>`|<h4>Heading level 1</h4>|
|`##### Heading Level 5`|`<h5>Heading level 5</h5>`|<h5>Heading level 1</h5>|
|`###### Heading Level 6`|`<h6>Heading level 6</h6>`|<h6>Heading level 1</h6>|

标题语法还支持其他可选语法，但笔者就不在此处赘述了，感兴趣的可以通过上面 Markdown 官网的超链接查看<br>
### 2. 段落语法
要创建段落，请使用空白行将一行或多行文本进行分隔。<br>

|Markdown语法|HTML|预览效果|
|---|---|---|
|`I really like using Markdown.`<br><br>`I think I'll use it to format all of my documents from now on.`|<p>`<p>I really like using Markdown.</p >`</p ><p>`<p>I think I'll use it to format all of my documents from now on.</p >`<p>|<p>I really like using Markdown.</p ><p>I think I'll use it to format all of my documents from now on.</p >|

需要注意的是，不要在 Markdown 中使用空格或制表符缩紧段落，因为这会变成代码语法(在后面会讲到)。<br>
### 3. 换行语法
在 Markdown 中，换行需要在一行的末尾添加两个空格，再回车键，即可创建一个换行(`<br>`)<br>

|Markdown语法|HTML|预览效果|
|---|---|---|
|`This is the first line.`<br>`And this is the second line.`|`This is the first line.<br>`<br>`And this is the second line.`|This is the first line.<br>And this is the second line.|

换行和段落的区别在于不同段落之间的行间距比较大。<br>
几乎每个 Markdown 程序都会支持使用两个或多个空格进行换行，但是空格是很难在编辑器中看见的，这就会有争议，因此笔者更推荐使用 `<br>` 来表示换行。(其中的实现机理涉及后面会讲到的内嵌HTML标签)<br>
### 4. 强调语法
我们习惯通过将文本设置为粗体或斜体来强调其需要得到注意。<br>
#### 粗体
我们在需要加粗的文本前后各添加两个星号。<br>

|Markdown语法|HTML|预览效果|
|---|---|---|
|`I just love **bold text**.`|`I just love <strong>bold text</strong>.`|I just love **bold text**.|

#### 斜体
我们在需要用斜体显示的文本前后各添加一个星号。<br>

|Markdown语法|HTML|预览效果|
|---|---|---|
|`Italicized text is the *cat's meow*.`|`Italicized text is the <em>cat's meow</em>.`|Italicized text is the *cat's meow*.|

#### 粗体和斜体
我们在需要同时使用粗体和斜体突出显示的文本前后各添加三个星号。<br>

|Markdown语法|HTML|预览效果|
|---|---|---|
|`This text is ***really important***.`|`This text is <strong><em>really important</em></strong>.`|This text is ***really important***.|

### 5. 引用语法
引用语法用于创建一个特色样式，表示这一块内容是引用他处的。<br>
要创建块引用，请在段落前添加一个 `>` 符号。<br>

    > Dorothy followed her through many of the beautiful rooms in her castle.

渲染效果如下所示：<br>

> Dorothy followed her through many of the beautiful rooms in her castle.

#### 多个段落的块引用
我们只需要为段落之间的空白行也添加一个 `>` 符号即可。<br>

    > Dorothy followed her through many of the beautiful rooms in her castle.  
    >  
    > The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.

渲染效果如下：<br>

> Dorothy followed her through many of the beautiful rooms in her castle.  
>  
> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.

#### 嵌套块引用
块引用还可以嵌套。再要嵌套的段落前添加一个 `>>` 符号。

    > Dorothy followed her through many of the beautiful rooms in her castle.
    >  
    >> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.

渲染效果如下：<br>

> Dorothy followed her through many of the beautiful rooms in her castle.
>  
>> The Witch bade her clean the pots and kettles and sweep the floor and keep the fire fed with wood.

#### 带有其他元素的块引用
块引用可以包含其他 Markdown 格式的元素。但并非所有元素都可以使用，你需要进行实验以查看哪些元素有效。<br>

    > #### The quarterly results look great!
        >
    > - Revenue was off the chart.
    > - Profits were higher than ever.
    >
    >  *Everything* is going according to **plan**.

渲染效果如下：<br>

> #### The quarterly results look great!
>
> - Revenue was off the chart.
> - Profits were higher than ever.
>
>  *Everything* is going according to **plan**.

### 6. 列表语法
我们可以将多个条目组织成有序或无序列表。
#### 有序列表
要创建有序列表，只需要在每个列表项前添加数字并紧跟一个英文句号。<br>
需要注意的是，数字和英文句号与列表项之间需要使用空格分隔。<br>
列表中的项的数字不需要有序，但列表会以第一项的数字作为起始序号为整个列表编码序号。<br>

|Markdown语法|HTML|预览效果|
|---|---|---|
|`1. First item`<br>`2. Second item`<br>`3. Third item`<br>`4. Fourth item`|   `<ol>`<br>`<li>First item</li>`<br>`<li>Second item</li>`<br>`<li>Third item</li>`<br>`<li>Fourth item</li>`<br>`</ol>`|<ol><li>First item</li><li>Second item</li><li>Third item</li><li>Fourth item</li></ol>|
|`2. First item`<br>`5. Second item`<br>`9. Third item`<br>`3. Fourth item`|   `<ol>`<br>`<li>First item</li>`<br>`<li>Second item</li>`<br>`<li>Third item</li>`<br>`<li>Fourth item</li>`<br>`</ol>`|<ol start="2"><li>First item</li><li>Second item</li><li>Third item</li><li>Fourth item</li></ol>|

#### 无序列表
要创建无序列表，只需要在每个列表项前添加破折号( `-` )即可。<br>
同时你可以通过使用缩进来实现列表项的嵌套<br>

|Markdown语法|HTML|预览效果|
|---|---|---|
|`- First item`<br>`- Second item`<br>` - Third item`<br>`- Fourth item`|`<ul>`<br>`<li>First item</li>`<br>`<li>Second item</li>`<br>`<ul>`<br>`<li>Third item</li>`<br>`</ul>`<br>`<li>Fourth item</li>`<br>`</ul>`|<ul><li>First item</li><li>Second item</li><ul><li>Third item</li></ul><li>Fourth item</li></ul>|

至于 Markdown 语法中有序列表是否可以做嵌套读者可以自己实践一下。<br>
#### 在列表中嵌套其他元素
要在保留列表连续性的同时在列表中添加另一种元素，可以将该元素缩进四个空格或一个制表符。<br>
简单的，我们可以在列表中嵌套段落、引用块、代码块、图片等，也可以在有序列表中嵌套无序列表。<br>

1. Open the file.
2. Find the following code block on line 21:

        <html>
         <head>
          <title>Text</title>
         </head>
        </html>

3. Update the title to match the name of your website.

### 7. 代码语法
要将单词或短语表示为代码，你可以将它们包裹在反引号( `` ` ``)中。<br>

|Markdown语法|HTML|预览效果|
|---|---|---|
|`` At he command prompt, type `nano`. ``|`At he command prompt, type<code>nano</code>.`|At he command prompt, type `nano`.|

#### 转义反引号
如果你要表示为代码的单词或短语中包含反引号，则可以通过将单词或短语包裹中双反引号(<code>``</code>)中。<br>
注意，请不要将反引号和双反引号拼接在一起，那样子 Markdown 会识别不出来的！<br>

<table>
 <tr>
  <th>Markdown语法</th>
  <th>HTML</th>
  <th>预览效果</th>
 </tr>
 <tr>
  <td><code>``Use `code` in your Markdown file.``</code></td>
  <td><code>&lt;code&gt;Use `code` in your Markdown file.&lt;/code&gt;</code></td>
  <td><code>Use `code` in your Markdown file.</code></td>
 </tr>
</table>

#### 代码块
要创建代码块，只需要在代码块的每一行缩进四个空格或一个制表符。<br>

        <html>
         <head>
         </head>
        </html> 

渲染效果如下：<br>

    <html>
     <head>
     </head>
    </html> 

在 Markdown 的扩展语法中，还提供了一种不用缩进就可以创建代码块的方式。<br>
### 8. 分隔线语法
要创建分隔线，只需要在单独的一行上使用三个或多个星号( `***` )、破折号( `---` )、下划线( `___` )，注意三种不能混用。<br>

    ***
    ---
    ——————

以上三种分隔线的渲染效果是一致的：<br>

-------------

为了兼容性，最好在分隔线前后均添加空白行。<br>
### 9. 链接语法
Markdown 的链接语法如下：<br>

    [超链接显示名](超链接地址 “超链接 title ”)

其中超链接 title 是可选的，添加 title 的效果为鼠标悬浮在链接上时会文字。注意超链接 title 和地址之间要用空格分隔。<br>

    这是一个链接[Markdown语法](https://markdown.com.cn "官方教程")。

渲染效果如下：<br>
这是一个链接 [Markdown语法](https://markdown.com.cn "官方教程")。<br>
需要注意的是，如果你的地址中存在空格，那最好使用%20替换，否则可能存在兼容性问题。<br>
#### 网址和 Email 语法
如果你不需要为你的超链接地址设置额外的显示文本，你可以直接用尖括号建起包裹变成超链接。<br>
    
    <https://markdown.com.cn>
    <fake@example.com>

渲染效果如下：<br>
<https://markdown.com.cn>
<fake@example.com>
#### 带格式的链接
你可以使用一些格式强调你的链接，比如粗体和斜体。<br>

    I love supporting the **[EFF](https://eff.org)**.
    I love supporting the *[EFF](https://eff.org)*.
    
I love supporting the **[EFF](https://eff.org)**.
I love supporting the *[EFF](https://eff.org)*.
事实上用粗体强调链接并不算太好的注意，对吗！<br>
除此之外，你也可以用代码格式强调链接。其语法与上述不同，它需要你在中括号中用代码语法包裹链接显示名。<br>

    I love supporting the [`EFF`](https://eff.org)

I love supporting the [`EFF`](https://eff.org)
#### 引用类型链接
这种链接格式多用于文献写中，由于不常用，故不在此记录。需要时查阅官网教程即可。<br>
### 10. 图片语法
图片语法与链接语法极为相似，仅仅只是在链接语法前增加一个感叹号( `!` )。<br>
<pre><code>![图片名](图片地址 "图片 title ")</code></pre><br>

![](https://github.com/illusorycat/MyPictureBase/blob/main/image/202403142336781.png?raw=true "这是 title")<br>
当鼠标悬浮在图片上时，则会显示图片 title。<br>
#### 带链接的图片
给图片增加链接，仅需要将图片的 Markdown 放在链接的中括号中即可。<br>
<pre><code>[![沙漠中的岩石图片](https://github.com/illusorycat/MyPictureBase/blob/main/image/202403191909796.png?raw=true "Shiprock")](https://markdown.com.cn)</code></pre><br>
[![沙漠中的岩石图片](https://github.com/illusorycat/MyPictureBase/blob/main/image/202403191909796.png?raw=true "Shiprock")](https://markdown.com.cn)<br>

### 11. 转义字符语法
转义字符语法用于显示原本用于格式化 Markdown文档的字符，只需要在字符签名添加反斜杠字符即可。

    \*Without the backslash, this would be a bullet in an unordered list.

渲染效果如下：<br>
\*Without the backslash, this would be a bullet in an unordered list.
#### 可转移的字符
以下列出的字符都可以通过增加反斜杠字符来实现转义。<br>

<table>
 <tr><th>Character</tr>
 <tr><td>\</tr>
 <tr><td>`</tr>
 <tr><td>*</tr>
 <tr><td>_</tr>
 <tr><td>{}</tr>
 <tr><td>[]</tr>
 <tr><td>()</tr>
 <tr><td>#</tr>
 <tr><td>+</tr>
 <tr><td>-</tr>
 <tr><td>.</tr>
 <tr><td>!</tr>
 <tr><td>|</tr>
</table>

### 12. 内嵌 HTML 标签
Markdown 支持内嵌 HTML 标签。
对于 HTML 的行级内联标签如`<span>`、`<cite>`、`<del>`不受限制，可以中 Markdown 的段落、列表或者标题中任意使用。<br>
但是对于区块标签如`<div>`、`<table>`、`<pre>`、`<p>`等标签，必须在前后加上空行，以便与内容区分。
同时， Markdown 语法无法在 HTML 区块中进行处理。也就是说，你无法在 HTML 区块中使用 Markdown 语法。<br>

<h1 id="3">Markdown 扩展语法</h1>

适当使用扩展语法可以更好的充实你的文档。但是，请注意，并不是所有扩展语法都被支持，也并不是所有的处理器都支持你使用的扩展语法。<br>
### 1. 表格
我们使用管道( `|` )分隔每列，使用至少三个连字符( `---`) 创建分隔标题和内容。<br>

    | Syntax     | Description|
    | -----------| -----------|
    | Header     | Title      |
    | Paragraph  | Text       |

呈现的输出如下:

| Syntax     | Description|
| -----------| -----------|
| Header     | Title      |
| Paragraph  | Text       |

**Tip:** 很显然，手动敲这一大串语法很麻烦，我们可以使用 [Markdown Tables Generator](https://www.tablesgenerator.com/markdown_tables) 。只需要使用图形界面构建表，然后将生成的 Markdown 格式的文本复制到你想要的地方即可。
#### 对齐
我们可以中标题行中使用冒号( `:`) 来设置该列的文本对齐方式。

    | Syntax    | Description |   Test Text |
    | :---      | :---:       |        ---: |
    | Header    |    TItle    | Here's this |
    | Paragraph |     Text    |    And more |

呈现的输出如下所示：<br>

| Syntax    | Description |   Test Text |
| :---      | :---:       |        ---: |
| Header    |    TItle    | Here's this |
| Paragraph |     Text    |    And more |

#### 格式化表格中的文字
我们可以在表格中添加链接，代码(不支持代码块)和强调。<br>
#### 在表中显示管道字符
我们可以使用表格的 HTML 字符代码( `&#124;` ) 在表中显示管道( `|` )字符。<br>

    | Character |
    | ---       |
    | &#124;    |

呈现的输出如下: <br>

| Character |
| ---       |
| &#124;    |

### 2. 围栏代码块
有时使用缩进来创建代码块很不方便。这时我们可以尝试使用围栏代码块语法。
在代码块前后的行使用三个反引号( <code>```</code> ) 或三个波浪号( <code>~~~</code> )。

````
```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```
````

呈现的输出如下:<br>

```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

和在代码行中显示反引号一样，在代码块中显示反引号同样不能仅靠转义符( `\` )。<br>
在代码块中显示反引号，你需要将代码块包裹在四个反引号之间。<br>
#### 语法高亮
许多 Markdown 处理器都支持维码代码块的语法突出显示，只需要在代码块的反引号旁边指定一种语言。<br>

````
```json
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```
````

呈现效果如下:<br>

```json
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
```

### 3. 标题编号
许多 Markdown 处理器支持标题的自定义 ID 。<br>

    ### My Great Heading {#custom-id}

HTML 语法如下:<br>

```HTML
    <h3 id="custom-id">My Great Heading</h3>
```

#### 链接到标题 ID
我们可以创建链接语法链接到文件中具有自定义 ID 的标题。<br>

    [Heading IDs](#heading-ids)

HTML 语法如下:<br>

```HTML
    <a href="custom-id">My Great Heading</a>
```

其他网站可以通过将自定义标题 ID 添加到我特的完整 URL (例如 `[Heading IDs](url#custom-id)` ) 来链接到标题。<br>
### 4. 定义列表
在基本语法中我们介绍了列表，包括有序列表和无序列表。现在我们介绍另外一种列表，定义列表。<br>
创建定义列表的语法是，在第一行键入术语。在下一行，键入一个冒号，后跟一个空格和定义。<br>

    First Term
    : This is the definition of the first term.

    Second Term
    : This is one definition of the second term.
    : This is another definition of the second term.

HTML 看起来像这样:<br>

    <dl>
     <dt>First Term</dt>
     <dd>This is the definition of the first term.</dd>
     <dt>Second Term</dt>
     <dd>This is one definition of the second term.</dd>
     <dd>This is another definition of the second term.</dd>

呈现的输出如下所示：<br>

First Term
: This is the definition of the first term.

Second Term
: This is one definition of the second term.
: This is another definition of the second term.

需要注意的是， Markdown 语法中，前一个定义术语的内容与后一个定义术语直接需要用一个空行隔开。<br>
### 5. 删除线
我们可以在单词前后使用两个波浪号( `~~` ) 来创建删除线的效果。<br>

    ~~世界是平坦的。~~ 我们现在知道世界是圆的。

~~世界是平坦的。~~ 我们现在知道世界是圆的。<br>
### 6. 任务列表语法
这次我们介绍另一种新的列表语法，任务列表。<br>
要创建任务列表，我们需要在任务列表项前添加破折号( `-` )和方括号( `[]` )。破折号、方括号和列表项之间都要使用空格隔开。<br>
如果你想勾选任务，则在方括号之间添加 `x` 。<br>

    - [x] Write the press release
    - [ ] Update the website
    - [ ] Contact the media

呈现的输出如下: <br>

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

### 7. 使用 Emoji 表情
我们可以简单地将表情符号复制并粘贴到 Markdown 格式的文本中，或者键入 emoji shortcodes 。<br>
#### 复制和粘贴表情符号
我们可以简单地从 [Emojipedia](https://emojipedia.org/) 等来源复制表情符号。<br>
**Tip:** 如果你使用的是静态网站生成器，请确保将 HTML 页面编码为 UTF-8 。<br>
#### 使用表情符号简码
一些 Markdown 应用程序允许通过键入表情符号简码来插入表情符号。这同样意味着表情符号简码因应用程序而异。<br>
表情符号简码以冒号开头和结尾。<br>

    去露营了！ :tent: 很快回来。
    真好笑！ :joy:

去露营了！ :tent: 很快回来。<br>
真好笑！ :joy: <br>
**Tip:** 你可以使用此[表情符号简码列表](https://gist.github.com/rxaviers/7360908)。