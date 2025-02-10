<template>
	<view style="padding-bottom: 100rpx;">
		<view>
			<swiper style="height: 383rpx;width: 750rpx;" autoplay="1500" :indicator-dots="false" :circular='true'
				indicator-active-color="#ffffff" indicator-color="#cccccc">
				<swiper-item v-for="(item,index) in consortiaImg" :key='index' style="height: 383rpx;width: 750rpx;"
					@click="saveImgs(consortiaImg,index)">
					<image class="swiper-image" :src="item" mode="aspectFill" style="height: 383rpx;width: 750rpx;">
					</image>
				</swiper-item>
			</swiper>
		</view>
		<view class="topName_view" v-if="consortiaName">
			<!-- <image :src="order.logo" @tap="saveImg(order.logo)" class="topName_logo"></image> -->
			<view class="flex align-center justify-between">
				<view class="top_name">{{consortiaName}}</view>
				<view class="top_scBtn" @click="follow">
					<image src="../../../static/images/index/shoucang.png"></image>
					<text v-if="!isFollow">收藏</text>
					<text v-else>已收藏</text>
				</view>
			</view>
			<view class="flex margin-top-xs">
				<view v-if="order.score" class="margin-right-sm">
					<image src="../../../static/images/start.png" style="width: 24upx;height: 22upx;">
					</image>
					<text class="margin-left-xs" style="color: #FF1200;">{{order.score}}</text>
				</view>
				<view class="top_yishou">销量{{order.sales}}</view>
			</view>
			<view class="flex flex-wrap margin-top-xs" v-if="orderLabels.length>0">
				<text class="margin-bottom-xs top_box" v-for="(ites,index) in orderLabels" :key="index">{{ites}}</text>
			</view>
			<view class="top_xian"></view>
			<view class="flex margin-top-sm align-center">
				<view class="top_address_view">
					<view class="top_address">{{order.address}}</view>
					<view class="top_time">营业时间：{{order.startBusinessHours}}-{{order.endBusinessHours}}</view>
				</view>
				<view class="top_address_right">
					<view class="text-center" @tap="bindGps(order.latitude,order.longitude,order.address)">
						<view class="top_address_dhview">
							<!-- <image src="/static/images/index/add.png"></image> -->
							<image src="/static/images/place1.png"></image>
						</view>
						<view class="top_address_dhtext">导航</view>
					</view>

					<view class="text-center" @tap="bindphone(order.phone)">
						<view class="top_address_phoneview">
							<image src="/static/images/phone.png"></image>
						</view>
						<view class="top_address_dhtext">电话</view>
					</view>
				</view>
			</view>
		</view>

		<view class="tabview_daodian">
			<view :class="djSel == 1 ? 'tabview_daodian_leftsel' : 'tabview_daodian_leftno'" @tap="topSel(1)">
				预约服务</view>
			<view :class="djSel == 2 ? 'tabview_daodian_rightsel' : 'tabview_daodian_rightno'" @tap="topSel(2)">助教/教练
			</view>
		</view>
		<view class="tab" v-if="djSel == 1 && orderList.length > 0">
			<view class="flex" style="margin: 30rpx 40rpx;background: #FFFFFF;border-radius: 24rpx;padding: 20rpx;"
				v-for="(item,index) in orderList" :key="index" @click="goNav(item.massageTypeId)">
				<image :src="item.massageImg" style="width: 210upx;height: 170upx;border-radius:10rpx"></image>
				<view class="margin-left-sm" style="width: 66%;">
					<view class="flex justify-between">
						<view class="text-bold text-lg"
							style="width: 260rpx;overflow: hidden;white-space: nowrap;text-overflow: ellipsis;">
							{{item.title}}
						</view>
						<view style="color: #999999;font-size: 22rpx;">已售{{item.sales}}</view>
					</view>
					<!-- <view class="flex align-center justify-between text-df margin-tb-sm" style="color: #999999;">
						<view>时长：{{item.duration}}分钟</view>
						<view>已售{{item.sales}}</view>
					</view> -->
					<view class="flex flex-wrap margin-top-sm" v-if="item.labels.length>0">
						<text class="margin-bottom-xs box" v-for="(ites,index) in item.labels.slice(0,2)"
							:key="index">{{ites}}</text>
					</view>
					<view class="flex align-center justify-between"
						:style="item.labels.length>0 ? 'margin-top:10rpx' : 'margin-top:80rpx'">
						<view class="text-green text-df" style="color: #FF1200;">
							￥<text class="text-green text-xl text-bold" style="color: #FF1200;">{{item.price}}</text>
							<text>/{{item.duration}}分钟</text>
						</view>
						<view class="btn">下单</view>
					</view>
				</view>
			</view>
		</view>


		<view v-if="djSel == 2 && artificerlist.length > 0"
			class="flex align-center margin-bottom-sm bg padding-sm radius24 margin"
			v-for="(item,index) in artificerlist" :key='index' @click.stop="goOrderDetail(item)"
			style="position: relative;">
			<view style="width:130upx;height:190upx;border-radius: 10rpx;">
				<image :src="item.artificerImg?item.artificerImg: '../../static/logo.png'"
					@click.stop="saveImg(item.artificerImg)" style="width: 130upx;height: 130upx;border-radius: 50%;"
					mode="aspectFill"></image>
				<view class="kyy_view" v-if="item.state == 1">可服务</view>
				<view class="kfw_view" v-else-if="item.state == 2">可预约</view>
				<view class="byy_view" v-else>休息中</view>
			</view>
			<view class="margin-left  flex flex-direction" style="width: 85%;">
				<view class="flex align-center justify-between">
					<view class="flex align-center">
						<view class="text-bold text-30 margin-right-xs">{{item.artificerName}}</view>
						<view class="zpmore_view" v-if="item.lifePhoto" @click.stop="saveImgs(item.lifePhoto,0)">更多照片
						</view>
					</view>
					<view class="zz_view" v-if="item.status == 1 && item.time">最早可约：{{item.time}}
					</view>
				</view>

				<view class="flex justify-between " style="margin:15rpx 0rpx 20rpx">
					<view class="flex align-center">
						<view v-if="item.ordersScore" class="margin-right-sm">
							<image src="../../../static/images/start.png" style="width: 24upx;height: 22upx;">
							</image>
							<text class="margin-left-xs" style="color: #FF1200;">{{item.ordersScore}}.0</text>
						</view>
						<view style="color: #999999;">已服务:{{item.ordersCount}}单</view>
					</view>

					<view class="flex align-center" v-if="item.tripWay != 3 && numIndex != 3">
						<image src="../../../static/images/address.png" style="width: 24rpx;height: 29rpx;"></image>
						<view style="color: #333333;margin-left: 8rpx;">{{item.distance}}</view>
					</view>
					<view class="flex align-center" style="font-size: 26rpx;color: #f49650;font-weight: bold;"
						v-if="item.tripWay == 3">
						免出行费
					</view>
				</view>
				<view style="width: 100%;display: flex;justify-content: space-between;align-items: center;">
					<view class="flex flex-wrap align-center" style="margin-top: 10rpx;">
						<image
							src="https://anmo.xianmxkj.com/file/uploadPath/2023/02/16/889dc6f201d78a86686fb08fafa79607.png"
							style="width: 34rpx;height: 36rpx;"></image>
						<view style="margin-left: 10rpx;color: #999;font-size: 26rpx;">商家</view>
						<image
							src="https://anmo.xianmxkj.com/file/uploadPath/2023/02/16/dcda4b6183f48a5912c43097a9fead3f.png"
							style="width: 40rpx;height: 32rpx;margin-left: 20rpx;"></image>
						<view style="margin-left: 10rpx;color: #999;font-size: 26rpx;">
							{{item.commentCount ? item.commentCount : 0}}
						</view>
						<image
							src="https://anmo.xianmxkj.com/file/uploadPath/2023/02/16/04a618b2192cff5ae90acca4d3361f8b.png"
							style="width: 40rpx;height: 36rpx;margin-left: 20rpx;"></image>
						<view style="margin-left: 10rpx;color: #999;font-size: 26rpx;">
							{{item.collectCount ? item.collectCount : 0}}
						</view>
					</view>
					<view>
						<view class="text-center text-sm"
							style="background: linear-gradient(90deg, #55B55C 0%, #308251 100%);color: #ffffff;width: 135upx;border-radius: 16upx;line-height: 50upx;">
							立即预约
						</view>
					</view>
				</view>
			</view>
		</view>

		<empty class="margin-bottom-sm bg radius24 margin" v-if="djSel == 1 && orderList.length==0"></empty>
		<empty class="margin-bottom-sm bg radius24 margin" v-if="djSel == 2 && artificerlist.length==0"></empty>

		<!-- <view class=" padding bg u-skeleton-fillet"
			style="padding-bottom: 100rpx;margin: 30rpx 40rpx;border-radius: 24rpx;">
			<view class=" text-bold padding-bottom-sm" style="font-size: 34rpx;">
				评价({{commentList.length}})
			</view>
			<view v-if="commentList.length<=0" class=""> 暂无评价</view>
			<view class="margin-bottom-sm padding-tb-sm" style="border-bottom: 1upx solid #f0f0f0;"
				v-for="(item, index) in commentList" :key='index' v-else>
				<view class="flex justify-between">
					<u-avatar src="/static/logo.png" size="48"></u-avatar>
					<view class="flex-sub margin-left-sm" style="line-height: 46upx;">匿名</view>
					<view class="flex">
						<view style="color: #999999;">{{item.createTime}}</view>
					</view>
				</view>
				<view class="margin-top-sm">{{item.content}}</view>
			</view>
		</view> -->

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
				orderList: [],
				commentList: [],
				djSel: 1,
				isFollow: false,
				order: {},
				latitude: '',
				longitude: '',
				consortiaId: '',
				consortiaName: '',
				consortiaImg: [],
				orderLabels: [],
				artificerlist: [],
				hyCheck: '否'
			}
		},
		onLoad(d) {
			this.latitude = uni.getStorageSync('latitude')
			this.longitude = uni.getStorageSync('longitude')

			this.hyCheck = this.$queue.getData('hyCheck');
			if (d.consortiaId) {
				this.consortiaId = d.consortiaId;
				this.getDetail();
				this.getArtificerList();
				this.getMassageList();
				// this.getOrderComment();
			}
		},
		methods: {
			goNav(massageTypeId) {
				uni.navigateTo({
					url: '/pages/index/order/order?isShop=1&massageTypeId=' + massageTypeId + '&consortiaId=' +
						this.consortiaId
				});
			},
			goOrderDetail(e) {
				uni.navigateTo({
					url: '/pages/therapist/orderDetail?isShop=1&artificerId=' + e.artificerId + '&consortiaId=' +
						this.consortiaId
				});
			},
			topSel(index) {
				this.djSel = index;
			},
			//服务技师
			getArtificerList() {
				this.$Request.get("/app/artificer/selectArtificerList", {
					consortiaId: this.consortiaId,
					latitude: this.latitude,
					longitude: this.longitude,
					isShop: 1
				}).then(res => {
					if (res.code == 0) {
						for (let i = 0; i < res.data.length; i++) {
							if (res.data[i].labels) {
								res.data[i].labels = res.data[i].labels.split(",");
							} else {
								res.data[i].labels = []
							}

							if (res.data[i].lifePhoto) {
								res.data[i].lifePhoto = res.data[i].lifePhoto.split(',')
							}

							if (res.data[i].distance > 1000) {
								res.data[i].distance = (res.data[i].distance / 1000).toFixed(1) +
									'km'
							} else {
								res.data[i].distance = (res.data[i].distance).toFixed(1) + 'm'
							}
						}

						this.artificerlist = res.data;
					}
				});
			},
			//服务项目
			getMassageList() {
				this.$Request.get("/app/artificer/selectConsortiaMassageList", {
					status: 0,
					consortiaId: this.consortiaId
				}).then(res => {
					if (res.code == 0) {
						for (let i = 0; i < res.data.length; i++) {
							if (res.data[i].labels) {
								res.data[i].labels = res.data[i].labels.split(",");
							} else {
								res.data[i].labels = []
							}
						}
						this.orderList = res.data
					}
				});
			},
			// 评价
			getOrderComment() {
				this.$Request.get("/app/takingComment/selectOrderTakingComment", {
					consortiaId: this.consortiaId,
					page: 1,
					limit: 10
				}).then(res => {
					if (res.code == 0) {
						this.commentList = [...this.commentList, ...res.data.list]
					}
				});
			},
			// 一键导航
			bindGps(latitude, longitude, name, detailsadd) {
				console.log(latitude, longitude, name, detailsadd)
				uni.openLocation({
					latitude: latitude - 0, //要去的纬度-地址       
					longitude: longitude - 0, //要去的经度-地址
					name: name, //地址名称
					address: detailsadd, //详细地址名称
					success: function() {
						console.log('导航成功');
					},
					fail: function(error) {
						console.log(error)
					}
				});
			},
			// 拨打电话
			bindphone(phone) {
				let that = this
				uni.showModal({
					title: '提示',
					content: '是否拨打电话',
					success: function(res) {
						if (res.confirm) {
							console.log('用户点击确定', phone);
							uni.makePhoneCall({
								phoneNumber: phone //仅为示例
							});
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			// 收藏
			follow() {
				let userId = this.$queue.getData('userId');
				let oldIsFollow = this.isFollow;
				let that = this
				let data = {
					consortiaId: that.consortiaId,
					userId: userId
				}
				that.$Request.postJson("/app/consortia/insertCollect", data).then(res => {
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
						that.getDetail()
					}, 500)
				});
			},
			getDetail() {
				this.$queue.showLoading('加载中...')
				let userId = this.$queue.getData('userId');
				this.$Request.getT('/app/consortia/selectConsortiaByUserId?consortiaId=' + this.consortiaId + '&userId=' +
					userId).then(res => {
					if (res.code == 0 && res.data) {
						uni.setNavigationBarTitle({
							title: res.data.consortiaName
						})
						this.order = res.data;
						this.consortiaName = res.data.consortiaName;
						if (res.data.isCollect && res.data.isCollect == 1) {
							this.isFollow = true;
						} else {
							this.isFollow = false;
						}


						if (res.data.consortiaImg) {
							this.consortiaImg = res.data.consortiaImg.split(',')
						}

						if (res.data.labels) {
							this.orderLabels = res.data.labels.split(",");
						}
					}
					uni.hideLoading();
				});
			},
			saveImg(imgs) {
				// console.log(imgs)
				let that = this;
				let imgArr = []
				imgArr.push(imgs);
				// //预览图片
				uni.previewImage({
					urls: imgArr,
					current: imgArr[0]
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

<style lang="scss">
	page {
		background: #F7F7F7;
	}

	.bg {
		background: #FFFFFF;
	}

	.box {
		background: #E8FAE1;
		color: #308251;
		padding: 5rpx 15rpx;
		font-size: 24upx;
		letter-spacing: 2rpx;
		border-radius: 8rpx;
		margin-right: 8upx;
	}

	.tab>view {
		border-bottom: 2upx solid #F5F5F2;
	}

	.tab>view:last-of-type {
		border: none;
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

	.tabview_daodian {
		width: 686rpx;
		height: 80rpx;
		background: #FFFFFF;
		border-radius: 24rpx 24rpx 0rpx 0rpx;
		font-size: 32rpx;
		font-family: PingFang SC;
		font-weight: bold;
		color: #308251;
		align-items: center;
		display: flex;
		position: relative;
		margin: 0 auto;
		margin-top: 30rpx;

		.tabview_daodian_leftsel {
			width: 345rpx;
			height: 97rpx;
			// background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
			background-image: url('https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/060a27c7863023939445aae2712832cc.png');
			background-size: 100%;
			// border: 4rpx solid #FFFFFF;
			// border-radius: 24rpx 100rpx 0px 24rpx;
			color: #FFFFFF;
			text-align: center;
			line-height: 97rpx;
			position: relative;
		}

		.tabview_daodian_rightsel {
			width: 345rpx;
			height: 97rpx;
			background-image: url('https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/ad7a4a76b641d1eeecfc31837fe15184.png');
			background-size: 100%;
			color: #FFFFFF;
			text-align: center;
			line-height: 97rpx;
			position: relative;
		}

		.tabview_daodian_leftno {
			width: 345rpx;
			text-align: center;
			color: #308251;
		}

		.tabview_daodian_rightno {
			width: 345rpx;
			text-align: center;
			color: #308251;
		}
	}

	.topName_view {
		width: 686rpx;
		// height: 311rpx;
		background: #FFFFFF;
		border-radius: 24rpx;
		margin: 0 auto;
		margin-top: -50rpx;
		position: relative;
		padding: 20rpx 30rpx 30rpx;

		.top_address_view {
			width: 72%;
			margin-right: 20rpx;

			.top_address {
				width: 100%;
				font-size: 26rpx;
				font-family: PingFang SC;
				font-weight: bold;
				color: #333333;
				text-overflow: ellipsis;
				white-space: nowrap;
				overflow: hidden;
			}

			.top_time {
				font-size: 24rpx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #666666;
				margin-top: 10rpx;
			}
		}

		.top_address_right {
			width: 25%;
			display: flex;
			justify-content: space-between;

			.top_address_phoneview {
				width: 57rpx;
				height: 57rpx;
				background: #E8FAE1;
				border-radius: 50rpx;
				align-items: center;
				display: flex;
				justify-content: center;

				image {
					width: 32rpx;
					height: 30rpx;
				}
			}

			.top_address_dhtext {
				font-size: 24rpx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #262626;
				margin-top: 10rpx;
			}

			.top_address_dhview {
				width: 57rpx;
				height: 57rpx;

				image {
					width: 57rpx;
					height: 57rpx;
				}
			}
		}


		.topName_logo {
			width: 110rpx;
			height: 110rpx;
			border-radius: 8rpx;
			position: absolute;
			right: 40rpx;
			top: -90rpx;
		}

		.top_xian {
			width: 637rpx;
			height: 1rpx;
			border: 1rpx solid #f5f5f5;
			margin-top: 20rpx;
		}

		.top_box {
			background: #E8FAE1;
			color: #308251;
			padding: 5rpx 15rpx;
			font-size: 24upx;
			letter-spacing: 2rpx;
			border-radius: 8rpx;
			margin-right: 8upx;
		}

		.top_yishou {
			font-size: 22rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #333333;
		}

		.top_scBtn {
			width: 132upx;
			line-height: 57upx;
			background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
			border-radius: 12rpx;
			text-align: center;
			display: flex;
			align-items: center;
			justify-content: center;

			image {
				width: 30upx;
				height: 28upx;
			}

			text {
				font-size: 24rpx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #FFFFFF;
				margin-left: 10rpx;
			}
		}

		.top_name {
			font-size: 32rpx;
			font-family: PingFang SC;
			font-weight: 800;
			color: #333333;
		}
	}

	.byy_view {
		background: #ebebeb;
		width: 120upx;
		text-align: center;
		border-radius: 50rpx;
		height: 46rpx;
		line-height: 46rpx;
		font-size: 24rpx;
		color: #000000;
		// border: 3upx solid #999999;
		margin-top: 6rpx;
	}

	.kfw_view {
		background: #ffaa00;
		width: 120upx;
		text-align: center;
		border-radius: 50rpx;
		height: 46rpx;
		line-height: 46rpx;
		font-size: 24rpx;
		color: #FFFFFF;
		margin-top: 6rpx;
	}

	.kyy_view {
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
		width: 120upx;
		text-align: center;
		border-radius: 50rpx;
		height: 46rpx;
		line-height: 46rpx;
		font-size: 24rpx;
		color: #FFFFFF;
		margin-top: 6rpx;
	}

	.zz_view {
		// width: 220upx;
		background: #CCFFD0;
		color: #55B55C;
		font-size: 24rpx;
		padding: 6rpx 10rpx;
		border-radius: 10rpx;
	}

	.zpmore_view {
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
		width: 120upx;
		text-align: center;
		border-radius: 10rpx;
		height: 42rpx;
		line-height: 42rpx;
		font-size: 24rpx;
		color: #FFFFFF;
		margin-left: 10rpx;
	}
</style>