# vue-pack-as-single-file


## 项目创建

### 安装nodejs
```
https://nodejs.org/en/
```

### 安装Vue CLI
```
npm install -g @vue/cli
```

### 创建Vue项目
在当前文件夹下创建
```
vue create .
```

### 添加Vuetify
```
vue add vuetify
```

### 修改配置，使之打包为单文件

参考如下链接
https://stackoverflow.com/questions/58274001/vue-cli-combine-build-output-to-a-single-html-file

```
npm install --save-dev html-webpack-plugin 
npm install --save-dev html-webpack-inline-source-plugin
```


```
// vue.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin')
const HtmlWebpackInlineSourcePlugin = require('html-webpack-inline-source-plugin');
module.exports = {
    css: {
        extract: false,
    },
    configureWebpack: {
      optimization: {
        splitChunks: false // makes there only be 1 js file - leftover from earlier attempts but doesn't hurt
      },
      plugins: [
        new HtmlWebpackPlugin({
          filename: 'output.html', // the output file name that will be created
          template: 'src/output-template.html', // this is important - a template file to use for insertion
          inlineSource: '.(js|css)$' // embed all javascript and css inline
        }),
        new HtmlWebpackInlineSourcePlugin()
      ]
    }
  }
```

```
<!-- output-template.html -->
<!DOCTYPE html>
<html lang=en>
    <head>
        <meta charset=utf-8>
        <meta http-equiv=X-UA-Compatible content="IE=edge">
        <meta name=viewport content="width=device-width,initial-scale=1">        
        <title>example title</title>
    </head>
    <body><noscript><strong>We're sorry but my example doesn't work properly without JavaScript enabled. Please enable it to continue.</strong></noscript>
        <div id=app>

        </div>
        <!-- plugin will insert js here by default -->
    </body>
</html>
```

运行`npm run build`，在dist文件夹下找到output.html; 打开文件，发现`<script src="/js/app.35e9ef65.js"></script>`， 去掉多余的`/`，然后可以在浏览器中打开测试。

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
