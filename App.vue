<script>
	export default {
		onLaunch: function() {
			// setInterval(d => { //定时器，定时去调取聊天未读消息
			// 	let userId = uni.getStorageSync('userId')
			// 	if (userId) {
			// 		this.$Request.get('/app/chat/selectChatCount').then(res => {
			// 			if (res.code === 0) {
			// 				let chatCount = res.data.chatCount
			// 				let messageCount = res.data.messageCount

			// 				uni.setStorageSync('messageCount', messageCount)

			// 				let num = chatCount + messageCount
			// 				if (num == 0) {
			// 					uni.removeTabBarBadge({
			// 						index: 1
			// 					})
			// 					return;
			// 				}
			// 				uni.setTabBarBadge({
			// 					index: 1,
			// 					text: num + ""
			// 				})

			// 			}
			// 		});
			// 	}
			// }, 3000);

			//#ifdef APP-PLUS
			// APP检测更新 具体打包流程可以参考：https://ask.dcloud.net.cn/article/35667
			plus.screen.lockOrientation('portrait-primary'); //竖屏正方向锁定
			//获取是否热更新过
			const updated = uni.getStorageSync('updated'); // 尝试读取storage

			if (updated.completed === true) {
				// 如果上次刚更新过
				// 删除安装包及安装记录
				console.log('安装记录被删除，更新成功');
				uni.removeSavedFile({
					filePath: updated.packgePath,
					success: res => {
						uni.removeStorageSync('updated');
					}
				});
			} else if (updated.completed === false) {
				uni.removeStorageSync('updated');
				plus.runtime.install(updated.packgePath, {
					force: true
				});
				uni.setStorage({
					key: 'updated',
					data: {
						completed: true,
						packgePath: updated.packgePath
					},
					success: res => {
						console.log('成功安装上次的更新，应用需要重启才能继续完成');
					}
				});
				uni.showModal({
					title: '温馨提示',
					content: '应用将重启以完成更新',
					showCancel: false,
					complete: () => {
						plus.runtime.restart();
					}
				});
			} else {
				//获取当前系统版本信息
				plus.runtime.getProperty(plus.runtime.appid, widgetInfo => {
					//请求后台接口 解析数据 对比版本
					let data = {
						classify: 1
					}
					this.$Request.getT('/app/user/selectNewApp', data).then(res => {
						res = res.data[0];
						if (res.wgtUrl && widgetInfo.version < res.version) {
							let downloadLink = '';
							let androidLink = res.androidWgtUrl;
							let iosLink = res.iosWgtUrl;
							let ready = false;
							let that = this;
							//校验是是不是热更新
							if (res.wgtUrl.match(RegExp(/.wgt/))) {
								// 判断系统类型
								if (plus.os.name.toLowerCase() === 'android') {
									console.log('安卓系统');
									if (androidLink && androidLink !== '#') {
										// 我这里默认#也是没有地址，请根据业务自行修改
										console.log('发现下载地址');
										// 安卓：创建下载任务
										if (androidLink.match(RegExp(/.wgt/))) {
											console.log('确认wgt热更新包');
											downloadLink = androidLink;
											ready = true;
										} else {
											console.log('安卓推荐.wgt强制更新，.apk的强制更新请您自行修改程序');
										}
									} else {
										console.log('下载地址是空的，无法继续');
									}
								} else {
									console.log('苹果系统');
									if (iosLink && iosLink !== '#') {
										// 我这里默认#也是没有地址，请根据业务自行修改
										console.log('发现下载地址');
										// 苹果(A)：进行热更新（如果iosLink是wgt更新包的下载地址）判断文件名中是否含有.wgt
										if (iosLink.match(RegExp(/.wgt/))) {
											console.log('确认wgt热更新包');
											downloadLink = iosLink;
											ready = true;
										} else {
											console.log('苹果只支持.wgt强制更新');
										}
									} else {
										console.log('下载地址是空的，无法继续');
									}
								}
								if (ready) {
									console.log('任务开始');
									let downloadTask = uni.downloadFile({
										url: downloadLink,
										success: res => {
											if (res.statusCode === 200) {
												// 保存下载的安装包
												console.log('保存安装包');
												uni.saveFile({
													tempFilePath: res.tempFilePath,
													success: res => {
														const packgePath = res
															.savedFilePath;
														// 保存更新记录到stroage，下次启动app时安装更新
														uni.setStorage({
															key: 'updated',
															data: {
																completed: false,
																packgePath: packgePath
															},
															success: () => {
																console
																	.log(
																		'成功保存记录'
																	);
															}
														});
														// 任务完成，关闭下载任务
														console.log(
															'任务完成，关闭下载任务，下一次启动应用时将安装更新'
														);
														downloadTask.abort();
														downloadTask = null;
													}
												});
											}
										}
									});
								} else {
									console.log('下载地址未准备，无法开启下载任务');
								}
							} else {
								//不是热更新是在线更新 校验是否强制升级
								if (res.method == 'true') {
									uni.showModal({
										showCancel: false,
										confirmText: '立即更新',
										title: '发现新版本',
										content: res.des,
										success: res => {
											if (res.confirm) {
												that.$queue.showLoading('下载中...');
												if (uni.getSystemInfoSync().platform ==
													'android') {
													uni.downloadFile({
														url: androidLink,
														success: downloadResult => {
															if (downloadResult
																.statusCode ===
																200) {
																plus.runtime
																	.install(
																		downloadResult
																		.tempFilePath, {
																			force: false
																		},
																		d => {
																			console
																				.log(
																					'install success...'
																				);
																			plus.runtime
																				.restart();
																		},
																		e => {
																			console
																				.error(
																					'install fail...'
																				);
																		}
																	);
															}
														}
													});
												}
												if (uni.getSystemInfoSync().platform ==
													'ios') {
													plus.runtime.openURL(iosLink, function(
														res) {});
												}
											} else if (res.cancel) {
												console.log('取消');
											}
										}
									});
								} else {
									uni.showModal({
										title: '发现新版本',
										confirmText: '立即更新',
										cancelText: '下次更新',
										content: res.des,
										success: res => {
											if (res.confirm) {
												that.$queue.showLoading('下载中...');
												if (uni.getSystemInfoSync().platform ==
													'android') {
													uni.downloadFile({
														url: androidLink,
														success: downloadResult => {
															if (downloadResult
																.statusCode ===
																200) {
																plus.runtime
																	.install(
																		downloadResult
																		.tempFilePath, {
																			force: false
																		},
																		d => {
																			console
																				.log(
																					'install success...'
																				);
																			plus.runtime
																				.restart();
																		},
																		e => {
																			console
																				.error(
																					'install fail...'
																				);
																		}
																	);
															}
														}
													});
												}
												if (uni.getSystemInfoSync().platform ==
													'ios') {
													plus.runtime.openURL(iosLink, function(
														res) {});
												}
											} else if (res.cancel) {
												console.log('取消');
											}
										}
									});
								}
							}
						}
					});
				});
			}

			//#endif
		},
		onShow: function() {
			this.$Request.get('/app/common/type/252').then(res => {
				if (res.code == 0) {
					// #ifdef MP-WEIXIN
					this.$queue.setData('XCXIsSelect', res.data.value);
					// #endif
					// #ifndef MP-WEIXIN
					this.$queue.setData('XCXIsSelect', '是');
					// #endif
				}
			});
			// this.$Request.get('/app/common/type/297').then(res => {
			// 	if (res.code == 0) {
			//  		// #ifdef MP-WEIXIN
			// 		this.$queue.setData('XCXIsSelect', res.data.value);
			// 		// #endif
			// 		// #ifndef MP-WEIXIN
			// 		this.$queue.setData('XCXIsSelect', '是');
			// 		// #endif
			// 	}
			// });
			// 用户端联系方式 1 手机号 2企业微信 301
			this.$Request.get('/app/common/type/301').then(res => {
				if (res.code == 0) {
					this.$queue.setData('kefu', res.data.value);
				}
			});
			// 用户端客服手机号 302
			this.$Request.get('/app/common/type/302').then(res => {
				if (res.code == 0) {
					this.$queue.setData('kefuPhone', res.data.value);
				}
			});
			// 用户端企业微信AppId 303
			this.$Request.get('/app/common/type/303').then(res => {
				if (res.code == 0) {
					this.$queue.setData('kefuAppId', res.data.value);
				}
			});
			//用户端企业微信链接 304
			this.$Request.get('/app/common/type/304').then(res => {
				if (res.code == 0) {
					this.$queue.setData('kefuUrl', res.data.value);
				}
			});
			//是否开启会员 321
			this.$Request.get('/app/common/type/321').then(res => {
				if (res.code == 0) {
					this.$queue.setData('hyCheck', res.data.value);
				}
			});
			//是否开启推广 322
			this.$Request.get('/app/common/type/322').then(res => {
				if (res.code == 0) {
					this.$queue.setData('tgCheck', res.data.value);
				}
			});
			//超出公里数 324
			this.$Request.get('/app/common/type/324').then(res => {
				if (res.code == 0) {
					this.$queue.setData('glsNum', res.data.value);
				}
			});
			//用户是否可以取消订单 377
			this.$Request.get('/app/common/type/377').then(res => {
				if (res.code == 0) {
					this.$queue.setData('yhqxSel', res.data.value);
				}
			});
			// 提现底部提示	326
			this.$Request.get('/app/common/type/326').then(res => {
				if (res.code == 0) {
					this.$queue.setData('txMsg', res.data.value);
				}
			});
			//是否开启阿里云隐私电话 383
 			this.$Request.get('/app/common/type/383').then(res => {
 				if (res.code == 0) {
 					this.$queue.setData('aliphone', res.data.value);
 				}
 			});
			// 是否开启微信提现	420
 			this.$Request.get('/app/common/type/420').then(res => {
 				if (res.code == 0) {
 					this.$queue.setData('wxTxSel', res.data.value);
 				}
 			});
 			// 是否开启银联提现	422
 			this.$Request.get('/app/common/type/422').then(res => {
 				if (res.code == 0) {
 					this.$queue.setData('ylTxSel', res.data.value);
 				}
 			});
			// 是否开启为技师充值	421
			this.$Request.get('/app/common/type/421').then(res => {
				if (res.code == 0) {
					this.$queue.setData('jsczSel', res.data.value);
				}
			});
			// 忙时时间切换 1半个小时 2一个小时 395
			this.$Request.get('/app/common/type/395').then(res => {
				if (res.code == 0) {
					this.$queue.setData('msTimeDate', res.data.value);
				}
			});
			
			// 取消订单扣除百分比	423
 			this.$Request.get('/app/common/type/423').then(res => {
 				if (res.code == 0) {
 					this.$queue.setData('closeKC', res.data.value);
 				}
 			});
 			// 技师详情顶部图高度	425
 			this.$Request.get('/app/common/type/425').then(res => {
 				if (res.code == 0) {
 					this.$queue.setData('imageHeight', res.data.value);
 				}
 			});
			// 是否开启用户端删除订单	427
 			this.$Request.get('/app/common/type/427').then(res => {
 				if (res.code == 0) {
 					this.$queue.setData('userDeletaSel', res.data.value);
 				}
 			});
			let userId = this.$queue.getData("userId")

			if (userId) {
				this.$Request.getT('/app/user/selectUserById').then(res => {

					if (res.code == 0) {

						if (res.data && res.data.status == 2) {

							uni.showModal({
								title: '账号已被封禁',
								content: '请在【我的客服】申诉，申诉成功后重新登录',
								showCancel: false,
								cancelText: '',
								confirmText: '我知道了',
								success: res => {
									uni.clearStorageSync()
								},
								fail: () => {},
								complete: () => {}
							});
						}

					}
				})
			}

			console.log('App Show')

			// #ifdef APP-PLUS
			let clientid = plus.push.getClientInfo().clientid;
			let sysPhone = 1;
			if (plus.os.name.toLowerCase() === 'android') {
				sysPhone = 1;
			} else {
				sysPhone = 2;
			}
			let userIds = this.$queue.getData('userId');
			if (userIds) {
				this.$Request.postT('/app/user/updateClientId?clientId=' + clientid + '&userId=' + userIds + '&sysPhone=' + sysPhone).then(
					res => {});
			}
			//#endif
			// #ifdef H5
			let ua = navigator.userAgent.toLowerCase();
			console.log(ua.indexOf('micromessenger'))
			if (ua.indexOf('micromessenger') === -1) {
				
				this.$Request.get('/app/common/type/318').then(res => {
					if (res.code == 0 && res.data && res.data.value === '否') {
						if (window.location.href.indexOf('/pages/my/erweimaRegister') === -1) {
							uni.reLaunch({
								url: '/pages/my/erweimaRegister'
							})
						}
					}
				});
			}
			// #endif
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
	@import "uview-ui/index.scss";
	@import 'components/colorui/main.css';
	@import 'components/colorui/icon.css';

	page {
		background-color: #FFFFFF;
		color: #343546;
	}

	.bg {
		background-color: #F7F7F7;
	}
</style>
