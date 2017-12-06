## 前言
本项目为学习慕课网Vue.js音乐App高级实战课之后的练习项目，需要学习者请转至黄佚老师账号下查看源码，或者慕课网购买原版视频。
本项目所使用的音乐及图片等资源，是直接抓取QQ音乐官网的线上接口，仅作为项目练习使用，不用做商业用途。

### 技术栈
vue2 + vuex + vue-router + axios + webpack + ES6 + styls
### 项目运行

```
git clone https://github.com/PosyMo/vue-music-app.git
cd vue-music-app
npm install
npm run dev
```

## 说明
### 涉及的技术点
1. jsonp跨域请求
2. 服务器请求代理
3. Base64格式编解码
4. 图片懒加载
5. SVG绘图
6. 原生touch事件

## 效果演示
### 部分截图
## 项目布局
```
.
├── build                                       // webpack配置文件
├── config                                      // 项目打包路径
├── src                                         // 源码目录
│   ├── api                                     // api相关封装
│   ├── base                                    // 基础组件
│   ├── common                                  // 公共资源文件
│   ├── components                              // 业务逻辑组件
│   ├── router                                  // 路由相关
│   ├── sotre                                   // vuex状态管理
│   │   ├── actions.js                          // 配置actions
│   │   ├── getter.js                           // 配置getter
│   │   ├── index.js                            // 引用vuex，创建sotre
│   │   ├── mutation-types.js                   // 定义muations常量
│   │   ├── mutations.js                        // 配置mutations
│   │   └── state.js                            // 定义state状态
│   ├── app.vue                                 // 页面入口文件
│   └── main.js                                 // 程序入口文件
├── index.html                                  // 入口html文件
.
```