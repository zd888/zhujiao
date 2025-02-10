<template>
	<view class="">
		<view class="padding-lr padding-top-sm bg flex " style="width: 100%;padding-bottom: 10rpx;background: #ffffff;">
			<view class="flex justify-between margin-right-sm" @tap="Cityshow = true" style="line-height: 68rpx;">
				<image src="../../static/images/index/place.png" style="width: 27rpx;height: 37rpx;"
					class="margin-top-xs"></image>
				<view class="localName  margin-left-sm">{{city}}</view>
			</view>
			<u-search class="flex-sub" placeholder="请输入助教名称" :focus="false" shape="square" v-model="artificerName"
				:show-action="true" :animation="true" bg-color="#F7F7F7" color="#1A1A1A" @search="onSearch()"
				action-text="搜索" @custom="onSearch()"></u-search>
		</view>
		<view style="padding-top: 1rpx;padding-bottom: 30rpx;">
			<view class="flex align-center margin-bottom-sm bg padding-sm radius margin-sm"
				v-for="(item,index) in orderList" :key='index' @click.stop="goOrderDetail(item)"
				style="position: relative;">
				<view style="position: absolute;z-index: 99;top: 0rpx;left: 0;" v-if="item.isGoods == 1">
					<image
						src="https://anmo.xianmxkj.com/file/uploadPath/2023/02/16/c551f3f0721aec54208011e6948a57cd.png"
						style="width: 42rpx;height: 36rpx;"></image>
				</view>
				<view style="width:130upx;height:190upx;border-radius: 10rpx;">
					<image :src="item.artificerImg?item.artificerImg: '../../static/logo.png'"
						@click.stop="saveImgs(item.artificerImg)"
						style="width: 130upx;height: 130upx;border-radius: 50%;" mode="aspectFill"></image>
					<image v-if="item.isHot == 1"
						src="https://anmo.xianmxkj.com/file/uploadPath/2023/02/16/9ae1b7e9fa931b5fc24cd8edecc10d9f.png"
						style="width: 30rpx;height: 36rpx;position: absolute;z-index: 99;top: 40rpx;left: 110rpx;">
					</image>
					<view class="kyy_view" v-if="item.state == 1">可服务</view>
					<view class="kfw_view" v-else-if="item.state == 2">可预约</view>
					<view class="byy_view" v-else>休息中</view>
				</view>
				<view class="margin-left  flex flex-direction" style="width: 85%;">
					<view class="flex align-center justify-between">
						<view class="flex align-center">
							<view class="text-bold text-30 margin-right-xs">{{item.artificerName}}</view>
							<image
								src="https://anmo.xianmxkj.com/file/uploadPath/2023/02/16/d45855dcf1e01210a4224a408225e8e0.png"
								style="width: 40rpx;height:40rpx;">
							</image>
							<view class="zpmore_view" v-if="item.lifePhoto" @click.stop="saveImgss(item.lifePhoto,0)">
								更多照片</view>
						</view>
						<view class="zz_view" v-if="item.status == 1 && item.time && numIndex == 1">最早可约：{{item.time}}
						</view>
						<view class="zz_view" v-if="numIndex == 3">可用余额：{{item.money ? item.money : 0}}</view>
					</view>

					<view class="flex justify-between " style="margin:15rpx 0rpx 20rpx">
						<view class="flex align-center">
							<view v-if="item.ordersScore" class="margin-right-sm">
								<image src="../../static/images/start.png" style="width: 24upx;height: 22upx;">
								</image>
								<text class="margin-left-xs" style="color: #FF1200;">{{item.ordersScore}}.0</text>
							</view>
							<view style="color: #999999;">已服务:{{item.ordersCount}}单</view>
						</view>

						<view class="flex align-center" v-if="item.tripWay != 3 && numIndex != 3">
							<image src="../../static/images/address.png" style="width: 24rpx;height: 29rpx;"></image>
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
						<view v-if="numIndex == 1">
							<view class="text-center text-sm" @tap.stop="goOrder(item)" v-if="item.status == 1"
								style="background: linear-gradient(90deg, #55B55C 0%, #308251 100%);color: #ffffff;width: 135upx;border-radius: 16upx;line-height: 50upx;">
								立即预约
							</view>
							<view class="text-center text-sm" v-else
								style="background: #d9d9d9;color: #ffffff;width: 135upx;border-radius: 16upx;line-height: 50upx;">
								立即预约
							</view>
						</view>
						<view v-if="numIndex == 2">
							<view class="text-center text-sm" @tap.stop="showPay(item)"
								style="background: linear-gradient(90deg, #55B55C 0%, #308251 100%);color: #ffffff;width: 135upx;border-radius: 16upx;line-height: 50upx;">
								为TA充值
							</view>
						</view>
						<view v-if="numIndex == 3">
							<view class="text-center text-sm"
								style="background: linear-gradient(90deg, #55B55C 0%, #308251 100%);color: #ffffff;width: 135upx;border-radius: 16upx;line-height: 50upx;">
								立即预约
							</view>
						</view>
					</view>
				</view>
			</view>
		</view>
		<empty v-if="orderList.length == 0"></empty>
		<u-select v-model="Cityshow" :list="cityList" @confirm="getCityData"></u-select>
		<view style="position: fixed;z-index: 9999;bottom: 160rpx;" v-if="showTP">
			<image src="https://wap.quanmindaojia.com/file/uploadPath/2023/02/16/15ade225e9fa8a1d82b325c06dc2b653.png"
				style="width: 690rpx;margin: 30rpx;height: 600rpx;"></image>
			<view
				style="position: absolute;bottom: 100rpx;width: 100%;text-align: center;display: flex;justify-content: center;">
				<view>
					<view style="color: #333333;font-size: 28rpx;"><text
							style="font-size: 44rpx;color:#ee6c54">{{tpCount}}</text>人已投票</view>
					<view class="tp_view" v-if="tpSel == 2" @tap="saveTP">投票</view>
					<view class="ytp_view" v-if="tpSel == 1">已投票</view>
				</view>
			</view>
		</view>
		<!-- 支付方式 -->
		<u-popup v-model="showpay" mode="bottom">
			<view class="popup_pay">
				<view style="background-color: #fff;">
					<view style="padding: 0 20upx;margin-top: 60rpx;margin-bottom: 20rpx;">
						<view class="padding margin-top" style="background: #f4f4f4;">
							<view class="text-lg margin-bottom">充值金额</view>
							<input type="number" placeholder="请输入充值金额" v-model="price">
							<view style="width: 100%;height: 1rpx;background: #e8e8e8;margin-top: 15rpx;"></view>
						</view>
						<view
							style="display: flex;height: 100upx;align-items: center;padding: 20upx 0;justify-content: center;"
							v-for="(item,index) in openLists" :key='index'>
							<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;">
							</image>
							<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">
								{{item.text}}
							</view>
							<radio-group name="openWay" style="margin-left: 45upx;" @tap='selectWay(item.id)'>
								<label class="tui-radio">
									<radio color="#2FB57A" :checked="openWay === item.id ? true : false" />
								</label>
							</radio-group>
						</view>
					</view>
				</view>
				<view class="pay_btn" @click="pay()">确认支付</view>
			</view>
		</u-popup>
	</view>

	<!-- <u-skeleton :loading="loading" :animation="true" elColor='#FFFFFF' bgColor='#FFFFFF'></u-skeleton> -->
	</view>
