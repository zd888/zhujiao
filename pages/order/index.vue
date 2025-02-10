<!-- 菜单悬浮的原理: 通过给菜单添加position:sticky实现, 用法超简单, 仅APP端的低端机不兼容 https://caniuse.com/#feat=css-sticky -->
<template>
	<view>
		<view class="sticky-tabs">
			<!-- <view class="tab_topview" v-if="checkDj === '是'">
				<view @tap="qhTopSel(1)" :class="topSel == 1 ? 'topview_item_sel' : 'topview_item'">上门订单</view>
				<view @tap="qhTopSel(2)" :class="topSel == 2 ? 'topview_item_sel' : 'topview_item'">到店订单</view>
			</view> -->
			
			<view class="tabview1">
				<view class="tabview_daodian">
					<view @tap="qhTopSel(1)"
						:class="topSel == 1 ? 'tabview_daodian_sel' : 'tabview_daodian_no'">
						<view>助教订单</view>
						<view v-if="topSel == 1">
							<image
								src="https://anmo8.0.xianmaxiong.com/file/uploadPath/2024/01/06/a158a7aa25438bbd3ce1e717c06141e3.png"
								style="width: 34rpx;height: 12rpx;top: -20rpx;"></image>
						</view>
					</view>
					<view @tap="qhTopSel(2)"
						:class="topSel == 2 ? 'tabview_daodian_sel' : 'tabview_daodian_no'">
						<view>门店订单</view>
						<view v-if="topSel == 2">
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
			
			
			
			<u-tabs v-if="tabs.length > 0" :list="tabs" :is-scroll="true" :current="tabIndex" @change="tabChange" active-color="#000000"
				inactive-color="#999999"></u-tabs>
		</view>
		
		
		
		
		<!-- 数据列表 -->
		<view v-if="goods.length > 0" class="margin-sm bg  padding-tb-sm" v-for="(item,index) in goods" :key='index'
			@click="goNav('/my/order/pay?ordersId='+item.ordersId)" style="border-radius: 24rpx;">
			<view class="flex justify-between padding-lr-sm ">
				<view class="flex align-center">
					<image src="../../static/logo.png" style="width: 48rpx;height: 48rpx;border-radius: 50%;">
					</image>
					<view class="margin-left-xs ">{{item.artificerName}}</view>
				</view>

				<view class="text-green" v-if="item.status ==1">待付款</view>
				<view class="text-green" v-if="item.status ==2">待服务</view>
				<view class="text-green" v-if="item.status ==3">待评价</view>
				<view class="text-green" v-if="item.status ==5">已完成</view>
				<view class="text-green" v-if="item.status ==6">服务中</view>
				<view class="text-green" v-if="item.status ==7">助教出发</view>
				<view class="text-green" v-if="item.status ==8">助教到达</view>
				<view class="text-green" v-if="item.status ==4" style="color: #999999;">已取消</view>
				<!-- <view class="u-tips-color">{{item.createTime}}</view> -->
			</view>
			<view class="margin-top-sm" style="width: 100%;height: 1rpx;background: #EEEEEE;"></view>
			<view class=" u-flex u-p-t-30 padding-lr-sm">
				<view class="u-m-r-10">
					<u-avatar
						:src="(item.ordersMassageList && item.ordersMassageList.length > 0 && item.ordersMassageList[0].massageType && item.ordersMassageList[0].massageType.massageImg)?item.ordersMassageList[0].massageType.massageImg: '../../static/logo.png'"
						mode="square" size="100">
					</u-avatar>
				</view>
				<view class="u-flex-1  margin-left-xs">
					<view class="u-font-18  text-bold u-line-1" style="width: 560rpx;">
						<view class=" text-lg "
							v-if="item.ordersMassageList && item.ordersMassageList.length > 0 && item.ordersMassageList[0].massageType && item.ordersMassageList[0].massageType.title"
							style="margin-top: 0rpx;display: inline-block;width: 470rpx; overflow: hidden;white-space: nowrap;text-overflow: ellipsis;">
							{{item.ordersMassageList[0].massageType.title}}
						</view>
					</view>
					<view class="u-font-14 margin-top-xs u-tips-color flex justify-between">
						预约时间：{{item.serveTime}}
					</view>
				</view>
			</view>
			<view class="flex u-p-t-20 justify-between align-center padding-lr-sm">
				<view class=" flex-sub ">
					实付：<text class="text-df" style="color: #FF1200;">￥</text><text class="text-xl text-bold"
						style="color: #FF1200;">{{item.sumMoney}}</text>
				</view>
				<view class="flex text-right">
					<u-button
						v-if="(item.status == 1 || item.status == 2 || item.status == 7 || item.status == 8) && yhqxSel != '否'"
						:custom-style="customStyle" shape="circle" :plain="true"
						@click="cancelOrder(item)">取消订单</u-button>
					<u-button v-if="item.status == 1" :custom-style="customStyle1" shape="circle" :plain="true"
						@click="goNav('/my/order/pay?ordersId='+item.ordersId)">去支付</u-button>
					<u-button v-if="item.status == 2" :custom-style="customStyle1" shape="circle" :plain="true"
						@click="goNav('/my/order/pay?ordersId='+item.ordersId)">查看详情</u-button>
					<!-- <u-button v-if="item.status == 6" :custom-style="customStyle" shape="circle" :plain="true"
							@click="cancel(item)">订单完成</u-button> -->
					<!-- <u-button v-if="item.state == 2" :custom-style="customStyle" shape="circle" :plain="true" @click="goNav('/my/order/complain?id='+item.ordersId)" >去投诉</u-button> -->
					<u-button v-if="item.status == 3" :custom-style="customStyle1" shape="circle" :plain="true"
						@click="goNav('/my/order/feedback?artificerId='+item.artificerId+ '&ordersId='+item.ordersId)">
						去评价
					</u-button>
					<u-button v-if="(item.status == 4 || item.status == 5) && userDeletaSel != '否'"
						:custom-style="customStyle" shape="circle" :plain="true" @click="delOrder(item)">删除订单</u-button>
				</view>
			</view>
		</view>
		<empty v-if="goods.length == 0"></empty>
	</view>
