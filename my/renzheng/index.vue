<template>
	<view style="padding-bottom: 100upx;">
		<view class="text-center text-red bg-white">{{form.remek}}</view>

		<view class="hb" style="width: 100%;">
			<view class="flex text-center text-white text-lg bg">
				<view class="title_btn flex-sub bg" @click="cut(1)" :class="title_color==1?'bgCol2': ''">个人认证
				</view>
				<view class="title_btn flex-sub bg" @click="cut(2)" :class="title_color==2?'bgCol2': ''">企业认证
				</view>
			</view>

			<view class=" padding" v-if="title_color==1">

				<view class=" padding bg radius">
					<u-form :model="form" ref="uForm" label-position="top" :label-style='lableStyle'>
						<u-form-item label="真实姓名" :border-bottom='false'>
							<u-input placeholder="请输入真实姓名" v-model="form.name" maxlength="11" />
						</u-form-item>
						<u-form-item label="证件号码" :border-bottom='false'>
							<u-input placeholder="请输入身份证号码" v-model="form.idNumber" />
						</u-form-item>
						<u-form-item label="出生日期" :border-bottom='false'>
							<u-input placeholder="请填写（必填）" v-model="form.birthdate" type="select"
								@click="birthData()" />
						</u-form-item>
						<u-form-item label="性别" :border-bottom='false'>
							<u-input placeholder="请填写（必填）" v-model="form.sex" type="select" @click="bindSex()" />
						</u-form-item>
						<u-form-item label="联系方式" :border-bottom='false'>
							<u-input placeholder="请填写（必填）" v-model="form.phone" type="number" maxlength="11" />
						</u-form-item>
					</u-form>
				</view>
				<view class=" padding bg radius margin-top">
					<view>
						<view class="text-lg margin-top-sm">上传身份证正面</view>
						<view class="flex" style="overflow: hidden;flex-direction: initial;">
							<view v-if="form.front.length">
								<view class="margin-top flex margin-right-sm">
									<view class="flex"
										style="width: 200rpx;height: 200rpx;margin-right: 10rpx;position: relative;">
										<image :src="form.front" style="width: 100%;height: 100%;"></image>
										<view style="z-index: 9;position: absolute;top: -15rpx;right: -15rpx;"
											@click="frontRemove(1)">
											<u-icon name="close-circle-fill" color="#2979ff" size="50rpx"></u-icon>
										</view>
									</view>
								</view>
							</view>
							<view class="margin-top" @click="addImage(1)" v-if="form.front.length<=0">
								<view style="width: 200rpx;height: 200rpx;background: #f4f5f6;"
									class="flex justify-center align-center">
									<view>
										<view class="text-center">
											<image src="../../static/images/my/add.png"
												style="width: 65rpx;height: 55rpx;"></image>
										</view>
										<view class="text-center">添加图片</view>
									</view>
								</view>
							</view>

						</view>
					</view>
					<view>
						<view class="text-lg margin-top-sm">上传身份证反面</view>
						<view class="flex" style="overflow: hidden;flex-direction: initial;">
							<view v-if="form.back.length">
								<view class="margin-top flex margin-right-sm">
									<view class="flex"
										style="width: 200rpx;height: 200rpx;margin-right: 10rpx;position: relative;">
										<image :src="form.back" style="width: 100%;height: 100%;"></image>
										<view style="z-index: 9;position: absolute;top: -15rpx;right: -15rpx;"
											@click="backRemove()">
											<u-icon name="close-circle-fill" color="#2979ff" size="50rpx"></u-icon>
										</view>

									</view>
								</view>
							</view>
							<view class="margin-top" @click="addImage(2)" v-if="form.back.length<=0">
								<view style="width: 200rpx;height: 200rpx;background: #f4f5f6;"
									class="flex justify-center align-center">
									<view>
										<view class="text-center">
											<image src="../../static/images/my/add.png"
												style="width: 65rpx;height: 55rpx;"></image>
										</view>
										<view class="text-center">添加图片</view>
									</view>
								</view>
							</view>

						</view>
					</view>
					<view>
						<view class="text-lg margin-top-sm">母婴护理师证</view>
						<view class="flex" style="overflow: hidden;flex-wrap: wrap;">
							<view v-if="infantImg.length">
								<view class="margin-top flex margin-right-sm flex-wrap">
									<view class="flex"
										style="width: 200rpx;height: 200rpx;margin-right: 5rpx;position: relative;"
										v-for="(image,index) in  infantImg" :key="index">
										<image :src="image" style="width: 100%;height: 100%;"></image>
										<view style="z-index: 9;position: absolute;top: -15rpx;right: -15rpx;"
											@click="infantImgremove(index)">
											<u-icon name="close-circle-fill" color="#2979ff" size="50rpx"></u-icon>
										</view>

									</view>
								</view>
							</view>
							<view class="margin-top" @click="addImages(3)" v-if="infantImg.length<6">
								<view style="width: 200rpx;height: 200rpx;background: #f4f5f6;"
									class="flex justify-center align-center">
									<view>
										<view class="text-center">
											<image src="../../static/images/my/add.png"
												style="width: 65rpx;height: 55rpx;"></image>
										</view>
										<view class="text-center">添加图片</view>
									</view>
								</view>
							</view>

						</view>
					</view>
					<view>
						<view class="text-lg margin-top-sm">育婴师资格证书</view>
						<view class="flex" style="overflow: hidden;flex-wrap: wrap;">
							<view v-if="infantImgs.length">
								<view class="margin-top flex margin-right-sm flex-wrap">
									<view class="flex"
										style="width: 200rpx;height: 200rpx;margin-right: 5rpx;position: relative;"
										v-for="(image,index) in infantImgs" :key="index">
										<image :src="image" style="width: 100%;height: 100%;"></image>
										<view style="z-index: 9;position: absolute;top: -15rpx;right: -15rpx;"
											@click="infantImgsremove(index)">
											<u-icon name="close-circle-fill" color="#2979ff" size="50rpx"></u-icon>
										</view>

									</view>
								</view>
							</view>
							<view class="margin-top" @click="addImages(4)" v-if="infantImgs.length<6">
								<view style="width: 200rpx;height: 200rpx;background: #f4f5f6;"
									class="flex justify-center align-center">
									<view>
										<view class="text-center">
											<image src="../../static/images/my/add.png"
												style="width: 65rpx;height: 55rpx;"></image>
										</view>
										<view class="text-center">添加图片</view>
									</view>
								</view>
							</view>

						</view>
					</view>
				</view>
				<view class="tabbar">
					<view>
						支付：<text style="color: #F1413D;">￥200</text>
					</view>
					<view v-if="renzheng==0||renzheng==6||renzheng==3">
						<u-button @click="submit" class="margin-top" :custom-style="customStyle" shape="circle"
							:hair-line="false">提交并支付
						</u-button>
					</view>
				</view>
			</view>

			<view class=" padding" v-if="title_color==2">
				<view class=" padding bg radius">
					<u-form :model="forms" ref="uForm" label-position="top" :label-style='lableStyle'>
						<u-form-item label="企业姓名" :border-bottom='false'>
							<u-input placeholder="请输入企业姓名" v-model="forms.name" />
						</u-form-item>
						<u-form-item label="企业地址" :border-bottom='false'>
							<u-input placeholder="请输入企业地址" v-model="forms.address" />
						</u-form-item>
						<u-form-item label="联系方式" :border-bottom='false'>
							<u-input placeholder="请输入电话号码" v-model="forms.phone" type="number" maxlength="11" />
						</u-form-item>
					</u-form>
				</view>
				<view class=" padding bg radius margin-top">
					<view>
						<view class="text-lg margin-top-sm">上传营业执照正面</view>
						<view class="flex" style="overflow: hidden;flex-direction: initial;">
							<view v-if="forms.front.length">
								<view class="margin-top flex margin-right-sm">
									<view class="flex"
										style="width: 200rpx;height: 200rpx;margin-right: 10rpx;position: relative;">
										<image :src="forms.front" style="width: 100%;height: 100%;"></image>
										<view style="z-index: 9;position: absolute;top: -15rpx;right: -15rpx;"
											@click="frontRemove(2)">
											<u-icon name="close-circle-fill" color="#2979ff" size="50rpx"></u-icon>
										</view>
									</view>
								</view>
							</view>
							<view class="margin-top" @click="addImage(0)" v-if="forms.front.length<=0">
								<view style="width: 200rpx;height: 200rpx;background: #f4f5f6;"
									class="flex justify-center align-center">
									<view>
										<view class="text-center">
											<image src="../../static/images/my/add.png"
												style="width: 65rpx;height: 55rpx;"></image>
										</view>
										<view class="text-center">添加图片</view>
									</view>
								</view>
							</view>

						</view>
					</view>
				</view>
			</view>


			<view class="margin-lr  bg radius " style="margin-bottom: 240rpx;">
				<view class="padding">
					<view style="color: #005DFF;">缴纳保证金</view>
					<view class="flex justify-between align-center" style="line-height: 60rpx;">
						<view style="color: #999999;" class="text-sm">满足一定条件即可申请返还保证金</view>
						<view style="color: #F1413D;">￥{{money}}</view>
					</view>
				</view>
			</view>
			<view class="tabbar">
				<view>
					支付：<text style="color: #F1413D;">￥{{money}}</text>
				</view>
				<view v-if="renzheng==0||renzheng==6||renzheng==3">
					<u-button @click="submit" class="margin-top" :custom-style="customStyle" shape="circle"
						:hair-line="false">提交并支付
					</u-button>
				</view>
			</view>
		</view>

		<!-- 性别选择 -->
		<u-action-sheet :list="actionSheetList" v-model="show" @click="actionSheetCallback"></u-action-sheet>
		<!-- 出生日期 -->
		<u-picker v-model="showData" :params="params" @confirm="bindData"></u-picker>
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
	import configdata from '../../common/config.js';

	export default {
		data() {
			return {
				title_color: 1,
				classify: 1,
				bgCol2: '#557EFD',
				form: {
					name: '',
					idNumber: '',
					front: '',
					back: '',
					remek: '',
					sex: '',
					sexs: 0,
					birthdate: '',
					birth: '',
					phone: '',
					//母婴护理师证
					infant: [],
					//育婴师资格证书
					infantImgs: [],
					
				},
				forms: {
					name: '',
					address: '',
					idNumber: '',
					front: '',
					back: ''
				},
				// disabled: false,
				lableStyle: {
					color: '#000000',
					fontSize: '36upx'
				},
				customStyle: {
					backgroundColor: '#005DFF',
					color: '#ffffff',
					border: 0
				},
				customStyle1: {
					backgroundColor: '#ccc',
					color: '#000000',
					border: 0
				},
				renzheng: '',

				show: false,
				actionSheetList: [{
					text: '男',

				}, {
					text: '女',
				}],
				showData: false,
				params: {
					year: true,
					month: true,
					day: true,
					hour: false,
					minute: false,
					second: false
				},
				//母婴护理师证
				infantImg: [],
				//育婴师资格证书
				infantImgs: [],
				money: 0,
				showpay: false,
				openLists: [],
				openWay: 1,
				sex: 0,
				closeable: true
			}
		},
		onLoad(e) {
			console.log(e)
			this.classify = parseInt(e.classify)
			// console.log(parseInt(e.classify))
			this.title_color = this.classify
			// console.log(this.title_color,this.classify)
			this.getUserInfo()

			this.renzheng = uni.getStorageSync("renzheng")
			// #ifdef APP
			this.openLists = [{
					image: '../../static/images/icon_weixin.png',
					text: '微信支付',
					id: 1
				}, {
					image: '../../static/images/zhifubao.png',
					text: '支付宝支付',
					id: 2
				}],
				this.openWay = 1;
			// #endif

			// #ifdef MP-WEIXIN
			this.openLists = [{
					image: '../../static/images/icon_weixin.png',
					text: '微信支付',
					id: 1
				}],
				this.openWay = 1;
			// #endif

			// #ifdef H5
			this.openLists = [{
				image: '../../static/images/zhifubao.png',
				text: '支付宝支付',
				id: 2
			}]
			this.openWay = 2;
			// #endif
		},
		onShow() {
			this.Getmoney()

		},
		methods: {
			//日期弹框
			birthData() {
				this.showData = true
			},
			//性别选择
			bindSex() {
				this.show = true
			},
			//支付方式
			selectWay(item) {
				this.openWay = item.id;
			},
			// 保证金
			Getmoney() {
				this.$Request.get("/app/common/type/239").then(res => {
					console.log(res)
					if (res.code == 0) {
						this.money = res.data.value
					}
				});
			},
			// 点击actionSheet回调 性别
			actionSheetCallback(index) {
				console.log(index)
				this.sex = index
				this.form.sex = this.actionSheetList[index].text;
				// this.from.sexs =  this.actionSheetList[index].label;
				console.log(this.sex)
			},
			//出生日期
			bindData(e) {
				console.log(e)
				this.form.birthdate = e.year + '年' + e.month + '月' + e.day + '日'
				this.form.birth = e.year + '-' + e.month + '-' + e.day

			},
			cut(e) {
				this.title_color = e
				console.log(this.title_color)
				if (this.title_color == 1) {
					this.classify = this.title_color
					console.log(this.title_color, this.classify)
				} else if (this.title_color == 2) {
					this.classify = this.title_color
					console.log(this.title_color, this.classify)
				}
				this.getUserInfo()

			},
			//身份证删除
			frontRemove(index) {
				if (index == 1) {
					this.form.front = ''
				} else if (index == 2) {
					this.forms.front = ''
				}
			},
			backRemove() {
				this.form.back = ''
			},
			infantImgremove(index) {
				this.infantImg.splice(index, 1)
			},
			infantImgsremove(index) {
				this.infantImgs.splice(index, 1)
			},
			submit() {
				this.form.infantImg = this.infantImg
				this.form.infantImg = this.form.infantImg.toString();

				this.form.infantImgs = this.infantImgs
				this.form.infantImgs = this.form.infantImgs.toString();

				if (this.classify == 1) {
					if (!this.form.name) {
						uni.showToast({
							title: '请输入真实姓名',
							icon: 'none',
							duration: 1000
						})
						return;
					}

					if (!this.form.idNumber) {
						uni.showToast({
							title: '请输入身份证号',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					let regX =
						/^[1-9]\d{5}(18|19|([23]\d))\d{2}((0[1-9])|(10|11|12))(([0-2][1-9])|10|20|30|31)\d{3}[0-9Xx]$/;
					if (!regX.test(this.form.idNumber)) {
						uni.showToast({
							title: '请输入正确的身份证号',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					if (!this.form.birthdate) {
						uni.showToast({
							title: '请输入出生日期',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					if (!this.form.sex) {
						uni.showToast({
							title: '请输入性别',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					if (!this.form.phone) {
						uni.showToast({
							title: '请输入联系方式',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					if (!this.form.front) {
						uni.showToast({
							title: '请上传身份证正面',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					if (!this.form.back) {
						uni.showToast({
							title: '请上传身份证反面',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					if (!this.form.infantImg) {
						uni.showToast({
							title: '请上传母婴护理师证',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					if (!this.form.infantImgs) {
						uni.showToast({
							title: '请上传育婴师资格证书',
							icon: 'none',
							duration: 1000
						})
						return;
					}

				}
				if (this.classify == 2) {
					if (!this.forms.name) {
						uni.showToast({
							title: '请输入企业姓名',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					if (!this.forms.address) {
						uni.showToast({
							title: '请输入企业地址',
							icon: 'none',
							duration: 1000
						})
						return;
					}
					if (!this.forms.phone) {
						uni.showToast({
							title: '请输入电话号码',
							icon: 'none',
							duration: 1000
						})
						return;
					}

					if (!this.forms.front) {
						uni.showToast({
							title: '请上传营业执照正面',
							icon: 'none',
							duration: 1000
						})
						return;
					}
				}

				this.showpay = true

			},

			getUserInfo() {
				console.log(this.classify, '222222222')
				this.$Request.get("/app/userCertification/queryInsert?classify=" + this.classify).then(res => {
					console.log(res)
					if (res.code == 0) {
						uni.setStorageSync('classify', res.data.classify)
						// this.classify = res.data.classify

						if (res.data.classify == 1) {
							this.form.name = res.data.name ? res.data.name : ''
							this.form.idNumber = res.data.idNumber ? res.data.idNumber : ''
							this.form.front = res.data.front ? res.data.front : ''
							this.form.back = res.data.back ? res.data.back : ''
							this.classify = res.data.classify

							this.form.birthdate = res.data.birthdate
							this.sex = res.data.sex
							if (res.data.sex == 0) {
								this.form.sex = '男'
							} else if (res.data.sex == 1) {
								this.form.sex = '女'
							}
							this.form.phone = res.data.phone
							this.infantImg = res.data.infantImg.split(',')
							this.form.infantImg = res.data.infantImg

							this.infantImgs = res.data.infantImgs.split(',')
							this.form.infantImgs = res.data.infantImgs;

						} else if (res.data.classify == 2) {
							this.classify = res.data.classify
							this.forms.name = res.data.name ? res.data.name : ''
							this.forms.address = res.data.address ? res.data.address : ''
							this.forms.phone = res.data.phone ? res.data.phone : ''
							this.forms.front = res.data.front ? res.data.front : ''
						}
						if (res.data.status == 1) {
							this.disabled = true
						}
						if (res.data.status == 2) {
							this.form.remek = res.data.remek ? res.data.remek : ''
						}
					}
				});
			},

			addImages(e) {
				uni.chooseImage({
					count: 6,
					sourceType: ['album', 'camera'],
					success: res => {
						for (let i = 0; i < res.tempFilePaths.length; i++) {
							this.$queue.showLoading("上传中...");
							uni.uploadFile({ // 上传接口
								url: this.config("APIHOST1") +
									'/alioss/upload', //真实的接口地址
								// url: 'https://jiazheng.xianmxkj.com/sqx_fast/alioss/upload',
								filePath: res.tempFilePaths[i],
								name: 'file',
								success: (uploadFileRes) => {
									if (e == 1) {
										this.form.front = JSON.parse(uploadFileRes.data).data
										// that.form.front.push(JSON.parse(uploadFileRes.data).data)
									} else if (e == 2) {
										this.form.back = JSON.parse(uploadFileRes.data).data
									} else if (e == 3) {
										if (this.infantImg.length <= 5) {
											this.infantImg.push(JSON.parse(uploadFileRes.data)
												.data)
										}
									} else if (e == 4) {
										if (this.infantImgs.length <= 5) {
											this.infantImgs.push(JSON.parse(uploadFileRes.data)
												.data)
										}
									} else if (e == 0) {
										this.forms.front = JSON.parse(uploadFileRes.data).data
									}

									uni.hideLoading();
								}
							});
						}
					}
				})
			},
			addImage(e) {
				uni.chooseImage({
					count: 1,
					sourceType: ['album', 'camera'],
					success: res => {
						for (let i = 0; i < res.tempFilePaths.length; i++) {
							this.$queue.showLoading("上传中...");
							uni.uploadFile({ // 上传接口
								url: this.config("APIHOST1") + '/alioss/upload', //真实的接口地址
								// url: 'https://yuesao.xianmxkj.com/sqx_fast/alioss/upload',
								filePath: res.tempFilePaths[i],
								name: 'file',
								success: (uploadFileRes) => {
									if (e == 1) {
										this.form.front = JSON.parse(uploadFileRes.data).data
										// that.form.front.push(JSON.parse(uploadFileRes.data).data)
									} else if (e == 2) {
										this.form.back = JSON.parse(uploadFileRes.data).data
									} else if (e == 0) {
										this.forms.front = JSON.parse(uploadFileRes.data).data
									}

									uni.hideLoading();
								}
							});
						}
					}
				})
			},
			config: function(name) {
				var info = null;
				if (name) {
					var name2 = name.split("."); //字符分割
					if (name2.length > 1) {
						info = configdata[name2[0]][name2[1]] || null;
					} else {
						info = configdata[name] || null;
					}
					if (info == null) {
						let web_config = cache.get("web_config");
						if (web_config) {
							if (name2.length > 1) {
								info = web_config[name2[0]][name2[1]] || null;
							} else {
								info = web_config[name] || null;
							}
						}
					}
				}
				return info;
			},

			pay() {
				let data
				if (this.classify == 1) {
					let userId = uni.getStorageSync('userId')
					data = {
						name: this.form.name,
						idNumber: this.form.idNumber,
						birthdate: this.form.birth,
						sex: this.sex,
						phone: this.form.phone,
						front: this.form.front,
						back: this.form.back,
						remek: this.form.remek,
						infantImg: this.form.infantImg,
						infantImgs: this.form.infantImgs,
						classify: this.classify,
						userId: userId
					}
				}

				if (this.classify == 2) {
					let userId = uni.getStorageSync('userId')
					data = {
						name: this.forms.name,
						address: this.forms.address,
						phone: this.forms.phone,
						classify: this.classify,
						front: this.forms.front,
						userId: userId,
					}
				}
				console.log(data, 'pppp1')
				this.showpay = false
				if (this.openWay == 1) { //微信支付
					// #ifdef MP-WEIXIN
					this.$Request.postJson('/app/wxPay/wxPayJsApiUserCertification', data).then(res => {
						console.log(res)

						if (res.code == 0) {
							// this.isCheckPay(res.code, 'wxpay', JSON.stringify(res.data));

							uni.requestPayment({
								provider: 'wxpay',
								timeStamp: res.data.timestamp,
								nonceStr: res.data.noncestr,
								package: res.data.package,
								signType: res.data.signType,
								paySign: res.data.sign,
								success: function(res) {
									console.log(res)
									// uni.showLoading({
									// 	title: '支付成功',
									// 	icon:'nones'
									// });
									// this.$queue.showToast('支付成功');

									uni.switchTab({
										url: '/pages/my/index'
									})
								},
								fail: function(err) {

									this.$queue.showToast('支付失败');
								}
							});
						}
					});
					// #endif


					// #ifdef H5
					this.$Request.postJson('/app/wxPay/wxPayMpUserCertification', data).then(res => {
						this.showpay = false
						that.callPay(res);
					});
					// #endif

					// #ifdef APP
					this.$Request.postJson('/app/wxPay/payAppUserCertification', data).then(res => {
						console.log(res)
						this.showpay = false
						if (res.code == 0) {

							this.isCheckPay(res.code, 'wxpay', JSON.stringify(res.data));
						}
					});
					// #endif


				} else if (this.openWay == 2) { //支付宝支付
					// #ifdef H5
					this.$Request.postJson('/app/aliPay/payOrderUserCertification', data).then(
						res => {
							this.showpay = false
							const div = document.createElement('div')
							div.innerHTML = res.data //此处form就是后台返回接收到的数据
							document.body.appendChild(div)
							document.forms[0].submit()
						});
					// #endif

					// #ifdef APP-PLUS
					this.$Request.postJson('/app/aliPay/payAppUserCertification', data).then(

						res => {
							this.showpay = false
							this.setPayment('alipay', res.data);
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
							uni.showLoading({
								title: '支付成功'
							});
							uni.hideLoading();

							uni.navigateTo({
								url: '/pages/my/index'
							})
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
						uni.showLoading({
							title: '支付成功'
						});
						uni.switchTab({
							url: '/pages/my/index'
						})
					},
					fail: function(err) {
						uni.hideLoading();
					},
					complete() {
						uni.hideLoading();
					}
				});
			}
		}
	}
</script>

<style>
	page {
		background-color: #F7F7F7;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.u-input__input {
		color: balck !important;
	}

	.title_btn {
		height: 78upx;
		line-height: 78upx;
		/* background: #f7f7f7; */
	}

	.tabbar {
		width: 100%;
		height: 130rpx;
		background-color: #FFFFFF;
		padding: 20rpx 30rpx;
		position: fixed;
		bottom: 0rpx;
		left: 0rpx;
		right: 0rpx;
		display: flex;
		justify-content: space-between;
		align-items: center;
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

	.hb {
		/* #ifdef H5 */
		padding-bottom: 180rpx;
		/* #endif */
	}
	.bgCol2 {
		color: #557EFD;
	}
</style>
