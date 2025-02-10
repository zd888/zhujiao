<template>
	<view style="padding-bottom: 150rpx;">
		<view>
			<view>
				<swiper :style="{height: imageHeight + 'upx'}" autoplay="1500" :indicator-dots="false" :circular='true'
					indicator-active-color="#ffffff" indicator-color="#cccccc">
					<swiper-item v-for="(item,index) in order.lifePhoto" :key='index'
						:style="{height: imageHeight + 'upx'}" @click="saveImgs(order.lifePhoto,index)">
						<image class="swiper-image" :src="item" :style="{height: imageHeight + 'upx'}">
						</image>
					</swiper-item>
				</swiper>
			</view>
			<!-- <view class="wrap">
				<u-swiper :list="order.lifePhoto" height="500" border-radius="30"></u-swiper>
			</view> -->


			<view class="padding-bottom bg margin_item">
				<view class="flex align-center padding-tb-sm padding-lr"
					style="background: linear-gradient(90deg, #55B55C 0%, #308251 100%);border-radius: 16rpx 16rpx 0px 0px;margin-top: -40rpx;position: relative;z-index: 99;">
					<view class="rir">
						<!-- 安心购 -->
						<image
							src="https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/009603feb20d2441808c664a8acecf2e.png"
							style="width: 87rpx;height:29rpx;"></image>
					</view>
					<view class="flex align-center" style="margin-right: 30upx;color:#FFFFFF;">
						<image src="../../static/images/index/renz1.png" style="width: 32upx;height: 28upx;"></image>
						<text class="margin-left-xs">实名认证</text>
					</view>
					<view class="flex align-center" style="margin-right: 30upx;color:#FFFFFF;">
						<image src="../../static/images/index/renz2.png" style="width:28upx;height: 33upx;"></image>
						<text class="margin-left-xs">资质认证</text>
					</view>
					<view class="flex align-center" style="color:#FFFFFF;">
						<image src="../../static/images/index/renz3.png" style="width: 28upx;height: 32upx;"></image>
						<text class="margin-left-xs">平台担保</text>
					</view>
				</view>
				<view class="flex padding align-center">
					<image :src="order.artificerImg" style="width: 140rpx;height: 135rpx;border-radius: 24rpx;"></image>
					<view>
						<view class="margin-left text-bold" style="letter-spacing: 3rpx; font-size: 35rpx;">
							{{order.artificerName}}
						</view>
						<view class="padding-sm flex justify-between" style="width: 490rpx;padding-left: 30rpx;padding-top: 10rpx;">
							<view class=" margin-right-xs text-26"
								style="color: #308251;background-color: #EDFAF7;border: 1rpx solid #05C25C;border-radius:8rpx;padding: 4rpx 10rpx;line-height: 30rpx;">
								助教陪练
							</view>
							<view class="flex align-center">
								<image src="../../static/images/address.png" style="width: 22rpx;height: 25rpx;">
								</image>
								<view style="color: #999999;margin-left: 5rpx;">{{order.distance}}</view>
							</view>

						</view>
					</view>
				</view>
				<view class="padding-sm flex justify-between align-center fu_view">
					<view v-if="order.ordersScore" class="flex align-center">
						<view>用户评分：</view>
						<view class="margin-left-xs fu_viewtext">{{order.ordersScore}}.0 <text style="font-size: 24rpx;">分</text> </view>
					</view>
					<view class="flex align-center">
						<view>已服务单数：</view>
						<view class="margin-left-xs fu_viewtext">{{order.ordersCount}}<text style="font-size: 24rpx;">单</text></view>
					</view>
				</view>
				<view style="margin: 20rpx 30rpx;font-size: 28rpx;font-family: PingFang SC;font-weight: 500;color: #999999;">
					自我介绍：{{order.content}}
				</view>
			</view>


		</view>

		<!-- <view class="bg padding-lr padding-tb-sm margin-bottom-sm margin_item" style="margin-top: 20rpx;">
			<view class="text-bold" style="font-size: 35rpx;letter-spacing: 3rpx;">自我介绍</view>
			<view class="margin-top" style="color: #666666;font-size: 28rpx;">
				{{order.content}}
			</view>
		</view> -->
		<view class="padding bg margin_item" style="margin-top: 20rpx;padding-bottom: 20rpx;">
			<view v-if="XCXIsSelect !='否'" class="tab ">
				<view class="text-bold" style="font-size: 28rpx;letter-spacing: 3rpx;">服务项目</view>
				<view class="flex padding-tb-sm" v-for="(item,index) in artificerlist" :key="index"
					@click="goNav(item.massageTypeId)">
					<image :src="item.massageImg" style="width: 210upx;height: 170upx;border-radius:10rpx"></image>
					<view class="margin-left-sm" style="width: 66%;">
						<view class="text-bold text-lg">{{item.title}}</view>
						<view class="flex align-center justify-between text-df margin-tb-sm" style="color: #999999;">
							<view class="flex justify-between align-center"
								style="color: #999999;font-size: 22rpx;">
								<image src="../../static/time.png"
									style="width: 26rpx;height: 26rpx;margin-right: 8rpx;"></image>
								<view>{{item.duration}}分钟</view>
							</view>
							<view>已售{{item.sales}}</view>
						</view>
						
						
						
						<view class="flex align-center justify-between">
							<view class="text-green text-df" style="color: #FF1200;">
								￥<text class="text-green text-xl text-bold"
									style="color: #FF1200;">{{item.price}}</text>
								<text v-if="hyCheck != '否'">/</text><text v-if="hyCheck != '否'"
									class="text-sm">会员价:￥{{item.memberPrice}}</text>
							</view>
							<view class="btn">预约</view>
						</view>
					</view>
				</view>
				<empty style="margin-top: 60rpx;" v-if="artificerlist.length == 0"></empty>
			</view>
		</view>
		<view class="padding bg margin_item" style="margin-top: 20rpx;">
			<view>
				<view class="text-bold" style="font-size: 28rpx;letter-spacing: 3rpx;">资质证书</view>
				<view class="flex justify-around flex-wrap" style="width: 100%;margin-top: 15rpx;">
					<view class="flex flex-wrap" style="width: 100%;">
						<view style="width: 220rpx;height: 160rpx;margin-right: 10rpx;margin-top: 10rpx;"
							v-for="(item,index) in order.certificate" :key="index">
							<image :src="item" style="width: 100%;height: 100%;"
								@click="saveImg(order.certificate,index)">
							</image>
						</view>
					</view>
				</view>
			</view>
		</view>
		<view class="bg margin_item" style="margin-top: 20rpx;padding: 30rpx 30rpx 10rpx 30rpx;">
			<view class="text-bold" style="font-size: 28rpx;letter-spacing: 3rpx;">服务评价</view>
			<view class="u-skeleton-fillet" style="padding: 10rpx 0rpx 0rpx 0rpx;">
				<view v-if="commentList.length<=0" class="margin-top-sm"> 暂无评价</view>
				<view class="margin-bottom-sm padding-tb-sm"
					:style="index != 0 ? 'border-bottom: 1upx solid #f0f0f0;' : ''" v-for="(item, index) in commentList"
					:key='index' v-else>
					<view class="flex justify-between">
						<u-avatar src="/static/logo.png" size="48"></u-avatar>
						<view class="flex-sub margin-left-sm" style="line-height: 46upx;">匿名</view>
						<view class="flex">
							<u-icon v-for="ite in item.score" :key='ite' color="#FFAA01" name="star-fill"></u-icon>
						</view>
					</view>
					<view class="margin-top-sm text-lg">{{item.content}}</view>
				</view>
			</view>
		</view>
		<!-- <view class="margin-top-sm bg radius u-skeleton-fillet" style="margin-bottom: 130rpx;">
			<view class="text-center padding text-bold" style="font-size: 35rpx;letter-spacing: 3rpx;">服务详情</view>
			<view v-for="(item,index) in order.lifePhoto" :key="index" style="width: 100%;"
				@click="saveImg(order.lifePhoto,index)">
				<image :src="item" style="width: 750upx;height: 963upx;" mode="widthFix"></image>
			</view>
		</view> -->




		<view class="u-skeleton-fillet text-lg cu-bar foot bg padding-lr" v-if="myId != order.userId">
			<view class="text-center"
				style="width: 665upx;line-height: 78upx;background: linear-gradient(90deg, #55B55C 0%, #308251 100%);border-radius:39upx;margin: 0 auto;"
				@click="follow">
				<image src="../../static/images/index/shoucang.png"
					style="width: 30upx;height: 28upx;position: relative;top: 4upx;"></image>
				<text style="color: #FFFFFF;margin-left: 10upx;" v-if="!isFollow">收藏</text>
				<text style="color: #FFFFFF;margin-left: 10upx;" v-else>已收藏</text>
			</view>
		</view>
		<!-- <u-picker v-model="show" mode="time" :params="params" @confirm="statusChange"></u-picker> -->
		<u-skeleton :loading="loading" :animation="true" bgColor="#FFF"></u-skeleton>
	</view>
