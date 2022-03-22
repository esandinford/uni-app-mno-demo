<template>
	<view class="content" align="center">

		<view align="center">
			<image class='esand_logo' src="../../static/esand_logo.png"></image>
		</view>

		<view class="btn-row">
			<button v-on:click="getPhoneNumber()">一键登录</button>
		</view>
		<div align="center">
			<textarea :value="msg" />
		</div>
	</view>
</template>

<script>
		var MNOModule = uni.requireNativePlugin("Esand-MNOModule")
		var secretKey;
		var authUIConfigJson = "{\n" +
                "    \"prefersStatusBarHidden\":true,\n" +
                "    \"logoImgPath\":\"cscs\"\n" +
                "}"
	export default {
		data() {
			return {
				msg: "日志"
			}
		},
		methods: {
			getPhoneNumber: function(e) {
				let platform=uni.getSystemInfoSync().platform
				if(platform=='ios'){
					authUIConfigJson = "{\"prefersStatusBarHidden\":true,\"logoImage\":\"logo\",\"checkBoxIsChecked\":true,\"logoWidth\":10.11123,\"logoHeight\":11.11,\"logoTopOffetY\":10.123,\"numberColor\":\"0x0000FF\",\"navTitle\":\"cscscscscs\",\"navTitleColor\":\"0x0000FF\",\"navTitleFont\":20}"
					// 密钥是和APP绑定的，需要向服务提供商申请，可加微信：esand_info, ANDROID和IOS的密钥不一样
					secretKey = "0wX1qyA0sPZh/sbTZPPGD8ZjlS2iVVVN+q/rLJ3H4brZvvuhUtD9NbJ+X0yB8RFlb0PJ+irtuW4B/WEOQcHrqO0vFvjV9DXotA7CgVi5YNyDp1RfevGNB5AZ6gdC7eWvv9POYC16F8MmvFS1Qh1Fno86d34zgjwxZkf1vpOJQKoMqNvYWkWkkeK+VeIxWb13IJ/mzYF5DPGxoZV/Zd2jZGy8ci8y4evlOMYczAjpfZAXOMCfylWytLuVbX0lQC71cCbdmzf2pBc="
				} else if(platform=='android') {
					let authUIConfig = {}
					authUIConfig.navColor = "0x00000000"
					authUIConfig.navTextColor = "0xFF000000"
					authUIConfig.isLightColor = true
					authUIConfig.navReturnImgWidth
					authUIConfig.navReturnImgWidth = 206
					authUIConfig.navReturnImgHeight = 20
					authUIConfig.navReturnScaleType = 6
					authUIConfig.navReturnImgPath = "back"
					authUIConfig.logoImgPath = "static/wulianwang-.png"
					authUIConfigJson = JSON.stringify(authUIConfig)
					// 这里客制化一键登录授权页面,详细可参考: https://esandinfo.yuque.com/books/share/5ddd649d-2afa-48e6-bb07-633105dfec88?#
					// authUIConfigJson = "{\"privacyState\":true,\"logoImgPath\":\"cscs\",\"navHidden\":true,\"iconLayoutOffSetY\":600,\"iconLayoutWidth\":150}"
					// 密钥是和APP绑定的，需要向服务提供商申请，可加微信：esand_info, ANDROID和IOS的密钥不一样
					secretKey = "fJutUWfS11a/PORxlijcowb+T1b1SrxGC4HqAvyv1q5ziqxK1810gJO8Jn2t0sAnAedRCDWuqRaEtL5LPo8te1JgaQhjf1oR3ByguahdedhWD8wbAdSlRqdLNIChpi584v/zEmU+o3exWlEuBqTnsLJBllR33AAO5MqdDXD+ct9tLKDCP+dRCPF6SKAhmkjT9KpFkAt+L3gcUYVolR51sFOIXYB/6TTWLdLbGbL4t0m1yvhBT7XF28Wug8I08MfcYSfXwMsmm3vajqkK4RqXN05ds+6APhAEXX0bKvRZQj5pKblW3xKM8A=="
				}
				MNOModule.getPhoneNum({
					'secretKey': secretKey,
					'authUIConfigJson': authUIConfigJson

				}, (ret) => {
					if (ret.code == '0') {
						let dataBody = ret.data
						let dataBodyJson = JSON.parse(dataBody)
						uni.request({
							url: 'http://edismno.market.alicloudapi.com/mno/getMobile',
							method: 'POST',
							data: {
								'deviceModel': dataBodyJson.deviceModel,
								'packageId':dataBodyJson.packageId,
								'platform':dataBodyJson.platform,
								'token':dataBodyJson.token,
								'transId':dataBodyJson.transId,
								'appName':dataBodyJson.appName,
								'phoneNum':dataBodyJson.phoneNum
							},
							header: {
								// APPCODE是阿里云网关密钥，不建议直接在APP端直接请求阿里云网关，因为会泄漏APPCODE, 可以把这段逻辑写到服务器端，然后再请求服务器
								'Authorization': 'APPCODE 填入您自己的APPCODE',  // appcode的获取可参考： https://esandinfo.yuque.com/docs/share/13ad611e-b9c3-4cf8-a9a8-fe23a419312e?# 《如何获取APPCODE/APPKEY&APPSECRET》
								'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8'
							},
							success: (res) => {
								res.header
								var rspMsg = JSON.stringify(res.data)
								console.log('网络请求成功' + rspMsg)
								this.msg = JSON.stringify(rspMsg);
								uni.showModal({
									title: "获取结果成功",
									content: JSON.stringify(res.data),
								})
							}
						})
					}else{
						console.log(ret)
						this.msg = JSON.stringify(ret);
					}
				})
			},
		}
	}
</script>


<style>
    .content {
        position: absolute;
        width: 100%;
        height: 100%;
    }

    .esand_logo {
        margin-top: 100rpx;
        margin-bottom: 100rpx;
        width: 200rpx;
        height: 200rpx;
        align-self: center;
    }

    .item {
        margin-bottom: 15rpx;
        margin-top: 15rpx;
        width: 400rpx;
    }

    img {
        position: relative;
        margin-top: 50rpx;
        width: 600rpx;
        height: 400rpx;
    }

    label {
        display: block;
        margin-top: 10rpx;
    }

    textarea {
        margin-top: 20rpx;
        height: 400rpx;
    }
</style>
