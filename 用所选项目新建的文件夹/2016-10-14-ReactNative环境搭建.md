---
layout: post
#mac react_native环境搭建
title:  react_native环境搭建
#时间配置
date:   2016-10-14 10:58:50 +0800
#大类配置
categories: 环境配置
#小类配置
tag: ReactNative
---

* content
{:toc}

**相关链接：**<br>
* <a href="https://reactnative.cn/docs/0.51/getting-started.html" target="_blank">React Native 中文网</a><br>
* <a href="https://www.zhihu.com/question/27852694" target="_blank">如何评价 React Native？</a><br>

> 前端工程师进行安卓与iOS的开发

### 配置环境
---

#### 安装必须的软件

##### Homebrew :
> /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

`如果遇到/usr/local目录不可写权限问题，可以使用下面的命令修复：`
> sudo chown -R `whoami` /usr/local

##### > Node :
`使用Homebrew来安装Node.js`
> brew install node

![752372-20161014103757687-344136098.png]({{ site.img_url }}04DB210E2446570A4F69C3D2AE4BD4F4.png)

`此时可使用npm命令`

![752372-20161014105010062-821324237.png]({{ site.img_url }}71BD17054F75C014896D59E100145DD3.png)


##### > React Native的命令行工具(react-native-cli)

> npm install -g react-native-cli

![752372-20161014105457796-1545664247.png]({{ site.img_url }}D376BAAECCFBBA48419E01F7C6E8A97A.png)

`运行项目：`

```shell
react-native init AwesomeProject  // 初始化项目
cd AwesomeProject  // 进入项目
react-native run-iso // 运行项目
```