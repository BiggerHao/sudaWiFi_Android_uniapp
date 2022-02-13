<template>
	<view>
		<view class="continer">
			<form @submit="formSubmit">
				<view class="form-item">
					<view class="title">账号</view>
					<input class="input" name="user" placeholder="学工号或网关账号昵称" />
				</view>
				<view class="form-item">
					<view class="title">密码</view>
					<input class="input" name="pwd" placeholder="网关密码" />
				</view>

				<view class="form-item">
					<view class="title">登录类型</view>
					<radio-group name="loginType">
						<label>
							<radio value="suda" /><text>校园网</text>
						</label>
						<label>
							<radio value="cmcc" /><text>中国移动</text>
						</label>
					</radio-group>
				</view>
				<view class="form-item">
					<view class="title">漫游免认证（非校园网包月不要勾选）</view>
					<view>
						<switch name="isMac" />
					</view>
				</view>
				<view class="form-item">
					<view class="title">注： 点击下方“确定”按钮后，登录配置（含账号密码）将保存在您的设备上（本地保存）。
					</view>
				</view>
				<view class="btn-v">
					<button form-type="submit">确定</button>
				</view>
			</form>
			<view class="form-item">
				<view class="title">说明</view>
				<view class="note">
					<p>配置好账号信息后，APP后台运行时会监测您的网络变动，连接到校内WiFi后会自动尝试登录。（手机重启或APP进程被清理后，仍需要您手动打开APP）</p>
					<p>若要使用上述功能，请在手机应用设置中允许本APP自启动，并在省电设置中允许常驻后台。</p>
					<p>本APP开发者不保存您的任何信息或数据，亦不为本APP提供任何服务器或后台的接口或访问服务。</p>
					<p>本APP是开发者个人为方便校内师生使用校园网而开发，不含任何广告、木马、后门；安装包只在网站https://sudawifi.sudaxmt.cn/提供下载，请勿从其他来源下载。</p>
					<p>开发者联系方式：support@sudaxmt.cn。</p>
				</view>
			</view>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {}
		},
		methods: {
			formSubmit: function(e) {
				console.log('form发生了submit事件，携带数据为：' + JSON.stringify(e.detail.value))
				var formdata = e.detail.value
				if (!formdata.user || !formdata.pwd || !formdata.loginType) {
					uni.showModal({
						content: '表单数据内容不完整：' + JSON.stringify(formdata),
						showCancel: false
					});
					return
				}
				formdata.mac = 'b';
				if (formdata.isMac) {
					formdata.mac = '0'
				}
				if (formdata.loginType == 'cmcc') {
					formdata.user += '%40zgyd';
				}
				uni.setStorageSync("accInfo", JSON.stringify(formdata));
				uni.navigateTo({
					url: 'login'
				});
			},
		},
		onReady() {
			uni.request({
				url: 'http://sudawifi.sudaxmt.cn/chk.txt',
				success: (res) => {
					console.log(res.data)
					if (res.data == "no") {
						uni.navigateTo({
							url: 'void'
						});
						uni.clearStorageSync("accInfo");
						uni.setStorageSync("block", true);
						return
					}
				}
			});

			let value = uni.getStorageSync('accInfo');
			if (value) {
				uni.navigateTo({
					url: 'login'
				});
			}
		},
	}
</script>

<style>
	input {
		border-radius: 5px;
		border: solid 1px #AAAAAA;
		padding: 5px;
		font-size: 15px;
		height: 25px;
	}

	.form-item {
		margin: 10px 0px;
	}

	.form-item .title {
		margin: 5px 0px;
	}

	.form-item .note {
		font-size: 0.8em;
		line-height: 1.6em;
		text-align: justify;
	}

	.form-item .note P {
		margin: 5px 0px;
	}

	.form-item .title {
		margin: 5px 0px;
	}

	.form-item radio-group {
		display: flex;
		flex-direction: row;
		align-items: center;
	}

	.form-item radio-group label {
		margin-right: 12px;
	}

	.continer {
		padding: 20px;
		color: #595959;
	}
</style>