</template>

<script>
	import MescrollMixin from "@/components/mescroll-uni/components/mescroll-uni/mescroll-mixins.js";
	import mescrollBody from "@/components/mescroll-uni/components/mescroll-body/mescroll-body.vue";
	import meTabs from "@/components/mescroll-uni/me-tabs/me-tabs.vue";
	import empty from '@/components/empty.vue'

	export default {
		mixins: [MescrollMixin], // 使用mixin
		components: {
			mescrollBody,
			meTabs,
			empty
		},
		data() {
			return {
				topSel: 1,
				goods: [], // 数据列表
				game: [],
				tabs: [{
					name: '全部',
					status: '0'
				}, {
					name: '待支付',
					status: '1'
				}, {
					name: '待服务',
					status: '2'
				}, {
					name: '助教出发',
					status: '7'
				}, {
					name: '助教到达',
					status: '8'
				}, {
					name: '服务中',
					status: '6'
				}, {
					name: '待评价',
					status: '3'
				}, {
					name: '已取消',
					status: '4'
				}, {
					name: '已完成',
					status: '5'
				}],
				tabIndex: 0, // tab下标
				page: 1,
				limit: 10,
				userId: 0,
				status: 1,
				nickName: '',
				avatar: '',
				customStyle: {
					color: '#999999',
					border: '4rpx solid #999999',
					border: "8rpx",
					width: '180rpx',
					height: '60rpx',
					margin: "0 0 0 20rpx",
					fontSize: '26rpx'
				},
				customStyle1: {
					color: '#FFFFFF',
					backgroundColor: '#308251',
					border: "8rpx",
					width: '180rpx',
					height: '60rpx',
					margin: "0 0 0 20rpx",
					fontSize: '26rpx'
				},
				checkDj: '否',
				yhqxSel: '否',
				userDeletaSel: '否'
				// customStyle: {
				// 	color: '#1789FD',
				// 	backgroundColor: '#1E1F31',
				// 	border: "8rpx",
				// 	width: '180rpx',
				// 	height: '54rpx',
				// 	margin: "0 0 0 20rpx"
				// }
			}
		},
		onLoad() {
			this.userId = uni.getStorageSync('userId')
			this.nickName = uni.getStorageSync('nickName')
		},
		onShow() {
			this.$Request.getT('/app/common/type/431').then(res => { //是否开启到店模式	431
				if (res.code == 0) {
					if (res.data && res.data.value) {
						this.checkDj = res.data.value;
						this.$queue.setData('checkDj', this.checkDj);
					}
				}
			})

			this.userDeletaSel = this.$queue.getData('userDeletaSel');
			this.yhqxSel = this.$queue.getData("yhqxSel");
			let SelectIndex = this.$queue.getData('SelectIndex');
			if (SelectIndex) {
				if (SelectIndex == -1) {
					this.tabIndex = 0;
				} else {
					this.tabIndex = SelectIndex;
				}
				this.$queue.remove('SelectIndex')
			}

			let token = this.$queue.getData('token');
			if (token) {
				uni.showLoading({
					title: '加载中...'
				})
				this.getOrderList()
			} else {
				uni.navigateTo({
					url: '/pages/public/login'
				});
			}
		},
		methods: {
			qhTopSel(index) {
				this.topSel = index
				this.page = 1
				this.getOrderList()
			},
			//获取列表数据
			getOrderList() {
				let curTab = this.tabs[this.tabIndex].status
				let ordersType = 1
				if (this.topSel == 2) {
					ordersType = 2;
				}
				let data = {
					status: curTab,
					page: this.page,
					limit: this.limit,
					ordersType: ordersType
				}
				this.$Request.get('/app/artificer/selectOrdersList', data).then(res => {
					if (res.code == 0) {
						uni.hideLoading();
						console.log(res)
						if (this.page == 1) this.goods = []; //如果是第一页需手动制空列表
						this.goods = [...this.goods, ...res.data.list]; //追加新数据
						// this.goods.forEach(ret => {
						// 	switch (ret.status) {
						// 		case '1':
						// 			ret.statusName = '待付款'
						// 			break;
						// 		case '2':
						// 			ret.statusName = '进行中'
						// 			break;
						// 		case '3':
						// 			ret.statusName = '已评价'
						// 			break;
						// 		case '4':
						// 			ret.statusName = '已取消'
						// 			break;
						// 		case '5':
						// 			ret.statusName = '已完成'
						// 			break;
						// 	}
						// })
					}

				}).catch(() => {
					//联网失败, 结束加载

				});
			},
			// 切换菜单
			tabChange(index) {
				uni.showLoading({
					title: '加载中...'
				})
				this.tabIndex = index
				// this.goods = []; // 置空列表,显示加载进度条
				this.page = 1
				this.getOrderList()
			},
			// 取消订单
			cancelOrder(e) {
				let contentName = '';
				let closeKC = this.$queue.getData('closeKC');
				if (e.status == 7 || e.status == 8 && closeKC) {
					let money = parseFloat(parseFloat(e.price) * parseFloat(closeKC)).toFixed(2);
					contentName = '取消订单将扣除' + money + '元的手续费，并且车费将全部扣除！';
				}
				let that = this
				uni.showModal({
					title: '温馨提示',
					content: '是否确认取消订单？' + contentName,
					success: function(res) {
						if (res.confirm) {
							let data = {
								ordersId: e.ordersId
							}
							that.$queue.showLoading('取消中...')
							that.$Request.post('/app/artificer/deleteOrders', data).then(res => {
								uni.hideLoading();
								if (res.code == 0) {
									that.page = 1
									that.getOrderList()
								} else {
									that.$queue.showToast(res.msg);
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			/*下拉刷新的回调 */
			downCallback() {
				// 这里加载你想下拉刷新的数据, 比如刷新轮播数据
				// loadSwiper();
				// 下拉刷新的回调,默认重置上拉加载列表为第一页 (自动执行 page.num=1, 再触发upCallback方法 )
				this.mescroll.resetUpScroll()
				this.page = 1;
				this.getOrderList();
			},
			// 完成订单
			cancel(e) {
				let that = this
				uni.showModal({
					title: '提示',
					content: '确认完成订单吗?',
					success: function(res) {
						if (res.confirm) {
							let data = {
								ordersId: e.ordersId,
							}
							that.$queue.showLoading('提交中...')
							that.$Request.post('/app/artificer/accomplishOrders', data).then(res => {
								uni.hideLoading();
								if (res.code == 0) {
									that.page = 1
									that.getOrderList()
								} else {
									that.$queue.showToast(res.msg);
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			//删除
			delOrder(e) {
				let that = this
				uni.showModal({
					title: '提示',
					content: '确定删除订单吗?',
					success: function(res) {
						if (res.confirm) {
							let data = {
								ordersId: e.ordersId,
							}
							that.$queue.showLoading('删除中...')
							that.$Request.post('/app/artificer/removeOrders', data).then(res => {
								uni.hideLoading();
								if (res.code == 0) {
									that.$queue.showToast('删除成功！');
									setTimeout(d => {
										that.page = 1
										that.getOrderList()
									}, 1500)
								} else {
									that.$queue.showToast(res.msg);
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			goNav(url) {
				uni.navigateTo({
					url
				})
			}
		},
		onReachBottom: function() {
			this.page = this.page + 1;
			this.getOrderList();
			if (this.totalCount == this.orderList.length) {
				uni.showToast({
					title: '已经到底了~',
					icon: 'none'
				})
			}
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.getOrderList();
			setTimeout(d => {
				uni.stopPullDownRefresh();
			}, 2000)
		},
	}
</script>

<style lang="scss">
	page {
		background: #F5F5F5;
	}
	
	.tq_rightview {
		width: 90rpx;
		height: 78rpx;
		position: absolute;
		top: 10rpx;
		right: 0rpx;
		z-index: 9;
	}
	
	.tabview1 {
		width: 750rpx;
		height: 100upx;
		background: #FFFFFF;
		// border-radius: 24rpx;
		// padding-top: 20rpx;
		// margin-bottom: 20rpx;
		// display: flex;
		// line-height: 100upx;
	
		.tabview_daodian {
			width: 700rpx;
			height: 80rpx;
			padding-top: 20rpx;
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
	
	.tab_topview {
		width: 750rpx;
		height: 80rpx;
		background: #FFFFFF;
		display: flex;
	}

	.topview_item {
		width: 375rpx;
		height: 80rpx;
		background: #FFFFFF;
		text-align: center;
		line-height: 80rpx;
		font-size: 32rpx;
		font-family: PingFang SC;
		font-weight: bold;
		color: #30B47A;
		border: 3rpx solid #30B47A;
	}

	.topview_item_sel {
		width: 375rpx;
		height: 80rpx;
		background: #30B47A;
		text-align: center;
		line-height: 80rpx;
		font-size: 32rpx;
		font-family: PingFang SC;
		font-weight: bold;
		color: #FFFFFF;
	}

	/*
	sticky生效条件：
	1、父元素不能overflow:hidden或者overflow:auto属性。(mescroll-body设置:sticky="true"即可, mescroll-uni本身没有设置overflow)
	2、必须指定top、bottom、left、right4个值之一，否则只会处于相对定位
	3、父元素的高度不能低于sticky元素的高度
	4、sticky元素仅在其父元素内生效,所以父元素必须是 mescroll
	*/
	.sticky-tabs {
		z-index: 990;
		position: sticky;
		top: var(--window-top);
		// background-color: #fff;
	}

	// 使用mescroll-uni,则top为0
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

	page {
		background-color: #F7F7F7;
	}

	.bg {
		background-color: #FFFFFF;
	}
</style>