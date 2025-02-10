<template>
	<view style="padding-bottom: 200upx;" v-if="XCXIsSelect !='否'">
		<view class="padding-sm">
			<view class="bg  u--fillet-skeleton" style="border-radius:24rpx;" v-if="isShop != 1">
				<view class="flex align-center padding" @click="getAddressList()">
					<view class="u-flex-1  margin-left-xs">
						<view v-if="!detailaddress">请选择地址</view>
						<view v-else>
							<view style="color: #333333;font-size: 32rpx;font-weight: bold;">{{name}}<text
									class="margin-left-sm">{{mobile}}</text>
							</view>
							<view class="" style="color: #999999;margin-top: 8rpx;">
								{{province}}/{{city}}/{{district}}/{{detailaddress}}
							</view>
						</view>
					</view>
					<u-icon name="arrow-right" color="#999999"></u-icon>
				</view>
			</view>
			<view class="bg radius margin-top u-skeleton-fillet" style="padding: 16rpx;">
				<view class="flex justify-around">
					<view class="" style="width: 200rpx;height: 200rpx;">
						<image :src="artificerlist.massageImg" style="width: 100%;height: 100%;border-radius: 24upx;">
						</image>
					</view>
					<view class="u-flex-1  margin-left-sm">
						<view class="text-lg text-bold">
							{{artificerlist.title}}
						</view>
						<view class="flex justify-between margin-tb" style="color: #999999;">
							<view>
								<image src="../../../static/time.png"
									style="width: 26rpx;height: 26rpx;margin-right: 8rpx;"></image>
								{{artificerlist.duration}}分钟
							</view>
							<view>已售{{artificerlist.sales}}</view>
						</view>
						<view class="flex justify-between">
							<view class=" text-df" style="color: #FF1200;">
								￥<text class=" text-xl text-bold">{{artificerlist.price}}</text>
								<text v-if="hyCheck != '否'">/</text><text v-if="hyCheck != '否'"
									class="text-sm">会员价:￥{{artificerlist.memberPrice}}</text>
							</view>
							<!-- <view>￥{{price}}</view> -->
							<view>
								<u-number-box v-model="number" @change="valChange" :min="1" :max="100"></u-number-box>
							</view>
						</view>

					</view>
				</view>
			</view>

			<view class="padding-sm bg radius u-skeleton-fillet margin-top-sm">
				<view class="flex justify-between align-center" style="padding: 0rpx 10rpx;line-height: 106upx;">
					<view class="text-30" style="width: 30%;">
						服务助教
					</view>
					<view class="flex align-center">
						<text class="margin-right-xs">{{order.artificerName}}</text>
						<image :src="order.artificerImg" style="width: 48upx;height: 48upx;border-radius: 50%;"></image>
					</view>
				</view>
				<view class="xian"></view>
				<view class="flex align-center justify-between"
					style="height: 100rpx;line-height: 100rpx;padding: 0rpx 10rpx;">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">预约时间</view>
					<view class="flex justify-between margin-tb-sm" @click="openData()">
						<view v-if="startTime">{{startTime}}</view>
						<view class="tetx-sm" style="color:#999999;" v-else>请选择预约时间</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<view class="xian" v-if="cxSel && isShop != 1"></view>
				<view class="flex justify-between align-center" v-if="cxSel && isShop != 1"
					style="height: 100rpx;line-height: 100rpx;padding: 0rpx 10rpx;">
					<view class="text-30" style="width: 30%;">
						出行方式
					</view>
					<view class="flex align-center" style="color: #333333;font-size: 26rpx;align-items: center;">
						<view style="font-size: 28rpx;">
							<text v-if="tripWay == 1">公交</text>
							<text v-if="tripWay == 2">出租</text>
							<text v-if="tripWay == 3">免费</text>
						</view>
					</view>
				</view>
				<!-- <view class="xian"></view> -->
				<view style="color: #999999;margin-left: 5rpx;" v-if="distances && cxSel && isShop != 1">
					全程{{distances}}公里，起步{{tripNum}}公里，起步价{{tripPrice}}元，超出每公里{{xuMoney}}元
				</view>
				<view class="flex justify-between align-center"
					style="height: 100rpx;line-height: 100rpx;padding: 0rpx 10rpx;"
					v-if="cheFei && cheFei > 0 && cxSel && isShop != 1">
					<view class="text-30" style="width: 30%;">
						车费
					</view>
					<view class="flex align-center" style="color: #999999;font-size: 22rpx;">
						<view class="text-bold" style="color: #FF1200;font-size: 32rpx;">{{cheFei}}元</view>
					</view>
				</view>

				<view class="xian" v-if="CouponIssueList.length > 0"></view>
				<view class="flex align-center justify-between" v-if="CouponIssueList.length > 0"
					style="height: 100rpx;line-height: 100rpx;padding: 0rpx 10rpx;">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">优惠券</view>
					<view class="flex justify-between margin-tb-sm" @click="showCoup">
						<view v-if="couponName">{{couponName}}</view>
						<view class="tetx-sm" style="color:#999999;" v-else>请选择优惠券</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>

				<view class="xian"></view>
				<view class="margin-top-sm" style="padding: 0rpx 10rpx;">
					<view class="text-30">
						备注
					</view>
					<view style="color: #999999;font-size: 22rpx;">
						<!-- <input v-model="beizhu" type="textarea" placeholder="请输入备注" maxlength="300" v-if="isTrues" />
						<input v-model="beizhu" type="textarea" placeholder="请输入备注" maxlength="300" :disabled="true"
							v-else /> -->
						<u-input v-model="beizhu" type="textarea" placeholder="请输入备注" :clearable="false" maxlength="300"
							:custom-style="boxStyle" height="200" />
					</view>
				</view>
			</view>
		</view>
		<!-- <view class="footer" style="font-size: 26upx;" @tap="isShowAgree">
			<view style="font-size: 26upx;display: flex;align-items: center;" class="cuIcon"
				:class="showAgree?'cuIcon-radiobox':'cuIcon-round'">
				<text>我已阅读并同意</text>
				<navigator url="/my/setting/about?id=391&name=下单协议" open-type="navigate">《下单协议》</navigator>
			</view>
		</view> -->
		<view class="text-white flex justify-between cu-bar foot bg padding-lr" style="color: #308251;" v-if="cxSel">
			<view style="color: #999999;">
				合计：<text style="color: #308251;font-size: 38rpx;">￥{{price + cheFei}}</text>
			</view>
			<view class="">
				<u-button :custom-style="customStyle" @click="openpay()" shape="circle" :hair-line="false">确认预约
				</u-button>
			</view>
		</view>

		<u-skeleton :loading="loading" :animation="true" elColor='#FFFFFF' bgColor='#FFFFFF'></u-skeleton>

		<u-popup v-model="isTrue" mode="bottom">
			<view>
				<scroll-view scroll-y="true">
					<its-calendar :sta_num="0" :end_num="24" :int_num="msTimeDate" :msTimeList="msTimeList"
						@getTime="TimeData">
					</its-calendar>
				</scroll-view>
			</view>
		</u-popup>

		<!-- 支付方式 -->
		<u-popup v-model="showpay" mode="center" :closeable="closeable" width="560rpx" border-radius="14">
			<view class="popup_pay">
				<view style="background-color: #fff;">
					<view style="padding: 0 20upx;margin-bottom: 20rpx;margin-top: 60rpx;">
						<view
							style="display: flex;height: 100upx;align-items: center;padding: 20upx 0;justify-content: center;"
							v-for="(item,index) in openLists" :key='index'>
							<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;">
							</image>
							<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">
								{{item.text}}
							</view>
							<radio-group name="openWay" style="margin-left: 45upx;" @tap.stop='selectWay(item)'>
								<label class="tui-radio">
									<radio color="#2FB57A" :checked="openWay === item.id ? true : false" />
								</label>
							</radio-group>
						</view>
					</view>
				</view>
				<view class="pay_btn" @click="pay()">确认支付</view>
			</view>
			<!-- </view> -->
		</u-popup>
		<u-popup v-model="showCoupon" mode="bottom" border-radius="14" :closeable="true">
			<view
				style="width: 100%;height: 1000rpx;background: #f8f8f8;border-top-left-radius: 20upx;border-top-right-radius: 20upx;padding: 40upx;">
				<view style="width: 100%;text-align: center;font-size: 32rpx;color: #1a1a1a;">我的优惠券</view>
				<scroll-view scroll-y="true" style="height: 900rpx;">
					<view class="flex align-center justify-end">
						<view class="btnyouhui" @click="quxiao()">取消使用</view>
					</view>
					<view v-for="(item,index) in CouponIssueList" :key="index" class="bg padding radius"
						style="margin: 24upx 0upx;">
						<view class="flex justify-between">
							<view class="text-lg text-bold">{{item.couponName}}</view>
							<view class="text-lg" style="color: #FF130A;">￥<text
									class="text-df text-xxl">{{item.money}}</text>
							</view>
						</view>
						<view>{{item.endDate}}</view>
						<view style="width: 638upx;height: 2upx;border: 2upx dashed #E6E6E6;margin: 29upx auto;"></view>
						<view class="flex justify-between">
							<view style="color: #999999;">满{{item.minMoney}}元可用</view>
							<view class="btn" @tap="goFaBu(item)">立即使用</view>
						</view>
					</view>
					<!-- <view style="width: 100%;text-align: center;" v-for='(item,index) in CouponIssueList' :key='index'>
						<view
							style="background: #fcf3e8;width: 100%;height: 130rpx;border-radius: 10rpx;margin-top: 20rpx;">
							<view style="display: flex;color: #1a1a1a;width: 100%;">
								<view
									style="line-height: 130rpx;margin-left: 0rpx;font-size: 40rpx;color: #1a1a1a;font-weight: 600;width: 150rpx;">
									<text style="font-size: 20upx;">￥</text>{{item.money}}
								</view>
								<view style="margin-left: 20rpx;width: 50%;text-align: left;">
									<view style="margin-top: 25rpx;">
										{{item.couponName}}</view>
									<view style="margin-top: 10rpx;font-size: 26rpx;">满{{item.minMoney}}减{{item.money}}</view>
								</view>
								<view
									style="height: 105rpx;width: 2rpx;background: #1a1a1a;margin-left: 20rpx;margin-top: 15rpx;">
								</view>
								<view
									style="color: #1a1a1a;line-height: 130rpx;height: 130rpx;width: 145rpx;font-weight: 600;"
									@tap='goFaBu(item)'>立即使用</view>
							</view>
						</view>
					</view> -->
				</scroll-view>
			</view>
		</u-popup>
		<!-- <u-popup v-model="showpay" mode="bottom" :closeable="closeable" style="position: fixed;bottom: 0;"> -->
		<!-- <view style="width: 100%;height: 100vh;background: rgba(0,0,0,0.8);z-index: 991;position: absolute;top: 0;left: 0;" v-if="showpay" @touchmove.stop.prevent="moveHandle">
			<view class="popup_pay" >
				<view style="background-color: #fff;">
					<view style="width: 90%;margin: 0 auto;text-align: end;margin-top: 15px;" @tap.stop='close()'>
						<u-icon name="close-circle" size="40" color="#CCCCCC"></u-icon>
					</view>
					<view style="padding: 0 20upx;margin-bottom: 20rpx;">
						<view
							style="display: flex;height: 100upx;align-items: center;padding: 20upx 0;justify-content: center;"
							v-for="(item,index) in openLists" :key='index'>
							<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;">
							</image>
							<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">
								{{item.text}}
							</view>
							<radio-group name="openWay" style="margin-left: 45upx;" @tap.stop='selectWay(item)'>
								<label class="tui-radio">
									<radio color="#2FB57A" :checked="openWay === item.id ? true : false" />
								</label>
							</radio-group>
						</view>
					</view>
				</view>
				<view class="pay_btn" @click="pay()">确认支付</view>
			</view>
		</view> -->
		<!-- </u-popup> -->

		<!-- <u-picker v-model="addshow" mode="region" @confirm="addData"></u-picker> -->
	</view>