</template>

<script>
	import empty from '@/components/empty.vue'
	export default {
		components: {
			empty
		},
		data() {
			return {
				tripWay: 1,
				game: [],
				loading: true, // 是否显示骨架屏组件
				customStyle: {
					width: '400upx',
					color: '#FFFFFF',
					background: "#005DFF",
					border: 0
				},
				isShop: '',
				artificerId: '',
				orderDet: {},
				page: 1,
				limit: 10,
				order: {},
				commentList: [],
				isFollow: false,
				myId: '',
				isVip: false,
				AUDIO: uni.createInnerAudioContext(),
				isPlay: false,
				show: false,
				params: {
					year: false,
					month: true,
					day: true,
					hour: true,
					minute: true,
					second: true,
					timestamp: true
				},
				startTime: '',
				mobile: '',
				name: '',
				cityaddress: '',
				detailaddress: '',
				latitude: '',
				longitude: '',
				province: '',
				city: '',
				district: '',
				age: '',
				tabs: [{
					name: '项目',
					status: 0
				}, {
					name: '评价',
					status: 1
				}, {
					name: '资质证书',
					status: 2
				}],
				tabIndex: 0, // tab下标
				fwIndex: 0,
				artificerlist: [],

				latitude: '',
				longitude: '',
				imgArr: [],
				status: "",
				XCXIsSelect: '否',
				hyCheck: '否',
				imageHeight: '358'
			}
		},
		onLoad(option) {
			if (option.isShop) {
				this.isShop = option.isShop;
			}
			this.hyCheck = this.$queue.getData('hyCheck');
			uni.showLoading({
				title: '加载中...',

			});
			this.XCXIsSelect = this.$queue.getData("XCXIsSelect");
			this.artificerId = option.artificerId
			this.myId = uni.getStorageSync('userId')
			this.isVip = uni.getStorageSync('isVIP')
			this.latitude = uni.getStorageSync('latitude')
			this.longitude = uni.getStorageSync('longitude')
			this.getDet()
			this.getordertherapist()
			this.getOrderComment()
		},
		onShow() {
			this.imageHeight = this.$queue.getData('imageHeight');
		},
		onReady() {
			this.AUDIO.onEnded(function(res) {
				this.isPlay = false;
			});
		},
		methods: {
			// 切换菜单
			tabChange(index) {
				this.tabIndex = index
				this.fwIndex = index
			},
			//换算年龄
			getAge(e) {
				console.log('出生日期：' + e)
				this.birthDate = e
				//创建系统日期
				var today = new Date();
				//把出生日期转换成日期
				this.birthDate = new Date(this.birthDate);
				//分别获取到年份后相减
				this.age = today.getFullYear() - this.birthDate.getFullYear();
				// console.log(this.age+'当前年龄')
			},
			// 详情
			getDet() {
				// uni.showLoading({
				// 	title: '加载中...',
				// 	icon: 'none'
				// });
				this.$Request.get("/app/artificer/selectArtificerById", {
					artificerId: this.artificerId,
					latitude: this.latitude,
					longitude: this.longitude,
					userId: this.myId
				}).then(res => {
					uni.hideLoading();
					this.loading = false;
					if (res.code == 0) {
						this.order = res.data
						this.status = res.data.status
						this.tripWay = res.data.tripWay
						this.order.lifePhoto = this.order.lifePhoto.split(',')
						this.order.certificate = this.order.certificate.split(',')
						if (this.order.distance > 1000) {
							this.order.distance = (this.order.distance / 1000).toFixed(1) + 'km'
						} else {
							this.order.distance = (this.order.distance).toFixed(1) + 'm'
						}

						if (res.data.collectCount == 1) {
							this.isFollow = true
						} else {
							this.isFollow = false
						}
					} else {
						this.loading = false;
						uni.hideLoading();
					}
					uni.hideLoading();

				});

			},
			//服务技师
			getordertherapist() {
				this.$Request.get("/app/artificer/selectArtificerMassageList", {
					artificerId: this.artificerId,
					isShop: this.isShop ? this.isShop : ''
				}).then(res => {
					if (res.code == 0) {
						this.artificerlist = res.data
						// this.commentList = [...this.commentList, ...res.data.list]
					}
				});
			},
			// 评价
			getOrderComment() {
				this.$Request.get("/app/takingComment/selectOrderTakingComment", {
					artificerId: this.artificerId,
					page: this.page,
					limit: this.limit
				}).then(res => {
					if (res.code == 0) {
						this.commentList = [...this.commentList, ...res.data.list]
					}
				});
			},

			playVoice() {
				console.log(this.isPlay)
				this.AUDIO.src = this.order.voiceIntroduce;
				if (this.isPlay) {
					this.AUDIO.stop();
				} else {
					this.AUDIO.play();
				}
				this.isPlay = !this.isPlay;
			},
			goNav(massageTypeId) {
				if (this.status == 2) {
					uni.showToast({
						title: '技师已下线',
						icon: 'none'
					})
				} else {
					uni.navigateTo({
						url: "/pages/index/order/orderDet?isShop=" + this.isShop + "&massageTypeId=" +
							massageTypeId + '&artificerId=' + this
							.artificerId + '&tripWay=' + this.tripWay
					})
				}

			},
			goMsg() {
				let data = {
					userId: this.myId,
					focusedUserId: this.order.userId
				}
				this.$Request.postJson('/app/chat/insertChatConversation ', data).then(res => {
					if (res.code == 0) {
						let id = this.order.userId == res.data.userId ? res.data.focusedUserId : this.order.userId
						uni.navigateTo({
							url: '/pages/msg/im?chatConversationId=' + res.data.chatConversationId +
								'&byUserId=' + id
						})
					}
				})
			},
			// 收藏
			follow() {
				let oldIsFollow = this.isFollow;
				let that = this
				let data = {
					artificerId: that.artificerId,
					userId: that.myId,
					classify: '1'
				}
				that.$Request.postJson("/app/artificer/insertCollect", data).then(res => {
					if (res.code == 0) {
						if (oldIsFollow) {
							uni.showToast({
								title: '取消成功',
								icon: 'none'
							})
						} else {
							uni.showToast({
								title: '收藏成功',
								icon: 'none'
							})
						}

					} else {
						uni.showToast({
							title: res.msg,
							icon: 'none'
						})
					}
					setTimeout(function() {
						that.getDet()
					}, 500)
				});
			},
			saveImg(imgs, index) {
				// console.log(imgs)
				let that = this;
				let imgArr = imgs
				// imgArr.push(imgs);
				// //预览图片
				uni.previewImage({
					urls: imgArr,
					current: imgArr[index]
				});
			},
			saveImgs(imgs, index) {
				console.log(imgs, index)
				// let that = this
				let imgArr = imgs
				// this.imgArr.push(imgs);
				// console.log(this.imgArr)
				// //预览图片
				uni.previewImage({
					urls: imgArr,
					current: imgArr[index]
				});
			},
		}

	}
