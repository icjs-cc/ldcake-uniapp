<template>
	<view class="t-vcode" :style="{color:defaultColor}">
		<view v-if="showText==true" @click="handleSend">{{sendText}}</view>
		<view v-else :style="[{color: waitColor}]">{{vcodeTimer}}{{waitText}}</view>
	</view>
</template>

<script>
	export default {
		name: "t-vcode",
		props: {
			second: {
				type: Number,
				default: 60
			},
			sendText: {
				type: String,
				default: '获取验证码'
			},
			waitText: {
				type: String,
				default: 's重新发送'
			},
			color: {
				type: String,
				default: ''
			},
			waitColor: {
				type: String,
				default: '#a2a2a2'
			},
			border: {
				type: Boolean,
				default: false
			},
			value: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				showText: true,
				vcodeTimer: 60,
				vcodeInterval: null,
				vcodeTimeout: null,
				defaultColor: null
			}
		},
		mounted() {
			this.defaultColor = this.color||this.$it.color.primary
			this.vcodeTimer = this.second
		},
		watch:{
			value(val){
				if(val){
					this.getCode()
				}
			}
		},
		destroyed() {
			if(this.vcodeInterval){
				clearInterval(this.vcodeInterval)
			}
			if(this.vcodeTimeout){
				clearTimeout(this.vcodeTimeout)
			}
		},
		methods: {
			handleSend(){
				this.$emit('send', this.showText)
			},
			getCode() {
				this.showText = false
				this.vcodeInterval = setInterval(() => {
					let times = --this.vcodeTimer
					this.vcodeTimer = times < 10 ? '0' + times : times //小于10秒补0
				}, 1000)
				this.vcodeTimeout = setTimeout(() => {
					clearInterval(this.vcodeInterval)
					this.vcodeTimer = this.second
					this.showText = true
					this.$emit('input', false)
				}, this.second * 1000)
			}
		}
	}
</script>

<style lang="scss">
.t-vcode{
	padding: 0 30upx;
	font-size: 28upx;
	
	&__border-left{
		border-left: 1upx solid #f2f2f2;
	}
}
</style>
