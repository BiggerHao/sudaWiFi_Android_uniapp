<template>
	<view>
		<web-view v-bind:src="webViewSrc"></web-view>
		<div class="cover" v-on:click="gomodify()">修改登录配置</div>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				webViewSrc: "http://a.suda.edu.cn/",
				url: 'false'
			}
		},
		async onShow() {
			let value = uni.getStorageSync('accInfo');
			if (value) {
				this.accInfo = value;
			} else {
				return
			}
			var formData = JSON.parse(value);
			console.log(formData);


			let url =
				`http://a.suda.edu.cn:801/eportal/?c=Portal&a=login&callback=dr1003&login_method=1&user_account=%2C${formData.mac}%2C${formData.user}&user_password=${formData.pwd}&wlan_user_ip=&wlan_user_ipv6=&wlan_user_mac=&wlan_ac_ip=&wlan_ac_name=&jsVersion=3.3.3&v=1900`
			this.url = url;
			this.req(url);

		},
		onReady() {
			var that = this;

			uni.onNetworkStatusChange(function(res) {
				console.log('uni.onNetworkStatusChange')
				console.log(res.isConnected);
				console.log(res.networkType);
				that.req(that.url)
			});
			setInterval(this.req(this.url), 1000 * 60 * 15);
		},
		methods: {
			gomodify: function() {
				uni.navigateTo({
					url: 'modify'
				});
			},
			getIP: function() {
				return new Promise(function(resove, reject) {
					uni.request({
						url: 'http://a.suda.edu.cn/',
						success: (res) => {
							resove(res.data)
						},
						fail: (err) => {
							uni.showModal({
								content: JSON.stringify(err),
								showCancel: false
							});
							reject({
								err: true,
								errMsg: err
							})
						}
					});
				})

			},
			req: async function(url) {
				let isblock = uni.getStorageSync('block');
				if (isblock) {
					return
				}
				var body = await this.getIP();
				if (body.indexOf("您已经成功登录") > -1) {
					return
				}
				let findStr = "v46ip='"
				let p = body.indexOf(findStr);
				body = body.substr(p + findStr.length, 100);
				p = body.indexOf("'");
				var ip = body.substr(0, p);
				console.log(ip);
				url = url.replace('wlan_user_ip=', 'wlan_user_ip=' + ip);
				uni.request({
					url,
					success: (res) => {
						this.webViewSrc = 'http://a.suda.edu.cn/?' + Date.parse(new Date());
						console.log(res)
						return {
							err: false,
							res
						}
					},
					fail: (err) => {
						uni.showModal({
							content: JSON.stringify(err),
							showCancel: false
						});
						return {
							err: true,
							errMsg: err
						}
					}
				});
			}
		}
	}
</script>

<style>
	.cover {
		z-index: 999;
		position: absolute;
		text-align: center;
		bottom: 20px;
		width: 100vw;
		cursor: pointer;
		color: #6641E2;
	}
</style>