</script>

<style>
	page {
		background-color: #F5F5F5;
	}

	.swiper-image {
		width: 100%;
		/* border-radius: 24upx; */
	}

	.margin_item {
		width: 686rpx;
		margin: 0 auto;
		border-radius: 24rpx;
	}

	.fu_view {
		width: 634rpx;
		height: 70rpx;
		background: #F4FFFC;
		border-radius: 16rpx;
		margin: 0 auto;
		font-size: 26rpx;
		font-family: PingFang SC;
		font-weight: 500;
		color: #999999;
	}
	
	.fu_viewtext{
		font-size: 34rpx;
		font-family: PingFang SC;
		font-weight: 500;
		color: #FF7709;
	}

	.bgs {
		background-color: #FFFFFF;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.wrap {
		padding: 20upx 23upx;
	}

	.line_s {
		display: inline-flex;
		width: 14rpx;
		height: 14rpx;
		background: #1AD566;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.line_x {
		display: inline-flex;
		width: 14rpx;
		height: 14rpx;
		background: #000000;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.tab>view {
		/* border-bottom: 2upx solid #F5F5F2; */
	}

	.tab>view:last-of-type {
		border: none;
	}

	.rir {
		font-size: 30upx;
		font-family: DFZongYi-Bd-80-Win-GB;
		font-weight: 800;
		color: #30B47A;
		margin-right: 30rpx;
	}

	.btn {
		width: 129rpx;
		height: 52rpx;
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
		border-radius: 26rpx;
		color: #FFFFFF;
		text-align: center;
		line-height: 52rpx;
	}

	.swiper {
		height: 385rpx;
	}
</style>