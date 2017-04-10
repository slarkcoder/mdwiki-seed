如果你也使用 Python，那么你是否遇到过在自己机器上运行良好，在别人的机器上却遇到缺少很多依赖的错误？最笨的方法是在缺少依赖的机器上逐个安装依赖，但是这样做不仅效率低，而且容易出错。其实我们可以使用 pip 来完整的导出项目依赖。

在项目的根目录，导出依赖：

```
pip freeze > requirements.txt
```

`requirements.txt` 这个文件会记录当前程序所有的依赖以及对应的版本。


安装依赖：

```
pip install -r requirements.txt
```

这个命令会安装 `requirements.txt` 文件中的所有依赖。安装完毕之后就可以正常运行项目了。




