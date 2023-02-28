# Vite创建Vue3项目

> 1.npm create vite@latest
>
> 2.vite中安装@vitejs/plugin-vue-jsx -D插件可以在vue中使用jsx语法
>
> 3.如果npm安装出现问题，可以使用yarn安装插件，npm i -g yarn安装yarn
>
> 4.安装好插件后在vite.config.js中配置插件

# 一：Vite创建Vue2项目

##     第一种方式

> ​	1.npm create vite@latest, 选择原生Vanilla
>
> ​	2.安装@vitejs/plugin-vue2 -D， 创建vite.config.js，将其配置在里面
>
> ​	3.安装vue2
>
> ​	4.手动创建缺少的目录并调整文件引用关系，如src

## 	第二种方式

> ​	在[awesome-vite](https://github.com/vitejs/awesome-vite#plugins) 中使用第三方的模板，即需要下载



# Vite中可以使用CSS的各项功能

>  如，原生CSS variable，postcss，cssModule，css pre-processors(SASS,LESS---css预处理工具)，@import alias。



# 二：Vite高级应用

### 2-1：HMR热更新

> 1. 在if语句中，render必须使用newModule调用，不能直接render(), 因为函数闭包的特性，直接调用render，执行的还是上次的render。
> 2. HMR过程和webSocket有关，服务端发现main.js的变化，主动告诉前端，前端再次发送请求，将请求过来的新的main.js替换掉老的main.js

```js
export function render(){
    document.querySelector('#app').innerHTML = `
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript" target="_blank">
      <img src="${javascriptLogo}" class="logo vanilla" alt="JavaScript logo" />
    </a>
    <h1>Hello Vite!11111221</h1>
    <div class="card">
      <button id="counter" type="button"></button>
    </div>
    <p class="read-the-docs">
      Click on the Vite logo to learn more
    </p>
  </div>
`
}

render()

if(import.meta.hot) {
    import.meta.hot.accept(newModule=>{
        newModule.render()
    })
}
```



### 2-2：Vite的glob-import批量导入功能

> 通过一组类似正则的表达式来引入一组文件, vite提供的glob功能来自第三方库fast-glob，此功能兼容性差，在其他编译器可能报错。

```js
// 将全部模块转化成一个函数，再异步import引入。
const globModules = import.meta.glob('./glob/*')
// 一开始编译代码时引入
const globModules = import.meta.globEager('./glob/*')
```

### 2-3：Vite预编译

> 1. 在第一次启动项目前，vite会将所依赖的包进行预编译并放在cache里，之后项目中用到的包会直接到cache中去取，不需要再次请求服务端。
> 2. Vite依赖于浏览器原生的ESM的加载方式去加载文件，所以Vite会将CJS编译成ESM，Vite的配置文件中进行修改，热更新也会响应。
> 3. Vite会将零散的文件打包在一起，减少请求次数，提高性能。如，引入lodash时，不编译和编译的区别。
> 4. Vite的配置中optimizeDeps用来指定哪些需要预编译，那些不需要。

### 2-4: 非node服务中集成vite项目（针对于老项目，服务端解析模板引擎，返回html）

> 1. 关注点：如何让js嵌入到html里面成为标签，即，引入路径的问题。
>
> 2. 解决：
>
>    （1）开发环境：把前端项目的所有代码放在服务端的一个子目录下面，在服务端做一个映 	射，将对一些资源的请求映射到该子目录下。
>
>    （2）生产环境：在vite配置文件中将manifest设为true，接下来将打包好的dist下的manifest中引入的文件写入到模板引擎中，再做一个请求资源的映射，这样就可以正确引用js。
>
> 

### 2-5：nodejs集成vite开发时的SSR

> vite本身是一个nodejs项目，可以再nodejs中直接调用vite。vite middleware处于ssr module，在其下直接返回html无法真正的完成App的渲染。一般来说所有的框架对服务端渲染的支持，都时有一些独特的内容需要处理的。serve-entry对vite的ssr有专门的一套规范，开发阶段，ssr集成。
