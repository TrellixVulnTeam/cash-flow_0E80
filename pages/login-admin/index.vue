<template>
	<view class="template-login1">
		<!-- 顶部自定义导航 -->
		<!-- <tn-nav-bar fixed alpha customBack>
			<view slot="back" class="tn-custom-nav-bar__back" @click="goBack"> <text class="icon tn-icon-left"></text> <text class="icon tn-icon-home-capsule-fill"></text> </view>
		</tn-nav-bar> -->

		<canvas canvas-id="star_canvas" class="mycanvas" :style="'width:' + screenWidth + 'px;height:' + screenHeight + 'px;'"></canvas>

		<view class="login">
			<!-- 顶部背景图片-->
			<!-- <view class="login__bg login__bg--top">
        <image class="bg" src="https://tnuiimage.tnkjapp.com/login/1/login_top2.jpg" mode="widthFix"></image>
      </view>
      <view class="login__bg login__bg--top">
        <image class="rocket rocket-sussuspension" src="https://tnuiimage.tnkjapp.com/login/1/login_top3.png" mode="widthFix"></image>
      </view> -->

			<view class="login__wrapper">
				<view class="login-tag"> <tn-tag background-color="tn-cool-bg-color-3" font-color="tn-color-white" margin="1vw 45vw" width="10vw" shape="radius">管理员</tn-tag> </view>
				<!-- 登录/注册切换 -->
				<view class="login__mode tn-flex tn-flex-direction-row tn-flex-nowrap tn-flex-col-center tn-flex-row-center">
					<view class="login__mode__item tn-flex-1" :class="[{ 'login__mode__item--active': currentModeIndex === 0 }]" @tap.stop="modeSwitch(0)"> 创建房间 </view>
					<view class="login__mode__item tn-flex-1" :class="[{ 'login__mode__item--active': currentModeIndex === 1 }]" @tap.stop="modeSwitch(1)"> 重进房间 </view>

					<!-- 滑块-->
					<view class="login__mode__slider tn-cool-bg-color-15--reverse" :style="[modeSliderStyle]"></view>
				</view>

				<!-- 输入框内容-->
				<view class="login__info tn-flex tn-flex-direction-column tn-flex-col-center tn-flex-row-center">
					<view class="tn-width-full tn-flex tn-flex-row-between tn-flex-col-center tn-text-bold">
						<view>剩余可玩次数：{{ count }}</view>
						<view>
							<tn-button :plain="false" background-color="#01BEFF" margin="10rpx 10rpx" @click="logout">退出登录</tn-button>
						</view>
					</view>
					<!-- 创建房间 -->
					<block v-if="currentModeIndex === 0">
						<view class="login__info__item__input tn-flex tn-flex-direction-row tn-flex-nowrap tn-flex-col-center tn-flex-row-left">

							<view class="tn-flex-1 login__info__item__input__left-icon"> <view class="tn-icon-home-capsule"></view> </view>

							<view class="tn-flex-5 tn-flex login__info__item__input__content">
								<view class="tn-text-lg tn-color-white tn-text-bold" style="padding-right: 5px;;letter-spacing: 2px;">{{ today }}</view> <input v-model="new_num" type="number" maxlength="4" placeholder-class="input-placeholder" placeholder="请输入房间号码后四位" /> </view>

							<view class="tn-flex-4 login__info__item__input__right-button">
								<view class="tn-margin-right-sm" style="float: right;">
									<tn-button background-color="#01BEFF" font-color="#FFFFFF" @click="getIntRandomValue()">获取随机号</tn-button>
								</view>
							</view>

						</view>
						<!-- <view class="login__info__item__input tn-flex tn-flex-direction-row tn-flex-nowrap tn-flex-col-center tn-flex-row-left">
							<view class="login__info__item__input__left-icon"> <view class="tn-icon-phone"></view> </view>
							<view class="login__info__item__input__content"> <input maxlength="20" placeholder-class="input-placeholder" placeholder="请输入登录手机号码" /> </view>
						</view>

						<view class="login__info__item__input tn-flex tn-flex-direction-row tn-flex-nowrap tn-flex-col-center tn-flex-row-left">
							<view class="login__info__item__input__left-icon"> <view class="tn-icon-lock"></view> </view>
							<view class="login__info__item__input__content"> <input :password="!showPassword" placeholder-class="input-placeholder" placeholder="请输入登录密码" /> </view>
							<view class="login__info__item__input__right-icon" @click="showPassword = !showPassword">
								<view :class="[showPassword ? 'tn-icon-eye' : 'tn-icon-eye-hide']"></view>
							</view>
						</view> -->
					</block>
					<!-- 重进房间 -->
					<block v-if="currentModeIndex === 1">
						<view class="login__info__item__input tn-flex tn-flex-direction-row tn-flex-nowrap tn-flex-col-center tn-flex-row-left">

							<view class="tn-flex-1 login__info__item__input__left-icon"> <view class="tn-icon-home-capsule"></view> </view>
							<view class="tn-flex-5 tn-flex login__info__item__input__content">
								<view class="tn-text-lg tn-color-white tn-text-bold" style="padding-right: 5px;;letter-spacing: 2px;">{{ today }}</view> <input v-model="old_num" type="number" maxlength="4" placeholder-class="input-placeholder" placeholder="请输入房间号码后四位" /> </view>

						</view>
						<!--
						<view class="login__info__item__input tn-flex tn-flex-direction-row tn-flex-nowrap tn-flex-col-center tn-flex-row-left">
							<view class="login__info__item__input__left-icon"> <view class="tn-icon-code"></view> </view>
							<view class="login__info__item__input__content login__info__item__input__content--verify-code">
								<input placeholder-class="input-placeholder" placeholder="请输入验证码" />
							</view>
							<view class="login__info__item__input__right-verify-code" @tap.stop="getCode">
								<tn-button backgroundColor="#01BEFF" fontColor="#FFFFFF" size="sm" padding="5rpx 10rpx" width="100%" shape="round">{{ tips }}</tn-button>
							</view>
						</view>

						<view class="login__info__item__input tn-flex tn-flex-direction-row tn-flex-nowrap tn-flex-col-center tn-flex-row-left">
							<view class="login__info__item__input__left-icon"> <view class="tn-icon-lock"></view> </view>
							<view class="login__info__item__input__content"> <input :password="!showPassword" placeholder-class="input-placeholder" placeholder="请输入登录密码" /> </view>
							<view class="login__info__item__input__right-icon" @click="showPassword = !showPassword">
								<view :class="[showPassword ? 'tn-icon-eye' : 'tn-icon-eye-hide']"></view>
							</view>
						</view> -->
					</block>

					<view class="login__info__item__button tn-cool-bg-color-7--reverse" hover-class="tn-hover" :hover-stay-time="150" @click="createGame()">{{
						currentModeIndex === 0 ? '创建房间' : '重进房间'
					}}</view>

					<!-- <view v-if="currentModeIndex === 0" class="login__info__item__tips">忘记密码?</view> -->
				</view>

				<!-- 其他登录方式 -->
				<!-- <view class="login__way tn-flex tn-flex-col-center tn-flex-row-center">
					<view class="tn-padding-sm tn-margin-xs">
						<view class="login__way__item--icon tn-flex tn-flex-row-center tn-flex-col-center tn-shadow-blur tn-bg-green tn-color-white">
							<view class="tn-icon-wechat-fill"></view>
						</view>
					</view>
					<view class="tn-padding-sm tn-margin-xs">
						<view class="login__way__item--icon tn-flex tn-flex-row-center tn-flex-col-center tn-shadow-blur tn-bg-red tn-color-white"> <view class="tn-icon-sina"></view> </view>
					</view>
					<view class="tn-padding-sm tn-margin-xs">
						<view class="login__way__item--icon tn-flex tn-flex-row-center tn-flex-col-center tn-shadow-blur tn-bg-blue tn-color-white"> <view class="tn-icon-qq"></view> </view>
					</view>
				</view> -->
			</view>

			<!-- 底部背景图片-->
			<!-- <view class="login__bg login__bg--bottom">
        <image src="https://tnuiimage.tnkjapp.com/login/1/login_bottom_bg.jpg" mode="widthFix"></image>
      </view> -->
		</view>

		<!-- 验证码倒计时 -->
		<!-- <tn-verification-code ref="code" uniqueKey="login-demo-2" :seconds="60" @change="codeChange"> </tn-verification-code> -->

		<view>
			<tn-toast ref="toast" @closed="toInterface()"></tn-toast>
		</view>

	</view>
