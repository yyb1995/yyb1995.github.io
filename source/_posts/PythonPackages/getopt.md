---
title: getopt
categories: Python Packages
tags: getopt Python
icon: note
---

例子：
```python
import sys, getopt
def main(argv):
    print(str(argv))
    inputfile = ''
    outputfile = ''
    try:
        opts, args = getopt.getopt(argv, 'hi:o:', ['help', 'ifile=', 'ofile='])
    except getopt.GetoptError:
        print('test.py -i <inputfle> -o <outputfile>')
        sys.exit()
    for opt, arg in opts:
        if opt in ['-h', '--help']:
            print(1)
        elif opt in (['-i', '--ifile']):
            inputfile = arg
        elif opt in (['-o', '--ofile']):
            outputfile = arg
    print('input file is : ', inputfile)
    print('output file is : ', outputfile)


if __name__ == '__main__':
    main(sys.argv[1:])
```
**核心函数**：`options, args = getopt.getopt(args, shortopts, longopts=[])`
**参数**：
- shortopts：短格式参数串。shortopts 后的冒号(:)表示如果设置该选项，必须有附加的参数，不带冒号表示该选项没有附加参数如："h f: v:"。h后面没有冒号，表示该选项没有附加参数，如用于输出命令用法f和v后面带有冒号，表示该选项有附加参数。无附加参数的示例为：`python test.py -h`。有附加参数的示例为：`python test.py -h content`。注意：加了冒号的参数的选项自带`-`，因此在获取opt时需要加上`-`

- longopts：长格式参数列表。longopts 后的等号(=)表示如果设置该选项，必须有附加的参数，否则就没有附加参数如：["help", "file=", "version="]。help 后面没有等号，表示该选项没有附加参数file 和 version 后面带冒号，表示如果设置该选项，必须有附加参数。无附加参数的示例为：`python test.py --help`。有附加参数的示例为：`python test.py --help content`

- 返回值含义：options：元组列表，每个元组的形式为：(选项, 附加参数)，如：('-f', 'data.csv')args：包含那些没有 '-' 或 '--' 的参数列表

