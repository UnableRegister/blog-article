# 2017-12-30-JavaScrip 零宽断言#JavaScrip#正则

一个前端项目，`yarn test` 在本地 Chrome（版本 66） 调试无误。使用 `yarn build` 编译时出现下面的错误：

```shell
ERROR in chunk app [initial]
static/js/[name].[chunkhash].js
Invalid regular expression: /(?<=#)[\u4E00-\u9FA5a-zA-Z\.]+/: Invalid group (8:46)
|  */
| function Tags(fileName) {
|   return fileName.replace(/\.md$/, '').match(/(?<=#)[\u4E00-\u9FA5a-zA-Z\.]+/g);
| }
|
  Build failed with errors.

error An unexpected error occurred: "Command failed.
```

Babel配置如下：

```
{
  "presets": [
    ["env", {
      "modules": false,
      "targets": {
        "browsers": ["> 1%", "last 2 versions", "not ie <= 8"]
      }
    }],
    "stage-2"
  ],
  "plugins": ["transform-vue-jsx", "transform-runtime"]
}

```

出错的函数：

```
function Tags (fileName) {
  return fileName.replace(/\.md$/, '')
  	.match(/(?<=#)[\u4E00-\u9FA5a-zA-Z\.]+/g)
}
```

上面的代码中使用了**后行断言**（也叫 **零宽断言**）。JavaScript 的**后行断言**在 ES2018 引入，Chrome 62 已经支持。所以才出现了上面的情形。

鉴于 ES2018 的浏览器支持情况，最好的解决办法是避免使用**后行断言**。

修改后的代码如下：

```
function Tags (fileName) {
  let tags = fileName.replace(/\.md$/, '')
	  .match(/#[\u4E00-\u9FA5a-zA-Z\.]+/g) || []

  return tags.map(tag => {
    return tag.replace('#', '')
  })
}
```

