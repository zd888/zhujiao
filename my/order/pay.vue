<template>
	<view style="padding-bottom: 140rpx;">
		<view class="bg radius padding-sm " style="font-size: 32upx;margin: 19upx 30upx;">
			<view v-if="order.status ==1">待付款...</view>
			<view v-if="order.status ==2">待服务</view>
			<view v-if="order.status ==5">已完成</view>
			<view v-if="order.status ==6">服务中</view>
			<view v-if="order.status ==7">助教出发</view>
			<view v-if="order.status ==8">助教到达</view>
			<view v-if="order.status ==3">待评价</view>
			<view v-if="order.status ==4" style="color: #999999;">已取消</view>
			<view v-if="order.overTimeOrders == 1" style="color: red;font-size:26rpx">(订单已超时)</view>
		</view>
		<view class="padding-lr padding-bottom u-skeleton">
			<view class="bgImg u-skeleton-fillet" style="padding: 30rpx;"
				v-if="order.ordersMassageList && order.ordersMassageList.length > 0">
				<view class="flex justify-around">
					<view class="" style="width: 220upx;height: 160upx;">
						<image :src="order.ordersMassageList[0].massageType.massageImg"
							style="width: 100%;height: 100%;border-radius: 24upx;"></image>
					</view>
					<view class="u-flex-1  margin-left-sm">
						<view class="flex">
							<image src="../../static/images/geren.png" style="width: 35rpx;height: 35rpx;">
							</image>
							<view class="margin-right-xs text-df margin-left-xs u-skeleton-fillet"
								style="margin-top: -2px;display: inline-block;width: 400rpx; overflow: hidden;white-space: nowrap;text-overflow: ellipsis;">
								{{order.ordersMassageList[0].massageType.title}}
							</view>
						</view>
						<view class="flex align-center">
							<view style="width: 70%;">
								<view class="flex justify-between margin-tb-sm" style="color: #999999;">
									<view>
										<image src="../../static/time.png"
											style="width: 26rpx;height: 26rpx;margin-right: 8rpx;"></image>
										{{order.ordersMassageList[0].massageType.duration}}分钟
										x{{order.ordersMassageList[0].num}}
									</view>
								</view>

								<view style="color:#FF1200;font-size: 24upx;">
									￥<text class="text-bold text-lg">{{order.price}}</text>
								</view>
							</view>

							<view class="jz_btn"
								v-if="order.addNum < order.ordersMassageList[0].massageType.addNum && order.status == 6"
								@tap="saveJZ(order.ordersMassageList[0].massageType.massageTypeId)">
								加钟/补价</view>
						</view>
					</view>
				</view>
				<view class="flex justify-between margin-top-lg" v-if="order.taxiMoney && order.ordersType != 2">
					<view style="width: 180upx;">出行交通费用</view>
					<view class="">￥{{order.taxiMoney}}</view>
				</view>
				<view class="flex justify-between margin-top" v-if="order.couponMoney">
					<view style="width: 120upx;">优惠券</view>
					<view class="">￥{{order.couponMoney}}</view>
				</view>
				<view class="flex justify-between margin-top" v-if="order.addNum">
					<view style="width: 120upx;">加钟次数</view>
					<view class="">{{order.addNum}}</view>
				</view>
				<view class="flex justify-between margin-top" v-if="order.addTime">
					<view style="width: 120upx;">加钟时长</view>
					<view class="">{{order.addTime}}分钟</view>
				</view>
				<view class="flex justify-between margin-top" v-if="order.addMoney">
					<view style="width: 120upx;">加钟费用</view>
					<view class="">￥{{order.addMoney}}</view>
				</view>
				<view style="width: 100%;height: 1rpx;background: #F2F2F2;margin-top:30rpx;"></view>
				<view class="flex align-center margin-top">
					<image :src="order.artificer.artificerImg" style="width:100upx;height: 100upx;border-radius: 50%;">
					</image>
					<view class="margin-left-sm">
						<view class="flex align-center">
							<view class="text-bold text-df ">{{order.artificer.artificerName}}</view>
							<image src="../../static/img/phone.png"
								v-if="order.status !=1 && order.status !=5 && order.status !=3 && order.status !=4 && order.status !=2"
								style="width: 10px;height: 13px;margin-left: 11px;"
								@click="bindphone(order.artificer.phone)"></image>
						</view>
						<view class="flex align-center flex-wrap" v-if="order.artificer.ordersCount">
							<view class="margin-right">
								<image src="../../static/images/start.png" style="width: 24rpx;height: 22rpx;"></image>
								<text style="color: #F93731;margin-left: 5rpx;">{{order.artificer.ordersCount}}</text>
							</view>
							<text class="margin-bottom-xs box" style="margin-right: 10upx;">助教陪练</text>
						</view>
					</view>
				</view>
				
				<view style="width: 100%;" v-if="shopOrder.consortiaName">
					<view class="flex margin-top align-center">
						<view class="top_address_view">
							<view class="top_address">{{shopOrder.address}}</view>
							<view class="top_time">营业时间：{{shopOrder.startBusinessHours}}-{{shopOrder.endBusinessHours}}</view>
						</view>
						<view class="top_address_right">
							<view class="text-center" @tap="bindGps(shopOrder.latitude,shopOrder.longitude,shopOrder.address)">
								<view class="top_address_dhview">
									<!-- <image src="/static/images/index/add.png"></image> -->
									<image src="/static/images/place1.png"></image>
								</view>
								<view class="top_address_dhtext">导航</view>
							</view>
					
							<view class="text-center" @tap="bindphone(shopOrder.phone)">
								<view class="top_address_phoneview">
									<image src="/static/images/phone.png"></image>
								</view>
								<view class="top_address_dhtext">电话</view>
							</view>
						</view>
					</view>
				</view>
				
				<view style="width: 100%;height: 1rpx;background: #F2F2F2;margin-top:30rpx;"></view>
				<view class="flex align-center justify-between margin-top" v-if="order.artificer && order.ordersType != 2">
					<view>
						<view style="color: #000000;font-size: 32rpx;">
							{{order.userName}}
							{{order.phone ? order.phone.substring(0,3) + '****' + order.phone.substring(7,11) : ''}}
						</view>
						<view style="color: #999999;margin-top: 8rpx;">{{order.address}}</view>
					</view>
					<!-- <image src="../../static/img/you.png" style="width: 15rpx;height: 28rpx;"></image> -->
				</view>
				<view class="margin-top-lg">
					<view class="text-lg text-bold">
						订单信息
					</view>
					<view class="margin-right-xs">
						<view class="flex justify-between margin-top-lg">
							<view style="width: 120upx;">订单编号</view>
							<view class="flex align-center" @tap="copy(order.ordersNo)">
								<view class="">{{order.ordersNo}}</view>
								<u-icon name="file-text"></u-icon>
							</view>
						</view>
						
						<view class="flex justify-between margin-top-lg">
							<view style="width: 120upx;">订单类型</view>
							<view class="">{{order.ordersType == 1 ? '助教订单' : '门店订单'}}</view>
						</view>
						
						<view class="flex justify-between margin-top-lg">
							<view style="width: 120upx;">预约时间</view>
							<view class="">{{order.serveTime}}</view>
						</view>
						<view class="flex justify-between margin-top-lg">
							<view style="width: 120upx;">下单时间</view>
							<view class="">{{order.createTime}}</view>
						</view>
						<view class="flex justify-between margin-top-lg" v-if="order.startTime">
							<view style="width: 200upx;">开始服务时间</view>
							<view class="">{{order.startTime}}</view>
						</view>
						<view class="flex justify-between margin-top-lg" v-if="order.endTimes">
							<view style="width: 200upx;">结束服务时间</view>
							<view class="">{{order.endTimes}}</view>
						</view>
						<view class="flex justify-between margin-top-lg" v-if="order.endTime">
							<view style="width: 200upx;">预估结束时间</view>
							<view class="">{{order.endTime}}</view>
						</view>
						<view class="flex justify-between margin-top-lg" v-if="order.tripWay && order.ordersType != 2">
							<view style="width: 120upx;">出行方式</view>
							<view v-if="order.tripWay == 1">公交</view>
							<view v-if="order.tripWay == 2">出租</view>
							<view v-if="order.tripWay == 3">免费</view>
						</view>
						<view class="flex justify-between margin-top-lg" v-if="order.km && order.ordersType != 2">
							<view style="width: 120upx;">全程距离</view>
							<view class="">{{order.km}}km</view>
						</view>
						<view class="flex justify-between margin-top-lg" v-if="order.remark">
							<view style="width: 170rpx;">备注内容</view>
							<view class="flex">
								<view class="">
									{{order.remark}}
								</view>
							</view>
						</view>
					</view>
				</view>
				<view style="width: 100%;height: 1rpx;background: #F2F2F2;margin-top:30rpx;"></view>

				<view class="flex justify-between margin-top-lg">
					<view style="width: 120upx;font-weight: bold;">合计</view>
					<view class="">￥<text style="font-size: 35rpx;font-weight: bold;">{{order.sumMoney}}</text></view>
				</view>
			</view>


			<!-- <view class="bg padding radius margin-top-sm">
				<view class="text-lg text-bold">
					服务信息
				</view>
				<view class="margin-right-xs">
					<view class="flex justify-between margin-top-lg">
						<view style="width: 170rpx;">服务时间</view>
						<view class="">
							<text>{{order.serveTime}}</text>
						</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view style="width: 170rpx;">服务地点</view>
						<view class="flex" @tap="bindGps(order.latitude,order.longitude,order.address)">
							<view class="">
								{{order.address}}
								<image src="../../static/images/address.png" style="width: 13px;height: 15px;"></image>
							</view>
						</view>
					</view>

					<view class="flex justify-between margin-top-lg">
						<view style="width: 170rpx;">联系方式</view>
						<view class="" @click="bindphone(order.phone)">
							{{order.phone}}
							<image src="../../static/images/phone.png" style="width: 15px;height: 15px;"></image>
						</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.remark">
						<view style="width: 170rpx;">备注内容</view>
						<view class="flex">
							<view class="">
								{{order.remark}}
							</view>
						</view>
					</view>
				</view>
			</view> -->

			<view class="flex tabber padding-top-sm padding-bottom-sm align-center">
				<u-button @click="cancelOrder(order)" shape="circle" :custom-style="customStyle" :hair-line="false"
					v-if="(order.status == 1 || order.status == 2 || order.status == 7 || order.status == 8) && yhqxSel != '否'">取消订单
				</u-button>
				<u-button @click="openpay()" shape="circle" :custom-style="customStyle2" :hair-line="false"
					v-if="order.status == 1">立即支付
				</u-button>
				<!-- <u-button @click="cancel(order)" class="margin-right" shape="circle" :custom-style="customStyle2"
					:hair-line="false" v-if="order.status == 6">订单完成
				</u-button> -->
				<u-button v-if="order.status == 2 || order.status == 5|| order.status == 3" :custom-style="customStyle2"
					shape="circle" :plain="true"
					@click="goNav('/my/order/complain?ordersId='+ordersId + '&byUserId=' + order.artificer.userId + '&byuserName=' + order.artificer.artificerName)">
					去投诉
				</u-button>
				<u-button v-if="order.status == 3" :custom-style="customStyle2" shape="circle" :plain="true"
					@click="goNav('/my/order/feedback?artificerId='+order.artificerId+ '&ordersId='+order.ordersMassageList[0].ordersId)">
					去评价
				</u-button>
				<u-button :custom-style="customStyle2" shape="circle" :plain="true" @click="goChat">联系客服</u-button>
			</view>
		</view>
		<u-skeleton :loading="loading" :animation="true" bgColor="#FFF"></u-skeleton>
		<u-popup v-model="showorder" mode="bottom" :closeable="closeable">
			<scroll-view scroll-y="true" style="width: 750rpx;height: 500rpx;">
				<view class="list_item" v-for="(item,index) in orderList" :key="index">
					<image :src="item.massageImg ? item.massageImg : '/static/logo.png'"></image>
					<view class="listitem_rightview">
						<view class="titleview">
							<view class="item_title">{{item.title}}</view>
							<view class="item_yishou">已售{{item.sales}}</view>
						</view>
						<view class="flex flex-wrap" style="margin-top: 10rpx;" v-if="item.labels.length>0">
							<text class="margin-bottom-xs box" v-for="(items,index) in item.labels"
								:key="index">{{items}}</text>
						</view>
						<view class="btnview">
							<view class="item_money"> <text style="font-size: 24upx;">￥</text> <text
									v-if="isVip">{{item.memberPrice}}</text><text v-if="!isVip">{{item.price}}</text> /<text
									style="font-weight: 500;font-size: 24upx;	">{{item.duration}}分钟</text></view>
							<view class="item_btn" @click="goOrder(item)">确认加钟</view>
						</view>
					</view>
				</view>
			</scroll-view>
		</u-popup>
		<!-- 支付方式 -->
		<u-popup v-model="showpay" mode="bottom" :closeable="closeable">
			<view class="popup_pay">
				<view style="background-color: #fff;">
					<view style="padding: 0 20upx;margin-top: 60rpx;margin-bottom: 20rpx;">
						<view
							style="display: flex;height: 100upx;align-items: center;padding: 20upx 0;justify-content: center;"
							v-for="(item,index) in openLists" :key='index'>
							<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;">
							</image>
							<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">
								{{item.text}}
							</view>
							<radio-group name="openWay" style="margin-left: 45upx;" @tap='selectWay(item)'>
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

