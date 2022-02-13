<template>
	<view>
		<view>
			<form @submit="formSubmit">
				<view class="uni-form-item uni-column">
					<view class="title">账号</view>
					<input class="uni-input" name="user" placeholder="学工号或网关账号昵称" />
				</view>
				<view class="uni-form-item uni-column">
					<view class="title">密码</view>
					<input class="uni-input" name="pwd" placeholder="网关密码" />
				</view>

				<view class="uni-form-item uni-column">
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
				<view class="uni-form-item uni-column">
					<view class="title">漫游免认证（非校园网包月不要勾选）</view>
					<view>
						<switch name="isMac" />
					</view>
				</view>
				<view class="uni-form-item uni-column">
					<view class="title">注：登录配置将保存在您的设备上</view>
				</view>

				<view class="uni-btn-v">
					<button form-type="submit">确定</button>
				</view>
			</form>
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
				if(formdata.isMac){
					formdata.mac = '0'
				}
				if(formdata.loginType == 'cmcc'){
					formdata.user += '%40zgyd';
				}
				uni.setStorageSync("accInfo", JSON.stringify(formdata));
				uni.navigateTo({
					url: 'login'
				});
			},
		}
	}
</script>

<style>
	.uni-form-item .title {
		padding: 20rpx 0;
	}
</style>
