<template>
	<view>
		<view class="content">
			<image src="https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/bf4f7fced040b620fffdde26df296515.png"
				style="width: 750rpx;height: 380rpx;"></image>
			<view style="position: absolute;z-index: 99;top: 30rpx;">
				<image
					src="https://anmo7.0.xianmaxiong.com/file/uploadPath/2023/12/12/df7227862820ba0157d6a272d80f043c.png"
					style="width: 66rpx;height: 81rpx;position: absolute;top: 40rpx;right: 10rpx;"></image>

				<swiper class="screen-swiper" style="height: 250upx;width: 680rpx;margin: 0 auto;margin-top: 10rpx;"
					:circular="true" :autoplay="true" interval="2500" duration="800">
					<swiper-item v-for="(item,index) in swiperList" :key="index" @tap="goNav(item.url)">
						<image :src="item.imageUrl" class="bannerImg"></image>
					</swiper-item>
				</swiper>
			</view>


			<!-- <view style="width: 686rpx;margin: 0 auto;margin: 20rpx 0rpx 20rpx;" v-if="jsList.length > 0">
				<view class="flex align-end">
					<image
						src="https://anmo5.0.xianmaxiong.com/file/uploadPath/2023/12/13/a22a7b62558c55318ddd72e5fb5fef7b.png"
						style="width: 156rpx;height: 36rpx;"></image>
					<view
						style="font-size: 22rpx;font-family: PingFang SC;font-weight: 500;color: #308251;margin-left: 20rpx;">
						为你提供高品质服务师傅
					</view>
				</view>

				<scroll-view :show-scrollbar="false" scroll-with-animation :scroll-x="true"
					style="width: 686rpx;height: 290rpx;white-space: nowrap;">
					<view class="flex">
						<view v-for="(item,index) in jsList" @tap="goJiShi(item)">
							<view class="js_view text-center padding-top">
								<image :src="item.artificerImg?item.artificerImg: '../../static/logo.png'"
									style="width: 132rpx;height: 132rpx;border-radius: 100rpx;border: 3rpx solid #20C675;">
								</image>

								<view v-if="item.isHot == 1" class="js_view_youzhi">优质</view>

								<view class="js_view_name">{{item.artificerName}}</view>
								<view class="js_view_fuwu">已服务:{{item.ordersCount}}单</view>
							</view>
						</view>
					</view>
				</scroll-view>
			</view> -->

			<view style="background: #FFFFFF;border-radius: 24rpx;margin-top: -70rpx;position: relative;z-index: 99;">
				<view class="tabsx">
					<view v-for="(item,index) in list" :key="index" :class="listIndex==index?'active':''"
						@click="change(index)">
						<view class="flex">
							<view style="text-align: center;">
								<view>{{item.name}}</view>
								<view v-if="listIndex==index" style="margin-top: -10rpx;">
									<image
										src="https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/a158a7aa25438bbd3ce1e717c06141e3.png"
										style="width: 34rpx;height: 12rpx;"></image>
								</view>
							</view>
						</view>
					</view>
				</view>
			</view>
			<view class="list_item" v-if="djSel == 1" v-for="(item,index) in orderList" :key="index"
				@tap="goOrder(item)">
				<image :src="item.massageImg" mode="aspectFill" style="border-radius: 24rpx;"></image>
				<view class="listitem_rightview">
					<view class="titleview">
						<view class="item_title">{{item.title}}</view>
						<view class="item_yishou">已售{{item.sales}}</view>
					</view>
					<view class="flex flex-wrap margin-top-sm" v-if="item.labels.length>0">
						<text class="margin-bottom-xs box" v-for="(ites,index) in item.labels"
							:key="index">{{ites}}</text>
					</view>
					<view class="btnview align-center" :style="item.labels.length > 0 ? '' : 'margin-top:20rpx'">
						<view class="item_money flex align-center">
							<view><text>￥</text>{{item.price}}
								<!-- <text class="oldPrice_view">{{item.duration}}分钟</text> -->
								<!-- ￥{{item.oldPrice}} -->
							</view>
							<view class="flex justify-between margin-tb-sm align-center margin-left"
								style="color: #999999;font-size: 22rpx;">
								<image src="../../static/time.png"
									style="width: 26rpx;height: 26rpx;margin-right: 8rpx;"></image>
								<view>{{item.duration}}分钟</view>
							</view>
						</view>

						<view class="item_btn" v-if="item.classifyName === '预约助教'" @click.stop="goOrder(item)">
							选择助教
						</view>
						<view class="item_btn" v-if="item.classifyName === '预约教练'" @click.stop="goOrder(item)">
							选择教练
						</view>
						<view class="item_btn" v-if="item.classifyName != '预约教练' && item.classifyName != '预约助教'"
							@click.stop="goOrder(item)">
							选择教练
						</view>
					</view>
				</view>
			</view>
			<view class="list_item" v-if="djSel == 2" v-for="(item,index) in orderList" :key="index"
				@tap="goOrder(item)">
				<image :src="item.logo" mode="aspectFill" class="shop_image"></image>
				<view class="listitem_rightview">
					<view class="titleview">
						<view class="item_title">{{item.consortiaName}}</view>
					</view>
					<view class="flex margin-top-xs justify-between">
						<view class="flex">
							<view v-if="item.score" class="margin-right-sm">
								<image src="../../static/images/start.png" style="width: 24upx;height: 22upx;">
								</image>
								<text class="margin-left-xs" style="color: #FF1200;">{{item.score}}</text>
							</view>
							<view class="item_yishou">销量{{item.sales}}</view>
						</view>
						<view class="item_yishou">{{item.distance ? item.distance : 0}}</view>
					</view>
					<view class="flex flex-wrap margin-top-xs" v-if="item.labels.length>0">
						<text class="margin-bottom-xs box" v-for="(ites,index) in item.labels.slice(0,2)"
							:key="index">{{ites}}</text>
					</view>
					<view class="btnview align-center" :style="item.labels.length > 0 ? '' : 'margin-top:20rpx'">
						<view class="item_time flex align-center">
							<text>营业时间：{{item.startBusinessHours}}-{{item.endBusinessHours}}
							</text>
						</view>
					</view>
				</view>
			</view>
		</view>
		<empty v-if="orderList.length==0" style="position: relative;top: -120rpx;z-index: 9;"></empty>
		<view style="position: fixed;z-index: 999;bottom: 160rpx;" v-if="showTP">
			<image src="https://anmo.xianmxkj.com/file/uploadPath/2023/02/16/15ade225e9fa8a1d82b325c06dc2b653.png"
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
		<!-- 新人红包 -->
		<uni-popup ref="popushongbao" type="center">
			<view>
				<image @tap="GetQuan"
					src="https://dangao.xianmaxiong.com/file/uploadPath/2022/08/26/5921dfad9a0003f819afe03332925f18.png"
					style="width: 564upx;height:618upx "></image>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import empty from '@/components/empty.vue'
	import RenDropdownFilter from '@/components/ren-dropdown-filter/ren-dropdown-filter.vue'
	export default {
		components: {
			RenDropdownFilter,
			empty
		},
		onShareAppMessage(res) { //发送给朋友
			return {
				title: this.tuiguang,
				path: '/pages/index/index?invitation=' + this.invitationCode,
				imageUrl: this.tuiguangImg,
			}
		},
		onShareTimeline(res) { //分享到朋友圈
			return {
				title: this.tuiguang,
				path: '/pages/index/index?invitation=' + this.invitationCode,
				imageUrl: this.tuiguangImg,
			}
		},
		data() {
			return {
				jsList: [],
				djSel: 2,
				tpSel: 0,
				tpCount: 0,
				showTP: false,
				invitationCode: '',
				tuiguang: '',
				tuiguangImg: '',
				city: '请选择城市',
				latitude: '',
				longitude: '',
				page: 1,
				limit: 10,
				orderList: [],
				tabIndex: 0,
				swiperList: [],
				typeList: [],
				value1: 0,
				value2: 0,
				value3: 0,
				defaultIndex: [0, 0, 0],
				classifyId: '',
				filterData: [
					[{
						label: '推荐',
						value: '',
					}, ],
					[{
							label: '价格',
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
							label: '销量',
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
				],
				txAppKey: '',
				list: [{
					name: '综合排序',
					id: 1
				}, {
					name: '价格',
					id: 2
				}, {
					name: '销量',
					id: 3
				}, {
					name: '好评度',
					id: 4
				}],
				listn: [{
					name: '综合排序',
					id: 1
				}, {
					name: '价格',
					id: 2
				}, {
					name: '销量',
					id: 3
				}, {
					name: '好评度',
					id: 4
				}],
				lists: [{
					name: '综合排序',
					id: 1
				}, {
					name: '销量',
					id: 2
				}, {
					name: '好评度',
					id: 3
				}],
				checkDj: '否',
				arr: [],
				showModal: true,
				listIndex: 0,
				XCXIsSelect: '否',
			}
		},
		onLoad(e) {
			let that = this
			// 获取邀请码保存到本地
			if (e.invitation) {
				that.$queue.setData('inviterCode', e.invitation);
			}
			// #ifdef MP-WEIXIN
			if (e.scene) {
				const scene = decodeURIComponent(e.scene);
				that.$queue.setData('inviterCode', scene.split(',')[0]);
			}
			// #endif
			that.$Request.getT('/app/common/type/250').then(res => { //技师完成通知
				if (res.code == 0) {
					if (res.data && res.data.value) {
						that.arr.push(res.data.value)
					}
				}
			})

			that.$Request.getT('/app/common/type/248').then(res => { //技师取消通知
				if (res.code == 0) {
					if (res.data && res.data.value) {
						that.arr.push(res.data.value)
					}
				}
			})
			that.$Request.getT('/app/common/type/217').then(res => { //腾讯地图key
				if (res.code == 0) {
					if (res.data && res.data.value) {
						that.txAppKey = res.data.value;
					}
				}
			})

			that.$Request.getT('/app/common/type/431').then(res => { //是否开启到店模式	431
				if (res.code == 0) {
					if (res.data && res.data.value) {
						that.checkDj = res.data.value;
						that.$queue.setData('checkDj', that.checkDj);
					}
				}
			})

			that.getBannerList();
			that.getZiZhi()
			that.XCXIsSelect = that.$queue.getData('XCXIsSelect');
			that.invitationCode = uni.getStorageSync('invitationCode')
			uni.getLocation({
				type: 'gcj02',
				geocode: true, //设置该参数为true可直接获取经纬度及城市信息
				success: function(res) {
					console.log(res, '地理位置')
					that.latitude = res.latitude
					that.longitude = res.longitude
					uni.setStorageSync('latitude', res.latitude)
					uni.setStorageSync('longitude', res.longitude)

					// #ifdef APP-PLUS
					that.city = res.address.city
					that.selectCity(that.longitude, that.latitude);
					uni.setStorageSync('city', res.address.city)
					// that.page = 1
					// that.getorderlist('')
					that.getKTCityList();
					that.getjsList();
					// #endif

					// #ifdef H5
					that.selectCity(that.longitude, that.latitude);
					// #endif

					// #ifdef MP-WEIXIN
					uni.request({
						url: 'https://apis.map.qq.com/ws/geocoder/v1/?location=' + that.latitude +
							',' + that.longitude + '&key=' + that.txAppKey,
						success(re) {
							if (re.statusCode === 200) {
								let citydata = re.data.result.address_component.city
								// console.log("获取城市名称成功", citydata)/
								that.city = citydata ? citydata : '未知'
								uni.setStorageSync('city', citydata)
								that.getKTCityList();
								that.getjsList();
								// that.page = 1
								// that.getorderlist('')
							} else {
								console.log("获取信息失败，请重试！")
							}
						}
					});
					// #endif
				},
				fail: function() {
					console.log('获取地址失败')
				}
			})
			that.page = 1
			that.getorderlist('')
		},
		onShow() {
			this.XCXIsSelect = this.$queue.getData("XCXIsSelect");
			this.$Request.get('/app/common/type/252').then(res => {
				if (res.code == 0) {
					// #ifdef MP-WEIXIN
					this.$queue.setData('XCXIsSelect', res.data.value);
					// #endif
					// #ifndef MP-WEIXIN
					this.$queue.setData('XCXIsSelect', '是');
					// #endif
					this.XCXIsSelect = this.$queue.getData("XCXIsSelect");
				}
			});
			let userId = this.$queue.getData('userId');
			if (userId) {
				this.getTpMy();
				this.getTpCount();
				this.checkNewUser();
				// #ifdef MP-WEIXIN
				//订阅
				if (this.showModal) {
					this.openMsg()
				}
				// #endif
				this.city = uni.getStorageSync('city')
				this.getKTCityList();
				this.latitude = uni.getStorageSync('latitude')
				this.longitude = uni.getStorageSync('longitude')

			}
			this.page = 1
			this.getorderlist('')
		},
		methods: {
			goJiShi(item) {
				let userId = this.$queue.getData('userId');
				if (!userId) {
					uni.navigateTo({
						url: '/pages/public/login'
					});
					return;
				}
				let isHome = 1;
				let isShop = 1;
				if (this.djSel == 1) {
					uni.navigateTo({
						url: '/pages/therapist/orderDetail?artificerId=' + item.artificerId + '&consortiaId=' +
							item.consortiaId
					});
				} else {
					uni.navigateTo({
						url: '/pages/therapist/orderDetail?isShop=1&artificerId=' + item.artificerId +
							'&consortiaId=' +
							item.consortiaId
					});
				}
			},
			getjsList() {
				let isHome = 1;
				let isShop = 1;
				if (this.djSel == 1) {
					isShop = '';
				} else {
					isHome = '';
				}
				let data = {
					// id: curTab,
					isRecommend: 1,
					artificerName: '',
					classifyId: this.classifyId,
					page: 1,
					limit: 20,
					isHome: isHome,
					isShop: isShop,
					// condition: this.value1, //智能优选
					// authentication: this.value2, //不限男女
					by: 2, //价格
					latitude: this.latitude,
					longitude: this.longitude,
					city: this.city
				}
				this.$Request.get("/app/artificer/selectArtificerList", data).then(res => {
					if (res.code == 0) {
						this.jsList = res.data.list
					}
				})
			},
			topDJSel(index) {
				if (index == 1) {
					this.list = this.listn;
				} else {
					this.list = this.lists;
				}
				this.listIndex = 0;
				this.djSel = index;
				this.page = 1
				this.orderList = []
				this.getjsList();
				this.getorderlist('')
			},
			goNav(url) {
				let userId = this.$queue.getData('userId');
				if (!userId) {
					uni.navigateTo({
						url: '/pages/public/login'
					});
					return;
				}
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
			},
			saveTP() {
				let userId = this.$queue.getData('userId');
				if (!userId) {
					uni.navigateTo({
						url: '/pages/public/login'
					});
					return;
				}
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
			getCity(e) {
				console.log(e[0].label);
				this.city = e[0].label;
				uni.setStorageSync('city', this.city)
				this.getKTCityList();
				this.page = 1
				this.getlist()
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
							// this.orderList = []
							// this.showTP = true;
							// uni.showToast({
							// 	title: '当前城市暂未开通，请切换城市',
							// 	duration: 2000,
							// 	icon: 'none'
							// });
						} else {
							this.showTP = false;
						}
					}
				});
			},
			// 开启订阅消息
			openMsg() {
				console.log('订阅消息')
				var that = this
				uni.getSetting({
					withSubscriptions: true, //是否获取用户订阅消息的订阅状态，默认false不返回
					success(ret) {
						console.log(ret.subscriptionsSetting, '------------------')
						// if (ret.subscriptionsSetting.itemSettings && Object.keys(ret.subscriptionsSetting.itemSettings).length == 2) {
						if (ret.subscriptionsSetting.itemSettings) {
							uni.setStorageSync('sendMsg', true)
							uni.openSetting({ // 打开设置页 
								success(rea) {
									console.log(rea.authSetting)
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
			// 分享文案和图片
			getZiZhi() {
				this.$Request.getT('/app/common/type/299').then(res => { //分享文案 299
					if (res.code === 0) {
						this.tuiguang = res.data.value;
					}
				});
				this.$Request.getT('/app/common/type/300').then(res => { //分享图片 300
					if (res.code === 0) {
						this.tuiguangImg = res.data.value;
					}
				});
			},
			// 筛选
			change(index) {
				// this.value1 = e[0][0].value
				// this.value2 = e[1][0].value
				// this.value3 = e[2][0].value
				this.listIndex = index
				this.page = 1
				this.getorderlist('')
			},
			GetQuan() {
				let userId = this.$queue.getData('userId');
				this.$Request.getT('/app/coupon/insertNewUserCoupon').then(res => {
					if (res.code == 0) {
						this.$refs.popushongbao.close();
						this.$queue.showToast('领取成功！')
						setTimeout(d => {
							uni.navigateTo({
								url: '/my/hongbao/youhuijuan'
							});
						}, 1000);
					} else {
						this.$queue.showToast(res.msg)
					}
				});
			},
			checkNewUser() {
				this.$Request.getT("/app/user/selectUserById").then(res => {
					if (res.code == 0) {
						if (res.data.isCoupon != 1) {
							setTimeout(d => {
								this.$refs.popushongbao.open();
							}, 2000);
						}
					}
				});
			},
			//获取轮播图
			getBannerList() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 1
				}).then(res => {
					if (res.code == 0) {
						this.swiperList = res.data
					}
				});

				this.$Request.get("/app/dict/list", {
					type: '服务类型'
				}).then(res => {
					if (res.code == 0) {
						if (res.data) {
							this.typeList = res.data
							this.classifyId = res.data[0].id;
							this.page = 1
							this.getorderlist('')
						}
					}
				});
			},
			// 跳转订单
			goOrder(e) {
				// #ifdef MP-WEIXIN
				if (uni.getStorageSync('sendMsg')) {
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
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
				// #endif
				let token = this.$queue.getData('token')
				if (token) {
					if (this.djSel == 1) {
						uni.navigateTo({
							url: '/pages/index/order/order?massageTypeId=' + e.massageTypeId
						});
					} else {
						uni.navigateTo({
							url: '/pages/index/order/consortiaDetail?consortiaId=' + e.consortiaId
						});
					}
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					});
				}

			},
			// 选择城市
			goSelectCity() {
				uni.navigateTo({
					url: '/pages/index/citys/citys'
				});
			},
			// 跳转搜索
			goSearch(index) {
				// #ifdef MP-WEIXIN
				if (uni.getStorageSync('sendMsg')) {
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
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
				// #endif
				uni.navigateTo({
					url: '/pages/index/search/index?index=' + index
				});
			},
			selectCity(longitude, latitude) {
				this.$Request.get('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					if (res.code == 0) {
						this.city = res.data.city ? res.data.city : '未知'
						uni.setStorageSync('city', res.data.city)
						this.page = 1
						this.getorderlist('')
						this.getKTCityList();
						this.getjsList();
					}
				});
			},
			goImg() {
				window.location.href = 'https://qmdj.xianmxkj.com/erweima.png';
			},

			selTab(index, id) {
				this.tabIndex = index;
				this.classifyId = id;
				this.page = 1
				this.getorderlist('')
			},
			getorderlist(type) {
				let city = this.city;
				if (this.city === '请选择城市') {
					// city = '';
					if (type === 'refresh') {
						uni.stopPullDownRefresh();
					}
					return;
				}
				if (this.djSel == 1) {
					let index = this.list[this.listIndex].id
					let data = {
						classifyId: this.classifyId,
						page: this.page,
						limit: this.limit,
						status: 1,
						isHome: 1,
						by: index, //销量
						latitude: this.latitude,
						longitude: this.longitude,
						city: city
					}
					this.$Request.get("/app/artificer/selectMassageTypePage", data).then(res => {
						if (res.code == 0) {
							if (this.page == 1) {
								for (let i = 0; i < res.data.list.length; i++) {
									if (res.data.list[i].labels) {
										res.data.list[i].labels = res.data.list[i].labels.split(",");
									} else {
										res.data.list[i].labels = []
									}
								}
								this.orderList = res.data.list
							} else {
								for (let i = 0; i < res.data.list.length; i++) {
									if (res.data.list[i].labels) {
										res.data.list[i].labels = res.data.list[i].labels.split(",");
									} else {
										res.data.list[i].labels = []
									}
								}
								this.orderList = [...this.orderList, ...res.data.list]
							}
						}

						if (type === 'refresh') {
							uni.stopPullDownRefresh();
						}
					})
				} else {
					let index = this.lists[this.listIndex].id
					let data = {
						classifyId: this.classifyId,
						page: this.page,
						limit: this.limit,
						status: 2,
						isShop: 1,
						sort: index, //销量
						latitude: this.latitude,
						longitude: this.longitude,
						city: city
					}
					this.$Request.get("/app/consortia/selectConsortiaList", data).then(res => {
						if (res.code == 0) {
							if (this.page == 1) {
								for (let i = 0; i < res.data.list.length; i++) {
									if (res.data.list[i].labels) {
										res.data.list[i].labels = res.data.list[i].labels.split(",");
									} else {
										res.data.list[i].labels = []
									}

									if (res.data.list[i].distance > 1000) {
										res.data.list[i].distance = (res.data.list[i].distance / 1000).toFixed(1) +
											'km'
									} else {
										res.data.list[i].distance = (res.data.list[i].distance).toFixed(1) + 'm'
									}
								}
								this.orderList = res.data.list
							} else {
								for (let i = 0; i < res.data.list.length; i++) {
									if (res.data.list[i].labels) {
										res.data.list[i].labels = res.data.list[i].labels.split(",");
									} else {
										res.data.list[i].labels = []
									}

									if (res.data.list[i].distance > 1000) {
										res.data.list[i].distance = (res.data.list[i].distance / 1000).toFixed(1) +
											'km'
									} else {
										res.data.list[i].distance = (res.data.list[i].distance).toFixed(1) + 'm'
									}
								}
								this.orderList = [...this.orderList, ...res.data.list]
							}
						}

						if (type === 'refresh') {
							uni.stopPullDownRefresh();
						}
					})
				}
			}
		},
		onReachBottom: function() {
			this.page = this.page + 1;
			this.getorderlist('')
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.getorderlist('refresh');
			this.getZiZhi()
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

	.bannerImg {
		width: 680rpx;
		height: 250upx;
		border-radius: 24rpx;
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

	.js_view {
		width: 190rpx;
		height: 290rpx;
		background: #FFFFFF;
		border-radius: 24rpx;
		margin-right: 20rpx;
		margin-top: 20rpx;
		position: relative;

		.js_view_youzhi {
			width: 64rpx;
			height: 29rpx;
			background: linear-gradient(90deg, #FD6416 0%, #FD3C16 100%);
			border-radius: 15rpx 0rpx 15rpx 0rpx;
			text-align: center;
			line-height: 29rpx;
			font-size: 22rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #FFFFFF;
			position: absolute;
			z-index: 99;
			top: 0rpx;
			right: 0rpx;
		}

		.js_view_name {
			width: 190rpx;
			font-size: 28rpx;
			font-family: PingFang SC;
			font-weight: 800;
			color: #1E1F31;
			margin-top: 6rpx;
			text-overflow: ellipsis;
			white-space: nowrap;
			overflow: hidden;
			text-align: center;
			padding: 0rpx 10rpx;
		}

		.js_view_fuwu {
			width: 190rpx;
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #999999;
			margin-top: 6rpx;
			text-overflow: ellipsis;
			white-space: nowrap;
			overflow: hidden;
			text-align: center;
			padding: 0rpx 10rpx;
		}
	}

	.list_item {
		// width: 650upx;
		// height: 160upx;
		width: 92%;
		// margin: 30rpx 0;
		background: #FFFFFF;
		border-radius: 24upx;
		padding: 20upx;
		display: flex;
		align-items: center;
		margin-top: 20upx;

		.shop_image {
			width: 173upx;
			height: 178upx;
			border-radius: 24upx;
		}

		.listitem_rightview {
			margin-left: 20upx;
			width: 80%;
			padding-top: 10rpx;

			.btnview {
				display: flex;
				align-items: center;
				justify-content: space-between;
				// margin-top: 12upx;
				width: 100%;
			}

			.titleview {
				display: flex;
				align-items: flex-end;
				justify-content: space-between
			}

			.moneyview {
				display: flex;
				align-items: flex-end;

				image {
					width: 30upx;
					height: 30upx;
					margin-left: 20upx;
					margin-bottom: 2upx;
				}
			}

			.item_time {
				font-size: 24upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #999999;
				margin-left: 10upx;
			}

			.item_money {
				font-size: 34upx;
				font-family: PingFang SC;
				font-weight: 800;
				color: #FF1200;

				.oldPrice_view {
					font-size: 22rpx;
					font-family: PingFang SC;
					font-weight: 400;
					// text-decoration: line-through;
					color: #FF1200;
					margin-left: 8rpx;
				}

				text {
					font-size: 24upx;
				}
			}

			.item_oldmoney {
				font-size: 24upx;
				font-family: PingFang SC;
				font-weight: 500;
				text-decoration: line-through;
				color: #999999;
				margin-left: 10upx;
			}

			.item_btn {
				width: 140upx;
				height: 56upx;
				background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
				border-radius: 16upx;
				text-align: center;
				line-height: 56upx;
				font-size: 24upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #FFFFFF;
			}

			.item_yishou {
				font-size: 22upx;
				font-family: PingFang SC;
				font-weight: 500;
				color: #999999;
			}

			.item_title {
				width: 320rpx;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
				font-size: 28upx;
				font-family: PingFang SC;
				font-weight: bold;
				color: #1E1F31;
			}
		}

		image {
			width: 220upx;
			height: 160upx;
			border-radius: 24upx;
		}
	}

	.tabview1 {
		width: 686upx;
		// height: 100upx;
		// background: #FFFFFF;
		border-radius: 24rpx;
		z-index: 99;
		margin-top: -60rpx;
		margin-bottom: 20rpx;
		// display: flex;
		// line-height: 100upx;

		.tabview_daodian {
			width: 686rpx;
			height: 80rpx;
			// background: #FFFFFF;
			// border-radius: 24rpx 24rpx 0rpx 0rpx;
			font-size: 32rpx;
			font-family: PingFang SC;
			font-weight: bold;
			color: #333333;
			align-items: center;
			display: flex;
			position: relative;

			.tabview_bottomxian {
				width: 50rpx;
				height: 14rpx;
				background: #308251;
				border-radius: 50rpx;
				margin: 0 auto;
				// margin-top: -10rpx;

				image {
					width: 50rpx;
					height: 14rpx;
				}
			}

			.tabview_daodian_sel {
				width: 200rpx;
				color: #141414;
				font-size: 32rpx;
				text-align: center;
			}

			.tabview_daodian_no {
				width: 200rpx;
				text-align: center;
				color: #999999;
			}
		}
	}

	.tabview {
		// width: 686upx;
		// // height: 100upx;
		background: #F2FFF9;
		border-radius: 24rpx 24rpx 0 0;
		// margin-top: -50upx;
		// z-index: 99;
		display: flex;
		line-height: 100upx;

		.tabItem_sel {
			font-size: 30upx;
			font-family: PingFang SC;
			font-weight: 800;
			color: #308251;
			flex: 1;
			text-align: center;
		}

		.tabItem {
			font-size: 26upx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #333333;
			flex: 1;
			text-align: center;
		}
	}
	
	.tq_rightview{
		width: 90rpx;
		height: 78rpx;
		position: absolute;
		right: 0rpx;
		z-index: 9;
	}

	.tabsx {
		width: 690upx;
		border-radius: 24rpx;
		padding-bottom: 10rpx;
		display: flex;
		align-items: center;
		justify-content: space-between;
		padding: 20rpx 30rpx;
		font-size: 26rpx;

		.active {
			color: #308251;
			font-size: 30rpx;
			font-weight: bold;
		}
	}

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		padding-bottom: 100upx;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>