# mitojs

[![npm version](https://img.shields.io/npm/v/mitojs.svg?style=flat-square)](https://www.npmjs.org/package/mitojs)
[![install size](https://packagephobia.now.sh/badge?p=mitojs)](https://packagephobia.now.sh/result?p=axios)
[![npm downloads](https://img.shields.io/npm/dm/mitojs.svg?style=flat-square)](http://npm-stat.com/charts.html?package=mitojs)
<!-- [![CDNJS](https://img.shields.io/cdnjs/v/mitojs.svg?style=flat-square)](https://cdn.jsdelivr.net/npm/mitojs/dist/mito.min.js) -->
<!-- [![build status](https://img.shields.io/travis/axios/axios/master.svg?style=flat-square)](https://travis-ci.org/axios/axios) -->
<!-- [![code coverage](https://img.shields.io/coveralls/mzabriskie/axios.svg?style=flat-square)](https://coveralls.io/r/mzabriskie/axios) -->

[TOC]




## 概要

一款轻量级的收集页面的用户点击行为、路由跳转、接口报错、代码报错、并上报服务端的SDK

## 安装

### 使用npm

`npm i mitojs`

### 使用CDN

`<script src="https://cdn.jsdelivr.net/npm/mitojs/dist/mito.min.js"></script>`

## 使用环境

### Vue2.6

```javascript
import MITO from 'mitojs'
import Vue from 'vue'
Vue.use(MITO.MitoVue)
MITO.init({
  apikey: 'aabbcc',
  version: '1.0.0',
  dsn: 'http://test.com/error'
})
```

### JS

```javascript
<script src="https://cdn.jsdelivr.net/npm/mitojs/dist/mito.min.js"></script>
MITO.init({})
```



# options
|            Name            | Type       | Default                  | Description                                                  |
| :------------------------: | ---------- | ------------------------ | ------------------------------------------------------------ |
|           `dsn`            | `string`   | `""`（需要来个默认地址） | dsn服务地址，上报接口的地址，post方法                        |
|         `disabled`         | `boolean`  | `true`                   | 默认是开启状态，为true时，会将sdk禁用                        |
|          `apikey`          | `string`   | `""`                     | 每个项目对应一个apikey，用于存放错误集的唯一标识             |
|          `debug`           | `boolean`  | `false`                  | 默认不会在控制台打印用户行为和错误信息，为true时将会在控台打印 |
|         `version`          | `string`   | `0.0.0`                  | 线上版本，服务端会做数据过滤，version就是其中一环，可以在页面更好的搜索错误日志 |
|      `maxBreadcrumbs`      | `number`   | `20`                     | 用户行为存放的最大容量，最大是100，当你配置超过100时，最终还是会设置成100，一方面是防止占更多的内存、一方面是保存超过100条用户行为没多大意义 |
|        `silentXhr`         | `boolean`  | `false`                  | 默认会监控xhr，为true时，将不再监控                          |
|       `silentFetch`        | `boolean`  | `false`                  | 默认会监控fetch，为true时，将不再监控                        |
|      `silentConsole`       | `boolean`  | `false`                  | 默认会监控console，为true时，将不再监控                      |
|        `silentDom`         | `boolean`  | `false`                  | 默认会监听click事件，当用户点击的标签不是body时就会被放入breadcrumb，为true，将不在监听 |
|      `silentHistory`       | `boolean`  | `false`                  | 默认会监控popstate、pushState、replaceState，为true时，将不再监控 |
|       `silentError`        | `boolean`  | `false`                  | 默认会监控error，为true时，将不在监控                        |
| `silentUnhandledrejection` | `boolean`  | `false`                  | 默认会监控unhandledrejection，为true时，将不在监控           |
|     `silentHashchange`     | `boolean`  | `false`                  | 默认会监控hashchange，为true时，将不在监控                   |
|        `silentVue`         | `boolean`  | `false`                  | 默认会监控Vue的错误，为true时，将不在监控                    |
|        `beforeSend`        | `function` | `null`                   | 钩子函数：在每次发送事件前会调，如果返回null \| undefined \| false时，将忽略本次上传 |
|     `beforeBreadcrumb`     | `function` | `null`                   | 钩子函数：在每次添加用户行为事件前都会调用，如果返回null \| undefined \| false时，将忽略本次的push操作 |

### hooks

#### beforeSend

```typescript
function(event: ReportDataType)
interface ReportDataType {
  type?: ERRORTYPES
  message?: string
  url: string
  name?: string
  stack?: any
  time?: number
  errorId?: number
  level: number
  // ajax
  elapsedTime?: number
  request?: {
    httpType?: string
    method: string
    url: string
    data: any
  }
  response?: {
    status: number
    statusText: string
    description: string
  }
  // vue
  componentName?: string
  propsData?: any
  // logerror
  info?: string
}
```

#### beforeBreadcrumb

```typescript
function(breadcrumb: Breadcrumb, hint: BreadcrumbPushData)
interface BreadcrumbPushData {
  type: string
  data: any
}
export class Breadcrumb{
  private maxBreadcrumbs:number
  private stack:BreadcrumbPushData[]
  push()
  getStack()
}

```

## demo
<!-- 需要做一个静态网页，然后在右侧打印出breadcrumb -->
