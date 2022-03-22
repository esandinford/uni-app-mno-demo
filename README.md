## 概述
APP端进行一键获取本机号码，支持Android,IOS，快速，稳定，用户体验好，可客制化认证界面，比短信更可靠，更便宜，可用于APP登录，注册等场景

## 演示视频
![](http://open.esandcloud.com/share/index.php/s/VqA9CVMS9QJHux5/download)

## 插件API接口说明

```js
/**
 * 获取手机号码
 * @param secretKey SDK授权密钥，和ios的bundleId/android的签名绑定，可向一砂索取,Android和IOS的密钥不一样
 * @param authUIConfigJson 登录授权⻚UI⾃定义配配置, 可参考Android/IOS授权界面客制化
 * @param mnoCallback 异步结果回调ret, 有三个字段：code, msg, data
        - code字段是执行状态码可能的值及描述：
            "0": 客户端成功
            "1": 客户端失败（预取号，token没取到，其他本地错误）
            "2": 用户取消操作
            "3": 切换登录方式
            "4": 超时
            "5": 网络异常，请检查本机是否插入Sim卡或者蜂窝网络是否打开了
        - msg字段是结果的描述，给程序员调试看的
        - data字段是执行结果(json字符串)，内容如下：
               {
                  "deviceModel": "",
                  "packageId": "",
                  "platform": "",
                  "token": "",
                  "transId": "",
                  "appName": ""
              }
 */
MNOModule.getPhoneNum(secretKey, authUIConfigJson, mnoCallback)
```

## 其他信息
1. 完整接入文档：[https://esandinfo.yuque.com/yv6e1k/caebif](https://esandinfo.yuque.com/yv6e1k/caebif)
2. 服务器端协议文档：[https://market.aliyun.com/products/57124001/cmapi00046021.html#sku=yuncode4002100001](https://market.aliyun.com/products/57124001/cmapi00046021.html#sku=yuncode4002100001)
3. 后端管理控制台地址: [http://openali.esandcloud.com](http://openali.esandcloud.com)
4. 技术支持/定制化开发 （定制化开发不收钱）
5. 服务看开通地址：[https://market.aliyun.com/products/57126001/cmapi00044151.html#sku=yuncode3815100001](https://market.aliyun.com/products/57126001/cmapi00044151.html#sku=yuncode3815100001)
```
微信：esand_info
qq: 3626921591
电话：13691664797
邮箱：ruide.li@esandinfo.com
```
![wechatqrcode](http://open.esandcloud.com/share/index.php/s/hzT4Gb0BN81svae/download)