</template>

<script>
	import itsCalendar from '@/components/its-calendar/its-calendar.vue';
	export default {
		components: {
			itsCalendar
		},
		data() {
			return {
				isShop: '',
				msTimeDate: 0,
				showAgree: true,
				hyCheck: '否',
				cxSel: true,
				CouponIssueList: [],
				couponName: '',
				couponId: '',
				couponMoney: 0,
				showCoupon: false,
				cheFei: 0,
				loading: true, // 是否显示骨架屏组件
				customStyle: {
					width: '160px',
					color: '#FFFFFF',
					background: "#308251",
					border: 0
				},
				boxStyle: {
					color: '#000000',
					padding: '20rpx 0 0 0'
				},
				value: '',
				remark: '',
				artificerId: '',
				order: [],
				isVip: false,
				// addshow: false,
				startTime: '',
				address: '',
				// detailAddress: '',
				name: '',
				phone: '',
				beizhu: '',
				price: 0,
				money: 0,
				number: 1,
				isTrue: false,
				artificerlist: [],
				latitude: '',
				longitude: '',
				massageTypeId: '',
				isVIP: false,
				showpay: false,
				openLists: [],
				openWay: 1,
				closeable: true,
				isTrues: true,
				XCXIsSelect: '否',
				tripWay: 0,
				msTimeList: [],
				detailaddress: '',
				province: '',
				city: '',
				district: '',
				name: '',
				mobile: '',
				addressId: '',
				chuxing: '',
				distances: '',
				tripNum: '',
				xuMoney: '',
				tripPrice: '',
				artificerMoney: 0
			}
		},
		onLoad(option) {
			if (option.isShop) {
				this.isShop = option.isShop;
			}
			if (this.$queue.getData('msTimeDate')) {
				this.msTimeDate = parseInt(this.$queue.getData('msTimeDate'));
			} else {
				this.msTimeDate = 60;
			}
			this.hyCheck = this.$queue.getData('hyCheck');
			console.log(option)
			this.artificerId = option.artificerId
			this.massageTypeId = option.massageTypeId
			if (option.tripWay) {
				this.tripWay = option.tripWay;
			}
			this.isVIP = uni.getStorageSync('isVIP')
			this.getDet()
			this.getordertherapist()
			this.XCXIsSelect = this.$queue.getData("XCXIsSelect");
			var date = new Date();
			var year = date.getFullYear();
			let month = (parseInt(date.getMonth()) + 1) > 9 ? (parseInt(date.getMonth()) + 1) : "0" + (parseInt(
				date.getMonth()) + 1) // 当前月份
			let days = (date.getDate()) > 9 ? date.getDate() : "0" + date.getDate() //当前日期
			let fullDate = `${month}-${days}`
			let Time = year + '-' + fullDate
			this.yearsDate = Time;
			this.getMsTime(Time);
			this.getMoneyJiShi();
			// #ifdef APP
			this.openLists = [{
					image: '../../../static/images/icon_weixin.png',
					text: '微信支付',
					id: 2
				}, {
					image: '../../../static/images/zhifubao.png',
					text: '支付宝支付',
					id: 3
				}, {
					image: '../../../static/images/lingqian.png',
					text: '零钱支付',
					id: 1
				}],
				this.openWay = 2;
			// #endif 

			// #ifdef MP-WEIXIN
			this.openLists = [{
					image: '../../../static/images/icon_weixin.png',
					text: '微信支付',
					id: 2
				}, {
					image: '../../../static/images/lingqian.png',
					text: '零钱支付',
					id: 1
				}],
				this.openWay = 2;
			// #endif

			// #ifdef H5
			this.openLists = [{
					image: '../../../static/images/icon_weixin.png',
					text: '微信支付',
					id: 2
				}, {
					image: '../../../static/images/zhifubao.png',
					text: '支付宝支付',
					id: 3
				}, {
					image: '../../../static/images/lingqian.png',
					text: '零钱支付',
					id: 1
				}],
				this.openWay = 2;
			// #endif
		},
		onShow() {
			this.addressId = this.$queue.getData('EditAddress');
			if (this.addressId) {
				this.getAddressList(this.addressId);
			} else {
				this.addressMy()
			}
		},
		methods: {
			isShowAgree() {
				//是否选择协议
				this.showAgree = !this.showAgree;
			},
			getMoneyJiShi() {
				this.$Request.getT('/app/userMoney/selectUserMoneyArtificer?page=1&limit=1&artificerId=' + this
					.artificerId).then(res => {
					if (res.code == 0 && res.data) {
						if (res.data.list && res.data.list.length > 0) {
							this.artificerMoney = res.data.list[0].money ? res.data.list[0].money : 0
						}
					}
				});
			},
			getMsTime(artificerDate) {
				this.$Request.getT('/app/artificerTime/selectArtificerTimeListByArtificerId?artificerId=' + this
					.artificerId + '&artificerDate=' + artificerDate).then(res => {
					if (res.code == 0) {
						this.msTimeList = res.data;
					}
				})
			},
			quxiao() { //取消使用
				this.showCoupon = false;
				this.couponId = ''
				this.couponName = ''
				this.price = parseFloat(this.money) * parseFloat(this.number);
			},
			goFaBu(item) {
				this.showCoupon = false;
				this.couponName = '￥-' + item.money;
				this.couponMoney = item.money
				this.couponId = item.id;
				let money = parseFloat(this.money) * parseFloat(this.number);
				this.price = parseFloat(money) - parseFloat(item.money);
			},
			showCoup() {
				this.showCoupon = true;
			},
			getYHQList() {
				this.$Request.getT('/app/coupon/selectUserCouponList?page=1&limit=50&minMoney=' + this.price).then(res => {
					if (res.code == 0) {
						this.CouponIssueList = res.data.list;
					}
				});
			},
			getChuXing() {
				if (this.isShop != 1) {
					this.$Request.getT('/app/artificer/selectTaxiMoney?artificerId=' + this.artificerId + '&latitude=' +
						this
						.latitude + '&longitude=' + this.longitude).then(res => {
						if (res.code == 0) {
							this.cheFei = res.data.taxiMoney ? res.data.taxiMoney : 0;
							// this.price = parseInt(this.price)+parseInt(this.cheFei).toFixed(2)
							this.distances = res.data.distances
							this.tripNum = res.data.tripNum
							this.xuMoney = res.data.xuMoney
							this.tripPrice = res.data.tripPrice
							this.cxSel = true;
						} else {
							this.cxSel = false;
							uni.showToast({
								title: '当前城市暂未开通，请切换城市',
								duration: 2000,
								icon: 'none'
							});
						}
					});
				}
			},
			// 根据地址id查询地址
			getAddressList(addressId) {
				if (addressId) {
					this.$Request.getT('/app/address/selectAddressByAddressId?addressId=' + this.addressId).then(res => {
						console.log(res)
						if (res.code == 0) {
							this.name = res.data.name;
							this.mobile = res.data.phone;
							this.cityaddress = res.data.province + res.data.city + res.data.district;
							this.detailaddress = res.data.detailsAddress;
							this.isDefault = res.data.isDefault;
							this.userId = res.data.userId;
							this.latitude = res.data.latitude;
							this.longitude = res.data.longitude;
							this.province = res.data.province
							this.city = res.data.city
							this.district = res.data.district

							this.getChuXing();
						}
						// uni.hideLoading();
					});
				} else {
					uni.navigateTo({
						url: '../../../my/address/address?id=' + 1
					})
				}
			},
			// 获取默认地址
			addressMy() {
				this.$Request.getT('/app/address/selectAddressById').then(res => {
					console.log(res)
					if (res.code == 0) {
						this.name = res.data.name;
						this.mobile = res.data.phone;
						this.cityaddress = res.data.province + res.data.city + res.data.district;
						this.detailaddress = res.data.detailsAddress;
						this.isDefault = res.data.isDefault;
						this.userId = res.data.userId;
						this.latitude = res.data.latitude;
						this.longitude = res.data.longitude;
						this.province = res.data.province
						this.city = res.data.city
						this.district = res.data.district
						this.addressId = res.data.addressId
						this.getChuXing();
						// this.$queue.setData('EditAddress', res.data.addressId);
					}
				});
			},
			moveHandle() {},
			//关闭弹框
			close() {
				this.showpay = false
				this.isTrues = true
			},
			//支付选择
			selectWay: function(item) {
				this.openWay = item.id;
			},
			openData() {
				this.isTrue = true
			},
			// 时间段数据
			TimeData(e) {
				console.log(e, e.days)
				var date = new Date();
				var year = date.getFullYear();
				// if(this.msTimeList.length > 0){
				// 	let startTime = year + '-' + e.days + ' ' + e.hours
				// 	for (var i = 0; i < this.msTimeList.length; i++) {
				// 		let msTime = this.msTimeList[i].artificerDate + ' ' + this.msTimeList[i].artificerTime
				// 		if(startTime === msTime){
				// 			this.isTrue = false
				// 			uni.showModal({
				// 				title: '温馨提示',
				// 				content: '当前时段已被预约，请更换预约时段！',
				// 				showCancel: true,
				// 				cancelText: '取消',
				// 				confirmText: '确定',
				// 				success: res => {
				// 					if(res.confirm){
				// 						this.isTrue = true
				// 					}
				// 				}
				// 			});
				// 			return;
				// 		}
				// 	}
				// }else{
				this.startTime = year + '-' + e.days + ' ' + e.hours
				this.isTrue = false
				// }
			},
			valChange(e) {
				console.log('当前值为: ' + e.value)
				this.couponId = ''
				this.couponName = ''
				this.number = e.value
				this.price = e.value * this.money

				// this.money = e.value * this.price
			},
			//获取省市区
			city(latitude, longitude) {
				this.$Request.get("/app/Login/selectCity", {
					lat: latitude,
					lng: longitude
				}).then(res => {
					console.log(res)
					this.address = res.data.province + res.data.city + res.data.district
					console.log(this.address)
				});
			},
			bindData(index) {
				let that = this
				uni.chooseLocation({
					success: function(res) {
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
						that.latitude = res.latitude
						that.longitude = res.longitude
						that.city(res.latitude, res.longitude)
						that.detailAddress = res.name
					}
				});
			},
			//服务项目详情
			getordertherapist() {
				let hyCheck = this.$queue.getData('hyCheck');
				if (hyCheck != '否') {
					this.isVip = uni.getStorageSync('isVIP')
				} else {
					this.isVip = false
				}
				this.$Request.get("/app/artificer/selectMassageTypeById", {
					massageTypeId: this.massageTypeId
				}).then(res => {
					if (res.code == 0) {
						console.log(this.isVip)
						if (this.isVip == false) {
							this.price = res.data.price
							this.money = res.data.price
							console.log(this.price, 1111111111)
						} else {
							this.price = res.data.memberPrice
							this.money = res.data.memberPrice
							console.log(this.price, 2222)
						}
						console.log(this.price)
						this.artificerlist = res.data
						this.getYHQList();
					}
				});
			},
			// 技师详情
			getDet() {
				this.$Request.get("/app/artificer/selectArtificerById", {
					artificerId: this.artificerId,
				}).then(res => {
					this.loading = false;
					if (res.code == 0) {
						this.order = res.data
						this.tripWay = res.data.tripWay
					} else {
						uni.showToast({
							title: res.msg,
							duration: 1000,
							icon: 'none'
						});
					}
				});
			},
			openpay() {
				if (!this.startTime) {
					uni.showToast({
						title: '请选择预约时间',
						icon: 'none',
						duration: 1000
					})
					return
				}

				if (!this.detailaddress && this.isShop != 1) {
					uni.showToast({
						title: '请选择服务地址',
						icon: 'none',
						duration: 1000
					})
					return
				}
				// if (!this.detailAddress) {
				// 	uni.showToast({
				// 		title: '请输入门牌号',
				// 		icon: 'none',
				// 		duration: 1000
				// 	})
				// 	return
				// }
				if (!this.name) {
					uni.showToast({
						title: '请输入预定人真实姓名',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.mobile) {
					uni.showToast({
						title: '请输入联系电话',
						icon: 'none',
						duration: 1000
					})
					return
				}
				let glsNum = this.$queue.getData('glsNum');
				if (parseFloat(this.distances) > parseFloat(glsNum)) {
					this.$queue.showToast('当前最大接单范围为' + glsNum + '公里，请重新选择地址信息！');
					return;
				}
				if (!this.showAgree) {
					this.$queue.showToast('请阅读并同意《下单协议》');
					return;
				}
				let that = this;
				if (this.artificerMoney && parseFloat(this.artificerMoney) >= parseFloat((this.price + this.cheFei))) {
					uni.showModal({
						title: '温馨提示',
						content: '是否从预存金额中直接扣除？',
						showCancel: true,
						cancelText: '取消',
						confirmText: '确认',
						success: res => {
							if (res.confirm) {
								let data = {
									address: that.province + '' + that.city + '' + that.district + '' +
										that.detailaddress,
									artificerId: that.artificerId,
									tripWay: that.tripWay,
									km: that.distances,
									ordersMassageList: [{
										massageId: that.massageTypeId,
										num: that.number,
									}],
									ordersType: that.isShop ? 2 : 1,
									city: uni.getStorageSync('city'),
									price: that.money,
									couponId: that.couponId,
									payMoney: (that.price + that.cheFei),
									userId: uni.getStorageSync('userId'),
									serveTime: that.startTime,
									userName: that.name,
									phone: that.mobile,
									remark: that.beizhu,
									latitude: that.latitude,
									longitude: that.longitude
								}
								that.$Request.postJson("/app/artificer/insertOrders", data).then(res => {
									if (res.code == 0) {
										that.$queue.showLoading('支付中...')
										that.$Request.post("/app/artificer/payOrders", {
											ordersId: res.data.ordersId,
										}).then(ret => {
											uni.hideLoading();
											if (ret.code == 0) {
												uni.showToast({
													title: '支付成功',
													icon: 'none'
												})
												setTimeout(function() {
													uni.switchTab({
														url: '/pages/order/index'
													})
												}, 1000)

											} else {
												uni.showToast({
													title: ret.msg,
													icon: 'none'
												})
											}
										});
									} else {
										that.showpay = true
										that.isTrues = false
									}
								});
							} else {
								that.showpay = true
								that.isTrues = false
							}
						}
					});
				} else {
					that.showpay = true
					that.isTrues = false
				}
			},

			pay() {
				let that = this
				let data = {
					address: that.province + '' + that.city + '' + that.district + '' + that.detailaddress,
					artificerId: that.artificerId,
					ordersMassageList: [{
						massageId: that.massageTypeId,
						num: that.number,
					}],
					city: uni.getStorageSync('city'),
					price: that.money,
					tripWay: that.tripWay,
					km: that.distances,
					ordersType: that.isShop ? 2 : 1,
					couponId: that.couponId,
					payMoney: (that.price + that.cheFei),
					userId: uni.getStorageSync('userId'),
					serveTime: that.startTime,
					userName: that.name,
					phone: that.mobile,
					remark: that.beizhu,
					latitude: that.latitude,
					longitude: that.longitude
				}
				that.$Request.postJson("/app/artificer/insertOrders", data).then(res => {
					that.showpay = false
					if (res.code == 0) {
						if (that.openWay == 1) { //零钱支付
							uni.showModal({
								title: '付款提示',
								content: '确认支付' + (that.price + that.cheFei) + '元吗?',
								success: function(re) {
									if (re.confirm) {
										that.$queue.showLoading('支付中...')
										// console.log('用户点击确定');
										that.$Request.post("/app/artificer/payOrders", {
											ordersId: res.data.ordersId,
										}).then(ret => {
											uni.hideLoading();
											if (ret.code == 0) {
												uni.showToast({
													title: '支付成功',
													icon: 'none'
												})
												setTimeout(function() {
													uni.switchTab({
														url: '/pages/order/index'
													})
												}, 1000)

											} else {
												uni.showToast({
													title: ret.msg,
													icon: 'none'
												})
											}
										});
									} else if (re.cancel) {
										uni.redirectTo({
											url: '/my/order/pay?ordersId=' + res.data.ordersId
										})
									}
								}
							})
						} else if (that.openWay == 2) { //微信支付
							// #ifdef MP-WEIXIN
							let data = {
								ordersId: res.data.ordersId,
								type: 3
							}
							that.$Request.post('/app/wxPay/payOrder', data).then(ret => {
								console.log(ret)
								if (ret.code == 0) {
									this.showpay = false
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
											setTimeout(function() {
												uni.switchTab({
													url: '/pages/order/index'
												})
											}, 1000)
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
											uni.redirectTo({
												url: '/my/order/pay?ordersId=' + res
													.data.ordersId
											})
										}
									});
								}
							});
							// #endif

							// #ifdef H5
							let ua = navigator.userAgent.toLowerCase();
							if (ua.indexOf('micromessenger') != -1) {
								let data = {
									ordersId: res.data.ordersId,
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
									ordersId: res.data.ordersId,
									type: 4
								}
								that.$Request.post('/app/wxPay/payOrder', data).then(rea => {
									that.showpay = false
									if (rea.code == 0) {
										const urlArr = window.location.href;
										const hostUrl = urlArr.split("/");
										const callBack = hostUrl[0] + "//" + hostUrl[2] + "/";
										const url = '&redirect_url=' + callBack + 'pages/order/index';
										window.location = rea.data.mweb_url + url
									} else {
										uni.showToast({
											title: rea.msg,
											icon: 'none'
										})
									}
									// const urlArr = window.location.href;
									// const hostUrl = urlArr.split("/");
									// const callBack = hostUrl[0] + "//" + hostUrl[2] + "/";
									// const url = '&redirect_url=' + callBack + 'pages/order/index';
									// window.location = rea.mweb_url + url
								});
							}
							// #endif

							// #ifdef APP
							console.log(res.data.ordersId, '9999999')
							let data = {
								ordersId: res.data.ordersId,
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
								ordersId: res.data.ordersId,
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
								ordersId: res.data.ordersId,
								type: 1
							}
							that.$Request.post('/app/aliPay/payOrder', data).then(
								rea => {
									that.showpay = false
									that.setPayment('alipay', rea.data);
								});
							// #endif


						}

					} else {
						that.$queue.showToast(res.msg)
					}

				});
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
							setTimeout(function() {
								uni.switchTab({
									url: '/pages/order/index'
								})
							}, 1000)
						} else {
							uni.hideLoading();
						}
						WeixinJSBridge.log(response.err_msg);
					}
				);
			},
			isCheckPay(code, name, order) {
				if (code == 0) {
					console.log('999999999999')
					this.setPayment(name, order);
				} else {
					uni.hideLoading();
					uni.showToast({
						title: '支付信息有误'
					});
				}
			},
			setPayment(name, order) {
				console.log(777777777, name, order)
				uni.requestPayment({
					provider: name,
					orderInfo: order, //微信、支付宝订单数据
					success: function(res) {
						uni.hideLoading();
						uni.showToast({
							title: '支付成功',
							icon: 'none'
						})
						setTimeout(function() {
							uni.switchTab({
								url: '/pages/order/index'
							})
						}, 1000)
					},
					fail: function(err) {
						uni.hideLoading();
					},
					complete() {
						uni.hideLoading();
					}
				});
			}
		},
		computed: {

		}
	}
