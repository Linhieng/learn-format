# vscode 中 c/cpp 语言的格式化

[最新官方文档](https://clang.llvm.org/docs/index.html)
[v14版本文档](https://releases.llvm.org/14.0.0/tools/clang/docs/index.html)

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
  ... # 多余的
  ```

- 在代码中使用注释来开启/关闭格式化:

  ```c
  int formatted_code;
  // clang-format off
      void    unformatted_code  ;
  // clang-format on
  void formatted_code_again;
  ```

## 🍕 配置项

[v14版本官方参考](https://releases.llvm.org/14.0.0/tools/clang/docs/ClangFormatStyleOptions.html)

[博客参考1](https://blog.csdn.net/core571/article/details/82867932)

[博客参考2](https://blog.csdn.net/deeplee021/article/details/100877960)