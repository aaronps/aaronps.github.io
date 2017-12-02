# vuejs

## Basic Steps

1. require nodejs (and npm) (>=8.0.0 seems working)
2. ensure taobao's npm registry or using proxy
3. ensure git use proxy or hosts configured properly
4. npm install --global vue-cli
5. git clone https://github.com/vuejs-templates/webpack webpack-template
6. vue init ./webpack-template new-project-name
7. cd new-project-name
8. npm install
9. npm run dev

For using JSPs also:

* `# npm install -D raw-loader`
* ??? and the HtmlWebpackPlugin use 'raw-loader!file.jsp'

## notes

* `<element v-bind:something="someVueObject">`
    * shorthand `:something=`
* `<element v-on:click="fun">`
    * shorthand `@click=`