</template>

<script>
	export default {
		data() {
			return {
				shopOrder: {
					consortiaName:''
				},
				tordersId: '',
				tpayMoney: '',
				paySel: 0,
				showorder: false,
				orderList: [],
				loading: true,
				customStyle: {
					// backgroundColor: '#e5e5e5',
					// color: '#FFFFFF',

					// #ifdef MP-WEIXIN
					width: "310upx",
					// #endif
					// #ifndef MP-WEIXIN
					width: '310upx',

					// #endif	
					margin: '0 10upx',
				},
				customStyle2: {
					backgroundColor: '#308251',
					color: '#FFFFFF',
					// #ifdef MP-WEIXIN
					width: "310upx",
					// #endif
					// #ifndef MP-WEIXIN
					width: '310upx',

					// #endif	
					margin: '0 10upx',
				},
				id: '',
				order: {
					user: {},
					game: {}
				},
				isTrue: 0,

				phone: '',
				isVip: false,
				birthDate: '',
				age: '',
				showpay: false,
				openLists: [],
				openWay: 1,
				closeable: true,
				yhqxSel: '否'
			}
		},
		onLoad(e) {
			uni.showLoading({
				title: '加载中......',

			})
			// setTimeout(() => {
			// 	// this.loading = false;
			// }, 2000)
			this.isTrue = e.isTrue
			if (this.isTrue) {
				uni.setNavigationBarTitle({
					title: '订单详情'
				})
			}
			this.ordersId = e.ordersId
			// this.getOrder()
		},
		onShow() {
			this.yhqxSel = this.$queue.getData("yhqxSel");
			this.getOrder()
			if (uni.getStorageSync('token')) {
				this.getIsVip()
			}

			// #ifndef MP-WEIXIN
			this.openLists = [{
					image: '../../static/images/icon_weixin.png',
					text: '微信支付',
					id: 2
				}, {
					image: '../../static/images/zhifubao.png',
					text: '支付宝支付',
					id: 3
				}, {
					image: '../../static/images/lingqian.png',
					text: '零钱支付',
					id: 1
				}],
				this.openWay = 2;
			// #endif

			// #ifdef MP-WEIXIN
			this.openLists = [{
					image: '../../static/images/icon_weixin.png',
					text: '微信支付',
					id: 2
				}, {
					image: '../../static/images/lingqian.png',
					text: '零钱支付',
					id: 1
				}],
				this.openWay = 2;
			// #endif
		},
		methods: {
			goChat() {
				let kefu = this.$queue.getData('kefu'); // 用户端联系方式 1 手机号 2企业微信 
				let kefuPhone = this.$queue.getData('kefuPhone');
				if (uni.getStorageSync('token')) {
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
			copy(num) {
				uni.setClipboardData({
					data: num,
					success: r => {
						this.$queue.showToast('复制成功');
					}
				});
			},
			goOrder(item) {
				let that = this
				let payMoney = that.isVip ? item.memberPrice : item.price;
				let data = {
					parentId: that.order.ordersId,
					artificerId: that.order.artificerId,
					ordersMassageList: [{
						massageId: item.massageTypeId,
						num: 1,
					}],
					price: payMoney,
					couponId: '',
					payMoney: payMoney,
					userId: uni.getStorageSync('userId'),
				}
				that.$Request.postJson("/app/artificer/insertOrders", data).then(res => {
					that.showorder = false
					if (res.code == 0) {
						that.tordersId = res.data.ordersId;
						that.tpayMoney = payMoney;
						that.showpay = true;
						that.paySel = 1;
					} else {
						that.$queue.showToast(res.msg)
					}

				});
			},
			payJZ() {
				let that = this;
				if (that.openWay == 1) { //零钱支付
					uni.showModal({
						title: '付款提示',
						content: '确认支付' + that.tpayMoney + '元吗?',
						success: function(re) {
							if (re.confirm) {
								that.$queue.showLoading('支付中...')
								// console.log('用户点击确定');
								that.$Request.post("/app/artificer/payOrders", {
									ordersId: that.tordersId,
								}).then(ret => {
									uni.hideLoading();
									if (ret.code == 0) {
										uni.showToast({
											title: '支付成功',
											icon: 'none'
										})
										that.getOrder()

									} else {
										uni.showToast({
											title: ret.msg,
											icon: 'none'
										})
									}
								});
							} else if (re.cancel) {
								that.getOrder()
							}
						}
					})
				} else if (that.openWay == 2) { //微信支付
					// #ifdef MP-WEIXIN
					let data = {
						ordersId: that.tordersId,
						type: 3
					}
					that.$Request.post('/app/wxPay/payOrder', data).then(ret => {
						console.log(ret)
						if (ret.code == 0) {
							uni.hideLoading();
							uni.requestPayment({
								provider: 'wxpay',
								timeStamp: ret.data.timestamp,
								nonceStr: ret.data.noncestr,
								package: ret.data.package,
								signType: ret.data.signType,
								paySign: ret.data.sign,
								success: function(ret) {
									console.log(ret)
									uni.showToast({
										title: '支付成功',
										icon: 'none'
									})
									that.getOrder()
									// this.$queue.showToast('支付成功');

									// uni.switchTab({
									// 	url: '/pages/my/index'
									// })
								},
								fail: function(err) {
									// this.$queue.showToast('支付失败');
									uni.showToast({
										title: '支付失败',
										icon: 'nones'
									});
									that.getOrder()
								}
							});
						}
					});
					// #endif

					// #ifdef H5
					let ua = navigator.userAgent.toLowerCase();
					if (ua.indexOf('micromessenger') != -1) {
						let data = {
							ordersId: that.tordersId,
							type: 2
						}
						that.$Request.post('/app/wxPay/payOrder', data).then(rea => {
							that.showpay = false
							if (rea.code == 0) {
								that.callPay(rea.data);
							} else {
								uni.showToast({
									title: rea.msg,
									icon: 'none'
								})
							}
						});
					} else {
						let data = {
							ordersId: that.tordersId,
							type: 4
						}
						that.$Request.post('/app/wxPay/payOrder', data).then(rea => {
							that.showpay = false
							if (rea.code == 0) {
								const urlArr = window.location.href;
								const hostUrl = urlArr.split("/");
								const callBack = hostUrl[0] + "//" + hostUrl[2] + "/";
								const url = '&redirect_url=' + callBack + 'my/order/pay';
								window.location = rea.data.mweb_url + url
							} else {
								uni.showToast({
									title: rea.msg,
									icon: 'none'
								})
							}
						});
					}

					// #endif

					// #ifdef APP
					let data = {
						ordersId: that.order.ordersId,
						type: 1
					}
					that.$Request.post('/app/wxPay/payOrder', data).then(rea => {
						console.log(rea)
						that.showpay = false
						if (rea.code == 0) {
							that.isCheckPay(rea.code, 'wxpay', JSON.stringify(rea.data));
						}
					});
					// #endif
				} else if (that.openWay == 3) { //支付宝支付
					// #ifdef H5
					let data = {
						ordersId: that.tordersId,
						type: 2
					}
					that.$Request.post('/app/aliPay/payOrder', data).then(
						rea => {
							that.showpay = false
							const div = document.createElement('div')
							div.innerHTML = rea.data //此处form就是后台返回接收到的数据
							document.body.appendChild(div)
							document.forms[0].submit()
						});
					// #endif

					// #ifdef APP-PLUS
					let data = {
						ordersId: that.tordersId,
						type: 1
					}
					that.$Request.post('/app/aliPay/payOrder', data).then(
						rea => {
							that.showpay = false
							that.setPayment('alipay', rea.data);
						});
					// #endif
				}

			},
			saveJZ(classifyId) {
				let data = {
					parentId: classifyId,
					page: 1,
					limit: 50,
					status: 1
				}
				let that = this;
				this.$queue.showLoading('查询中...')
				that.$Request.get("/app/artificer/selectMassageTypePage", data).then(res => {
					if (res.code == 0) {
						if (res.data.list && res.data.list.length > 0) {
							that.orderList = res.data.list
							for (let i = 0; i < that.orderList.length; i++) {
								if (that.orderList[i].labels) {
									that.orderList[i].labels = that.orderList[i].labels.split(",");
								}
							}
							uni.hideLoading();
							that.showorder = true;
						} else {
							uni.hideLoading();
							that.$queue.showToast('暂无可加钟项目');
						}
					} else {
						uni.hideLoading();
						that.$queue.showToast(res.msg);
					}

				})
			},
			//支付选择
			selectWay: function(item) {
				this.openWay = item.id;
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
			// 修改订单地址
			undateAdd(order) {
				if (order.state == 0) {
					uni.navigateTo({
						url: '../address/address?order=' + order.ordersId + '&id=' + 3
					})
				}
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
			ysPhone(phoneA, phoneB) {
				this.$Request.getT('/app/user/insertVirtualPhone?phoneA=' + phoneA + '&phoneB=' + phoneB).then(res => {
					if (res.code == 0) {
						this.order.artificer.phone = res.data;
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
			goNav(e) {
				uni.navigateTo({
					url: e
				})
			},
			getOrder() {
				let data = {
					ordersId: this.ordersId
				}
				this.$Request.get('/app/artificer/selectOrdersDetails', data).then(res => {
					if (res.code == 0) {
						this.loading = false
						uni.hideLoading()
						this.order = res.data
						let aliphone = this.$queue.getData('aliphone');
						if (aliphone === '是' && this.order.artificer) {
							this.ysPhone(this.order.phone, this.order.artificer.phone);
						}
						
						if (res.data.ordersType == 2 && res.data.consortiaId) {
							this.getShopDetail(res.data.consortiaId)
						}
					}
				})
			},
			getShopDetail(consortiaId) {
				let userId = this.$queue.getData('userId');
				this.$Request.getT('/app/consortia/selectConsortiaByUserId?consortiaId=' + consortiaId).then(res => {
					if (res.code == 0 && res.data) {
						this.shopOrder = res.data;
					}
				});
			},
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
									uni.showToast({
										name: "删除成功"
									})
									uni.navigateBack()
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
			getIsVip() {
				this.$Request.get("/app/UserVip/isUserVip").then(res => {
					if (res.code == 0) {
						this.isVip = res.data
						uni.setStorageSync('isVIP', res.data)
					}
				});
			},
			openpay() {
				this.showpay = true
			},
			// 支付订单
			pay() {
				let that = this
				this.showpay = false
				if (this.paySel != 0) {
					this.payJZ();
					return;
				}
				if (that.openWay == 1) { //零钱支付
					uni.showModal({
						title: '付款提示',
						content: '确认支付' + that.order.payMoney + '元吗?',
						success: function(re) {
							if (re.confirm) {
								that.$queue.showLoading('支付中...')
								console.log('用户点击确定');
								that.$Request.post("/app/artificer/payOrders", {
									ordersId: that.order.ordersId,
								}).then(res => {
									uni.hideLoading();
									if (res.code == 0) {
										uni.showToast({
											title: '支付成功'
										})
										that.getOrder()
									} else {
										uni.showToast({
											title: res.msg,
											icon: 'none'
										})
									}
								});
							} else if (re.cancel) {
								uni.hideLoading();
								console.log('用户点击取消');
							}
						}

					})
				} else if (that.openWay == 2) { //微信支付
					that.$queue.showLoading('支付中...')
					// #ifdef MP-WEIXIN
					let data = {
						ordersId: that.order.ordersId,
						type: 3
					}
					that.$Request.post('/app/wxPay/payOrder', data).then(ret => {
						console.log(ret)
						if (ret.code == 0) {
							uni.hideLoading();
							uni.requestPayment({
								provider: 'wxpay',
								timeStamp: ret.data.timestamp,
								nonceStr: ret.data.noncestr,
								package: ret.data.package,
								signType: ret.data.signType,
								paySign: ret.data.sign,
								success: function(ret) {
									console.log(ret)
									uni.showToast({
										title: '支付成功',
										icon: 'none'
									})
									that.getOrder()
									// this.$queue.showToast('支付成功');

									// uni.switchTab({
									// 	url: '/pages/my/index'
									// })
								},
								fail: function(err) {
									// this.$queue.showToast('支付失败');
									uni.showToast({
										title: '支付失败',
										icon: 'nones'
									});
									that.getOrder()
								}
							});
						}
					});
					// #endif

					// #ifdef H5
					let ua = navigator.userAgent.toLowerCase();
					if (ua.indexOf('micromessenger') != -1) {
						let data = {
							ordersId: that.order.ordersId,
							type: 2
						}
						that.$Request.post('/app/wxPay/payOrder', data).then(rea => {
							that.showpay = false
							if (rea.code == 0) {
								that.callPay(rea.data);
							} else {
								uni.showToast({
									title: rea.msg,
									icon: 'none'
								})
							}
						});
					} else {
						let data = {
							ordersId: that.order.ordersId,
							type: 4
						}
						that.$Request.post('/app/wxPay/payOrder', data).then(rea => {
							// const urlArr = window.location.href;
							// const hostUrl = urlArr.split("/");
							// const callBack = hostUrl[0] + "//" + hostUrl[2] + "/";
							// const url = '&redirect_url=' + callBack + 'my/order/pay';
							// window.location = rea.mweb_url + url
							that.showpay = false
							if (rea.code == 0) {
								const urlArr = window.location.href;
								const hostUrl = urlArr.split("/");
								const callBack = hostUrl[0] + "//" + hostUrl[2] + "/";
								const url = '&redirect_url=' + callBack + 'my/order/pay';
								window.location = rea.data.mweb_url + url
							} else {
								uni.showToast({
									title: rea.msg,
									icon: 'none'
								})
							}
						});
					}

					// #endif

					// #ifdef APP
					let data = {
						ordersId: that.order.ordersId,
						type: 1
					}
					that.$Request.post('/app/wxPay/payOrder', data).then(rea => {
						console.log(rea)
						that.showpay = false
						if (rea.code == 0) {
							that.isCheckPay(rea.code, 'wxpay', JSON.stringify(rea.data));
						}
					});
					// #endif
				} else if (that.openWay == 3) { //支付宝支付
					that.$queue.showLoading('支付中...')
					// #ifdef H5
					let data = {
						ordersId: that.order.ordersId,
						type: 2
					}
					that.$Request.post('/app/aliPay/payOrder', data).then(
						rea => {
							const div = document.createElement('div')
							div.innerHTML = rea.data //此处form就是后台返回接收到的数据
							document.body.appendChild(div)
							document.forms[0].submit()
						});
					// #endif

					// #ifdef APP-PLUS
					let data = {
						ordersId: that.order.ordersId,
						type: 1
					}
					that.$Request.post('/app/aliPay/payOrder', data).then(
						rea => {
							that.setPayment('alipay', rea.data);
						});
					// #endif
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
								title: '支付成功',
								icon: 'none'
							})
							that.getOrder()
						} else {
							uni.hideLoading();
						}
						WeixinJSBridge.log(response.err_msg);
					}
				);
			},
			isCheckPay(code, name, order) {
				if (code == 0) {
					this.setPayment(name, order);
				} else {
					uni.hideLoading();
					uni.showToast({
						title: '支付信息有误'
					});
				}
			},
			setPayment(name, order) {
				let that = this
				uni.requestPayment({
					provider: name,
					orderInfo: order, //微信、支付宝订单数据
					success: function(res) {
						uni.hideLoading();
						uni.showToast({
							title: '支付成功',
							icon: 'none'
						})
						that.getOrder()
					},
					fail: function(err) {
						uni.hideLoading();
					},
					complete() {
						uni.hideLoading();
					}
				});
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
							that.$queue.showLoading('提交中...')
							that.$Request.post('/app/artificer/deleteOrders', data).then(res => {
								uni.hideLoading();
								if (res.code == 0) {
									that.getOrder()
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
									uni.navigateBack()
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
			goMsg() {
				let data = {
					userId: uni.getStorageSync('userId'),
					focusedUserId: this.order.user.userId
				}
				this.$Request.postJson('/app/chat/insertChatConversation ', data).then(res => {
					if (res.code == 0) {
						let id = this.order.user.userId == res.data.userId ? res.data.focusedUserId : this.order
							.user.userId
						uni.navigateTo({
							url: '/pages/msg/im?chatConversationId=' + res.data.chatConversationId +
								'&byUserId=' + id
						})
					}
				})
			},
		}
	}
</script>

<style lang="scss">
	page {
		background: #f7f7f7;
	}

	.bgImg {
		background-image: url('https://anmo.xianmxkj.com/file/uploadPath/2023/01/03/02952e7260e553d0f3bb26a1cd154921.png');
		background-size: 100% 1161rpx;
	}

	.bg {
		background: #FFFFFF;
	}
	
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
	
	.list_item {
		// width: 650upx;
		// height: 160upx;
		width: 92%;
		// margin: 30rpx 0;
		background: #FFFFFF;
		border-radius: 24upx;
		padding: 20upx;
		display: flex;
		margin-top: 26upx;

		.listitem_rightview {
			margin-left: 20upx;
			width: 80%;
			padding-top: 10rpx;

			.btnview {
				display: flex;
				align-items: center;
				justify-content: space-between;
				margin-top: 12upx;
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
				font-size: 32upx;
				font-family: PingFang SC;
				font-weight: 800;
				color: #FF1200;
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

	.jz_btn {
		width: 200rpx;
		height: 70rpx;
		background: linear-gradient(90deg, #55B55C 0%, #308251 100%);
		border-radius: 35rpx;
		line-height: 70rpx;
		text-align: center;
		font-size: 28rpx;
		font-family: PingFang SC;
		font-weight: bold;
		color: #FFFFFF;
	}

	.tabber {
		width: 100%;
		background: #ffffff;
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		justify-content: flex-end;
		height: 127rpx;
		padding-right: 30rpx;
	}

	.box {
		/* border: 1px solid #005dff; */
		background: #E8FAE1;
		border-radius: 4rpx;
		color: #308251;
		padding: 5upx 10upx;
		font-size: 22rpx;
		margin-top: 10rpx;
	}

	.popup_pay {
		width: 100%;
		position: relative;
		padding-bottom: 45rpx;
		/* height: 160px; */
		/* #ifndef MP-WEIXIN */
		/* height: 130px; */
		/* #endif */

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
</style>