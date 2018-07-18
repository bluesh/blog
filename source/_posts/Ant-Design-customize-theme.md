---
title: Ant Design 定制主题
date: 2018-07-18 19:24:15
tags: 假使要我跟你再耗个十年
---

按照[Ant Design 官网](https://ant.design/docs/react/customize-theme-cn)推荐的方案定制了网站主题，使用了一段时间之后发现不满足需求：
开发中也需要用到样式变量，总不能再拷贝一份`less`变量文件吧？

虽然主题可能修改了之后变动并不大，但总归存在隐患。而且我也不能忍受在`package.json`中一份`json`变量，开发目录下一份`less`变量，还是一毛一样！

珰珰～这时候我们就要用到 [less-vars-to-js](https://github.com/michaeltaranto/less-vars-to-js)啦～

有同样的需求的就可以继续往下看操作步骤：

### 1. 安装包
```
npm install --save less-vars-to-js
```

### 2. 增加配置文件
```javascript
// generateTheme.js
const path = require('path');
const lessToJs = require('less-vars-to-js');
const fs = require('fs');

module.exports = lessToJs(
  fs.readFileSync(path.join(__dirname, '../src/themes/index.less'), 'utf8')
)
```

### 3. 修改webpack配置
```javascript
{
    loader: "less-loader", options: {
      sourceMap: true,
      modifyVars: require('./generateTheme')
    }
}
```
这时候`src/themes/index.less`文件中的样式变量就可以作用在`modifyVars`中，也能在项目中引入文件来使用变量。
这样才能为主题切换做好准备啊～

最近一年开发维护框架，都把Ant Design作为基础组件，组件齐全文档清晰，小伙伴都用得很开心，我也不需要花太多精力。平常也拿来开发业务组件。

关于动态切换主题有两个方案：

1. webpack生成多个主题包，不同的路径访问不同的主题包

2. 主题样式都在一个包里，切换时切换样式表

目前使用的是2方案，Ant Design的动态主题切换需要用到[antd-theme-generator](https://github.com/mzohaibqc/antd-theme-generator)，等到方案成熟会再写一篇，踩了很多坑。

顺便吐槽Echarts(链接都不想给它)真的相当烂，文档差劲，属性搜不到，版本文档不对应，官方给出的主题定制站点上都不生效的样式，还指望项目里飞天？！🌶️🐔
