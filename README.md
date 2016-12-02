#### 关于wbepack+gulp的工程化方案 &copy;     -2016.11.15

> 1.最近刚入职，接手公司一个老项目的项目重构，项目要求是：讲项目组件化和模块化，并且引入新的前端框架React（项目未要求兼容IE8,未做兼容考虑），看起来工作量非常大，其实项目整体来说是个相对简单的地图项目，除了主要地图块之外，都是展示类页面，后期用一个react组件覆盖了大部分页面功能。但是在重构的过程中，遇到的问题还是相当多的，下面列举主要问题：
+ 项目前端后耦合度非常大，前后端逻辑杂糅
+ 缺少统一的项目管理，第三方类库和插件存放位置没有明显规则（譬如jQuery在多个文件夹中存在多个版本）
+ 前端代码没有编程规范，几乎都是声明函数，作用域不明确，环境污染严重。
+ 没有组件和模块的概念，意大利面条和胶水在页面四处存在
+ 前端规范不明确，没有考虑web在浏览器渲染的底层原理
+ 静态资源没有版本控制，无法处理浏览器缓存问题。
+ 以上只是主要问题，可能项目比较老，参与开发人员流动性比较大，导致业务堆砌比较严重，各位前辈手法不一致。

---
> 2.针对以上的问题，重构的主要思路是：
+ 使用Mock模拟静态数据+node跑前端本地服务，进行前后端分离
+ 统一工程化目录，划分具体文件区域
+ 引入Eslint代码规范，具体规范依照AirBnb/AirCss，对js和css进行编写规范
+ 引入Node-sass(权衡一下，有考虑Compass+sass，但是不喜欢compass的每次全文件编译，当然可能是我没找到解决点..😢，compass的雪碧图不错..)
+ 引入webpack做前端包管理和项目文件打包依赖关系，使用Gulp做项目构建）
+ 项目都是面向政府，IE8用户比较大，在MVC框架选型上，目前尚不明确（react最新版本完全放弃IE8及以下，VUE不支持IE8,NG曲线太高，不适合低成本，Backbone可以考虑），目前ES6在只在V8引擎浏览器实现，低版本不支持，使用bable等转译未完全测试，语法使用ES5,规范使用AMD.引用es5-shim.

---
> 3.项目结构

        |____css      项目样式文件
        | |____main.css   
        |____dev      项目功能模块
        | |____main
        | | |____main.js
        |____dist     项目最终发布模块
        |____img
        |____js      脚本存放
        | |____components     组件
        | | |____dialog.js
        | |____lib      第三方库&插件
        |____mock
        | |____main.json
        |____sass    sass文件
        | |____base   基本配置类
        | | |_____setting.scss
        | |____components   组件类
        | | |____dialog.scss
        | |____variables   公共变量
        | | |_____color.scss
        |____web    项目页面
        | |____main
        | | |____main.html
        |____webpack.config.js   webpack配置文件
        |____.eslintrc          eslint配置文件
        |____config.js          项目配置文件
        |____package.json       
        |____README.md           
        |____map.json           项目路径
        |____gulpfile.js        gulp执行文件

---
> 4.使用方式
 + cnpm i  下载包
 + npm start 开发环境
 + npm run build 生产环境  ps：这里有个问题，可能会因为项目文件实际发布地址问题，导致引用路径不对，这个时候，可以先走生产环境
 + npm gulp  处理文件版本控制，gulp构建

 ---
 > 5. 操作系统是OSX 10.11.6，window7 测试无碍。
