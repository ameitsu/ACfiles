---
title: Notorial vol.1|Prompt toolkit库第一集：打印文本
date: 2023-09-08 22:17:01
tags:
    - Notorial
    - SofTY
categories: 
    - [SofTY, Notorial]
cover: https://i.imgs.ovh/i/2023/09/08/64fb370a2c72d.jpg
---

:::info
此为`Prompt toolkit`系列的页面。本系列其他文章如下：
    第一集：**打印文本**
    第二集：输入文本 *未更新*
    第三集：对话框 *未更新*
    第四集：进度条 *未更新*
    第五集：全屏程序制作 *未更新*
    第六集：拓展内容 *未更新*
    第七集：一些实例 *未更新*
    ova：有价值的内容与结束语 *未更新*
:::

:::default
在下文中，Prompt toolkit简称为PT。
:::
# 写在前面
Python 的第三方库 prompt_toolkit 用于打造交互式命令行。本系列主要参考它的[官方文档](https://python-prompt-toolkit.readthedocs.io/en/stable/index.html)。
# 下载PT
可以用`pip`或者`anaconda`。但是STY主要使用前者。
~~~python
pip install prompt_toolkit
~~~
# 打印普通文本
~~~python
from prompt_toolkit import print_formatted_text

print_formatted_text('Hello world')
~~~
![1.jpg](https://cdnjson.com/images/2023/09/08/1.jpg)
`print_formatted_text()`可以用来打印普通文本。如果想以假乱真，可以酱紫：
~~~python
from prompt_toolkit import print_formatted_text as print

print('Hello world')
~~~
# 打印格式化文本
打印格式化文本需要这三种方法之一：创建HTML对象、创建ANSI对象、创建`(样式,文本)`元组或使用`pygments`库。STY主要使用HTML，毕竟已经学过了。
~~~python
from prompt_toolkit import print_formatted_text, HTML

print_formatted_text(HTML('<b>This is bold</b>'))
print_formatted_text(HTML('<i>This is italic</i>'))
print_formatted_text(HTML('<u>This is underlined</u>'))
~~~
![2.jpg](https://cdnjson.com/images/2023/09/08/2.jpg)
:::warning
实际上，这里的`<i></i>`标签没有被渲染成斜体，现在这大概率是个未解之谜。
:::
使用HTML对象时，需要先import到程序中。这样一来，就可以直接使用HTML标签了。
~~~python
print_formatted_text(HTML('<skyblue>This is sky blue</skyblue>'))
print_formatted_text(HTML('<seagreen>This is sea green</seagreen>'))
print_formatted_text(HTML('<violet>This is violet</violet>'))
~~~
![3.jpg](https://cdnjson.com/images/2023/09/08/3.jpg)
也可以用颜色标签设置背景色。
为了更好编辑格式信息，可以将格式内容在一个字典中编写：
~~~python
from prompt_toolkit import print_formatted_text, HTML
from prompt_toolkit.styles import Style

style = Style.from_dict({
    'aaa': '#ff0066',
    'bbb': '#44ff00 italic',
})

print_formatted_text(HTML('<aaa>Hello</aaa> <bbb>world</bbb>!'), style=style)
~~~
![4.jpg](https://cdnjson.com/images/2023/09/08/4.jpg)
:::info
在上一个实例中，像`<aaa></aaa>`的标签是自定义的。在PT中，可以使用自定义的标签，前提是和样式字典匹配。
:::
~~~python
from prompt_toolkit import print_formatted_text
from prompt_toolkit.formatted_text import FormattedText

text = FormattedText([
    ('#ff0066', 'Hello'),
    ('', ' '),
    ('#44ff00 italic', 'World'),
])

print_formatted_text(text)
~~~

和HTML很像的方法是样式元组。此方法的大致内容与HTML对象类似，不再赘述。

一个有用的函数是`to_formatted_text()`。它可以使输入符合格式化文本的要求：
~~~python
from prompt_toolkit.formatted_text import to_formatted_text, HTML
from prompt_toolkit import print_formatted_text

html = HTML('<aaa>Hello</aaa> <bbb>world</bbb>!')
text = to_formatted_text(html, style='class:my_html bg:#00ff00 italic')

print_formatted_text(text)
~~~