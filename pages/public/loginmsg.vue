<template>
	<view class="container">
		<!-- <view class="cu-form-group" style="margin: 30upx;border: 2upx solid whitesmoke;margin-bottom: 20px;border-radius: 30px">
			<view class="title">手机号</view>
			<input type="number" :value="mobile" placeholder="请输入手机号" maxlength="11" data-key="mobile" @input="inputChange" />
		</view>
		 <view class="cu-form-group" style="margin: 30upx;border: 2upx solid whitesmoke;margin-bottom: 20px;border-radius: 30px">
		 	<text class="title">验证码</text>
		 	<input type="number" :value="code" placeholder="请输入验证码" maxlength="6" data-key="code" @input="inputChange"
		 	 @confirm="toLogin" />
		 	<button class="send-msg" @click="sendMsg" :disabled="sending">{{ sendTime }}</button>
		 </view> -->
		 
		 <view class="input-content">
		 	<view class="cu-form-group"
		 		style="border: 2upx solid whitesmoke;margin-bottom: 20px;border-radius: 30px">
		 		<view class="title text-black">手机号</view>
		 		<input type="number" :value="mobile" placeholder="请输入手机号" maxlength="11" data-key="mobile"
		 			@input="inputChange" />
		 	</view>
		 	<view class="cu-form-group" style="border: 2upx solid whitesmoke;border-radius: 30px">
		 		<view class="title text-black">验证码</view>
		 		<input type="number" placeholder="请输入验证码" maxlength="6" :value="code" data-key="code"
		 			@input="inputChange" @confirm="toLogin" />
		 		<button class="send-msg" @click="sendMsg" :disabled="sending">{{ sendTime }}</button>
		 	</view>
		 </view>
		 
		<button class="confirm-btn" @click="toLogin" :disabled="logining">登录</button>
	</view>
	</view>
</template>

<script>
	import listCell from '@/components/com-input';
	export default {
		components: {
			listCell
		},
		data() {
			return {
				mobile: '',
				code: '',
				logining: false,
				sending: false,
				sendTime: '获取验证码',
				count: 60,
			}
		},

		methods: {
			inputChange(e) {
				const key = e.currentTarget.dataset.key;
				this[key] = e.detail.value;
			},
			navBack() {
				uni.navigateBack();
			},
			countDown() {
				const {
					count
				} = this;
				if (count === 1) {
					this.count = 60;
					this.sending = false;
					this.sendTime = '获取验证码'
				} else {
					this.count = count - 1;
					this.sending = true;
					this.sendTime = count - 1 + '秒后重新获取';
					setTimeout(this.countDown.bind(this), 1000);
				}
			},
			sendMsg() {
				const {
					mobile
				} = this;
				if (!mobile) {
					this.$queue.showToast("请输入手机号");
				} else if (mobile.length !== 11) {
					this.$queue.showToast("请输入正确的手机号");
				} else {
					this.$queue.showLoading("正在发送验证码...");
					this.$Request.getT('/app/Login/sendMsg/' + mobile + '/gzg').then(res => {
						if (res.code === 0) {
							this.sending = true;
							this.$queue.showToast('验证码发送成功请注意查收');
							this.countDown();
							uni.hideLoading();
						} else {
							uni.hideLoading();
							uni.showModal({
								showCancel: false,
								title: '短信发送失败',
								content: res.msg ? res.msg : '请一分钟后再获取验证码'
							});
						}
					});
				}
			},
			toLogin() {
				const {
					mobile,
					code
				} = this;
				let userId = this.$queue.getData("userId");
				if (!mobile) {
					this.$queue.showToast("请输入手机号");
				} else if (mobile.length !== 11) {
					this.$queue.showToast("请输入正确的手机号");
				} else if (!code) {
					this.$queue.showToast("请输入验证码");
				} else {
					this.$queue.showLoading("正在登录中...");
					let openId = this.$queue.getData('openid') ? this.$queue.getData('openid') : '';
					let invitation = this.$queue.getData('inviterCode') ? this.$queue.getData('inviterCode') : '';
					this.$Request.post(`/app/Login/registerCode`, {
						phone: mobile,
						openId: openId,
						inviterCode: invitation,
						msg: code
					}).then(res => {
						if (res.code === 0) {
							this.$queue.setData("token", res.token);
							this.$queue.setData('userId', res.user.userId);
							this.$queue.setData('userName', res.user.userName);
							this.$queue.setData('phone', res.user.phone);
							this.$queue.setData('avatar', res.user.avatar?res.user.avatar: '../../static/logo.png');
							this.$queue.showToast('登录成功');
							setTimeout(function() {
								uni.switchTab({
									url:'/pages/index/index'
								})
							},1000)
						} else {
							uni.showModal({
								showCancel: false,
								title: '登录失败',
								content: res.msg,
							});
						}
						uni.hideLoading();
					});
				}
			},
		},

	}
</script>

<style lang='scss'>

	.send-msg {
		border-radius: 30px;
		color: white;
		height: 30px;
		font-size: 14px;
		line-height: 30px;
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
	}

	.container {
		top: 0;
		padding-top: 32upx;
		position: relative;
		width: 100%;
		height: 100%;
		overflow: hidden;
		background: #FFFFFF !important;
	}
	
	.wrapper {
		position: relative;
		z-index: 90;
		background: #FFFFFF;
		padding-bottom: 32upx;
	}
	
	
	.input-content {
		/* margin-top: 300upx; */
		/* padding-top: 300upx; */
		padding: 32upx 80upx;
	
	}
	
	
	.confirm-btn {
		width: 600upx;
		height: 80upx;
		line-height: 80upx;
		border-radius: 60upx;
		margin-top: 32upx;
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
		color: #fff;
		font-size: 32upx;
	
		&:after {
			border-radius: 60px;
		}
	}


</style>
