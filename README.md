# font-cutting 集字

[![npm version](https://badge.fury.io/js/font-cutting.svg)](http://badge.fury.io/js/font-cutting)

字体库分割工具, 自动解析 html、js 等文件中的中文，按需压缩生成 web 所用格式字体文件

# 安装

    npm install font-cutting

# 功能

- 支持从 html、js 等任何文本文件中提取中文字符 支持 utf-8、gbk 编码

- 自动切分压缩(ttf 格式)字体文件，导出到指定目录

- 支持导出 woff2

# 注意

只能使用 ttf 格式的字体进行切割

# 使用

可以使用命令行参数，和配置文件方式使用此工具

## 命令行：

    font-cutting -h

        Usage: font-cutting [options] <path pattern or filepath ...>

    Options:

        -h, --help               output usage information
        -V, --version            output the version number

        // 含中文文件的文件路径或者文件夹
        -s, --source <path or file>      path pattern

        // （可选） 忽略文件路径
        -i, --ignore <path>      ignore path pattern

        // ttf字体文件路径
        -f, --font <path>        origin font file path

        // 输出字体的文件路径
        -o, --output <filepath>  filepath to output font files

        // 指定配置文件路径
        -c, --config <filepath>  load font.config.json file to run compile mission or set config file

### 示例命令：

    $ font-cutting -f test/lib/handfont.ttf -s test/index.html -o test/fonts/handfont

### 生成结果如下

```
font-cutting/
└── test/
    ├── fonts/
    │   ├── handfont.eot
    │   ├── handfont.svg
    │   ├── handfont.ttf
    │   ├── handfont.woff
    ├── lib/
    │   └── handfont.ttf
    └──index.html

```

## 配置文件：

`推荐` 可以使用配置文件的方式保存配置，方便构建。配置文件默认名称为 `font.config.json` 放在工程根目录。

执行 `font-cutting` 命令会自动读取当前目录下的配置文件

手动指定配置文件： `font-cutting -c myfont.confong.json`

配置文件格式如下：

    {
        "source": {
            "path": ["pattern", "another/pattern"],
            "ignore": ["ignore/pattern", "another/ignore/pattern"]
        },

        "font": "origin/font/path",

        "output": "output/fonts/path"
    }

source.path : 含中文字符文件目录，字符串或者数组，支持路径通配符;

source.ignore : （可选) 不想被匹配的文件目录，字符串或者数组，支持路径通配符;

font: 字体源文件路径;

output: 输出字体文件路径; ps: 需指定文件名并且不带后缀;

例：fonts/myfont; 会生成 fonts/myfont.eot | svg | ttf | woff 四种字体

### 示例配置文件:

搜索 app 文件夹下所有文件

    font.config.json

    {
        "source": {
            "path": "app/**/*",
        },

        "font": "./lib/handfont.ttf",

        "output": "./fonts/handfont"
    }

搜索当前目录所有文件，不包括 fonts 文件夹中文件，不包括 lib 文件夹中.tff 文件

    font.config.json

    {
        "source": {
            "path": "**/*",
            "ignore": ["fonts/*", "lib/*.ttf"]
        },

        "font": "./lib/handfont.ttf",

        "output": "./fonts/handfont"
    }

## thanks

> <https://github.com/purplebamboo/font-carrier> 提供字体底层操作支持

> <https://github.com/JailBreakC/font-collector> 根据 JailBreakC 的代码库升级过后的使用
