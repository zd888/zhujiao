<template>
	<view class="text-black ">
		<view class="bg">
			<view class=" u-flex padding justify-between" style="padding-top: 50rpx;">
				<view class="flex align-center">
					<view class="u-m-r-10">
						<image :src="avatar" style="width: 100rpx;height: 100rpx;border-radius: 100rpx;"
							@click="goNav('/pages/my/userinfo')"></image>
						<view style="position: absolute; top: 30rpx; left: 99rpx;z-index: 9;"
							v-if="isVip&&XCXIsSelect !='否'">
							<image src="../../static/images/vp.png" style="width: 25px;height:24px;"></image>
						</view>

					</view>
					<view class="u-flex-1 u-m-l-10 text-black" v-if="!isLogin">
						<view>
							<view class="u-font-18 margin-left-sm margin-top-xs text-bold">{{userName}}</view>
							<view class="u-font-15 margin-left-sm margin-top-xs">ID:{{invitationCode}}</view>
						</view>
					</view>
					<view v-else class="text-xl u-p-l-20 text-bold" @click="goLogin('/pages/public/login')">
						登录
					</view>
				</view>
				<image @click="goNav('/my/setting/index')"
					src="https://anmo5.0.xianmaxiong.com/file/uploadPath/2023/12/13/3dcde027c0364444de86dcd8bf70fcd5.png"
					style="width: 45rpx;height: 48rpx;margin-right: 10rpx;"></image>
			</view>

			<view class="flex justify-between" style="margin-top: 20rpx;margin-bottom: 30rpx;">
				<view style="flex: 1;text-align: center;" @click="goNav('/my/wallet/index')" v-if="XCXIsSelect != '否'">
					<view class="money_view">{{money}}</view>
					<view class="money_name">我的钱包</view>
				</view>
				<view style="flex: 1;text-align: center;" @click="goNav('/my/gird/shoucang')">
					<view class="money_view">{{collect}}</view>
					<view class="money_name">我的收藏</view>
				</view>
				<view style="flex: 1;text-align: center;" @click="goNav('/my/hongbao/youhuijuan')"
					v-if="XCXIsSelect != '否'">
					<view class="money_view">{{coupon}}</view>
					<view class="money_name">优惠券</view>
				</view>
			</view>


			<view class="vip_view" v-if="XCXIsSelect != '否' && hyCheck != '否'">
				<view class="flex justify-between align-center" style="height: 90rpx;">
					<view>
						<view v-if="!isVip" class="vipttl">开通会员</view>
						<view v-if="isVip" class="vipttl">会员中心</view>
						<view class="flex-sub text-26" style="color: #FAEBC9;margin-top: 4rpx;">会员可享贵族福利</view>
					</view>
					<view v-if="!isVip" class="btn-bg" @click="goNav('/my/vip/index')">立即开通
						<image src="/static/images/index/right.png" class="margin-left-xs"
							style="width: 11rpx;height: 18rpx;"></image>
					</view>
					<view v-if="isVip" class="btn-bg" @click="goNav('/my/vip/index')">会员特权
						<image src="/static/images/index/right.png" class="margin-left-xs"
							style="width: 11rpx;height: 18rpx;"></image>
					</view>
				</view>
			</view>


		</view>

		<view
			style="width: 750rpx;background: #F8F8FA;border-top-right-radius: 40rpx;border-top-left-radius: 40rpx;margin-top: -54rpx;padding-top: 40rpx;">

			<view class="box">
				<view class="flex justify-between" style="margin: 0upx 0upx 0upx 14upx;">
					<view class="flex align-center">
						<view class="box_xian"></view>
						<view style="font-size: 32upx;font-weight: bold;">我的订单</view>
					</view>
					<view class="flex align-center" @click="goNavOrder(-1)">
						<view style="font-size: 24rpx;font-family: PingFang SC;font-weight: 500;color: #999999;">查看全部
						</view>
						<image src="../../static/images/my/right_icon.png"
							style="width: 11rpx;height: 18rpx;margin-left: 10rpx;"></image>
					</view>
				</view>

				<view style="background: #FFFFFF;border-radius: 24rpx;margin-top: 20rpx;">
					<view class="flex justify-around text-center">
						<view class=" padding-tb" @click="goNavOrder(1)">
							<image src="../../static/images/my/order1.png" style="width:54upx;height: 54upx;" mode="">
							</image>
							<view class="text-sm" style="line-height: 50upx;color: #666666;">待付款</view>
						</view>
						<view class=" padding-tb" @click="goNavOrder(2)">
							<image src="../../static/images/my/order2.png" style="width: 54upx;height: 54upx;" mode="">
							</image>
							<view class="text-sm" style="line-height: 50upx;color: #666666;">待服务</view>
						</view>
						<view class=" padding-tb" @click="goNavOrder(6)">
							<image src="../../static/images/my/order4.png" style="width: 54upx;height: 54upx;" mode="">
							</image>
							<view class="text-sm" style="line-height: 50upx;color: #666666;">待评价</view>
						</view>
						<view class=" padding-tb" @click="goNavOrder(8)">
							<image src="../../static/images/my/order3.png" style="width: 54upx;height: 54upx;" mode="">
							</image>
							<view class="text-sm" style="line-height: 50upx;color: #666666;">已完成</view>
						</view>
					</view>
				</view>
			</view>

			<image src="https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/b37c8e7575c6d99cf197587893d2d4cd.png"
				class="kefu_view" @click="goChat"></image>

			<view class="box">
				<view class="flex align-center" style="margin: 0upx 0upx 0upx 14upx;">
					<view class="box_xian"></view>
					<view style="font-size: 32upx;font-weight: bold;">推荐工具</view>
				</view>

				<view class="flex text-center flex-wrap"
					style="background: #FFFFFF;border-radius: 24rpx;margin-top: 20rpx;padding: 20rpx 0rpx;">
					<view class="padding-tb-sm" @click="bindFb()" v-if="XCXIsSelect != '否'" style="width: 33%;">
						<image src="../../static/images/my/ruzhu.png" style="width: 50upx;height: 50upx;" mode="">
						</image>
						<view class="text-df" style="line-height: 50upx;">成为教练</view>
					</view>
					<!-- <view v-if="isShop == 1" class="padding-tb-sm" @click="goNav('/my/gonghui/index')"
						style="width: 33%;">
						<image src="../../static/images/my/gonghui.png" style="width: 50upx;height: 50upx;"></image>
						<view class="text-df" style="line-height: 50upx;">我的商户</view>
					</view> -->
					<view class="padding-tb-sm" @click="goNav('/pages/my/myteam')" style="width: 33%;"
						v-if="isAgency == 1 && tgCheck != '否'">
						<image src="../../static/images/my/mydl.png" style="width: 50upx;height: 50upx;" mode="">
						</image>
						<view class="text-df" style="line-height: 50upx;">我的团队</view>
					</view>
					<!-- <view class="padding-tb-sm" @click="goNav('/pages/my/myteam')" style="width: 33%;"
						v-if="isChannel == 1 && tgCheck != '否'">
						<image src="../../static/images/my/mydl.png" style="width: 50upx;height: 50upx;" mode="">
						</image>
						<view class="text-df" style="line-height: 50upx;">我的渠道</view>
					</view> -->
					<view class="padding-tb-sm" @click="goNav('/pages/my/invitationUser')" style="width: 33%;">
						<image src="../../static/images/my/myyao.png" style="width: 50upx;height: 50upx;" mode="">
						</image>
						<view class="text-df" style="line-height: 50upx;">分享好友</view>
					</view>
					<view class="padding-tb-sm" @click="goNav('/pages/my/cooperation?class=1&className=推广员')"
						style="width: 33%;" v-if="isAgency != 1 && tgCheck != '否'">
						<image src="../../static/images/my/mydl.png" style="width: 50upx;height: 50upx;" mode="">
						</image>
						<view class="text-df" style="line-height: 50upx;">成为推广员</view>
					</view>

					<!-- <view class="padding-tb-sm" @click="goNav('/pages/my/cooperation?class=2&className=渠道商')"
						style="width: 33%;" v-if="isAgency != 1 && isChannel != 1 && tgCheck != '否'">
						<image src="../../static/images/my/mydl.png" style="width: 50upx;height: 50upx;" mode="">
						</image>
						<view class="text-df" style="line-height: 50upx;">成为渠道商</view>
					</view> -->

					<view class="padding-tb-sm" @click="goNav('/my/address/address')" style="width: 33%;">
						<image src="../../static/images/my/yijian.png" style="width: 50upx;height: 50upx;" mode="">
						</image>
						<view class="text-df" style="line-height: 50upx;">地址管理</view>
					</view>
					<view class="padding-tb-sm" @click="goNav('/my/help/feedbackIndex')" style="width: 33%;">
						<image src="../../static/images/my/help.png" style="width: 50upx;height: 50upx;" mode="">
						</image>
						<view class="text-df" style="line-height: 50upx;">帮助中心</view>
					</view>
					<view class="padding-tb-sm" @click="goshop" style="width: 33%;">
						<image src="../../static/images/my/gonghui.png" style="width: 50upx;height: 50upx;"></image>
						<view class="text-df" style="line-height: 50upx;">球厅入驻</view>
					</view>
				</view>
			</view>
			
			<uni-popup ref="popup" type="bottom">
				<view style="width: 100%;height: 500upx;background: #FFFFFF;">
					<view
						style="width: 100%;text-align: center;font-size: 34upx;color: #333333;height: 80upx;line-height: 80upx;">
						商家小程序二维码</view>
					<view style="width: 100%;text-align: center;margin-top: 20upx;">
						<image v-if="shopImage" :src="shopImage" @tap="selecImage(shopImage)"
							style="width: 300upx;height: 300upx;"></image>
						<!-- #ifdef H5 -->
						<view style="margin-top: 20upx;font-size: 24upx;">长按图片可识别进入</view>
						<!-- #endif -->
						<!-- #ifdef APP-PLUS -->
						<view style="margin-top: 20upx;font-size: 24upx;">保存图片后微信打开识别进入</view>
						<!-- #endif -->
					</view>
				</view>
			</uni-popup>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				isChannel: 0,
				money: 0,
				collect: 0,
				coupon: 0,
				isShop: 0,
				hyCheck: '否',
				tgCheck: '否',
				isAgency: 0,
				avatar: '../../static/logo.png',
				isLogin: true,
				userName: '匿名',
				browse: 0, //浏览数
				fans: 0, //粉丝数
				follow: 0, //关注数
				visitor: 0, //访客数
				userId: '',
				isVip: false,
				invitationCode: '', //邀请码
				list: [{
						image: '',
						title: '',
						url: ''
					}, {
						image: '',
						title: '我的收藏',
						url: ''
					}, {
						image: '',
						title: '',
						url: ''
					}, {
						image: '',
						title: '成为助教/教练',
						url: ''
					}, {
						image: '',
						title: '联系客服',
						url: ''
					}, {
						image: '',
						title: '意见反馈',
						url: ''
					},
					// {
					// 	image: '',
					// 	title: '帮助中心',
					// 	url: ''
					// }, 
					{
						image: '',
						title: '设置中心',
						url: ''
					}, {
						image: '',
						title: '退出登录',
						url: ''
					}
				],
				renzheng: 0,
				XCXIsSelect: '否',
				shopImage: ''
			}
		},
		onLoad() {
			this.XCXIsSelect = this.$queue.getData("XCXIsSelect");
			this.$Request.get('/app/common/type/435').then(res => {
				if (res.code == 0) {
					this.shopImage = res.data.value
				}
			});
		},
		onShow() {
			this.hyCheck = this.$queue.getData('hyCheck');
			this.tgCheck = this.$queue.getData('tgCheck');
			this.userId = uni.getStorageSync('userId')
			if (this.userId) {
				this.isLogin = false
				this.getUserInfo()

				this.getAmount()
				this.getIsVip()
			} else {
				this.money = 0;
				this.collect = 0;
				this.coupon = 0;
				this.isShop = 0;
				this.isAgency = 0;
				this.isChannel = 0;
				this.isLogin = true
				this.userName = '匿名'
				this.browse = 0
				this.fans = 0
				this.follow = 0
				this.visitor = 0
				this.avatar = '../../static/logo.png'
				this.isVip = false
			}

		},
		methods: {
			selecImage(item) {
				let imgsArray = [];
				imgsArray[0] = item;
				uni.previewImage({
					current: 0,
					urls: imgsArray
				});
			},
			goshop() {
				this.$refs.popup.open();
			},
			goChat() {
				let kefu = this.$queue.getData('kefu'); // 用户端联系方式 1 手机号 2企业微信 
				let kefuPhone = this.$queue.getData('kefuPhone');
				if (kefu == 1) {
					uni.makePhoneCall({
						phoneNumber: kefuPhone //仅为示例
					});
				} else if (kefu == 3) {
					uni.navigateTo({
						url: '/my/setting/customer'
					});
				} else {
					// #ifdef MP-WEIXIN
					let that = this
					try {
						wx.openCustomerServiceChat({
							extInfo: {
								url: that.$queue.getData('kefuUrl')
							},
							corpId: that.$queue.getData('kefuAppId'),
							success(res) {},
							fail(res) {
								console.error(res)
							}
						})
					} catch (error) {
						console.error("catchcatch" + error)
						uni.showToast({
							title: '请更新至微信最新版本'
						});
					}
					// #endif
					// #ifndef MP-WEIXIN
					let url = this.$queue.getData('kefuUrl');
					if (url.indexOf('/pages/') !== -1 || url.indexOf('/my/') !== -1) {
						uni.navigateTo({
							url
						});
					} else {
						//#ifndef H5
						uni.navigateTo({
							url: '/pages/index/webView?url=' + url
						});
						//#endif
						//#ifdef H5
						window.location.href = url;
						//#endif
					}
					// #endif
				}
			},
			//退出登录
			TuiLogin() {
				let that = this
				if (that.userId) {
					uni.showModal({
						title: '提示',
						content: '确定退出登录吗？',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.removeStorageSync('userName')
								uni.removeStorageSync('avatar')
								uni.removeStorageSync('userId')
								uni.removeStorageSync('token')
								uni.removeStorageSync('phone')
								uni.removeStorageSync('zhiFuBaoName')
								uni.removeStorageSync('zhiFuBao')
								uni.removeStorageSync('invitationCode')
								uni.removeStorageSync('unionId')
								uni.removeStorageSync('openId')
								uni.removeStorageSync('isVIP')
								uni.showToast({
									title: '退出成功！',
									icon: 'none'
								})
								that.isLogin = true
								that.userName = '匿名'
								that.browse = 0
								that.fans = 0
								that.follow = 0
								that.visitor = 0
								that.isVip = false
								that.avatar = '../../static/logo.png'
								that.userId = uni.getStorageSync('userId')

							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}


			},
			//成为按摩师  跳转商户端
			bindFb() {
				if (this.userId) {
					uni.navigateTo({
						url: '/pages/my/hehuo'
					});
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
			},
			goNavOrder(index) {
				let token = this.$queue.getData('token');
				if (token) {
					this.$queue.setData('SelectIndex', index);
					uni.switchTab({
						url: '/pages/order/index'
					})
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
			},
			goNav(e, name) {
				// console.log(e)
				if (this.userId) {
					uni.navigateTo({
						url: e
					})
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
			},
			goLogin(e) {
				uni.navigateTo({
					url: e
				})
			},
			getAmount() {
				this.$Request.get("/app/userBrowse/selectAmount").then(res => {
					if (res.code == 0) {
						this.browse = res.data.browse
						this.fans = res.data.fans
						this.follow = res.data.follow
						this.visitor = res.data.visitor
					}
				});
			},
			getUserInfo() {
				this.$Request.get("/app/user/selectUserById").then(res => {
					if (res.code == 0) {
						this.userName = res.data.userName
						this.invitationCode = res.data.invitationCode
						this.avatar = res.data.avatar ? res.data.avatar : '../../static/logo.png'
						this.isAuthentication = res.data.isAuthentication
						this.isAgency = res.data.isAgency ? res.data.isAgency : 0; //是否是推广员 1是 2否
						this.isChannel = res.data.isChannel ? res.data.isChannel : 0; //是否是渠道商 1是 2否
						// uni.setStorageSync('isAuthentication', res.data.isAuthentication)
						this.isShop = res.data.isShop ? res.data.isShop : 0;
						uni.setStorageSync('avatar', res.data.avatar)
						uni.setStorageSync('userName', res.data.userName)
						uni.setStorageSync('invitationCode', res.data.invitationCode)
						uni.setStorageSync('zhiFuBao', res.data.zhiFuBao)
						uni.setStorageSync('zhiFuBaoName', res.data.zhiFuBaoName)

						if (res.data.isAuthentication == 0 || res.data.isAuthentication == null) {
							this.renzheng = 0
							uni.setStorageSync("renzheng", this.renzheng)
						} else if (res.data.isAuthentication == 1) {
							this.renzheng = 1
							uni.setStorageSync("renzheng", this.renzheng)
						} else if (res.data.isAuthentication == 2) {
							this.renzheng = 2
							uni.setStorageSync("renzheng", this.renzheng)
						} else if (res.data.isAuthentication == 3) {
							this.renzheng = 3
							uni.setStorageSync("renzheng", this.renzheng)
						} else if (res.data.isAuthentication == 4) {
							this.renzheng = 4
							uni.setStorageSync("renzheng", this.renzheng)
						} else if (res.data.isAuthentication == 5) {
							this.renzheng = 5
							uni.setStorageSync("renzheng", this.renzheng)
						} else if (res.data.isAuthentication == 6) {
							this.renzheng = 6
							uni.setStorageSync("renzheng", this.renzheng)
						}

					}
				});

				this.$Request.get("/app/userMoney/selectMyMoney").then(res => {
					if (res.code == 0 && res.data) {
						this.money = res.data.money
					}
				});

				this.$Request.getT('/app/coupon/selectCouponUserList?page=1&limit=10&status=0').then(res => {
					if (res.code == 0) {
						this.coupon = res.data.totalCount;
					}
				});

				this.$Request.getT("/app/artificer/selectCollectList?page=1&limit=10").then(res => {
					if (res.code == 0) {
						this.collect = res.data.totalCount;
					}
				});

				// #ifdef APP-PLUS
				let clientid = plus.push.getClientInfo().clientid;
				let sysPhone = 1;
				if (plus.os.name.toLowerCase() === 'android') {
					sysPhone = 1;
				} else {
					sysPhone = 2;
				}
				let userId = this.$queue.getData('userId');
				if (userId) {
					this.$Request.postT('/app/user/updateClientId?clientId=' + clientid + '&userId=' + userId +
						'&sysPhone=' + sysPhone).then(
						res => {});
				}
				//#endif
			},

			getIsVip() {
				this.$Request.get("/app/UserVip/isUserVip").then(res => {
					if (res.code == 0) {
						this.isVip = res.data
						uni.setStorageSync('isVIP', res.data)
					}
				});
			}

		}
	}
</script>

<style lang="scss">
	page {
		background-color: #F8F8FA;
	}

	.money_view {
		font-size: 38rpx;
		font-family: PingFang SC;
		font-weight: 800;
		color: #212121;
	}

	.money_name {
		margin-top: 10rpx;
		font-size: 24rpx;
		font-family: PingFang SC;
		font-weight: 500;
		color: #212121;
	}

	.vip_view {
		width: 686rpx;
		height: 130rpx;
		background: linear-gradient(90deg, #151515 0%, #333333 100%);
		border-radius: 32rpx 32rpx 0rpx 0rpx;
		margin: 0 auto;
		margin-top: 30rpx;
		padding: 20rpx 30rpx;
	}

	.box_xian {
		width: 9rpx;
		height: 29rpx;
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
		border-radius: 5rpx;
		margin-right: 14rpx;
	}

	.kefu_view {
		width: 100rpx;
		height: 100rpx;
		position: fixed;
		right: 30rpx;
		bottom: 200rpx;
		z-index: 999;
	}

	.bg {
		// background: #05C25C;
		background-image: url('https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/d627a90b3afd061b9614e96dcc3257d3.png');
		width: 750rpx;
		height: 500rpx;
		background-size: 100%;
	}

	.box {
		color: #333333;
		width: 92%;
		// margin: 30rpx auto;
		margin: 0 auto;
		margin-bottom: 40rpx;
		padding-top: 1rpx;
	}

	.camera {
		width: 54px;
		height: 44px;

		&:active {
			background-color: #ededed;
		}
	}

	.btn-bg {
		width: 150rpx;
		height: 60rpx;
		background: linear-gradient(90deg, #F9E3AA 0%, #FAEBCE 100%);
		border-radius: 28px;
		text-align: center;
		line-height: 60rpx;
		color: #5C4216;
		font-size: 24rpx;
		font-family: PingFang SC;
		font-weight: bold;
	}

	.images {
		width: 18rpx;
		height: 30rpx;
	}

	.vipttl {
		font-size: 32rpx;
		font-family: DFZongYi-Bd-80-Win-GB;
		font-weight: 800;
		color: #F7D69B;
	}
</style>