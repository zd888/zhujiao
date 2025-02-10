<template>
	<view>
		<view class="margin-lr-sm margin-top-sm" v-if="dataList.length">
			<view class="flex justify-between margin-bottom radius bg padding-sm" @click="goOrder(item)"
				v-for="(item,index) in dataList" :key='index'>
				<image :src="item.homepageImg?item.homepageImg: '../../../static/logo.png'"
					style="width: 200rpx;height: 200rpx;border-radius: 10rpx;"></image>
				<view class="flex-sub margin-left text-white flex flex-direction justify-between">
					<view class="flex justify-between">
						<view class="flex">

							<image src="../../../static/images/geren.png"
								style="width: 35rpx;height: 35rpx;margin-left: 10rpx;"></image>
							<view class="margin-right-xs u-line-1"
								style="display: inline-block;margin-left: 10rpx;width: 400rpx;margin-top: -2px;">
								{{item.myLevel}}
							</view>
						</view>
					</view>
					<view class="flex radius" style="line-height: 34upx;">
						<view style="width: 100%;position: relative;line-height: 40rpx;color: #999999;">
							<text v-for="(item,index) in item.gameName" class="margin-right-sm">{{item}}</text>
						</view>
					</view>
					<view class="flex" style="align-items: center;font-size: 24rpx;padding: 5rpx;" v-if="item.salesNum">
						<view style="color: #999999;background: #F2F2F2; padding: 5rpx 10rpx;">已售{{item.salesNum}}
						</view>
					</view>
					<view style="width: 100%;display: flex;justify-content: space-between;align-items: center;">
						<view style="color:#FF1200;font-size: 31rpx;">
							￥{{isVip? item.memberMoney :item.money}}元/<text>{{item.unit}}</text>
						</view>
						<view style="background: #557EFD;color: #ffffff;padding: 15rpx 25rpx;border-radius: 45rpx;">
							预约服务
						</view>
					</view>
				</view>
			</view>
		</view>

		<empty v-if="!dataList.length"></empty>
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
				dataList: [],
				isVip: false,
				myId: ''
			}
		},
		onLoad() {
			let hyCheck = this.$queue.getData('hyCheck');
 			if(hyCheck != '否'){
 				this.isVip = uni.getStorageSync('isVIP') ? uni.getStorageSync('isVIP') : false
 			}
			this.myId = uni.getStorageSync('userId')
			this.getDataList()
		},
		methods: {
			getDataList() {
				this.$Request.get("/app/orderTaking/queryLowTaking", {
					page: this.page,
					limit: this.limit,
				}).then(res => {
					if (res.code == 0) {
						if (this.page == 1) {
							this.dataList = res.data.list
							for (let i = 0; i < this.dataList.length; i++) {
								this.dataList[i].gameName = this.dataList[i].gameName.split(",");
							}
						} else {
							this.dataList = [...this.dataList, ...res.data.list]
							for (let i = 0; i < this.dataList.length; i++) {
								this.dataList[i].gameName = this.dataList[i].gameName.split(",");
							}
						}
					}
					uni.stopPullDownRefresh();
				});
			},

			// 跳转订单
			goOrder(e) {
				let token = uni.getStorageSync('token')
				if (token) {
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
		background-color: #f7f7f7;
	}

	.bg {
		background: #ffffff;
	}
</style>
