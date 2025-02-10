<template>
	<view style="padding-bottom: 100rpx;">
		<view>
			<view style="width: 100%;text-align: center;" @click="saveImgs(order.massageImg)">
				<image style="width: 750upx;height: 480upx;" mode="widthFix" :src="order.massageImg"></image>
			</view>
			<view class="margin">
				<view
					style="width: 686rpx;background: #FFFFFF;border: 1rpx solid #30B47A;border-radius: 24rpx;padding: 26rpx 20rpx;position: relative;z-index: 9;">
					<view class="padding-bottom-xs flex justify-between ">
						<view class="flex align-center" v-if="!isVip">
							<view style="color:#FF1200;font-size:26upx;font-weight: bold;">
								￥<text style="font-size: 40upx;">{{order.price}}</text>
								<!-- <text class="oldPrice_view">￥{{order.oldPrice}}</text> -->
							</view>
							<view class="flex align-center margin-left-sm"
								style="background: #EDFAF7;border-radius: 20rpx;padding: 6rpx 14rpx;">
								<image src="../../../static/time.png"
									style="width: 28rpx;height: 28rpx;margin-right: 5rpx;"></image>
								<view style="color: #308251;font-size: 24rpx;">{{order.duration}}分钟</view>
							</view>
						</view>
						<view class="flex align-center" style="color:#26C278;font-size: 24upx;" v-else>
							<view class="box">会员价</view>
							<view style="color:#FF1200;font-size:26upx;font-weight: bold;">
								￥<text style="font-size: 40upx;">{{order.memberPrice}}</text>
							</view>
							<view class="flex align-center margin-left-sm"
								style="background: #EDFAF7;border-radius: 20rpx;padding: 6rpx 14rpx;">
								<image src="../../../static/time.png"
									style="width: 28rpx;height: 28rpx;margin-right: 5rpx;"></image>
								<view style="color: #308251;font-size: 24rpx;">{{order.duration}}分钟</view>
							</view>
						</view>
						<view style="color: #999999;font-size: 26rpx;">已售{{order.sales}}</view>
					</view>
					<view class="margin-right-xs text-bold" style="letter-spacing: 3rpx; font-size: 36rpx;">
						{{order.title}}
					</view>

				</view>







				<view class="flex align-center padding-tb-sm"
					style="width: 686rpx;background: linear-gradient(90deg, #55B55C 0%, #308251 100%);border-radius: 24rpx;padding-top: 60rpx;margin-top: -40rpx;padding-left: 20rpx;">
					<!-- 安心购 -->
					<image
						src="https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/009603feb20d2441808c664a8acecf2e.png"
						style="width: 91rpx;height:32rpx;margin-right: 40rpx;"></image>

					<view class="flex align-center" style="margin-right: 30upx;color: #FFFFFF;">
						<image src="../../../static/images/index/renz1.png" style="width: 32upx;height: 28upx;"></image>
						<text class="margin-left-xs text-sm">实名认证</text>
					</view>
					<view class="flex align-center" style="margin-right: 30upx;color: #FFFFFF;">
						<image src="../../../static/images/index/renz2.png" style="width:28upx;height: 33upx;"></image>
						<text class="margin-left-xs text-sm">资质认证</text>
					</view>
					<view class="flex align-center" style="color: #FFFFFF;">
						<image src="../../../static/images/index/renz3.png" style="width: 30upx;height: 32upx;"></image>
						<text class="margin-left-xs text-sm">平台担保</text>
					</view>
				</view>
				<view class="govip_view" v-if="!isVip && hyCheck != '否'">
					<image
						src="https://anmo.xianmxkj.com/file/uploadPath/2023/02/17/0b2a9576259a35f7f0cbf8b9d3ee3df7.png">
					</image>
					<view class="govip_viewitem flex justify-between">
						<view style="color: #e5c588;">开通会员立享会员折扣价</view>
						<view class="govip_btn" @tap="goVip">去开通</view>
					</view>
				</view>
			</view>
		</view>

		<view class="bg padding margin radius24">
			<view class="text-bold padding-bottom flex align-center" style="font-size: 34rpx;">
				<view class="box_xian"></view>适用说明
			</view>
			<view class="flex padding-bottom-sm">
				<view style="color: #666666;">性别限制</view>
				<view class="margin-left">{{order.isSex}}</view>
			</view>
			<view class="flex padding-bottom-xs">
				<view style="color: #666666;">适用人群</view>
				<view class="margin-left">{{order.applyPeople}}</view>
			</view>
		</view>
		<view class="bg padding margin radius24" v-if="order.content">
			<view class="text-bold padding-bottom flex align-center" style="font-size: 34rpx;">
				<view class="box_xian"></view>服务内容：
			</view>
			<view v-html="order.content"></view>
		</view>
		
		<view class="margin radius24 radius bg">
			<!-- <view class="padding text-bold" style="font-size: 34rpx;">项目详情</view> -->
			<view class="padding text-bold flex align-center" style="font-size: 34rpx;color: #333333;">
				<view class="box_xian"></view>项目详情
			</view>
			<view v-for="(item,index) in order.contentImg" :key="index" style="width: 100%;"
				@click="saveImg(order.contentImg,index)">
				<image :src="item" style="width: 750upx;height: 963upx;" mode="widthFix"></image>
			</view>
		</view>
		
		<view class=" padding bg u-skeleton-fillet margin radius24" style="padding-bottom: 100rpx;">
			<view class="text-bold padding-bottom-sm flex align-center" style="font-size: 34rpx;">
				<view class="box_xian"></view>服务评价({{commentList.length}})
			</view>
		
			<view v-if="commentList.length<=0" class=""> 暂无评价</view>
			<view class="margin-bottom-sm padding-tb-sm" style="border-bottom: 1upx solid #f0f0f0;"
				v-for="(item, index) in commentList" :key='index' v-else>
				<view class="flex justify-between">
					<u-avatar src="/static/logo.png" size="48"></u-avatar>
					<view class="flex-sub margin-left-sm" style="line-height: 46upx;">匿名</view>
					<view class="flex">
						<u-icon v-for="ite in item.score" :key='ite' color="#FFAA01" name="star-fill"></u-icon>
					</view>
				</view>
				<view class="margin-top-sm">{{item.content}}</view>
			</view>
		</view>
		
		<view class=" text-lg cu-bar foot padding-lr padding-tb-sm" v-if="XCXIsSelect !='否'" style="box-shadow: none;">
			<view class="text-center save_btn" v-if="order.classifyName === '预约助教'"
				@click="goNav(order.massageTypeId,order.classifyId)">
				选择助教
			</view>
			<view class="text-center save_btn" v-if="order.classifyName === '预约教练'"
				@click="goNav(order.massageTypeId,order.classifyId)">
				选择教练
			</view>
			<view class="text-center save_btn" v-if="order.classifyName != '预约教练' && order.classifyName != '预约助教'"
				@click="goNav(order.massageTypeId,order.classifyId)">
				选择教练
			</view>
		</view>
		<!-- <u-picker v-model="show" mode="time" :params="params" @confirm="statusChange"></u-picker> -->
	</view>
