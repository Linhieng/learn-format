## 🍕 我的想法

[官网](https://prettier.io/docs/en/options.html)提供的配置项太少了.

但主要的原因还是 `prettier` 管的东西太多了, 某一些格式化我无法关闭它, 最终我还是选择不使用 prettier.

下面的这些, 是在使用过程中积累的配置项, 就留着吧.

## 🍕 参考

> [官方文档](https://prettier.io/docs/en/install.html)
>
> [配置](https://prettier.io/docs/en/options.html)

## 🍕  普通使用

安装 `Prettier - Code formatter` 插件,
然后就可以直接使用了.

可以在 `setting.json` 中配置格式化规则,
比如 `"prettier.semi": true,`.
这种情况下的配置可选项比较少.

可以在项目根目录下创建 `.prettierrc.js` 文件,
然后填写对应的配置项, 比如

```js
module.exports = {
  tabWidth: 2,
  semi: false,
  singleQuote: true,
  trailingComma: 'all',
}
```

> `package.json` 和是否可以格式化, 没啥影响,
> 不安装 `prettier` 包也可以格式化.

## 🍕 配置项

有一些很简单的配置项, 就不解释了. 只保留一些测试过那些配置项和看英文无法立马知道什么意思的那些吧.

- `bracketSpacing` 是否在对象内加上空格, 如 { a:1 }
- `arrowParens` 箭头函数单参数时是否带上括号, 有 `always` 和 `avoid` 可选
- `requirePragma` 是否需要要求有顶部注释, 才进行格式化
- `insertPragma` 格式化时, 是否自动在顶部加上格式化注释
- `bracketSameLine` 是否将 `>` 符号贴紧在最后, 设置为 `false` 时, `>` 始终单独成行
- `htmlWhitespaceSensitivity` 指定 `html` 中的 **单个空格** 是否是重要的.

  - 值为 `strict` 时, 头尾多个空格会合并成一个, 比如 `<div> 1 </div>`;
  - 值为 `ignore` 时, 空格被视为不重要的, 头尾空格会被删除, 比如 `<div>1</div>`.