</template>

<script>
	import meTabs from "@/components/mescroll-uni/me-tabs/me-tabs.vue";
	import empty from '@/components/empty.vue'

	import RenDropdownFilter from '@/components/ren-dropdown-filter/ren-dropdown-filter.vue'

	export default {
		components: {
			meTabs,
			empty,
			RenDropdownFilter
		},
		data() {
			return {
				Cityshow: false,
				cityList: [],
				tpSel: 0,
				tpCount: 0,
				showTP: false,
				showpay: false,
				price: '',
				numIndex: 1,
				classifyId: '',
				artificerName: '',
				loading: true, // 是否显示骨架屏组件
				defaultSelected: [],
				value1: '',
				value2: '',
				value3: '',
				game: [],
				defaultIndex: [0, 0, 0],
				filterData: [
					[{
							label: '智能优选',
							value: '',
						},
						{
							label: '距离优先',
							value: 3,
						},
						{
							label: '人气优先',
							value: 2,
						},
						{
							label: '同城',
							value: 1,
						}
					],
					[{
							label: '服务',
							value: '0',
						},

						{
							label: '从高到低',
							value: '1',
						},
						{
							label: '从低到高',
							value: '2',
						}
					],
					[{
							label: '距离',
							value: '0',
						},
						{
							label: '从远到近',
							value: '1',
						},
						{
							label: '从近到远',
							value: '2',
						}
					],
				],
				list: [{
					name: '综合',
					id: 1
				}, {
					name: '距离',
					id: 2
				}, {
					name: '销量',
					id: 3
				}, {
					name: '好评',
					id: 4
				}],
				listIndex: 0,
				city: '请选择城市',
				isShop: '',
				latitude: '',
				longitude: '',
				orderList: [],
				page: 1,
				limit: 10,
				token: '',
				XCXIsSelect: '否',
				isVip: false,
				myId: uni.getStorageSync('userId') ? uni.getStorageSync('userId') : '',
				showModal: true,
				arr: [],
				tab: '',
				artificerId: '',
				massageTypeId: '',
				openWay: 1,
				openLists: [],
				consortiaId: '',
				isHome: 1
			}
		},
		onLoad(e) {
			if (e.isShop) {
				this.isShop = e.isShop;
				this.isHome = ''
			}
			if (e.consortiaId) {
				this.consortiaId = e.consortiaId;
			}


			if (e.index) {
				this.numIndex = e.index;
				if (e.index == 2) {
					uni.setNavigationBarTitle({
						title: '为TA充值'
					})
				} else {
					uni.setNavigationBarTitle({
						title: '我的助教'
					})
				}
			} else {
				uni.setNavigationBarTitle({
					title: '选择教练'
				})
			}
			uni.showLoading({
				title: '加载中...',

			});
			// #ifdef MP-WEIXIN
			this.openWay = 1;
			this.openLists = [{
				image: '../../my/static/icon_weixin.png',
				text: '微信',
				id: 1
			}, {
				image: '../../static/images/lingqian.png',
				text: '零钱支付',
				id: 3
			}]
			// #endif
			// #ifndef MP-WEIXIN
			this.openWay = 1;
			this.openLists = [{
				image: '../../my/static/icon_weixin.png',
				text: '微信',
				id: 1
			}, {
				image: '../../my/static/zhifubao.png',
				text: '支付宝',
				id: 2
			}, {
				image: '../../static/images/lingqian.png',
				text: '零钱支付',
				id: 3
			}]
			// #endif
			let that = this
			that.token = uni.getStorageSync('token')
			that.city = uni.getStorageSync('city') ? uni.getStorageSync('city') : '请选择城市'
			if (e.massageTypeId) {
				that.massageTypeId = e.massageTypeId
			}
			if (e.classifyId) {
				that.classifyId = e.classifyId
			}
			uni.getLocation({
				type: 'gcj02',
				geocode: true, //设置该参数为true可直接获取经纬度及城市信息
				success: function(res) {
					console.log(res, '地理位置')
					that.latitude = res.latitude
					that.longitude = res.longitude
					// uni.setStorageSync('latitude', res.latitude)
					// uni.setStorageSync('longitude', res.longitude)

					that.page = 1
					that.getlist()

				},
				fail: function() {
					console.log('获取地址失败')
				}
			})

			// 获取邀请码保存到本地
			if (e.invitation) {
				that.$queue.setData('inviterCode', e.invitation);
			}

			if (this.myId) {
				that.$Request.getT('/app/common/type/235').then(res => { //报名成功通知
					if (res.code == 0) {
						if (res.data && res.data.value) {
							that.arr.push(res.data.value)
						}
					}
				})
				that.$Request.getT('/app/common/type/236').then(res => { //活动即将开始提醒
					if (res.code == 0) {
						if (res.data && res.data.value) {
							that.arr.push(res.data.value)
						}
					}
				})
			}
		},
		onShow() {
			let that = this

			that.city = uni.getStorageSync('city') ? uni.getStorageSync('city') : '请选择城市'
			that.token = uni.getStorageSync('token')
			that.getKTCityList();
			that.getTpCount();
			that.getTpMy();
			if (uni.getStorageSync('token')) {
				that.getIsVip()
			}
			that.myId = uni.getStorageSync('userId')
			// #ifdef MP-WEIXIN
			//订阅
			if (that.myId) {
				if (this.showModal) {
					// this.openMsg()
				}
			}
			// #endif


		},
		methods: {
			getCityData(e) {
				console.log(e[0].label);
				this.city = e[0].label;
				uni.setStorageSync('city', this.city)
				this.getKTCityList();
				this.page = 1
				this.getlist()
			},
			saveTP() {
				if (!this.token) {
					uni.navigateTo({
						url: '/pages/public/login'
					});
					return;
				}
				let userId = this.$queue.getData('userId');
				let userName = this.$queue.getData('userName');
				this.$queue.showLoading('投票中...');
				this.$Request.postJson('/app/message/insertMessage', {
						state: 11,
						content: this.city,
						userName: userName,
						userId: userId
					})
					.then(res => {
						if (res.code === 0) {
							uni.showToast({
								title: '投票成功！'
							});
							this.getTpCount();
							this.getTpMy();
						} else {
							uni.hideLoading();
							uni.showModal({
								showCancel: false,
								title: '投票失败',
								content: res.msg
							});
						}
					});
			},
			getTpMy() {
				this.$Request.getT('/app/message/selectMessageByUserId?page=1&limit=1&state=11').then(res => {
					if (res.code == 0) {
						if (res.data && res.data.list && res.data.list.length > 0) {
							this.tpSel = 1;
						} else {
							this.tpSel = 2;
						}
					}
				});
			},
			getTpCount() {
				this.$Request.getT('/app/message/selectCityCount?city=' + this.city).then(res => {
					if (res.code == 0) {
						this.tpCount = res.data ? res.data : 0;
					}
				});
			},
			getKTCityList() {
				this.$Request.getT('/app/trip/selectTripList').then(res => {
					if (res.code == 0) {
						this.cityList = [];
						let isOpen = false
						let cityName = uni.getStorageSync('city') ? uni.getStorageSync('city') : '请选择城市'
						res.data.forEach((d, index) => {
							if (cityName === d.city) {
								isOpen = true;
							}

							let data = {}
							data.label = d.city
							data.value = index
							this.cityList.push(data);
						});

						if (!isOpen) {
							this.orderList = []
							// this.showTP = true;
						} else {
							this.showTP = false;
						}
					}
				});
			},
			showPay(e) {
				this.showpay = true
				this.artificerId = e.artificerId
			},
			selectWay: function(id) {
				this.openWay = id;
			},
			// 跳转订单
			goOrder(e) {
				uni.navigateTo({
					url: "/pages/index/order/orderDet?isShop=" + this.isShop + '&artificerId=' + e.artificerId +
						'&massageTypeId=' + this
						.massageTypeId + '&tripWay=' + e.tripWay
				})
			},
			saveImgss(imgs, index) {
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
			saveImgs(imgs) {
				let imgArr = [];
				imgArr.push(imgs);
				// //预览图片
				uni.previewImage({
					urls: imgArr,
					current: 0
				});
			},
			onSearch() {
				// this.artificerName = e;
				this.page = 1;
				this.getlist()
			},

			// 开启订阅消息
			openMsg() {
				var that = this
				uni.getSetting({
					withSubscriptions: true, //是否获取用户订阅消息的订阅状态，默认false不返回
					success(ret) {
						// if (ret.subscriptionsSetting.itemSettings && Object.keys(ret.subscriptionsSetting.itemSettings).length == 2) {
						if (ret.subscriptionsSetting.itemSettings) {
							uni.setStorageSync('sendMsg', true)
							uni.openSetting({ // 打开设置页 
								success(rea) {
									// console.log(rea.authSetting)
								}
							});
						} else { // 用户没有点击“总是保持以上，不再询问”则每次都会调起订阅消息
							uni.setStorageSync('sendMsg', false)
							uni.showModal({
								title: '提示',
								content: '为了更好的体验,请绑定消息推送',
								confirmText: '确定',
								cancelText: '取消',
								success: function(res) {
									if (res.confirm) {
										wx.requestSubscribeMessage({
											tmplIds: that.arr,
											success(re) {
												var datas = JSON.stringify(re);
												if (datas.indexOf("accept") != -1) {
													console.log(re)
													// uni.setStorageSync('sendMsg', true)
												}
											},
											fail: (res) => {
												console.log(res)
											}
										})
										// uni.setStorageSync('sendMsg', true)
										console.log('确认')
										that.showModal = false
									} else if (res.cancel) {
										console.log('取消')
										// uni.setStorageSync('sendMsg', false)
										that.showModal = true
									}
								}
							})
						}
					}
				})
			},
			getIsVip() {
				this.$Request.get("/app/UserVip/isUserVip").then(res => {
					if (res.code == 0) {
						this.isVip = res.data
						uni.setStorageSync('isVIP', res.data)
					}
				});
			},
			getlist() {
				let index = this.list[this.listIndex].id
				let data = {
					classifyId: this.classifyId,
					artificerName: this.artificerName,
					page: this.page,
					limit: this.limit,
					consortiaId: this.consortiaId,
					isShop: this.isShop,
					isHome: this.isHome,
					massageTypeId: this.massageTypeId,
					by: index, //价格
					latitude: this.latitude,
					longitude: this.longitude,
					city: this.city
				}
				if (this.token) {
					if (this.numIndex != 3) {

						this.$Request.get("/app/artificer/selectArtificerList", data).then(res => {
							this.loading = false;
							uni.hideLoading()
							if (res.code == 0) {
								if (this.page == 1) {
									this.orderList = [];
								}
								if (res.data.list.length > 0) {
									for (var i = 0; i < res.data.list.length; i++) {
										if (res.data.list[i].lifePhoto) {
											res.data.list[i].lifePhoto = res.data.list[i].lifePhoto.split(',')
										}
										if (res.data.list[i].distance > 1000) {
											res.data.list[i].distance = (res.data.list[i].distance / 1000).toFixed(
													1) +
												'km'
										} else {
											res.data.list[i].distance = (res.data.list[i].distance).toFixed(1) +
												'm'
										}

									}
									if (this.page == 1) {
										this.orderList = res.data.list
									} else {
										this.orderList = [...this.orderList, ...res.data.list]
									}
								} else {
									// this.orderList = []
									// uni.showToast({
									// 	title: '当前城市暂未开通，请切换城市',
									// 	duration: 2000,
									// 	icon: 'none'
									// });
								}

								uni.stopPullDownRefresh();
							}

						})
					} else {
						let userId = this.$queue.getData('userId')
						let data = {
							userId: userId,
							page: this.page,
							limit: this.limit,
							isHome: this.isHome,
							consortiaId: this.consortiaId,
							isShop: this.isShop
						}
						this.$Request.get("/app/userMoney/selectUserMoneyArtificer", data).then(res => {
							this.loading = false;
							uni.hideLoading()
							if (res.code == 0) {
								if (this.page == 1) {
									this.orderList = [];
								}
								res.data.list.forEach(d => {
									if (d.artificer.lifePhoto) {
										d.artificer.lifePhoto = d.artificer.lifePhoto.split(',')
									}
									d.artificer.money = d.money;
									this.orderList.push(d.artificer)
								});
								uni.stopPullDownRefresh();
							}

						})
					}
				} else {
					uni.hideLoading()
				}

			},

			// 筛选
			change(index) {
				this.listIndex = index
				// this.value1 = e[0][0].value
				// this.value2 = e[1][0].value
				// this.value3 = e[2][0].value
				this.page = 1
				this.getlist()
			},
			// 选择城市
			goSelectCity() {
				// uni.navigateTo({
				// 	url: '/pages/index/citys/citys'
				// });
				let that = this
				uni.chooseLocation({
					success: function(res) {
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
						that.latitude = res.latitude
						that.longitude = res.longitude
						that.selectCity(that.longitude, that.latitude);
					}
				});
			},
			selectCity(longitude, latitude) {
				this.$Request.get('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					if (res.code == 0) {
						this.city = res.data.city ? res.data.city : '未知'
						uni.setStorageSync('city', res.data.city)
						this.page = 1
						this.getlist()
					}
				});
			},
			// 跳转游戏列表
			goNav(url) {
				console.log(url, '1111112333')
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				if (url.indexOf('/pages/') !== -1) {
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
			},
			// 跳转搜索
			goSearch() {
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							// console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				uni.navigateTo({
					url: '/pages/index/search/index'
				});
			},
			// 跳转订单
			goOrder(e) {
				console.log('授权', uni.getStorageSync('sendMsg'))
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				if (this.token) {
					uni.navigateTo({
						url: "/pages/index/order/orderDet?artificerId=" + e.artificerId + '&massageTypeId=' + this
							.massageTypeId + '&isShop=' + this.isShop
					})
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					});
				}
			},
			goOrderDetail(e) {
				if (this.numIndex == 1) {

					if (e.status != 1) {
						this.$queue.showToast('当前助教已下线，请预约其他助教！')
						return;
					}
					uni.navigateTo({
						url: "/pages/index/order/orderDetail?artificerId=" + e.artificerId + '&massageTypeId=' +
							this
							.massageTypeId
					})
				} else {
					uni.navigateTo({
						url: '/pages/therapist/orderDetail?artificerId=' + e.artificerId
					});
				}
			},
			pay() {
				this.showpay = false;
				if (this.price === '') {
					this.$queue.showToast('请输入充值金额!')
					return;
				}
				this.$queue.showLoading('支付中...')
				let that = this;
				if (this.openWay == 1) {
					// #ifdef MP-WEIXIN
					that.$Request.postT("/app/wxPay/payUserMoneyArtificer", {
						artificerId: that.artificerId,
						money: that.price,
						type: 3
					}).then(red => {
						if (red.code == 0) {
							uni.requestPayment({
								provider: 'wxpay',
								timeStamp: red.data.timestamp,
								nonceStr: red.data.noncestr,
								package: red.data.package,
								signType: red.data.signType,
								paySign: red.data.sign,
								success: function(redd) {
									uni.hideLoading();
									uni.showToast({
										title: '充值成功！'
									})
								},
								fail: function(err) {
									uni.hideLoading();
									that.$queue.showToast(
										'支付失败');
								}
							});
						} else {
							uni.showToast({
								title: red.msg,
								icon: 'none'
							})
						}
					});
					// #endif

					// #ifdef H5
					let ua = navigator.userAgent.toLowerCase();
					console.log(ua)
					if (ua.indexOf('micromessenger') !== -1) {
						that.$Request.post("/app/wxPay/payUserMoneyArtificer", {
							money: that.price,
							artificerId: that.artificerId,
							type: 2
						}).then(red => {
							if (red.code == 0) {
								that.callPay(red.data);
							} else {
								uni.showToast({
									title: red.msg,
									icon: 'none'
								})
							}
						});
					} else {
						that.$Request.post("/app/wxPay/payUserMoneyArtificer", {
							money: that.price,
							artificerId: that.artificerId,
							type: 4
						}).then(red => {
							if (red.code == 0) {
								const urlArr = window.location.href;
								const hostUrl = urlArr.split("/");
								const callBack = hostUrl[0] + "//" + hostUrl[2] + "/";
								const url = '&redirect_url=' + callBack + 'my/vip/index';
								window.location = red.data.mweb_url + url
							} else {
								uni.showToast({
									title: red.msg,
									icon: 'none'
								})
							}
						});
					}
					// #endif

					// #ifdef APP-PLUS
					that.$Request.post("/app/wxPay/payUserMoneyArtificer", {
						money: that.price,
						artificerId: that.artificerId,
						type: 1
					}).then(red => {
						if (red.code == 0) {
							console.log(JSON.stringify(red))
							that.setPayment('wxpay', JSON.stringify(red
								.data));
						} else {
							uni.showToast({
								title: red.msg,
								icon: 'none'
							})
						}
					});
					// #endif
				} else if (this.openWay == 2) {
					// #ifdef H5
					that.$Request.post("/app/aliPay/payUserMoneyArtificer", {
						money: that.price,
						artificerId: that.artificerId,
						classify: 2
					}).then(red => {
						if (red.code == 0) {
							const div = document.createElement('div')
							div.innerHTML = red.data //此处form就是后台返回接收到的数据
							document.body.appendChild(div)
							document.forms[0].submit()
						} else {
							uni.showToast({
								title: red.msg,
								icon: 'none'
							})
						}
					});
					// #endif
					// #ifdef APP-PLUS
					that.$Request.post("/app/aliPay/payUserMoneyArtificer", {
						money: that.price,
						artificerId: that.artificerId,
						classify: 1
					}).then(red => {
						if (red.code == 0) {
							that.setPayment('alipay', red.data);
						} else {
							uni.showToast({
								title: red.msg,
								icon: 'none'
							})
						}
					});
					// #endif
				} else if (this.openWay == 3) {
					uni.hideLoading();
					uni.showModal({
						title: '付款提示',
						content: '确认支付' + that.price + '元吗?',
						success: function(re) {
							if (re.confirm) {
								that.$queue.showLoading('支付中...')
								// console.log('用户点击确定');
								that.$Request.post("/app/userMoney/payUserMoneyArtificer", {
									money: that.price,
									artificerId: that.artificerId,
									classify: 1
								}).then(ret => {
									uni.hideLoading();
									if (ret.code == 0) {
										uni.showToast({
											title: '充值成功',
											icon: 'none'
										})
									} else {
										uni.showToast({
											title: ret.msg,
											icon: 'none'
										})
									}
								});
							}
						}
					})
				}
			},
			callPay: function(response) {
				if (typeof WeixinJSBridge === "undefined") {
					if (document.addEventListener) {
						document.addEventListener('WeixinJSBridgeReady', this.onBridgeReady(response), false);
					} else if (document.attachEvent) {
						document.attachEvent('WeixinJSBridgeReady', this.onBridgeReady(response));
						document.attachEvent('onWeixinJSBridgeReady', this.onBridgeReady(response));
					}
				} else {
					this.onBridgeReady(response);
				}
			},
			onBridgeReady: function(response) {
				let that = this;
				if (!response.package) {
					return;
				}
				WeixinJSBridge.invoke(
					'getBrandWCPayRequest', {
						"appId": response.appid, //公众号名称，由商户传入
						"timeStamp": response.timestamp, //时间戳，自1970年以来的秒数
						"nonceStr": response.noncestr, //随机串
						"package": response.package,
						"signType": response.signType, //微信签名方式：
						"paySign": response.sign //微信签名
					},
					function(res) {
						if (res.err_msg === "get_brand_wcpay_request:ok") {
							// 使用以上方式判断前端返回,微信团队郑重提示：
							//res.err_msg将在用户支付成功后返回ok，但并不保证它绝对可靠。
							uni.hideLoading();
							uni.showToast({
								title: '充值成功！'
							})
						} else {
							uni.hideLoading();
						}
						WeixinJSBridge.log(response.err_msg);
					}
				);
			},
			setPayment(name, order) {
				let that = this;
				uni.requestPayment({
					provider: name,
					orderInfo: order, //微信、支付宝订单数据
					success: function(res) {
						uni.hideLoading();
						uni.showToast({
							title: '充值成功！'
						})
					},
					fail: function(err) {
						uni.hideLoading();
						console.log(12)
					}
				});
			}
		},
		onReachBottom: function() {
			this.page = this.page + 1;
			this.getlist()
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.getlist()
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #F7F7F7;
	}

	.ytp_view {
		background: #e3e3e3;
		width: 400rpx;
		border-radius: 10rpx;
		font-size: 28rpx;
		text-align: center;
		height: 80rpx;
		line-height: 80rpx;
		color: #848484;
		margin-top: 40rpx;
	}

	.tp_view {
		background: #ee6c54;
		width: 400rpx;
		border-radius: 10rpx;
		font-size: 28rpx;
		text-align: center;
		height: 80rpx;
		line-height: 80rpx;
		color: #FFFFFF;
		margin-top: 40rpx;
	}

	.popup_pay {
		width: 100%;
		position: relative;
		padding-bottom: 45rpx;
	}

	.pay_btn {
		width: 90%;
		margin: 0 auto;
		text-align: center;
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
		height: 80rpx;
		border-radius: 16rpx;
		color: #ffffff;
		line-height: 80rpx;

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

	.bg {
		background: #FFFFFF;
	}

	.ytp_view {
		background: #e3e3e3;
		width: 400rpx;
		border-radius: 10rpx;
		font-size: 28rpx;
		text-align: center;
		height: 80rpx;
		line-height: 80rpx;
		color: #848484;
		margin-top: 40rpx;
	}

	.tp_view {
		background: #ee6c54;
		width: 400rpx;
		border-radius: 10rpx;
		font-size: 28rpx;
		text-align: center;
		height: 80rpx;
		line-height: 80rpx;
		color: #FFFFFF;
		margin-top: 40rpx;
	}

	.sticky-tabs {
		z-index: 990;
		position: sticky;
		top: var(--window-top);
		// background-color: #fff;
	}

	/* // 使用mescroll-uni,则top为0 */
	.mescroll-uni,
	/deep/.mescroll-uni {
		.sticky-tabs {
			top: 0;
		}
	}

	.demo-tip {
		padding: 18upx;
		font-size: 24upx;
		text-align: center;
	}

	.line_s {
		display: inline-flex;
		width: 10rpx;
		height: 10rpx;
		background: #1AD566;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.line_x {
		display: inline-flex;
		width: 10rpx;
		height: 10rpx;
		background: #000000;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.box {
		// border: 3rpx solid #005dff;
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