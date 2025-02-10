<template>
	<view>
		<!-- <u-dropdown height="80" active-color="#2B7DE2" inactive-color='#fff' >
			<u-dropdown-item v-model="value1" title="智能优选"  @change="change" :options="options1"></u-dropdown-item>
			<u-dropdown-item v-model="value2" title="不限性别" @change="change" :options="options2"></u-dropdown-item>
			<u-dropdown-item v-model="value3" title="价格" @change="change" :options="options3"></u-dropdown-item>
		</u-dropdown> -->
		<ren-dropdown-filter :filterData='filterData' :border="false" :defaultIndex='defaultIndex' @onSelected='change'
			class="u-skeleton-rect">
		</ren-dropdown-filter>

		<view class="margin-lr-sm" v-if="orderList.length">
			<view class="flex margin-bottom-sm bg padding-sm radius" v-for="(item,index) in orderList" :key='item.id'
				@click="goOrder(item)">
				<!-- <image :src="item.headImg?item.headImg: '../../../static/logo.png'"
					style="width: 220rpx;height: 200rpx;border-radius: 10rpx;">
				</image> -->
				<view style="width: 220upx;height: 160upx;border-radius: 10rpx;">
					<image :src="item.headImg?item.headImg: '../../static/logo.png'"
						style="width: 220upx;height: 160upx;"></image>
				</view>
				<view class=" margin-left-sm text-white flex-direction" style="width: 65%;">
					<view class="flex justify-between">
						<view class="flex">
							<image src="../../../static/images/geren.png"
								style="width: 35rpx;height: 35rpx;margin-left: 10rpx;"></image>
							<!-- <view class="margin-right-xs u-line-1"
								style="margin-top: -2px;display: inline-block;width: 320upx; overflow: hidden;white-space: nowrap;text-overflow: ellipsis;">
								{{item.myLevel}}
							</view> -->
							<view class="text-bold text-df margin-left-xs">大美女</view>
						</view>
						<view style="color: #999999;">1.5km</view>
					</view>

					<!-- <view style="color: #999999;font-size: 24rpx;line-height: 40rpx;" v-if="item.authentication == 1">
						<text v-if="item.sex == 1">女</text>
						<text v-if="item.sex == 0">男</text>
						/<text>{{age}}岁</text>
						/<text>{{item.orderLevel}}</text>
						/<text >{{item.region}}</text>
					</view>
					<view style="color: #999999;" v-if="item.authentication == 2">
						<text>{{item.name}}</text> /<text>{{item.region}}</text>
					</view> -->
					<view class="flex flex-wrap" style="margin-top: 10rpx;">
						<text class="margin-bottom-xs box" v-for="(item,index) in item.gameName"
							:key="index">{{item}}</text>
					</view>
					<view class="text-cut" style="color: #999999;">{{item.describes}}</view>
					<!-- <view style="width: 100%;display: flex;justify-content: space-between;align-items: center;">
						<view style="color:#FF1200;font-size: 31rpx;">
							￥{{item.oldMoney}}/月
						</view>
						<view style="background: #557EFD;color: #ffffff;padding: 15rpx 25rpx;border-radius: 15rpx;">
							预约服务
						</view>
					</view> -->
				</view>
			</view>
		</view>
		<empty v-if="orderList.length == 0"></empty>
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
				page: 1,
				limit: 10,
				gameId: '',
				gameName: '',
				value1: '',
				value2: '',
				value3: '',

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
							label: '类别',
							value: '0',
						},

						{
							label: '女',
							value: '2',
						},
						{
							label: '男',
							value: '1',
						}
					],
					[{
							label: '价格',
							value: '',
						},
						{
							label: '从远到近',
							value: 'desc',
						},
						{
							label: '从近到远',
							value: 'asc',
						}
					],
				],

				options1: [{
						label: '智能排序',
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
				options2: [{
						label: '类别',
						value: '0',
					},

					{
						label: '女',
						value: '2',
					},
					{
						label: '男',
						value: '1',
					}
				],
				options3: [{
						label: '价格',
						value: '',
					},
					{
						label: '从远到近',
						value: 'desc',
					},
					{
						label: '从近到远',
						value: 'asc',
					}
				],
				city: null,
				sex: null,
				order: null,
				latitude: null,
				longitude: null,
				orderList: [],
				token: '',
				isVip: false,
				myId: '',
				age: 0,
				birthDate: ''
			}
		},
		onLoad(option) {
			uni.setNavigationBarTitle({
				title: option.name
			})
			this.gameId = option.gameId
			this.gameName = option.name;
			this.city = uni.getStorageSync('city')
			this.latitude = uni.getStorageSync('latitude')
			this.longitude = uni.getStorageSync('longitude')
			this.token = uni.getStorageSync('token')
			this.isVip = uni.getStorageSync('isVIP') ? uni.getStorageSync('isVIP') : false
			this.myId = uni.getStorageSync('userId')
			this.getDataList()
		},
		methods: {
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
			// 筛选
			change(e) {
				console.log(e[0][0], '+++++++++')
				this.page = 1
				this.value1 = e[0][0].value
				this.value2 = e[1][0].value
				this.value3 = e[2][0].value

				this.getDataList()
			},
			getDataList() {
				let data = {
					massageTypeId: this.gameName,
					page: this.page,
					limit: this.limit,
					isRecommend: 2,
					condition: this.value1, //智能优选
					authentication: this.value2, //不限男女
					by: this.value3, //价格
					latitude: this.latitude,
					longitude: this.longitude,
					city: this.city
				}
				// console.log(data)
				if (this.token) {
					this.$Request.get("/app/orderTaking/queryTaking", data).then(res => {
						if (res.code == 0) {
							if (this.page == 1) {
								this.orderList = res.data.list
								for (let i = 0; i < this.orderList.length; i++) {
									this.orderList[i].gameName = this.orderList[i].gameName.split(",");
									// console.log(this.orderList[i].region)
									if (this.orderList[i].region) {
										let region = this.orderList[i].region.split(",");
										this.orderList[i].region = region[1]
									} else {
										this.orderList[i].region = '不限地区'
									}
									this.getAge(this.orderList[i].birthdate)
								}
							} else {
								this.orderList = [...this.orderList, ...res.data.list]
								for (let i = 0; i < this.orderList.length; i++) {
									this.orderList[i].gameName = this.orderList[i].gameName.split(",");

									if (this.orderList[i].region) {
										let region = this.orderList[i].region.split(",");
										this.orderList[i].region = region[1]
									} else {
										this.orderList[i].region = '不限地区'
									}
									this.getAge(this.orderList[i].birthdate)
								}
							}
						}
						uni.stopPullDownRefresh();
					})
				} else {
					this.$Request.get("/app/orderTaking/queryTakings", data).then(res => {
						if (res.code == 0) {
							if (this.page == 1) {
								this.orderList = res.data.list
								for (let i = 0; i < this.orderList.length; i++) {
									this.orderList[i].gameName = this.orderList[i].gameName.split(",");
									if (this.orderList[i].region) {
										let region = this.orderList[i].region.split(",");
										this.orderList[i].region = region[1]
									} else {
										this.orderList[i].region = '不限地区'
									}
									this.getAge(this.orderList[i].birthdate)
								}
							} else {
								this.orderList = [...this.orderList, ...res.data.list]
								for (let i = 0; i < this.orderList.length; i++) {
									this.orderList[i].gameName = this.orderList[i].gameName.split(",");
									if (this.orderList[i].region) {
										let region = this.orderList[i].region.split(",");
										this.orderList[i].region = region[1]
									} else {
										this.orderList[i].region = '不限地区'
									}
									this.getAge(this.orderList[i].birthdate)
								}
							}
						}
						uni.stopPullDownRefresh();
					})
				}
			},
			// change(value) {
			// 	this.getDataList()
			// },
			// 跳转订单
			goOrder(e) {
				if (this.token) {
					uni.navigateTo({
						url: '/pages/index/order/order?id=' + e.id
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
			this.getDataList();
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.getDataList();
		},
	}
</script>

<style>
	page {
		background-color: #F5F5F5;
	}

	.bg {
		background: #ffffff;
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
		background: #E8FAE1;
		color: #308251;
		padding: 5rpx 15rpx;
		font-size: 26rpx;
		letter-spacing: 2rpx;
		border-radius: 8rpx;
		margin-right: 8upx;
	}
</style>
