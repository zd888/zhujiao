<template>
	<view class="">
		<view class="bg flex"
			style="position: fixed;top: 0rpx;z-index: 999;width: 100%;background: #ffffff;padding: 20rpx 10rpx 10rpx 30rpx;">
			<view v-if="XCXIsSelect !='否'" class="flex justify-between margin-right-sm" @tap="Cityshow = true"
				style="line-height: 68rpx;">
				<image src="../../static/images/index/place.png" style="width: 27rpx;height: 37rpx;"
					class="margin-top-xs"></image>
				<view class="localName  margin-left-sm">{{city}}</view>
			</view>
			<u-search class="flex-sub" placeholder="请输入助教名字" :focus="false" shape="square" v-model="artificerName"
				:show-action="true" :animation="true" bg-color="#F7F7F7" color="#1A1A1A" @search="onSearch()"
				action-text="搜索" @custom="onSearch()"></u-search>
			<view style="width: 120rpx;text-align: center;" v-if="topSel == 1" @tap="topSelIndex(2)">
				<image
					src="https://anmo5.0.xianmaxiong.com/file/uploadPath/2023/11/28/3d764e0c8eb33e03fd050b83a9196bc3.png"
					style="width: 38rpx;height: 36rpx;"></image>
				<view class="topSel_name">地图</view>
			</view>
			<view style="width: 120rpx;text-align: center;" v-if="topSel == 2" @tap="topSelIndex(1)">
				<image
					src="https://anmo5.0.xianmaxiong.com/file/uploadPath/2023/11/28/063f050271b245b833047263e41c6f0e.png"
					style="width: 38rpx;height: 36rpx;"></image>
				<view class="topSel_name">列表</view>
			</view>
		</view>

		<view v-if="topSel == 1">
			<!-- <view class="tabview1" v-if="XCXIsSelect != '否'">
				<view class="tabview" v-if="tablist && tablist.length > 1">
					<view v-for="(item,index) in tablist" :key="index" @tap="tanChange(index,item)"
						:class="tabIndex == item.id ? 'tabItem_sel' : 'tabItem'">
						<view>{{item.value}}</view>
					</view>
				<image src="https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/b349e6aee04f0d9e9411040809b18ced.png" class="tq_rightview"></image>
				</view>
			</view> -->
			<view class="tabview1" v-if="XCXIsSelect != '否'">
				<view class="tabview_daodian" v-if="tablist && tablist.length > 1">
					<view v-for="(item,index) in tablist" @tap="tanChange(index,item)"
						:class="tabIndex == item.id ? 'tabview_daodian_sel' : 'tabview_daodian_no'">
						<view>{{item.value}}</view>
						<view v-if="tabIndex==item.id">
							<image
								src="https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/a158a7aa25438bbd3ce1e717c06141e3.png"
								style="width: 34rpx;height: 12rpx;top: -20rpx;"></image>
						</view>
					</view>
					<image
						src="https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/b349e6aee04f0d9e9411040809b18ced.png"
						class="tq_rightview"></image>
				</view>
			</view>




			<view v-if="orderList.length" :style="XCXIsSelect != '否' ? '' : 'margin-top: 114rpx;'">
				<view class="flex align-center margin-bottom-sm bg padding-sm radius margin-sm"
					v-for="(item,index) in orderList" :key='index' @click="goOrder(item)"
					style="box-shadow: 6rpx 10rpx 4px #F5F5F5;position: relative;">
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
								<view class="text-bold text-30 margin-right-xs js_name">{{item.artificerName}}</view>
								<image
									src="https://anmo.xianmxkj.com/file/uploadPath/2023/02/16/d45855dcf1e01210a4224a408225e8e0.png"
									style="width: 40rpx;height:40rpx;">
								</image>
								<view class="zpmore_view" v-if="item.lifePhoto"
									@click.stop="saveImgss(item.lifePhoto,0)">更多照片</view>
							</view>
							<view class="zz_view" v-if="item.status == 1 && item.time">最早可约：{{item.time}}</view>
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

							<view class="flex align-center" v-if="item.tripWay != 3">
								<image src="../../static/images/address.png" style="width: 24rpx;height: 29rpx;">
								</image>
								<view style="color: #333333;margin-left: 8rpx;">{{item.distance}}</view>
							</view>
							<view class="flex align-center" style="font-size: 26rpx;color: #f49650;font-weight: bold;"
								v-if="item.tripWay == 3">
								免出行费
							</view>
						</view>
						<view style="width: 100%;display: flex;justify-content: space-between;align-items: center;"
							v-if="XCXIsSelect !='否'">
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
							<view class="text-center text-sm" v-if="item.status == 1"
								style="background: linear-gradient(90deg, #55B55C 0%, #308251 100%);color: #ffffff;width: 135upx;border-radius: 16upx;line-height: 50upx;"
								@click.stop="goOrder(item)">
								立即预约
							</view>
							<view class="text-center text-sm" v-else
								style="background: #d9d9d9;color: #ffffff;width: 135upx;border-radius: 16upx;line-height: 50upx;">
								立即预约
							</view>
						</view>
					</view>
				</view>
			</view>
			<empty v-if="orderList.length == 0"></empty>
		</view>

		<view v-if="topSel == 2">
			<map style="width: 100%; height: 100vh;" :latitude="latitude" :longitude="longitude" :markers="covers"
				@markertap="markerTap">
			</map>
		</view>

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
		<!-- 经验筛选 -->
		<u-select v-model="Cityshow" :list="cityList" @confirm="getCity"></u-select>
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
				covers: [],
				topSel: 2,
				artificerName: '',
				tpSel: 0,
				tpCount: 0,
				showTP: false,
				Cityshow: false,
				loading: true, // 是否显示骨架屏组件
				defaultSelected: [],
				value1: '',
				value2: '',
				value3: '',
				game: [],
				defaultIndex: [0, 0, 0],
				filterData: [
					[{
						label: '推荐',
						value: '',
					}],
					[{
							label: '服务热度',
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
							value: '2',
						},
						{
							label: '从近到远',
							value: '1',
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
				massageTypeId: '',
				cityList: [],
				tablist: [],
				tabIndex: 0,

				orderIndex: 0
			}
		},
		onLoad(e) {
			console.log(e.massageTypeId)
			let that = this
			that.massageTypeId = e.massageTypeId

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
			this.getTypeList();
		},
		onShow() {
			let that = this

			if (that.$queue.getData('topSel')) {
				that.topSel = that.$queue.getData('topSel');
				if (this.topSel == 1) {
					this.limit = 100
				} else {
					this.limit = 10
				}
			}

			that.getKTCityList();
			that.XCXIsSelect = this.$queue.getData("XCXIsSelect");
			that.city = uni.getStorageSync('city') ? uni.getStorageSync('city') : '请选择城市'
			that.getTpCount();
			that.getTpMy();
			that.token = uni.getStorageSync('token')
			// if (uni.getStorageSync('token')) {
			if (that.latitude && that.longitude) {
				that.page = 1
				that.getlist()
			} else {
				that.page = 1
				that.getlist()
			}
			// }
			that.myId = uni.getStorageSync('userId')
			// console.log(that.showModal, '------', that.myId)
			// #ifdef MP-WEIXIN
			//订阅
			if (that.myId) {
				that.getIsVip()
				if (this.showModal) {
					// this.openMsg()
				}
			}
			// #endif
		},
		methods: {
			topSelIndex(index) {
				this.topSel = index;
				this.page = 1
				if (this.topSel == 1) {
					this.limit = 100
					this.getlist();
				} else {
					this.limit = 10
					this.getlist();
				}
				this.$queue.setData('topSel', this.topSel);
			},
			markerTap(e) {
				console.log(e.detail.markerId)
				let item = this.orderList[e.detail.markerId - 1]
				this.goOrder(item);
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
							this.orderList = []
							this.showTP = true;
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
			getTypeList() {
				this.$Request.get("/app/dict/list", {
					type: '服务类型'
				}).then(res => {
					if (res.code == 0) {
						if (res.data) {
							this.tablist = res.data
							this.tabIndex = res.data[0].id;
						}
					}
				});
			},
			// selectCity(longitude, latitude) {
			// 	this.$Request.get('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
			// 		if (res.code == 0) {
			// 			// this.city = res.data.city ? res.data.city : '未知'
			// 			// uni.setStorageSync('city', res.data.city)
			// 			let data = {
			// 				num: 1,
			// 				size: 10
			// 			}
			// 			this.getData(data)
			// 		}
			// 	});
			// },
			tanChange(index, item) {
				this.orderIndex = index
				this.tabIndex = item.id
				this.page = 1
				this.getlist()
			},
			// 开启订阅消息
			openMsg() {
				console.log('订阅消息')
				var that = this
				uni.getSetting({
					withSubscriptions: true, //是否获取用户订阅消息的订阅状态，默认false不返回
					success(ret) {
						if (ret.subscriptionsSetting.itemSettings) {
							uni.setStorageSync('sendMsg', true)
							uni.openSetting({ // 打开设置页 
								success(rea) {
									console.log(rea.authSetting)
								}
							});
						} else { // 用户没有点击“总是保持以上，不再询问”则每次都会调起订阅消息
							console.log(99999)
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
			onSearch(e) {
				// this.artificerName = e;
				this.page = 1;
				this.getlist()
			},
			getlist() {
				if (this.city === '请选择城市') {
					return;
				}
				// let index = this.list[this.listIndex].id
				let index = 2
				let data = {
					// id: curTab,
					artificerName: this.artificerName,
					classifyId: this.tabIndex,
					page: this.page,
					limit: this.limit,
					// condition: this.value1, //智能优选
					// authentication: this.value2, //不限男女
					by: index, //价格
					latitude: this.latitude,
					longitude: this.longitude,
					city: this.city
				}
				// console.log(data)
				this.$Request.get("/app/artificer/selectArtificerList", data).then(res => {
					this.loading = false;
					if (res.code == 0) {
						this.covers = [];
						for (var i = 0; i < res.data.list.length; i++) {
							if (res.data.list[i].lifePhoto) {
								res.data.list[i].lifePhoto = res.data.list[i].lifePhoto.split(',')
							}
							if (res.data.list[i].distance > 1000) {
								res.data.list[i].distance = (res.data.list[i].distance / 1000).toFixed(1) +
									'km'
							} else {
								res.data.list[i].distance = (res.data.list[i].distance).toFixed(1) + 'm'
							}

							let coversList = {
								id: i + 1,
								latitude: res.data.list[i].latitude,
								longitude: res.data.list[i].longitude,
								iconPath: 'https://anmo5.0.xianmaxiong.com/file/uploadPath/2023/12/04/19648c2ec4f18bc18f5f8e157d5b797a.png',
								callout: {
									content: res.data.list[i].distance,
									color: '#333333',
									fontsize: 10,
									padding: 4,
									borderRadius: 4,
									bgColor: '#e5e5e5', //背景颜色
									display: 'ALWAYS', //常显
									boxShadow: 'none !important',
								},
								width: 50,
								height: 50,
								borderRadius: 2, //边框圆角
							}
							this.covers.push(coversList)
						}
						if (this.page == 1) {
							this.orderList = res.data.list
						} else {
							this.orderList = [...this.orderList, ...res.data.list]
						}
					}
					uni.stopPullDownRefresh();
				})
			},
			// 切换菜单
			tabChange() {
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								// console.log(re)
							}
						},
						fail: (res) => {
							// console.log(res)
						}
					})
				}
				this.defaultIndex = [0, 0, 0]
				// this.$refs.uDropdown.close();
				this.page = 1
				this.getlist()
			},

			// 筛选
			change(index) {

				// this.value1 = e[0][0].value
				// this.value2 = e[1][0].value
				// this.value3 = e[2][0].value
				this.listIndex = index
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
						uni.setStorageSync('latitude', res.latitude)
						uni.setStorageSync('longitude', res.longitude)
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
			// 乐享低价
			goLowTaking() {
				uni.navigateTo({
					url: '/pages/index/order/lowTaking'
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
			goSearch(index) {
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
					url: '/pages/index/search/index?index=' + index
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
						url: '/pages/therapist/orderDetail?artificerId=' + e.artificerId
					});
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					});
				}

			},
		},
		onReachBottom: function() {
			this.page = this.page + 1;
			if (this.topSel == 2) {
				this.getlist();
			}
		},
		onPullDownRefresh: function() {
			this.page = 1;
			if (this.topSel == 2) {
				this.getlist();
			}

			setTimeout(d => {
				uni.stopPullDownRefresh();
			}, 2000)
		},
	}
