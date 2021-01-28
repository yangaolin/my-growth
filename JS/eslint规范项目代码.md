#### 安装一系列eslint插件后，填写eslint配置，配置如下

```
.editorconfig

root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true



.eslintignore

/build/
/config/
/dist/
/static/js
/src/test
**/*.js


.eslintrc.js

module.exports = {
  root: true,
  parserOptions: {
     parser: 'babel-eslint'
  },
  env: {
    browser: true,
  },
  globals: {
    'AMap': true,
    '$': true
  },
  extends: [
// https://github.com/vuejs/eslint-plugin-vue#priority-a-essential-error-prevention
      // consider switching to `plugin:vue/strongly-recommended` or `plugin:vue/recommended` for   		stricter rules.
    'plugin:vue/essential', 
    // https://github.com/standard/standard/blob/master/docs/RULES-en.md
    'standard'
  ],
  plugins: [
    'html',
    'vue'
  ],
  rules: {
    'generator-star-spacing': 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'semi': ['error', 'never'],
    'no-extra-semi': "error",
    'eqeqeq': 0,
    'spaced-comment': 0,
    'keyword-spacing': 0,
    "quotes": [0, "single"],
    'comma-dangle': 0,
    'curly': 0,
    'no-unused-expressions': 0,
    'padded-blocks': ["error", "never"],
    'space-before-function-paren': ["error", "always"],
    'no-mixed-operators': 0,
    'operator-linebreak': 0,
    'camelcase': 0,
    'no-lone-blocks': 0,
    'no-new': 0,
    'one-var': 0,
    'no-unneeded-ternary': 0
  }
}

webpack.base.conf.js

   {
        test: /\.(js|vue)$/,
        loader: 'eslint-loader',
        enforce: 'pre',
        include: [resolve('src'), resolve('test')],
       exclude: [resolve('src/components/upload')],
        options: {
          formatter: require('eslint-friendly-formatter'),
        }
   },
   {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
   },

User Settings

{
    "window.zoomLevel": 1,
    "files.autoSave": "off",
    "vetur.validation.template": false,
    "search.followSymlinks": false,
    "editor.tabSize": 2,
    "editor.parameterHints": true,
    "editor.quickSuggestions": {
        "other": true,
        "comments": true,
        "strings": true
    },
    "workbench.colorTheme": "Solarized Dark",

    // 单引号
    "prettier.singleQuote": true,
    // 不加分号
    "prettier.trailingComma": "all",
    //去掉代码结尾的分号
    "prettier.semi": false,
    "vetur.format.defaultFormatter.js": "vscode-typescript",
    // 使用 js-beautify-html 插件格式化 html
    "vetur.format.defaultFormatter.html": "js-beautify-html",
    //开启 eslint 支持
    "prettier.eslintIntegration": true,
    //每次保存自动格式化
    "editor.formatOnSave": true, 
    // 每次保存的时候将代码按eslint格式进行修复
    "eslint.autoFixOnSave": true,
    // 添加 vue 支持
    "eslint.validate": [
        "javascript",
        "javascriptreact",
        {
            "language": "html",
            "autoFix": true
        },
        {
            "language": "vue",
            "autoFix": true
        }
    ],
    // 格式化插件的配置
    "vetur.format.defaultFormatterOptions": {
        "js-beautify-html": {
            // 属性强制折行对齐
            "wrap_attributes": "force-aligned",
        }
    },
    //让函数(名)和后面的括号之间加个空格
    "javascript.format.insertSpaceBeforeFunctionParenthesis": true,
    //去掉分号
    "stylusSupremacy.insertSemicolons": false,
    //#去掉大括号
    "stylusSupremacy.insertBraces": false,
    ```