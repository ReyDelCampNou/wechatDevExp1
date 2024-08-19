# 2022年夏季《移动软件开发》实验报告

<center>姓名：陈正元  学号：22020007159</center>

| 姓名和学号？      | 陈正元，22020007159                                |
| ----------- | ---------------------------------------------- |
| 本实验属于哪门课程？  | 中国海洋大学22夏《移动软件开发》                              |
| 实验名称？       | 实验1：第一个微信小程序                                   |
| 博客地址？       | 无                                              |
| Github仓库地址？ | https://github.com/ReyDelCampNou/wechatDevExp1 |

（备注：将实验报告发布在博客、代码公开至 github 是 **加分项**，不是必须做的）

## **一、实验目标**

1、学习使用快速启动模板创建小程序的方法；2、学习不使用模板手动创建小程序的方法。

## 二、实验步骤

1. 在微信官网上注册开发者信息，并进行实名认证、邮箱验证、微信绑定等步骤。下图为完成后的状态。
   
   ![](C:\Users\chen\AppData\Roaming\marktext\images\2024-08-19-16-13-14-image.png)

2. 下载开发者工具，完成安装
   
   ![](C:\Users\chen\AppData\Roaming\marktext\images\2024-08-19-16-21-54-image.png)

3. 创建项目并修改组件，添加一个按钮
   
   最初按照实验手册使用的按钮：
   
   ```html
   <view class="userinfo-button">
         <button class="userinfo-button" open-type="getUserInfo" bindgetuserinfo="setUserProfile"> 获取用户信息 </button>
   </view>
   ```
   
   但是发现返回的都是默认灰色头像以及默认昵称“微信用户”，经过查找发现微信小程序在2021年4月13日起已经不再支持 `open-type="getUserInfo"` 的方式获取用户信息。于是按照文档[开发指引 / 低码开发 / 小程序开发实践 / 获取用户信息 (qq.com)](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/practice/getuserinfo.html#%E6%AD%A5%E9%AA%A42%EF%BC%9A%E5%88%9B%E5%BB%BA%E7%94%A8%E6%88%B7%E4%BF%A1%E6%81%AF%E8%8E%B7%E5%8F%96%E6%96%B9%E6%B3%95)中的方式更改了按钮：
   
   ```html
   <view class="userinfo-button">
       <button class="userinfo-button" bindtap="setUserProfile"> 获取用户信息 </button>
   </view>
   ```
   
   同时更改了绑定的函数：
   
   ```javascript
   setUserProfile: function () {
     const that = this;
     wx.getUserProfile({
       desc: '展示用户信息',
       success: (res) => {
         console.log(res)
         that.setData({
           userInfo: res.userInfo,
           hasUserInfo: true
         });
       },
       fail: (res) => {
         console.log(res);
       },
     });
   },
   ```
   
   但是仍然发现返回值为默认值，如图：
   
   ![](C:\Users\chen\AppData\Roaming\marktext\images\2024-08-19-16-35-35-image.png)
   
   之后又经过查找，在官方文档[小程序用户头像昵称获取规则调整公告 | 微信开放社区 (qq.com)](https://developers.weixin.qq.com/community/develop/doc/00022c683e8a80b29bed2142b56c01)中发现此方法也已经过时，最后通过调整基础库解决。（最后为了兼容一些新的特性采用了2.26.2版本的基础库）
   
   ![屏幕截图 2024-08-19 154556.png](C:\Users\chen\Pictures\Screenshots\屏幕截图%202024-08-19%20154556.png)
   
   ![](C:\Users\chen\AppData\Roaming\marktext\images\2024-08-19-16-41-38-image.png)
   
   ![](C:\Users\chen\AppData\Roaming\marktext\images\2024-08-19-16-41-56-image.png)
   
   之后通过修改css进行美化。

## 三、程序运行结果

![](C:\Users\chen\AppData\Roaming\marktext\images\2024-08-19-16-54-49-image.png)

![](C:\Users\chen\AppData\Roaming\marktext\images\2024-08-19-16-55-08-image.png)

![](C:\Users\chen\AppData\Roaming\marktext\images\2024-08-19-16-55-26-image.png)

## 四、问题总结与体会

遇到的问题在实验步骤中已经写出。本次实验让我体会到了兼容与文档的重要性，官方文档应该标出相应属性的废弃版本，而微信在这方面还需要完善，如：[表单组件 / button (qq.com)](https://developers.weixin.qq.com/miniprogram/dev/component/button.html)中的getUserInfo词条，只标出了最低版本，却没有写明最新版本不能使用。