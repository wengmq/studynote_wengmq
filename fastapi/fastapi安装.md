# fastapi 安装

# FastAPI

[![FastAPI](https://fastapi.tiangolo.com/img/logo-margin/logo-teal.png)](https://fastapi.tiangolo.com/)

*FastAPI 框架，高性能，易于学习，高效编码，生产可用*

[![Test](https://github.com/tiangolo/fastapi/workflows/Test/badge.svg) ](https://github.com/tiangolo/fastapi/actions?query=workflow%3ATest)[![Coverage](https://img.shields.io/codecov/c/github/tiangolo/fastapi?color=%2334D058) ](https://codecov.io/gh/tiangolo/fastapi)[![Package version](https://img.shields.io/pypi/v/fastapi?color=%2334D058&label=pypi%20package)](https://pypi.org/project/fastapi)

------

**文档**： [https://fastapi.tiangolo.com](https://fastapi.tiangolo.com/)

**源码**： https://github.com/tiangolo/fastapi

------

FastAPI 是一个用于构建 API 的现代、快速（高性能）的 web 框架，使用 Python 3.6+ 并基于标准的 Python 类型提示。

关键特性:

- **快速**：可与 **NodeJS** 和 **Go** 比肩的极高性能（归功于 Starlette 和 Pydantic）。[最快的 Python web 框架之一](https://fastapi.tiangolo.com/zh/#_11)。
- **高效编码**：提高功能开发速度约 200％ 至 300％。*
- **更少 bug**：减少约 40％ 的人为（开发者）导致错误。*
- **智能**：极佳的编辑器支持。处处皆可自动补全，减少调试时间。
- **简单**：设计的易于使用和学习，阅读文档的时间更短。
- **简短**：使代码重复最小化。通过不同的参数声明实现丰富功能。bug 更少。
- **健壮**：生产可用级别的代码。还有自动生成的交互式文档。
- **标准化**：基于（并完全兼容）API 的相关开放标准：[OpenAPI](https://github.com/OAI/OpenAPI-Specification) (以前被称为 Swagger) 和 [JSON Schema](https://json-schema.org/)。





### 安装

```
pip install fastapi

pip install uvicorn[standard]
```



### 启动

文件：main.py

```
from typing import Optional

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Optional[str] = None):
    return {"item_id": item_id, "q": q}
```

启动命令：

```
uvicorn main_test:app --reload --port 8001
```



访问：

``` 
http://127.0.0.1:8001/items/5?q=somequery
```

响应：

```
{"item_id": 5, "q": "somequery"}
```



