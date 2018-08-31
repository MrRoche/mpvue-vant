# 介绍

`mpvue-vant`记录了我们团队开发中在`mpvue`中使用`Vant Weapp`组件库所踩下的坑，在这里分享给大家，让`mpvue`开发者可以使用`vant`组件库进行开发，避免踩不必要的坑。


此教程是在[dov-yih](https://github.com/dov-yih)一同协助下完成。经过测试，[Vant Weapp](https://youzan.github.io/vant-weapp/#/intro)下所有组件都能够在`mpvue`中使用


# 使用方法

## 克隆仓库

> 注意：由于一些BUG我修改了源代码，所以请克隆本仓库代码。

```
git clone https://github.com/xxxsimons/mpvue-vant.git
```

**将仓库中的vant文件夹复制到你的项目目录static下**

## 配置webpack为vant支持ES6

```
// build/webpack.base.conf.js 的babel-loader中
      {
        test: /\.js$/,
        include: [resolve('src'), resolve('test'), resolve('static/vant')], // 添加vant文件目录
        use: [
          'babel-loader',
          {
            loader: 'mpvue-loader',
            options: {
              checkMPEntry: true
            }
          },
        ]
      },

```

## 配置eslint，忽略vant目录检查

```
// 根目录.eslintignore文件

static/vant/*/*.js //添加这一行
```


## 引入

在需要引入的页面目录下的`main.json`文件中

```
{
  "usingComponents": {
    "van-button": "/static/vant/button/index",
  }
}

```

## 使用

```
<van-button>测试</van-button>
```

# 注意事项

具体组件api文档参考[Vant Weapp](https://youzan.github.io/vant-weapp/#/intro)

## 1.使用方式

mpvue和原生小程序的方式有所不同。

### 1.1 数据绑定
	
原生小程序使用方式为

```
value="{{value}}"
```

mpvue使用方式

```
v-bind:value="value"
//或者
:value="value"
```

### 1.2 事件监听

原生小程序使用方式

```
bind:click="onClick"
```

mpvue使用方式

```
@click="onClick"
```

## 2. BUG及处理方法

### 2.1 监听名
mpvue里面无法使用`@click-icon`这样的监听名,因此如果API文档里面有出现这样的监听名，那么需要手动修改源代码。

可以改成驼峰式的监听名。

eg: 我在`field`组件中就遇到这个问题，我的做法是：

```
// static/vant/field/index.js 105行

this.triggerEvent('click-icon'); 

// 修改为:

this.triggerEvent('clickIcon');
```

### 2.2  报错

我在迁移过程中遇到如下报错信息

```
Cannot assign to read only property 'exports' of object '#<Object>' (mix require and export
```

这是`module.export`和`import`混用的原因导致的。解决办法就是修改`module.export`为`export`

eg:

```
// static/vant/utils/index.js 10行

modules.export = {
  isObj,
  isDef
}

// 修改为

export {
  isObj,
  isDef
}
```



