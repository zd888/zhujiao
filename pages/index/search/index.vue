<template>
	<view class="content">
		<view class="search-box flex align-center">
			<!-- mSearch组件 如果使用原样式，删除组件元素-->
			<!-- <mSearch class="mSearch-input-box" :mode="2" button="inside" :placeholder="defaultKeyword"
				@search="doSearch(false)" @input="inputChange" @confirm="doSearch(false)" v-model="keyword"></mSearch> -->
			<!-- 原样式 如果使用原样式，恢复下方注销代码 -->

			<!-- <view class="input-box">
				<input type="text" :adjust-position="true" :placeholder="defaultKeyword" @input="inputChange" v-model="keyword" @confirm="doSearch(false)"
				 placeholder-class="placeholder-class" confirm-type="search">
			</view>
			<view class="search-btn" @tap="doSearch(false)">搜索</view> -->
			<!-- <view style="width: 6%;" @click="goBack()">
				<u-icon name="arrow-left" color="#909399" size="28"></u-icon>
			</view> -->
			<u-search style="width: 100%;" placeholder="搜索需要的助教/教练" :focus="true" v-model="keyword" :show-action="true"
				:animation="true" shape="square" bg-color="#F7F7F7" color="#1A1A1A" action-text="取消" @custom="goBack()"
				@search="doSearch(false)"></u-search>
			<!-- 原样式 end -->
		</view>

		<view v-show="isShowKeywordList">
			<!-- <ren-dropdown-filter :filterData='filterData' :border="false" :defaultIndex='defaultIndex'
				@onSelected='change' class="u-skeleton-rect" v-if="type==1">
			</ren-dropdown-filter>
			<ren-dropdown-filter :filterData='filterDatas' :border="false" :defaultIndex='defaultIndexs'
				@onSelected='changes' class="u-skeleton-rect" v-if="type==2">
			</ren-dropdown-filter> -->
			<view class="bg flex align-center justify-between padding-lr padding-tb-sm">
				<view v-for="(item,index) in list" :key="index" @click="change(index)"
					:style="listIndex==index?'color:#20C675;font-size:30rpx;':''">
					{{item.name}}
				</view>
			</view>
		</view>

		<view class="search-keyword">

			<scroll-view class="keyword-list-box" v-show="isShowKeywordList" scroll-y>
				<view class="flex align-center bg padding-sm  margin-lr-sm padding-bottom margin-top-sm" style="border-radius:24rpx;"
					v-for="(item,index) in keywordList" :key='index' @click="goOrderdetail(item)">
					<view style="width: 130upx;height: 130upx;">
						<image :src="item.artificerImg?item.artificerImg: '../../../static/logo.png'"
							style="width: 130upx;height: 130upx;border-radius: 50%;" mode="aspectFill"></image>
					</view>
					<view class="margin-left text-black flex flex-direction" style="width: 80%;">
						<view class="flex ">
							<image src="../../../static/images/geren.png" style="width: 35rpx;height: 35rpx;">
							</image>
							<view class="text-bold text-df margin-left-xs">{{item.artificerName}}</view>
						</view>

						<view class="flex justify-between margin-tb-sm">
							<view class="flex align-center">
								<view v-if="item.ordersScore" class="margin-right-sm">
									<image src="../../../static/images/start.png" style="width: 24upx;height: 22upx;">
									</image>
									<text class="margin-left-xs" style="color: #FF1200;">{{item.ordersScore}}.0</text>
								</view>
								<view style="color: #999999;">已服务:{{item.ordersCount}}单</view>
							</view>

							<view class="flex align-center">
								<image src="../../../static/images/address.png" style="width: 24rpx;height: 29rpx;">
								</image>
								<view style="color: #999999;margin-left: 8rpx;">{{item.distance}}</view>
							</view>
						</view>
						<view style="width: 100%;display: flex;justify-content: space-between;align-items: center;">
							<view class="flex flex-wrap" style="margin-top: 10rpx;">
								<text class="margin-bottom-xs box">助教陪练</text>
							</view>
							<view class="text-center text-sm"
								style="background: linear-gradient(90deg, #55B55C 0%, #308251 100%);color: #ffffff;width: 135upx;border-radius: 16upx;line-height: 50upx;"
								@click.stop="goOrderdetail(item)" v-if="XCXIsSelect !='否'">
								立即预约
							</view>
						</view>
					</view>
				</view>
				<empty v-if="keywordList.length == 0"></empty>
			</scroll-view>

			<scroll-view class="keyword-box" v-show="!isShowKeywordList" scroll-y>
				<view class="keyword-block">
					<view class="keyword-list-header">
						<view>热门搜索</view>
						<!-- <view>
											<image @tap="hotToggle" :src="'/static/images/index/attention'+forbid+'.png'">
											</image>
										</view> -->
					</view>
					<view class="keyword" v-if="forbid==''">
						<view v-for="(keyword,index) in hotKeywordList" @tap="doSearch(keyword)" :key="index">
							{{keyword}}
						</view>
					</view>
					<view class="hide-hot-tis" v-else>
						<view>当前搜热已隐藏</view>
					</view>
				</view>
				<view class="keyword-block" v-if="oldKeywordList.length>0">
					<view class="keyword-list-header">
						<view>历史记录</view>
						<view>
							<image @tap="oldDelete" src="/static/images/index/delete.png"></image>
						</view>
					</view>
					<view class="keyword">
						<view v-for="(keyword,index) in oldKeywordList" @tap="doSearch(keyword)" :key="index">
							{{keyword}}
						</view>
					</view>
				</view>
			</scroll-view>
		</view>
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
				defaultKeyword: "",
				keyword: "",
				oldKeywordList: [], //历史记录
				hotKeywordList: [], //热搜
				keywordList: [], //搜索列表
				forbid: '',
				isShowKeywordList: false,
				limit: 10,
				page: 1,
				userId: '',
				isVip: false,
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
							label: '销量优先',
							value: 2,
						},
					],
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
				defaultIndexs: [0, 0, 0],
				filterDatas: [
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
							label: '服务人数',
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
				value1: 0,
				value2: 0,
				value3: 0,
				city: '',
				type: '',
				longitude: '',
				latitude: '',
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
			}
		},
		onLoad(option) {
			console.log(option)
			this.type = option.index
			this.userId = uni.getStorageSync('userId')
			this.city = uni.getStorageSync('city')
			if (this.userId) {
				this.getSearchList()
			}
			this.isVip = uni.getStorageSync('isVIP')
			let that = this
			uni.getLocation({
				type: 'gcj02',
				geocode: true, //设置该参数为true可直接获取经纬度及城市信息
				success: function(res) {
					console.log(res, '地理位置')
					that.latitude = res.latitude
					that.longitude = res.longitude

				},
				fail: function() {
					console.log('获取地址失败')
				}
			})
		},
		methods: {

			// 获取搜索历史
			getSearchList() {
				let data = {
					userId: this.userId
				}
				this.$Request.get("/app/Search/selectAppSearchNum", data).then(res => {
					console.log(res)
					if (res.code == 0) {
						this.oldKeywordList = res.data.userSearchName
						// for (let i = 0; i < this.oldKeywordList.length; i++) {
						// 	this.oldKeywordList[i].gameName = this.oldKeywordList[i].gameName.split(",");
						// }
						this.hotKeywordList = res.data.allSerchName


					}
				});
			},
			//清除历史搜索
			oldDelete() {
				uni.showModal({
					content: '确定清除历史搜索记录？',
					success: (res) => {
						if (res.confirm) {
							console.log('用户点击确定');
							this.$Request.get("/app/Search/deleteAppSearch").then(res => {
								if (res.code == 0) {
									this.getSearchList()
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			// 筛选
			change(index) {
				this.listIndex = index
				// this.value1 = e[0][0].value
				// this.value3 = e[1][0].value
				// this.value2 = e[2][0].value
				this.page = 1
				this.doSearch(false)
			},
			// 筛选
			changes(e) {

				this.value1 = e[0][0].value
				this.value2 = e[1][0].value
				this.value3 = e[2][0].value

				this.mescroll.resetUpScroll()
			},
			//执行搜索
			doSearch(keyword) {
				this.keyword = keyword === false ? this.keyword : keyword;
				this.isShowKeywordList = true;
				if (!this.keyword) {
					uni.showToast({
						title: '请输入内容',
						icon: 'none',
						duration: 1000
					})
					return
				}
				console.log(this.type)

				if (this.type == 1) {
					let data = {
						title: this.keyword,
						limit: this.limit,
						page: this.page,
						userId: this.userId,
						condition: this.value1, //智能优选
						by: this.value2, //销量
						authentication: this.value3, //价格
						city: this.city,

					}
					this.$Request.get("/app/artificer/selectMassageTypePage", data).then(res => {
						if (res.code == 0) {
							if (this.page == 1) this.keywordList = [];
							this.keywordList = [...this.keywordList, ...res.data.list]
							for (let i = 0; i < this.keywordList.length; i++) {
								this.keywordList[i].labels = this.keywordList[i].labels.split(",");
							}
						}
					});
				} else if (this.type == 2) {
					let data = {
						// title: this.keyword,
						limit: this.limit,
						page: this.page,
						userId: this.userId,
						condition: this.value1, //智能优选
						by: this.value2, //销量
						authentication: this.value3, //价格
						latitude: this.latitude,
						longitude: this.longitude,
						city: this.city,
						artificerName: this.keyword,
					}
					this.$Request.get("/app/artificer/selectArtificerList", data).then(res => {
						if (res.code == 0) {
							for (var a = 0; a < res.data.list.length; a++) {
								if (res.data.list[a].distance > 1000) {
									res.data.list[a].distance = (res.data.list[a].distance / 1000).toFixed(1) +
										'km'
									console.log(res.data.list[a].distance)
								} else {
									res.data.list[a].distance = (res.data.list[a].distance).toFixed(1) + 'm'
								}
							}
							if (this.page == 1) this.keywordList = [];
							this.keywordList = [...this.keywordList, ...res.data.list]


							// for (let i = 0; i < this.keywordList.length; i++) {
							// 	this.keywordList[i].labels = this.keywordList[i].labels.split(",");

							// }
						}
					});
				}

			},
			// 点击取消返回首页
			goBack() {
				uni.navigateBack()
			},
			//热门搜索开关
			hotToggle() {
				this.forbid = this.forbid ? '' : '_forbid';
			},
			// 跳转订单
			goOrder(e) {
				if (this.userId) {
					uni.navigateTo({
						url: '/pages/index/order/order?massageTypeId=' + e.massageTypeId
					});
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					});
				}
			},
			// 跳转订单
			goOrderdetail(e) {
				console.log(e)
				if (this.userId) {
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
			this.doSearch(false);
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.doSearch(false);
		},
	}
</script>
<style>
	page {
		background-color: #FFFFFF;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.search-box {
		width: 100%;
		/* background-color: rgb(242, 242, 242); */
		padding: 15upx 2.5%;
		display: flex;
		justify-content: space-between;
		position: sticky;
		background-color: #FFFFFF;
	}

	.search-box .mSearch-input-box {
		width: 100%;
	}

	.search-box .input-box {
		width: 85%;
		flex-shrink: 1;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.search-box .search-btn {
		width: 15%;
		margin: 0 0 0 2%;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-shrink: 0;
		font-size: 28upx;
		color: #fff;
		background: linear-gradient(to right, #ff9801, #ff570a);
		border-radius: 60upx;
	}

	.search-box .input-box>input {
		width: 100%;
		height: 60upx;
		font-size: 32upx;
		border: 0;
		border-radius: 60upx;
		-webkit-appearance: none;
		-moz-appearance: none;
		appearance: none;
		padding: 0 3%;
		margin: 0;
		background-color: #ffffff;
	}

	.placeholder-class {
		color: #9e9e9e;
	}

	.search-keyword {
		width: 100%;
		/* background-color: #FFFFFF; */
	}

	.keyword-list-box {
		height: calc(100vh - 110upx);
		/* padding-top: 10upx; */
		background-color: #F5F5F5;
		/* border-radius: 20upx 20upx 0 0; */
		/* background-color: #fff; */
	}

	.keyword-entry-tap {
		background-color: #eee;
	}

	.keyword-entry {
		width: 94%;
		height: 80upx;
		margin: 0 3%;
		font-size: 30upx;
		color: #333;
		display: flex;
		justify-content: space-between;
		align-items: center;
		border-bottom: solid 1upx #e7e7e7;
	}

	.keyword-entry image {
		width: 60upx;
		height: 60upx;
	}

	.keyword-entry .keyword-text,
	.keyword-entry .keyword-img {
		height: 80upx;
		display: flex;
		align-items: center;
	}

	.keyword-entry .keyword-text {
		width: 90%;
	}

	.keyword-entry .keyword-img {
		width: 10%;
		justify-content: center;
	}

	.keyword-box {
		/* height: calc(100vh - 110upx); */
		height: fit-content;
		/* border-radius: 20upx 20upx 0 0; */
		background-color: #FFFFFF;
	}

	.keyword-box .keyword-block {
		padding: 10upx 0;
	}

	.keyword-box .keyword-block .keyword-list-header {
		width: 94%;
		padding: 10upx 3%;
		font-size: 27upx;
		font-weight: 700;
		/* color: #FFFFFF; */
		display: flex;
		justify-content: space-between;
	}

	.keyword-box .keyword-block .keyword-list-header image {
		width: 40upx;
		height: 40upx;
	}

	.keyword-box .keyword-block .keyword {
		width: 94%;
		padding: 3px 3%;
		display: flex;
		flex-flow: wrap;
		justify-content: flex-start;
	}

	.keyword-box .keyword-block .hide-hot-tis {
		display: flex;
		justify-content: center;
		font-size: 28upx;
		color: #FFFFFF;
	}

	.keyword-box .keyword-block .keyword>view {
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 10upx;
		padding: 0 20upx;
		margin: 10upx 20upx 10upx 0;
		height: 60upx;
		font-size: 28upx;
		background-color: #F7F7F7;
		color: #343546;
		/* box-shadow: 0 0 10rpx #CCCCCC; */
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
