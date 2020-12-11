# ipywidgets

ipywidgets可以用于在jupyter notebook当中进行界面设计，以及一些简单的交互式控件操作。

官方文档有详细介绍，本文主要将常用的部件进行了演示，如需详细研究，请移步官方文档[ipywidgets](https://ipywidgets.readthedocs.io/en/stable/#)

## 一、安装

```python
pip install ipywidgets
1
```

## 二、基础方法

### 1、滑块interact

interact方法可以实现一些基础的交互式控件,可以自动生成函数参数的UI控件。

```python
from ipywidgets import interact,fixed

# 定义一个可供操作的函数
def foo(x):
    return x
12345
```

> 当传递一个参数x=10,会生成一个滑块并且绑定到函数参数

```python
interact(foo,x=10)
1
```

> 传递布尔值，生成复选框

```python
interact(foo,x=True)
1
```

> 传递字符串，生成文本框

```python
interact(foo,x="hello world")
1
```

> 传递列表，生成下拉框

```python
interact(foo,x=['a','b','c','d'])
1
```

> 传递字典，生成下拉框，键值对应

```python
interact(foo,x={"a":1,"b":2,"c":3})
1
```

> 传递元组，（min,max,step）最小值，最大值，步长

```python
interact(foo,x=(1,9,1))
1
```

补充：还可以使用浮点数，生成浮点数滑块。

以上方法都是将一个参数生成为特定值，当需要多个参数时就需要用到**`fixed`**参数。

```python
def func(p,q):
    return (p,q)

interact(func,p=5,q=fixed(20))  # 设定20为固定值
1234
```

### 2、按钮

```python
import ipywidgets as widgets
from ipywidgets import Layout
12
```

#### 1、button（普通按钮）

```python
widgets.Button(
    description='点我啊！！！',  # 按钮提示
    disabled=False,
    button_style='info', # 'success', 'info', 'warning', 'danger' or '' 按钮样式
    tooltip='Click me',
    layout = Layout(width="98%",height="50px"),  # 按钮大小调整
)
1234567
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224205831761.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

#### 2、ToggleButton（布尔值按钮）

用于显示布尔值

```python
widgets.ToggleButton(
    value=False,
    description='点我！！',
    disabled=False,
    button_style='warning', # 'success', 'info', 'warning', 'danger' or ''
    tooltip='Description',
    icon='check',
    layout = Layout(width="60%",height='30px')
)
123456789
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224205907846.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

#### 3、RadioButtons（单选按钮）

```python
widgets.RadioButtons(
    options=['numpy', 'pandas', 'matplotlib'],
#     value='pineapple',
    description='',
    disabled=False
)
123456
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224205926217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

#### 4、ToggleButtons

```python
widgets.ToggleButtons(
    options=['numpy', 'pandas', 'matplotlib'],
    description='Speed:',
    disabled=False,
    button_style='success', # 'success', 'info', 'warning', 'danger' or ''
#     tooltips=['Description of slow', 'Description of regular', 'Description of fast'],
#     icons=['check'] * 3
)
12345678
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224205940362.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

### 3、选择小部件

1、Dropdown（下拉框）

```python
widgets.Dropdown(
    options=['1', '2', '3'],
    value='2',
    description='Number:',
    disabled=False,
)
123456
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224205951772.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

2、Select（单选框）

```python
widgets.Select(
    options=['Linux', 'Windows', 'OSX'],
    value='OSX',
    # rows=10,
    description='OS:',
    disabled=False
)
1234567
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210004682.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

3、SelectionSlider（滑动部件）

```python
widgets.SelectionSlider(
    options=['low level', 'ordinary', 'well', 'excellent'],
    value='ordinary',
    description='我的Python等级',
    disabled=False,
    continuous_update=False,
    orientation='horizontal',
    readout=True
)
123456789
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020022421001758.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

4、SelectMultiple（复选框）

```python
widgets.SelectMultiple(
    options=['Apples', 'Oranges', 'Pears'],
    value=['Oranges'],
    #rows=10,
    description='Fruits',
    disabled=False
)
1234567
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210029274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

### 3、进度条

#### 1、IntProgress (整数型进度条)

```python
widgets.IntProgress(
    value=5,  # 进度条数值
    min=0,
    max=10,
    step=1,
    description='Loading:',
    bar_style='danger', # 'success', 'info', 'warning', 'danger' or ''
    orientation='horizontal'
)
123456789
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210040242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

#### 2、FloatProgress (浮点型进度条)

```python
widgets.FloatProgress(
    value=5.5,
    min=0,
    max=10.0,
    step=0.1,
    description='Loading:',
    bar_style='warning',
    orientation='horizontal'
)
123456789
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210051714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

### 4、文本

#### 1、Text(固定大小)

```python
widgets.Text(
    value='Hello World',
    placeholder='Type something',
    description='String:',
    disabled=False
)
123456
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210106176.png)

#### 2、Textarea(可拉伸)

```python
widgets.Textarea(
    value='Hello World',
    placeholder='Type something',
    description='String:',
    disabled=False
)
123456
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210116231.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

#### 3、BoundedIntText、BoundedFloatText（限值文本）

```python
# 整数
widgets.BoundedIntText(
    value=7,
    min=0,
    max=10,
    step=1,
    description='Text:',
    disabled=False
)
# 浮点数
widgets.BoundedFloatText(
    value=7.5,
    min=0,
    max=10.0,
    step=0.1,
    description='Text:',
    disabled=False
)
123456789101112131415161718
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210131334.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

### 5、图片

```python
file = open("小宝贝.jpg", "rb")
image = file.read()
widgets.Image(
    value=image,
    format='jpg',
    width=300,
    height=400,
)
12345678
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210143623.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

### 6、日期选择器

```python
widgets.DatePicker(
    description='Pick a Date',
    disabled=False
)
1234
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210156112.png)

### 7、容器/布局小部件

#### 1、Box

用于显示组件当中的多个小部件

```python
words = ['correct', 'horse', 'battery', 'staple']
items = [Button(description=w) for w in words]
Box([items[0], items[1], items[2], items[3]])
123
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210211337.png)

