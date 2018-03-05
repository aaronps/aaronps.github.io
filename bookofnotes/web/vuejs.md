# vuejs

## Basic Steps

1. require nodejs (and npm) (>=8.0.0 seems working)
1. ensure taobao's npm registry or using proxy
1. ensure git use proxy or hosts configured properly
1. npm install --global vue-cli
1. git clone https://github.com/vuejs-templates/webpack webpack-template
1. vue init ./webpack-template new-project-name
1. cd new-project-name
1. npm install
1. npm run dev

## notes

```html
<template>
    <div>
        <bindings>
            <element v-bind:something="someVueObject"/>
            <element :something="someVueObject"/>
        </bindings>
        <events>
            <element v-on:click="fun">
            <element @click="fun">
        </events>
    </div>
</template>
```

## element-ui

```html
<!-- Menu using Vue Router -->
<el-menu :router="true" :default-active="$route.path"/>`
```