</script>

<style>
	textarea::-webkit-input-placeholder {
		color: red;

	}

	page {
		background-color: #F5F5F5;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.box {
		border: 1px solid #2FB57A;
		border-radius: 4rpx;
		color: #308251;
		padding: 0rpx 8rpx;
		font-size: 22rpx;
		margin-top: 10rpx;
	}

	.btn {
		width: 140upx;
		height: 55upx;
		border: 2upx solid #FF130A;
		border-radius: 30upx;
		text-align: center;
		line-height: 55upx;
		color: #FF130A;
		font-size: 24upx;
	}

	/* 底部 */
	.footer {
		display: flex;
		flex-direction: row;
		font-size: 28upx;
		margin-top: 10upx;
		color: rgba(0, 0, 0, 0.7);
		text-align: center;
		height: 40upx;
		line-height: 40upx;
		margin-left: 30rpx;
	}

	.footer text {
		color: #333333;
		font-size: 24upx;
		margin-left: 10rpx;
		margin-right: 15upx;
	}

	.xian {
		width: 100%;
		height: 1rpx;
		background: #f8f8f8;
		/* border: 1rpx solid #f8f8f8; */
	}

	.popup_pay {
		/* height: 160px; */
		/* #ifndef MP-WEIXIN */
		/* height: 130px; */
		/* #endif */
		width: 100%;
		padding-bottom: 45rpx;
		/* background: #ffffff;
		position: fixed;
		bottom: 0px;
		left: 0;
		right: 0;
		z-index: 999; */
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

	.btnyouhui {
		border: 1rpx solid #FF130A;
		color: #FF130A;
		border-radius: 24rpx;
		width: 130rpx;
		height: 55rpx;
		text-align: center;
		line-height: 55rpx;
		font-size: 24rpx;
	}
</style>