</template>

<script>
	export default {
		data() {
			return {
				game: [],
				isShop: '',
				customStyle: {
					width: '400upx',
					color: '#FFFFFF',
					background: "#005DFF",
					border: 0
				},
				massageTypeId: '',
				orderDet: {},
				page: 1,
				limit: 10,
				order: {},

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
					name: '服务',
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
				fw: [],
				count: 5,
				value: 4,
				XCXIsSelect: '否',
				hyCheck: '否',
				commentList: [],
				page: 1,
				limit: 10,
				consortiaId: ''
			}
		},
		onLoad(option) {
			console.log(option)
			if (option.isShop) {
				this.isShop = option.isShop;
			}
			if (option.consortiaId) {
				this.consortiaId = option.consortiaId
			}
			this.$queue.showLoading("加载中...");
			this.massageTypeId = option.massageTypeId
			this.XCXIsSelect = this.$queue.getData("XCXIsSelect");
			this.getDet()
			this.getOrderComment()

			this.myId = uni.getStorageSync('userId')
			this.hyCheck = this.$queue.getData('hyCheck');
			if (this.hyCheck != '否') {
				this.isVip = uni.getStorageSync('isVIP')
			}
		},
		onShow() {
			if (this.massageTypeId) {
				this.getDet()
				this.hyCheck = this.$queue.getData('hyCheck');
				if (this.hyCheck != '否') {
					this.isVip = uni.getStorageSync('isVIP')
				}
				this.myId = uni.getStorageSync('userId')
				this.getDet()
			}
		},
		onReady() {
			this.AUDIO.onEnded(function(res) {
				this.isPlay = false;
			});
		},
		methods: {
			goVip() {
				uni.navigateTo({
					url: '/my/vip/index'
				});
			},
			// 评价
			getOrderComment() {
				this.$Request.get("/app/takingComment/selectOrderTakingComment", {
					massageTypeId: this.massageTypeId,
					page: this.page,
					limit: this.limit
				}).then(res => {
					if (res.code == 0) {
						this.commentList = [...this.commentList, ...res.data.list]
					}
				});
			},
			// 切换菜单
			tabChange(index) {
				this.tabIndex = index
				this.fwIndex = index
			},
			// 详情
			getDet() {
				this.$Request.get("/app/artificer/selectMassageTypeById", {
					massageTypeId: this.massageTypeId,
				}).then(res => {

					if (res.code == 0) {
						this.order = res.data
						if (this.order.isSex == 1) {
							this.order.isSex = '男';
						} else if (this.order.isSex == 2) {
							this.order.isSex = '女';
						} else {
							this.order.isSex = '不限性别';
						}
						this.order.contentImg = this.order.contentImg.split(",");
						this.selectFollow()

					}
					uni.hideLoading();
				});

			},

			// 是否关注
			selectFollow() {
				this.$Request.get("/app/userFollow/selectFollowUser", {
					followUserId: this.order.userId
				}).then(res => {
					if (res.data == true) {
						this.isFollow = true
					} else {
						this.isFollow = false
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
			goNav(massageTypeId, classifyId) {
				uni.navigateTo({
					url: '/pages/therapist/therapistList?isShop=' + this.isShop + '&massageTypeId=' +
						massageTypeId + '&classifyId=' +
						classifyId + '&consortiaId=' + this.consortiaId
				})
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
			// 关注
			follow() {
				let that = this
				that.$Request.get("/app/userFollow/insert", {
					followUserId: that.order.userId
				}).then(res => {
					uni.showToast({
						title: res.msg,
						icon: 'none'
					})
					setTimeout(function() {
						that.selectFollow()
					}, 500)
				});
			},
			saveImg(imgs, index) {
				let that = this;
				let imgArr = imgs
				// imgArr.push(imgs);
				// //预览图片
				uni.previewImage({
					urls: imgArr,
					current: imgArr[index]
				});
			},
			saveImgs(imgs) {
				console.log(imgs)
				// let that = this
				// let imgArr = imgs
				this.imgArr.push(imgs);
				console.log(this.imgArr)
				// //预览图片
				uni.previewImage({
					urls: this.imgArr,
					current: this.imgArr[0]
				});
			},
		}

	}
</script>

<style lang="scss">
	page {
		background-color: #F7F7F7;
		// background-color: #333333;
	}

	.bgs {
		background-color: #FFFFFF;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.box_xian {
		width: 9rpx;
		height: 29rpx;
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
		border-radius: 5rpx;
		margin-right: 14rpx;
	}

	.save_btn {
		width: 665upx;
		line-height: 90upx;
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
		border-radius: 50upx;
		margin: 0 auto;
		color: #FFFFFF;
		// border-bottom: 10rpx solid #fae959;
		font-weight: bold;
	}

	.govip_view {
		margin-top: 20rpx;
		width: 686rpx;
		height: 90rpx;

		.govip_viewitem {
			position: absolute;
			left: 120rpx;
			height: 90rpx;
			line-height: 90rpx;
			width: 580rpx;
			align-items: center;

			.govip_btn {
				background: #e5c588;
				border-radius: 50rpx;
				width: 140rpx;
				text-align: center;
				height: 50rpx;
				line-height: 50rpx;
				color: #333333;
			}
		}

		image {
			width: 686rpx;
			height: 90rpx;
			position: absolute;
			border-radius: 24rpx;
		}
	}

	.wrap {
		padding: 20upx 23upx;
	}

	.oldPrice_view {
		font-size: 22rpx;
		font-family: PingFang SC;
		font-weight: 400;
		text-decoration: line-through;
		margin-left: 8rpx;
		color: #c7c7c7;
	}

	.rir {
		font-size: 30upx;
		font-family: DFZongYi-Bd-80-Win-GB;
		font-weight: 800;
		color: #30B47A;
		margin-right: 30rpx;
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
		border-bottom: 2upx solid #F5F5F2;
	}

	.tab>view:last-of-type {
		border: none;
	}

	.box {

		background: #E8FAE1;
		color: #308251;
		padding: 5rpx 15rpx;
		font-size: 26rpx;
		letter-spacing: 2rpx;
		border-radius: 8rpx;
		// margin-top: 10rpx;
		margin-right: 8upx;
	}
</style>