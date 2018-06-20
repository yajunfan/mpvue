## mpvue
   这个是基于vue做的可以转成小程序的框架，本质上来说就是可以在我们平时使用的开发工具上，使用vue的的语言，使用div，h2等等html中的标签，来进行开发小程序，
   不局限于只能使用小程序的的开发工具

## 怎么开始

   1.首先有node，依然是按照之前的安装vue脚手架的方式来进行安装

   2.npm install vue-cli

   3.这里就不一样了，以前是vue init webpack projectname，现在是vue init mpvue/mpvue-quickstart mympvuename
   
   4.进入对应的文件夹 npm install 

   5.最后npm run dev就可以了
  
## 笼统的不一样的地方

   1.一般我们启动后会在浏览器打开，这个是会生成一个dist文件，然后将整个mympvuename的文件（不只是dist，而是完整的文件夹）
   在微信开发工具打开，就可以看到我们的项目
   然后我们每次在vue中进行了更改，微信开发工具中是可以直接看到效果的，（强大）

   2.我们可以看到创建之后的模板中是和我们之前的vue，模板是些许不一样的，会增加一个pages和util的文件夹，而且我们
   可以将
    最外边的main.js -- 小程序中的app.json 
    app.vue -- 小程序中的app.js(上半部分) 和app.css（下半部分）
    每一个文件夹下的main.js  -- 小程序中的每个页面的js
    在当前文件下的main.js中的
	```
    import Vue from 'vue'
    import App from './index'

   const app = new Vue(App)
    app.$mount()

   export default {
   config: {
     navigationBarTitleText: '用户'  这里是修改每页的title
    }
   }
	```
   3.最直观的感受是：
    -> 在微信开发工具上：我们的标签使用很受局限，view，text，image这几个，在css方面，也是使用class名或者id名来
    识别，进行样式的书写；
    -> 在mpvue创建的小程序中，我们可以像开发普通项目一样，正常使用我们的html语言，包括vue中的method等等，有一点
    区别就是，我们在这里不用使用路由，因为我们的微信小程序中有跳转的功能，wx.navigateTo({ url });

##  微信的特殊标签
    在vue中是可以正常使用的，写法会有一些不一样，遵循vue的语法就ok

## 创建新的页面
   -> 在pages文件夹下创建user文件夹，在user文件夹下创建index.vue和main.js

   -> 这里可以使用index.vue，也可以使用别的名称，这个和自己文件夹下的main.js会有关系
   在main.jsz中，import App from './index' ，如果我的user的文件夹下不是叫index.vue,是user.vue，那么这里就
   是引入import App from './user'

   -> 当我新增页面之后，在最外面的main.js中进行配置pages: ['pages/logs/main', '^pages/index/main', 'pages/user/main']，
   这时候是需要进行重新启动，微信开发工具中才可以进行查看

   -> 在index.vue页面中必须写script，然后写export default{}，如果不写这个的话，必须将这个暴露，否则会出现卡死现象，虽然不会报错

## 其他的一些知识
   
   在main.js中设置全局的tabBar中的icon路径的时候，需要注意的时，不需要写../，直接写在static/images/111.png
   等，因为在dist中，这个是app.json和static是平级

   在请求数据的时候，直接使用wx.request({})进行请求，这里获取到的数据直接使用vue的语法，不需要跟wx中一样，
   写this.setData({}),这样更新数据

   mpvue不仅支持vue的生命周期，也支持小程序的生命周期，

## 传递值，传参
   在onLoad周期中的参数options中获取其他页面传过来的值  -- 注意：这个on*L*oad中L 大写

## 解析html代码

   微信小程序可以使用wxParse解析html

   在mpvue中，最新版本的是可以解析html的，所以直接在页面中使用v-html就可以了；会解析成rich-text，没法控制样式
   
   对于稍微复杂的文本解析，需要插件 http://github.com/htzhanglong/mpvue-wxParse
   1.安装 npm i mpvue-wxparse --save；
   2.在用到的页面中引入  import  wxParse from ‘mpvue-wxparse’
   3.注册组件
     components:{
	wxParse
     }
   4.使用组件解析html
   ```
    <wxParse :content="article" @preview="preview" @navigate="navigate" />
    ```
   5.引入wxparse的css
     ```
    <style>
  
      @import url("~mpvue-wxparse/src/wxParse.css");
    </style>
    ```

   


   

