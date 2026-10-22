# materials

选择一个 Makefile 模板，复制到自己的 C++ 工程根目录，即可开始构建。

本仓库收集可复用的开发资料，目前提供两个 GNU Make 模板：

| 你要构建什么 | 模板 | 默认产物 |
| --- | --- | --- |
| C++ 可执行程序 | [Makefile.bin](common_make/Makefile.bin) | `app_foo` |
| C++ 静态库 | [Makefile.libs](common_make/Makefile.libs) | `libs/libfoo.a` |

## 快速使用

1. 准备 GNU Make、支持 `-MD` / `-MP` 的 C++ 编译器（如 GCC）。构建静态库还需要 `ar` 和 `ranlib`。
2. 按下面的示例组织自己的源码，选择对应模板。
3. 在自己的工程根目录执行复制和构建命令。将 `/path/to/materials` 替换为本仓库的实际路径。

### 构建可执行程序

模板扫描工程根目录和 `src/` 下的 `*.cpp`，头文件搜索路径为 `include/`。源码中需要包含 `main()`：

```text
your-project/
├── main.cpp
├── include/
│   └── greeting.h
└── src/
    └── greeting.cpp
```

```sh
cp /path/to/materials/common_make/Makefile.bin Makefile
make
./app_foo
```

默认链接 `pthread` 和 `m`。产物名称由 `Target` 控制。

### 构建静态库

模板扫描 `src/` 下的 `*.cpp`，头文件搜索路径为工程根目录和 `include/`：

```text
your-library/
├── include/
│   └── greeting.h
└── src/
    └── greeting.cpp
```

```sh
cp /path/to/materials/common_make/Makefile.libs Makefile
make
```

产物为 `libs/libfoo.a`，输出目录会自动创建。静态库本身不需要 `main()`。

## 常用配置

复制模板后，按工程需要修改以下变量；也可以通过 `make CXX=g++` 指定编译器。

| 变量 | 用途 |
| --- | --- |
| `Target` | 可执行程序名称，仅用于 `Makefile.bin` |
| `LIB_DIR` / `LIB_TARGET` | 静态库输出目录与完整路径，仅用于 `Makefile.libs` |
| `INC` | 头文件搜索路径，例如 `-I./include` |
| `CXXFLAGS` | 编译选项，默认包含 `-Wall`、`-g`、`-O2` 和 `-D_GNU_SOURCE` |
| `LIB` | 可执行程序的链接库，默认 `-lpthread -lm` |

源码扫描不递归进入更深的子目录。新增源码目录时，需要同步调整源码匹配规则和依赖目录的创建规则。

## 构建与清理

在复制了模板的工程根目录运行：

| 命令 | 效果 |
| --- | --- |
| `make` 或 `make all` | 构建默认目标 |
| `make clean` | 删除对象文件和构建产物 |
| `make distclean` | 额外删除当前源码对应的依赖文件 |

编译时会在 `.deps/` 下生成依赖文件，用于跟踪头文件变化。清理命令可能留下空目录。

## 使用 Codex

[AGENTS.md](AGENTS.md) 说明了仓库结构、编辑规范和验证方式。
工程内已安装 [i-have-adhd](.agents/skills/i-have-adhd/SKILL.md) skill：
在本仓库启动 Codex 会话，输入 `$i-have-adhd`，启用行动优先的简洁输出。

