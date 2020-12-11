# python数据可视化神器--pyecharts 快速入门

### 简介

**pyecharts** 以及基本使用方法：

- 简洁的 API 设计，使用如丝滑般流畅，支持链式调用
- 囊括了 30+ 种常见图表，应有尽有
- 支持主流 Notebook 环境，Jupyter Notebook 和 JupyterLab
- 可轻松集成至 Flask，Django 等主流 Web 框架
- 高度灵活的配置项，可轻松搭配出精美的图表
- 详细的文档和示例，帮助开发者更快的上手项目
- 多达 400+ 地图，为地理数据可视化提供强有力的支持

官网示例：https://pyecharts.org/#/zh-cn/intro



### Bar：柱状图/条形图

柱状图对应的模块是 Bar
除此之外可以设置全局配置和系列配置项。配置项都是基于 options

```
1# coding: utf-8
 2from example.commons import Faker
 3from pyecharts import options as opts
 4from pyecharts.charts import Bar
 5
 6def bar_base():
 7
 8    bar = Bar(init_opts=opts.InitOpts(page_title="bar页面"))  # 设置html页面标题
 9    # bar.add_xaxis(["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"])  # 设置x轴的参数
10
11    bar.add_xaxis(Faker.choose())
12    bar.add_yaxis("A", Faker.values())
13    bar.add_yaxis("B", Faker.values())
14
15    # 设置全局配置项，可选
16    bar.set_global_opts(opts.TitleOpts(title="主标题", subtitle="副标题"))
17    # render 会生成本地 HTML 文件，默认会在当前目录生成 render.html 文件
18    bar.render("bar.html")  # 也可以自己指定文件名
19
20if __name__ == "__main__":
21    bar_base()

```