</script>

<style lang="scss">
	page {
		background-color: #F7F7F7;
	}

	.tq_rightview {
		width: 90rpx;
		height: 78rpx;
		position: absolute;
		top: 0rpx;
		right: 0rpx;
		z-index: 9;
	}

	.tabview1 {
		width: 750rpx;
		height: 100upx;
		background: #FFFFFF;
		// border-radius: 24rpx;
		margin-top: 100upx;
		padding-top: 20rpx;
		// margin-bottom: 20rpx;
		// display: flex;
		// line-height: 100upx;

		.tabview_daodian {
			width: 700rpx;
			height: 80rpx;
			// background: #FFFFFF;
			// border-radius: 24rpx 24rpx 0rpx 0rpx;
			font-size: 32rpx;
			font-family: PingFang SC;
			font-weight: bold;
			color: #333333;
			// align-items: center;
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
				padding: 0rpx 20rpx;
				color: #308251;
				font-size: 32rpx;
				// flex: 1;
				text-align: center;
			}

			.tabview_daodian_no {
				width: 200rpx;
				text-align: center;
				color: #999999;
			}
		}
	}

	.js_name {
		max-width: 140rpx;
		text-overflow: ellipsis;
		overflow: hidden;
		white-space: nowrap;
	}

	.topSel_name {
		font-size: 20rpx;
		font-family: PingFang SC;
		font-weight: 500;
		color: #333333;
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

	.actve {
		font-size: 30rpx;
		font-family: PingFang SC;
		font-weight: 800;
		color: #308251;
	}

	// .tabview1 {
	// 	padding: 20rpx 20rpx;
	// 	background: #FFFFFF;
	// 	margin-top: 60upx;

	// 	.tabview {
	// 		display: flex;
	// 		line-height: 100upx;

	// 		.tabItem_sel {
	// 			font-size: 30upx;
	// 			font-family: PingFang SC;
	// 			font-weight: 800;
	// 			color: #308251;
	// 			// flex: 1;
	// 			text-align: center;
	// 		}

	// 		.tabItem {
	// 			font-size: 26upx;
	// 			font-family: PingFang SC;
	// 			font-weight: 500;
	// 			color: #333333;
	// 			text-align: center;
	// 		}
	// 	}

	// 	.tabsx {
	// 		width: 686upx;
	// 		border-radius: 24rpx;
	// 		padding-bottom: 10rpx;
	// 		display: flex;
	// 		align-items: center;
	// 		justify-content: space-between;
	// 		padding: 20rpx 30rpx;
	// 		font-size: 26rpx;

	// 		.active {
	// 			color: #308251;
	// 			font-size: 30rpx;
	// 			font-weight: bold;
	// 		}
	// 	}
	// }
</style>