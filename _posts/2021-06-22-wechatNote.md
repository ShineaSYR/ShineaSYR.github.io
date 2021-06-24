---
layout: post
title: 企业微信定时提醒机器人🤖️
categories: [plugin]
description: 通过云函数及企业微信机器人，创建定时任务，定时发送消息提醒
keywords: 云函数, serverless、企业微信
---

## 前期准备
- 需要有一个企业微信的群（3人以上），主要用于拿到WebHook；
- 腾讯云 or 阿里云的账号，此处主要以腾讯云进行说明～
## 创建企业微信机器人🤖️
步骤如下：

- 在PC版本的企业微信，鼠标右键出现「添加群机器人」
- 出现弹窗，可以选择使用已有机器人or新建机器人，点击最上方「新创建一个机器人」

![添加机器人]({{ site.url }}/assets/images/wechat_webhook001.png)
- 最后在群里会出现一个机器人，显示WebHook地址为`https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=XXXX-XXXX`（PS：此处为key为安全用X简单示意，非真实，WebHook比较关键，不要随意泄露）

![成功创建机器人]({{ site.url }}/assets/images/wechat_webhook002.png)
## 配置云函数
### 1.登录[腾讯云](https://cloud.tencent.com/)

此处默认大家已有腾讯云账号（若无，直接使用微信登录+实名认证+服务分配即可）

### 2.创建一个函数服务
进入[云函数模块](https://serverless.cloud.tencent.com/start?c=scf)，选择一个函数模板，创建一个函数服务。

![选定一个函数模板]({{ site.url }}/assets/images/wechat_webhook003.png)
### 3.函数服务配置
基础配置可按照提示进行修改，点击完成，函数创建成果，此时会默认部署默认的HelloWorld代码，并跳转到工作台。

### 4.函数服务-添加代码
点击左侧的【函数管理】，选择函数代码tab选项卡，里面出现了代码编辑区域，可以看到之前默认的代码为“已部署”状态。

![添加代码]({{ site.url }}/assets/images/wechat_webhook004.png)
在代码编辑区域添加如下代码
```javascript
'use strict'
/**************************************************
 demo-show 说明
 postData里的content需要修改为自己需要的
 request里的url需要填写之前创建的机器人对应的WebHook
 ***************************************************/
const request = require('request')

exports.main_handler = async (event, context, callback) => {

  return new Promise((resolve, reject) => {
    const date = dateFormat({
      time: +new Date(),
      format: 'yyyymmdd'
    })
    const postData = {
      "msgtype": "text",
      "text": {
        "content": "又是学习知识的一天\n大家可以访问链接:https://shineasyr.github.io/:",
        "mentioned_list":["@all"]
      }
    }
    request({
      url: '创建的机器人的WebHook',
      method: 'POST',
      headers: {
        "content-type": "application/json",
      },
      body: JSON.stringify(postData)
    }, function(err, res) {
      if (!err && res.statusCode == 200) {
        resolve('success')
      } else {
        reject(err)
      }
    });

  })

}

function dateFormat (options) {
  // options支持两个参数
  // format，返回字符串的格式,"yyyy/mm/dd hh:ii",
  // 除了其中的字母之外，标点，分隔符等，可以随意修改
  // time为一个有效的时间，可以是字符串，可以是对象
  // 如果只传入一个参数，那么该参数为format的值

  const format = options.format
  const t = new Date(options.time)
  const tf = num => (num < 10 ? '0' : '') + num

  return format.replace(/yyyy|mm|dd|hh|ii|ss/g, function (a) {
    switch (a) {
      case 'yyyy':
        return tf(t.getFullYear())
      case 'mm':
        return tf(t.getMonth() + 1)
      case 'dd':
        return tf(t.getDate())
      case 'hh':
        return tf(t.getHours())
      case 'ii':
        return tf(t.getMinutes())
      case 'ss':
        return tf(t.getSeconds())
    }
  })
}
```
添加后点击部署，代码即可部署成功，可以点击测试按钮，此时群里会有一个消息提示。
### 5.函数服务创建触发器。
点击左侧的【触发管理】-【创建触发器】，在弹出框里修改相应设定，一般触发周期选择「自定义触发周期」，填写「Cron表达式」

![创建触发器]({{ site.url }}/assets/images/wechat_webhook005.png)

#### Cron表达式举例
- `0 30 15 * * WED *`为每周三的15:30点触发，其中第一位0代表秒，30代表分钟，15代表时（24时计时法），WED为周三
- `*/5 * * * * * *` 表示每5秒触发一次
- `0 0 2 1 * * *` 表示在每月的1日的凌晨2点触发
- `0 0 10,14,16 * * * *` 表示在每天上午10点，下午2点，4点触发

更多关于Cron的说明，可以查看[文档](https://cloud.tencent.com/document/product/583/9708#cron-.E8.A1.A8.E8.BE.BE.E5.BC.8F)