</template>

<script>
// import template_page_mixin from '@/libs/mixin/template_page_mixin.js'

// 接口
import { GetCurrentUser, GetGameCount } from 'config/api.js'

const Point = class {
	constructor(x, y) {
		this.x = x
		this.y = y
		this.r = 1 + Math.random() * 2
		this.sx = Math.random() * 2 - 1
		this.sy = Math.random() * 2 - 1
	}

	draw(ctx) {
		ctx.beginPath()
		ctx.arc(this.x, this.y, this.r, 0, 2 * Math.PI)
		ctx.closePath()
		ctx.fillStyle = '#fff'
		ctx.fill()
	}

	move(w, h) {
		this.x += this.sx
		this.y += this.sy
		if (this.x > w || this.x < 0) this.sx = -this.sx
		if (this.y > h || this.y < 0) this.sy = -this.sy
	}

	drawLine(ctx, p) {
		const dx = this.x - p.x
		const dy = this.y - p.y
		const d = Math.sqrt(dx * dx + dy * dy)
		if (d < 100) {
			var alpha = ((100 - d) / 300) * 1
			ctx.beginPath()
			ctx.moveTo(this.x, this.y)
			ctx.lineTo(p.x, p.y)
			ctx.closePath()
			ctx.strokeStyle = 'rgba(255, 255, 255, ' + alpha + ')'
			ctx.strokeWidth = 1
			ctx.stroke()
		}
	}
}

