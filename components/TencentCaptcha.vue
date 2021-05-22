<template>
	<view class="verify-code" @click="show">
		{{ text }}
		<!-- #ifdef MP-WEIXIN -->
		<t-captcha
			id="captcha"
			:app-id="appId"
			:lang="locale"
			@close="handlerClose"
			@verify="handlerVerify" />
		<!-- #endif -->
	</view>
</template>

<script>
	export default {
		props: {
			// 申请的appId
			appId: {
				type: String,
				default: ''
			},
			// 多语言 支持 zh-cn zh-hk en
			locale: {
				type: String,
				default: 'zh-cn'
			},
			// 滑块处理的事件昵称，用于多场景使用时，区分当前触发的滑块验证类型
			eventName: {
				type: String,
				default: ''
			},
			// 显示的文字内容
			text: {
				type: String,
				default: ''
			}
		},
		methods: {
			/*
			 * 显示滑块验证码
			 */
			show() {
				// #ifdef MP-WEIXIN
				const slideVerify = this.selectComponent('#captcha')
				slideVerify.show()
				// #endif

				// #ifdef H5
				const captcha = new TencentCaptcha(this.appId, (res) => {
					// 验证成功
					if (res.ret === 0) {
						this.$emit('verified', { eventName: this.eventName, ticket: res.ticket, randstr: res.randstr })
					} else if (res.ret === 2) {
						// 用户主动关闭
						this.$emit('close')
					}
				}, { forceLang: this.locale })
				captcha.show()
				// #endif
			}
			// #ifdef MP-WEIXIN
			,
			/*
			 * 微信小程序验证成功回调
			 */
			handlerVerify(ev) {
				if (ev && ev.mp.detail && ev.mp.detail.ret === 0) {
					this.$emit('verified', { eventName: this.eventName, ticket: ev.mp.detail.ticket, randstr: ''})
				}
			},
			/*
			 * 用户主动关闭
			 */
			handlerClose() {
				this.$emit('close')
			}
			// #endif
		}
	}
</script>

<style>
</style>
