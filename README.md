# 微前端特性
* **技术栈无关** 主框架不限制接入应用的技术栈，子应用可自主选择技术栈（vue,react,jq,ng等）
* **独立开发/部署** 各个团队之间仓库独立，单独部署，互不依赖
* **增量升级** 当一个应用庞大之后，技术僧及或重构相当麻烦，而微应用具备渐进式僧级的特性
* **独立运行时** 微应用之间运行时互不依赖，有独立的状态管理


# 微前端方案

* iframe方案
* qiankun 方案
* micro-app 方案
* EMP 方案
* 无界微前端 方案

# 微前端
vue3 + pnpm + wujie
官网：https://wujie-micro.github.io/doc/guide/
## 前置
* webComponents
* 硬链接与软链接（符号链接）
```powershell
# 假设demo文件下有一个index.js文件
cd demo
mklink

# 创建硬链接
# 基于index.js创建ying.js
# 共享同一个内存地址，index.js改变会导致ying.js也会改变
mklink /H ying.js index.js

# 创建软链接
# 需要管理员模式打开
# 可理解为：快捷方式，只记录一个路径
mklink ruan.js index.js
```
## 创建
* 全局安装pnpm
```powershell
# 全局安装pnpm
npm i pnpm -g
pnpm -v
```
* 创建应用
```powershell
mkdir monorepo-pnpm-wujie-micr
cd monorepo-pnpm-wujie-micr

# 创建主应用，vue3
npm init vue

# 创建子应用，这里创建一个react项目和一个vue3项目
mkdir web
cd web
npm init vite

cd ..
pnpm init

# 创建pnpm-workspace.yaml文件并写入配置
  packages:
    - 'main'
    - 'web/**'

# 安装依赖   
pnpm install
```
* 子模块复用
```powershell
# 创建通用文件夹
mkdir common
cd common

# pnpm init后将 - 'common' 配置到pnpm-workspace.yaml里
pnpm init

# main主文件中添加公用的common
cd ..
cd main
pnpm -F main add common
```
* 安装wujie
```powershell
cd main
# pnpm i wujie
# wujie-vue3是进行了二次封装，无需main.js里配置
pnpm i wujie-vue3
```
* 引入并注册
```js
// main/main.js
import Wujie from 'wujie-vue3'
app.use(router).use(Wujie)
```
* 启动
```powershell
# 启动主应用，http://localhost:5173/
pmpn dev 

# 启动子应用
cd web
cd .\react-demo\
# http://localhost:5174/
pnpm run dev 

cd .\vue-demo\
# http://localhost:5175/
pnpm run dev 
```
* main/App.vue中使用
```html
<!--main/App.vue-->
<template>
  <h1>这是主应用，主应用用的vue3</h1>

  <!-- 子应用 -->
  <wujieVue url="http://localhost:5174/" name="react"></WujieVue>
  <wujieVue url="http://localhost:5175/" name="vue3"></WujieVue>
</template>
```