const sysinfo = uni.getSystemInfoSync()
const w = 400
const h = 1000

export default {
	name: 'LoginAdmin',
	// mixins: [template_page_mixin],
	data() {
		return {
			ctx: null,
			screenWidth: sysinfo.screenWidth,
			screenHeight: sysinfo.screenHeight,
			timer: null,
			points: [],

			// 当前选中的模式
			currentModeIndex: 0,
			// 模式选中滑块
			modeSliderStyle: {
				left: 0
			},
			toast_significance: '',
			// 是否显示密码
			// showPassword: false,
			// 倒计时提示文字
			// tips: '获取验证码'
			new_num: '',
			old_num: '',
			today: '',
			// intRandomValue: ''
			count: ''
		}
	},
	onLoad(options) {
		// 鉴权
		uni.getStorage({
			key: 'token',
			success: (res) => {
				console.log(res)
				if (!res.data || !res.data.token) uni.redirectTo({ url: '/pages/register/index' })
			},
			fail(res) {
				console.log(res)
				uni.redirectTo({ url: '/pages/register/index' })
			}
		})

		if (!getApp().globalData.phone) return uni.redirectTo({ url: '/pages/register/index' })
		GetGameCount({
			phone: getApp().globalData.phone
		})
			.then((res) => {
				if (res[1].data.status === 200) {
					this.count = res[1].data.data.items[0].game_count
				} else {
					this.$t.message.toast('获取可玩次数失败')
				}
			})
			.catch((err) => {
				console.log(err)
			})

		getApp().globalData.admin = this

		this.from = options.from || ''

		for (let i = 0; i < 80; i++) {
			this.points.push(new Point(Math.random() * w, Math.random() * h))
		}
		this.ctx = uni.createCanvasContext('star_canvas')
		// console.log(points)

		this.gameloop() // 进行
		// this.ctx.setFillStyle('red')
		// this.ctx.fillRect(200, 300, 50, 50)
		// this.ctx.draw()
		this.getToday()
	},
	onUnload() {
		clearTimeout(this.timer)
		console.log('卸载admin组件')
		getApp().globalData.admin = null
	},
	onShow() {
		// getApp().globalData.admin = this
	},
	onHide() {
		console.log('隐藏admin组件')
		// getApp().globalData.admin = null
	},

	watch: {
		currentModeIndex(value) {
			const sliderWidth = uni.upx2px(476 / 2)
			this.modeSliderStyle.left = `${sliderWidth * value}px`
		}
	},
	methods: {
		logout() {
			uni.removeStorage({
				key: 'token',
				success(res) {
					console.log('移除token成功')
				}
			})
			uni.redirectTo({ url: '/pages/register/index' })
		},

		/** 粒子进行*/

		gameloop() {
			this.timer = setTimeout(this.gameloop, 100)
			// console.log('gameloop')
			this.paint()
		},

		/** 清空画布*/

		paint() {
			this.ctx.clearRect(0, 0, w, h)
			for (var i = 0; i < this.points.length; i++) {
				this.points[i].move(w, h)
				this.points[i].draw(this.ctx)
				for (var j = i + 1; j < this.points.length; j++) {
					this.points[i].drawLine(this.ctx, this.points[j])
				}
			}
			this.ctx.draw()
		},
		// 切换模式
		modeSwitch(index) {
			this.currentModeIndex = index
			// this.showPassword = false
		},
		// 获取验证码
		// getCode() {
		// 	if (this.$refs.code.canGetCode) {
		// 		this.$t.message.loading('正在获取验证码')
		// 		setTimeout(() => {
		// 			this.$t.message.closeLoading()
		// 			this.$t.message.toast('验证码已经发送')
		// 			// 通知组件开始计时
		// 			this.$refs.code.start()
		// 		}, 2000)
		// 	} else {
		// 		this.$t.message.toast(this.$refs.code.secNum + '秒后再重试')
		// 	}
		// },
		// 获取验证码倒计时被修改
		// codeChange(event) {
		// 	this.tips = event
		// }
		getToday() {
			const date = new Date()
			const year = String(date.getFullYear()).substring(2)
			let month = String(date.getMonth() + 1)
			let day = String(date.getDate())
			// console.log(date, year, month, day)
			month = month.length > 1 ? month : '0' + month
			day = day.length < 2 ? '0' + day : day
			this.today = year + month + day
		},

		createGame() {
			// console.log(getApp().globalData.wsHandle)
			if (this.currentModeIndex === 0) {
				if (!this.count) return this.$t.message.toast('可玩次数为0，无法创建房间')

				if (this.new_num.length !== 4) return this.globalNotice('提示', '请输入房间号后四位', 'tip-fill')
				getApp().globalData.init({
					method: 'createGame',
					data: this.today + this.new_num // 房间秘钥
				})
			} else if (this.currentModeIndex === 1) {
				if (this.old_num.length !== 4) return this.globalNotice('提示', '请输入房间号后四位', 'tip-fill')
				getApp().globalData.init({
					method: 'rejoinGame',
					data: this.today + this.old_num // 房间秘钥
				})
			}
			// console.log(getApp().globalData.wsHandle)
			// getApp().globalData.wsHandle.send({})
		},
		globalNotice(title, content, icon, significance) {
			if (title === 'toPlay') return this.toast_significance = 'toPlay'
			this.$refs.toast.show({
				title,
				content,
				icon,
				image: '',
				duration: 1500
			})
			if (significance) this.toast_significance = significance
		},
		toInterface() {
			// console.log('xxx')
			if (this.toast_significance === 'toDrag') {
				// this.toast_significance = ''
				uni.redirectTo({
					url: '/pages/drag/index?role=admin'
				})
			} else if (this.toast_significance === 'toPlay') {
				// this.toast_significance = ''
				GetCurrentUser({
					game_id: getApp().globalData.gameId
				})
					.then((res) => {
					// console.log(res)
					// console.log(res[1].data.data)
						if (res[1].data.status === 200) {
							getApp().globalData.round = [res[1].data.data.game_user_id, res[1].data.data.index]
							uni.redirectTo({ url: '/pages/manipulate/index?role=admin' })
						} else {
						}
					})
					.catch((err) => {
						console.log(err)
					})
			}
		},
		// 获取整数随机值
		getIntRandomValue() {
			const temp_num = this.$t.number.randomInt(0, 9999) // 包括头和尾
			// (Array(n).join(0) + num).slice(-n); //js 数字前面自动补零。// // num传入的数字，n需要的字符长度。// slice当参数为负值时，表示按从右到左的顺序进行定位，即倒数定位法，而不再按正数顺序定位（从左到右），但取值顺序依然是从左到右。
			this.new_num = (Array(4).join(0) + temp_num).slice(-4)
			// console.log(String(temp_num).length, temp_num, (Array(4).join(0) + temp_num).slice(-4), this.new_num)
		}

	}
}
</script>

<style lang="scss" scoped>
	@import '@/static/css/particle/particle.scss';

</style>
