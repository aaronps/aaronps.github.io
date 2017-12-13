# webpack

From <https://webpack.js.org/guides/getting-started/>

```shell
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install --save-dev webpack
```


stop antivirus (360) before installing things with node


`npm install -D webpack webpack-dev-server babel-core babel-loader babel-preset-env`
`npm install -D vue-loader vue-template-compiler css-loader cross-env`
`npm install axios es6-promise vue vue-echarts`

in scripts:
* dev: "cross-env NODE_ENV=development webpack-dev-server --hot"
* build: "cross-env NODE_ENV=production webpackl --progress --hide-modules"

* This is not helping with the work computer problem

```
npm config set registry http://registry.npmjs.org
npm config set strict-ssl false
```

### If webpack build gives uglifyjs error (>)

* Check that you have `.babelrc`

```json
{
    "presets": [
        ["env", {"modules": false}]
    ]
}
```