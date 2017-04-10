对于程序员来说，使用命令行几乎是家常便饭了。因为命令行下面的操作都比较便捷，不需要使用鼠标，手指可以不离开键盘，简单的一行命令能调用脚本执行很复杂的功能。而几乎每个命令行工具都提供了命令行的参数选项，方便使用者的学习、使用。

![git](https://oj44a8qm0.qnssl.com/git.png)

比如在命令行下输入 `git --help` 就能看到 Git 支持的命令行参数，每个对应的参数还有一些说明，能让没有使用过 Git 的人也能很快明白命令如何使用。

昨天我做的这个系统虽然用户只有自己，但是也加上了命令行参数的解析功能，让整个程序的逻辑看起来更清晰，使用起来也更方便。命令行参数解析是用 argparse 这个模块实现的，比如下面的例子就是 argparse 的简单使用。

```
import argparse
parser = argparse.ArgumentParser(description='Search some files')

parser.add_argument(dest='filenames',metavar='filename', nargs='*')

parser.add_argument('-p', '--pat',metavar='pattern', required=True,
                    dest='patterns', action='append',
                    help='text pattern to search for')

parser.add_argument('-v', dest='verbose', action='store_true',
                    help='verbose mode')

parser.add_argument('-o', dest='outfile', action='store',
                    help='output file')

parser.add_argument('--speed', dest='speed', action='store',
                    choices={'slow','fast'}, default='slow',
                    help='search speed')

args = parser.parse_args()
```

## Argparse 使用要点

- argparse 主要包含 `定位参数` 和 `可选参数`，通过这两种参数的组合，可以配置出你想要的命令行参数；
- 首先需要 `import argparse`，然后初始化，接着添加 `add_argument` 参数，并设置相应的选项；
- 定位参数在每次调用中都是必填的，如果并不是每次都使用定位参数，可以在定位参数中设置默认值；
- 定位参数和可选参数的顺序在使用的时候可以不用关心，argparse 会识别出哪个是可选参数，哪个是定位参数；
- 如果有多个定位参数，只要设置好对应的类型，argparse 也会自动帮你区分清楚；
- 设置完毕后，在最后判断 args 就能获取到命令行输入的参数了。





