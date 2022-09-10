# vscode 中 c/cpp 语言的格式化

[最新官方文档](https://clang.llvm.org/docs/index.html)
[v14版本文档](https://releases.llvm.org/14.0.0/tools/clang/docs/index.html)

**说在前头** clang-format 的配置, 不是简单的, 配置了就一定会生效的,
每一个配置项都不是孤立的, 他们之间会互相影响,
所以直接测试单个配置项, 很多时候可能会是无效的.
下面的这些配置项, 只是给出了该配置项的作用,
但实际效果还是得看具体的 `.clang-format` 文件

## 🍕 基本使用

- vscode 基础配置信息:

  ```json
  "C_Cpp.formatting": "clangFormat", // 使用 `clang-format` 格式化
  "C_Cpp.clang_format_style": "file", // 从当前目录或父目录中的 `.clang-format` 文件加载样式
  "C_Cpp.clang_format_path": "C:/Users/Linhieng/.vscode/extensions/ms-vscode.cpptools-1.12.4-win32-x64/LLVM/bin/clang-format.exe" // 指定 clang-format.exe 可执行文件路径
  ```

- 查看 clang-format 版本

  ```bash
  C:/Users/Linhieng/.vscode/extensions/ms-vscode.cpptools-1.12.4-win32-x64/LLVM/bin/clang-format.exe --version
  ```

- 书写配置信息,🌰:

  创建 `.clang-format` 文件, 写入配置内容, 比如:

  ```yaml
  ---
  IndentWidth: 4 # 缩进宽度
  ```

  如果配置文件格式不正确, 或者乱写, 则会导致格式化出错, 比如下面这几种情况:

  **补充:** 准确的来说, 下面这些写法, 不一定是错误的, 可能是因为 vscode 的原因或者其他说明原因而导致错误.
  因为 linux 大佬的 `.clang-format` 直接复制来用的时候, 就会报错.

  > 附上 linux 的格式化[配置参考](https://github.com/torvalds/linux/blob/master/.clang-format)

  ```yaml
  AlignConsecutiveStyle: Consecutive # 暂时不清楚为什么会错
  ```

  ```yaml
  ---
  IndentWidth: 4
  --- # 多余的
  ```

  ```yaml
  IndentWidth: 4
  ... # # 暂时不清楚为什么会错
  ```

- 在代码中使用注释来开启/关闭格式化:

  ```c
  int formatted_code;
  // clang-format off
      void    unformatted_code  ;
  // clang-format on
  void formatted_code_again;
  ```

## 🍕 积累的配置项(简单)

- `IndentWidth: 4`  缩进宽度
- `IndentCaseLabels: true` 对 `switch` 语句的 `case` 和 `default` 缩进, 如果为 `false`, 则 `switch` 和 `case` 在垂直方向上对齐
- `IndentGotoLabels: true` 对 `goto` 的 `label` 进行缩进, 如果为 `false`, 则 `goto` 语句的 `label` 始终在最左边
- `IndentExternBlock: true` 对 `extern` 块的内容缩进, 如果为 `false`, 则 `extern` 块中的内容和 `extern` 的缩进相同

- `AlignTrailingComments: true` 对齐尾随的注释
- `SpacesBeforeTrailingComments: 3` 尾随注释前的空格数, 即 `//` 前至少会有的空格数
- `ColumnLimit: 80` 限制行宽度, `0` 代表不限制, 即尊重每一行的宽度, 但也可能会因为其他配置而导致换行.

  **注**: 有些配置不生效, 可能就是因为 `ColumnLimit` 的值较大, 压根没换行, 所以没效果.

- `DisableFormat: false` 是否关闭格式化
- `MaxEmptyLinesToKeep: 10` 允许的最大连续空行
- `KeepEmptyLinesAtTheStartOfBlocks: true` 保留块开始处的空行. 块: 即作用域块, 一般由 {} 包裹
- `BreakBeforeBraces: Custom` 让 `BraceWrapping` 配置项生效

- `SpacesInParentheses: true` 括号内部两侧是否要加空格.

## 🍕 积累的配置解释项(需解释)

### BreakBeforeBinaryOperators

处理 "操作符" 的换行位置.

> 操作符: 比如 `=`, `+`, `-` 等等

- `None` 在操作符后面换行, 即操作符在行尾
- `All` 在操作符前面换行, 即操作符在行首
- `NonAssignment` 赋值型的相当于 `None`, 计算型的相当于 `All`

### AlignOperands

> 它的值还可以是 `true` 官网的解释是 **If true, horizontally align operands of binary and ternary expressions.** 暂时搞不懂为什么可以是 `true`, 也可以是下面的值.

定义 "操作符" 的对齐方式. 搭配 `BreakBeforeBinaryOperators` 使用

- `DontAlign` 不处理
- `Align`

  ```c
  int aaa = bbbbbbbbbbbbbbb +
            ccccccccccccccc;
  int bbb = bbbbbbbbbbbbbbb
            + ccccccccccccccc;
  ```

- `AlignAfterOperator`

  ```c
  int aaa = bbbbbbbbbbbbbbb
          + ccccccccccccccc;
  ```

### BraceWrapping

要求 `BreakBeforeBraces: Custom`, 该配置项才会生效

BraceWrapping 代表的是一个 "组", 在他下面, 可以缩进然后配置多个样式, 比如:

```yaml
BraceWrapping:
  AfterCaseLabel: false
  AfterControlStatement: MultiLine
```

下面给出一些可以配置的样式

- `AfterCaseLabel`

  - `true`: `{` 在 `case` 下一行;
  - `false`: `{` 和 `case` 在同一行;

  后面的就只给出 `true` 或 `false` 的情况, 另外一种就是相反的.

- `AfterEnum`: 同上
- `AfterFunction`: 同上
- `AfterNamespace`: 同上
- `AfterStruct`: 同上
- `AfterUnion `: 同上
- `AfterExternBlock`: 同上
- `BeforeCatch`: 同上
- `BeforeElse`: 同上
- `BeforeWhile `: 同上

- `IndentBraces`: `true` 则对 `{` 和 `}` 本身再进行缩进, `false` 则是我们常规的那样子.

- `AfterControlStatement`:

  > **control statement**: `if` / `for` / `while` / `switch` /..

  - `Never`: `{` 永远不换行
  - `Always`: `{` 永远用于换行
  - `MultiLine`: 不同情况会有不同的结果, 具体依据是什么我也不清楚(测试后和官网的解释不太一样). 如果你将 `{` 换行, 那么它就换行, 你不换行, 它也就不换行

### AlignAfterOpenBracket

对 **open bracket** 的缩进

适用于 round brackets (parentheses), angle brackets and square brackets, 即: `()`, `<>`, `[]`

- `Align` 与 open bracket 对齐, 比如:

  ```c
  someLongFunction(argument1,
                   argument2);
  ```

- `DontAlign` 不对齐
- `AlwaysBreak` 在 open bracket 后总是换行
- `BlockIndent` 在 open bracket 后换行, 并且 Closing brackets 也会在新的一行上, e.g.:

  ```c
  someLongFunction(
      argument1, argument2
  )
  ```

### ContinuationIndentWidth

控制连续的语句的缩进宽度, 直接解释有点难解释, 看例子比较方便:

- 案例1

  ```c
  // IndentWidth: 8
  // ContinuationIndentWidth: 2
  int isLeapYear(int year) {
          return (
            (
              year % 4 == 0
              && year % 100 != 0
            )
            || (year % 400) == 0
          );
  }
  ```

  缩进默认是 8, `return` 语句就是 **line continuations**,
  当我们将他们换行时, 他们的缩进是 2, 这就是 `ContinuationIndentWidth` 控制的缩进

- 案例2

  有关系的配置(核心):
  ```yaml
  ColumnLimit: 0 #　下面的配置能够生效都是因为这里设置了 0, 即不限制宽度
  AlignAfterOpenBracket: BlockIndent
  ContinuationIndentWidth: 8
  ```

  ```c
  void f1(int a, // 因为 `void f1(` 小于 ContinuationIndentWidth, 所以程序能够理解你是要垂直对齐.
          int b, // 当然, 如果你不是手动分行的话, 程序会认为你要水平放置
          int c) {
  }
  void f22(int a, int b, int c) {
           // 因为 `void f22(` 这个宽度已经大于等于 ContinuationIndentWidth 的值,
           // 所以 三个形参会被合并到一行
  }

  void f33(
          int a, // 因为把第一个形参放到了新的一行, 这样的话, 就不会考虑 `void f33(` 的宽度了,
                 // 程序会始终按照 ContinuationIndentWidth 的值对形参进行缩进
          int b,
          int c
  ) { // 这个括号能够在新的一行, 而不会贴在 int c 后面, 是因为有 AlignAfterOpenBracket: BlockIndent
  }
  ```

  这个格式化, 解释起来还是挺困难的, 只能 "意会",
  因为一个配置会因为其他的配置, 而产生出不一样的格式化效果.

  所以直接复制上面的配置, 想要测试的时候, 有时候总会失败,
  就是因为配置与配置之间会互相影响

### AlignConsecutiveAssignments

解释起来太麻烦了, 直接看例子吧, 后面的如果解释不清就不做说明了, 直接给出例子.

- `None`
- `Consecutive`

  ```c
  int a            = 1;
  int somelongname = 2;
  double c         = 3;

  int d = 3;
  /* A comment. */
  double e = 4;
  ```

- `AcrossEmptyLines`

  ```c
  int a            = 1;
  int somelongname = 2;
  double c         = 3;

  int d            = 3;
  /* A comment. */
  double e = 4;
  ```

- `AcrossComments`

  ```c
  int a            = 1;
  int somelongname = 2;
  double c         = 3;

  int d    = 3;
  /* A comment. */
  double e = 4;
  ```

- `AcrossEmptyLinesAndComments`

  ```c
  int a            = 1;
  int somelongname = 2;
  double c         = 3;

  int d            = 3;
  /* A comment. */
  double e         = 4;
  ```

### SpaceBeforeParensOptions

这是一个配置组, 配置括号前方是否要有空格

想生效, 需要 `SpaceBeforeParens: Custom`.

可配置项如下:

- `AfterControlStatements` 配置 **control statement** (`if` / `for` / `while` / `switch` /..) 的前方是否要有空格, `true` 则代表带上空格
- `AfterFunctionDefinitionName` 同上, 只不过 control statement 换成了函数定义, 比如 `void fn() {}`
- `AfterFunctionDeclarationName` 同上, 这次是函数声明, 比如 `void fn();`
- `AfterIfMacros` 同上, 这次是 IF 宏
- `AfterOverloadedOperator` 同上, Overloaded Operator 不懂是啥, 形式是: `void operator++ (int a);` 和 `object.operator++ (10);`
- `BeforeNonEmptyParentheses` 同上, 这个是调用函数时是否带上空格, 一般不带.

###

## 🍕 配置参考文档

使用方法: 先看一些博客参考, 然后根据字段去官网文档查找.
找了几个配置项后, 就会有 "感觉" 了, 此时就根据自己的 "感觉" 在官网自己搜索.

[v14版本官方参考](https://releases.llvm.org/14.0.0/tools/clang/docs/ClangFormatStyleOptions.html)

[博客参考1](https://blog.csdn.net/core571/article/details/82867932)

[博客参考2](https://blog.csdn.net/deeplee021/article/details/100877960)

[博客参考3](https://www.cnblogs.com/oloroso/p/14699855.html)