#### 2、HBox

水平显示组件中多个小部件

```python
words = ['correct', 'horse', 'battery', 'staple']
items = [Button(description=w) for w in words]
HBox([items[0], items[1], items[2], items[3]])
123
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210224296.png)

#### 3、VBox

垂直显示组件中的多个小部件

```python
words = ['correct', 'horse', 'battery', 'staple']
items = [Button(description=w) for w in words]
VBox([items[0], items[1], items[2], items[3]])
123
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210239666.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

#### 4、折叠数据

```python
accordion = widgets.Accordion(children=[widgets.IntSlider(), widgets.Text()])
accordion.set_title(0, '滑块')
accordion.set_title(1, '文本')
accordion
1234
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210250938.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

#### 5、标签

```python
tab_contents = ['基本', '股池', '买策', '卖策', '选股']
children = [widgets.Text(description=name) for name in tab_contents]
tab = widgets.Tab()
tab.children = children
for index,i in enumerate(tab_contents):
    tab.set_title(index,i)  # 需要索引与值两个参数
tab
1234567
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210303721.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

#### 6、折叠数据与标签嵌套

```python
tab_nest = widgets.Tab()
tab_nest.children = [accordion, accordion]
tab_nest.set_title(0, '第一块')
tab_nest.set_title(1, '第二块')
tab_nest
12345
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210318204.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

### 8、播放动画小部件

```python
# 播放动画结合进度条
play = widgets.Play(
#     interval=10,
    value=0,
    min=0,
    max=100,
    step=1,
    description="Press play",
    disabled=False
)

slider = widgets.IntProgress(
    value=100,# 进度条数值
    min=0,
    max=100,
    step=1,
    description='Loading:',
    bar_style='danger', # 'success', 'info', 'warning', 'danger' or ''
    orientation='horizontal'
)

widgets.jslink((play, 'value'), (slider, 'value'))
widgets.HBox([play, slider])
1234567891011121314151617181920212223
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210330359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

## 三、常用事件

### 1、on_click()

```python
# 查看某个部件的文档
print(widgets.Button.on_click.__doc__)
12
```

按钮点击是无状态发生的，因此想要将事件由前端传递到后端，就需要通过`on_click`方法，在点击时执行相应的函数：

```python
button = widgets.Button(description="点我啊！！")
display(button)

# 点击执行事件，点击一次，执行一次
def on_button_clicked(b):
    print("Button clicked.")

button.on_click(on_button_clicked)
12345678
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210344821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)

### 2、Traitlet事件

#### observe

observe方法可以用于注册回调

```python
int_range = widgets.IntSlider()
display(int_range)

# 每一次事件都会回调当前函数，然后打印`change['new']`中对应的值
def on_value_change(change):
    print(change['new'])

int_range.observe(on_value_change, names='value')
12345678
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200224210405164.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjAzMjM1MQ==,size_16,color_FFFFFF,t_70)
button_clicked(b):
print(“Button clicked.”)

button.on_click(on_button_clicked)

```
[外链图片转存中...(img-t6WOeJRd-1578992575048)]

### 2、Traitlet事件

#### observe

observe方法可以用于注册回调

​```python
int_range = widgets.IntSlider()
display(int_range)

# 每一次事件都会回调当前函数，然后打印`change['new']`中对应的值
def on_value_change(change):
    print(change['new'])

int_range.observe(on_value_change, names='value')
```