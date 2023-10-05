---
title: Notorial vol.2|Prompt toolkit库第二集：获取输入-上
date: 2023-09-15 22:17:01
tags:
    - Notorial
    - SofTY
categories: 
    - [SofTY, Notorial]
cover: https://cdnjson.com/images/2023/09/28/-2023-09-28-224231.jpg
---
:::info
此为`Prompt toolkit`系列的页面。本系列其他文章如下：
    第一集：[打印文本](https://ameitsu.github.io/SofTY/Notorial/SofTY/Notorial/prompt-toolkit-print/)
    第二集： **输入文本**
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
# 前期提要
在【[Notorial vol.1|Prompt toolkit库第一集：打印文本](https://ameitsu.github.io/SofTY/Notorial/SofTY/Notorial/prompt-toolkit-print/)】中，我们记录了打印格式化文本的方法。但命令行程序需要用户输入指令，所以我们需要知道如何获取用户输入的信息。

:::info
PT库中关于获取文本的功能较多，这里仅是一些。完整版见[官方页面](https://python-prompt-toolkit.readthedocs.io/en/stable/pages/asking_for_input.html)。
:::
# 获取输入
~~~python
from prompt_toolkit import prompt

text = prompt('Give me some input: ')
print('You said: %s' % text)
~~~
用`prompt()`获取输入，类似于`input()`。

# 提示文本上色
~~~python
from prompt_toolkit.shortcuts import prompt
from prompt_toolkit.styles import Style

style = Style.from_dict({
    # User input (default text).
    '':          '#ff0066',

    # Prompt.
    'username': '#884444',
    'at':       '#00aa00',
    'colon':    '#0000aa',
    'pound':    '#00aa00',
    'host':     '#00ffff bg:#444400',
    'path':     'ansicyan underline',
})

message = [
    ('class:username', 'john'),
    ('class:at',       '@'),
    ('class:host',     'localhost'),
    ('class:colon',    ':'),
    ('class:path',     '/user/john'),
    ('class:pound',    '# '),
]

text = prompt(message, style=style)
~~~
类似打印文本时的相关操作，在此不再赘述。
# 补全
## 自动补全
~~~python
from prompt_toolkit import prompt
from prompt_toolkit.completion import WordCompleter

html_completer = WordCompleter(['<html>', '<body>', '<head>', '<title>'])
text = prompt('Enter HTML: ', completer=html_completer)
print('You said: %s' % text)
~~~
创建自动补全的方法是在`WordCompleter([])`的列表中将所有可能涉及的指令全部添加。

## 嵌套补全
嵌套补全是一种依据前文来提示关键词的自动补全方式。选择地址时的城市-区域-社区选项就是一种嵌套补全。
~~~python
from prompt_toolkit import prompt
from prompt_toolkit.completion import NestedCompleter

completer = NestedCompleter.from_nested_dict({
    'show': {
        'version': None,
        'clock': None,
        'ip': {
            'interface': {'brief'}
        }
    },
    'exit': None,
})

text = prompt('# ', completer=completer)
print('You said: %s' % text)
~~~
当输入`show`时，`version` `clock` `ip`这三个选项就会弹出。特别的，如果输入`show`+`ip`时又有`interface`显示。

## 输入时补全
~~~python
text = prompt('Enter HTML: ', completer=my_completer,
              complete_while_typing=True)
~~~
这样可以在按下`tab`键或正常输入时弹出补全。

## 异步补全
~~~python
text = prompt('> ', completer=MyCustomCompleter(), complete_in_thread=True)
~~~



