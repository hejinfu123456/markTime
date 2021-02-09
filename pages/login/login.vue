<template>
	<view>
		<uni-status-bar></uni-status-bar>
		<view>
			<view class="iconfont icon-guanbi flex align-center justify-center font-lg" style="width: 100rpx; height: 100rpx;"
			 hover-class="bg-light" @click="back">
			</view>
		</view>

		<view class="text-center" style="padding-top: 130rpx; padding-bottom: 70rpx; font-size: 55rpx;">
			{{status ? '手机验证码登录' : '账号密码登录'}}
		</view>

		<view class="px-2">
			<template v-if="!status">
				<view class="mb-2">
					<input type="text" v-model="username" placeholder="昵称/手机号/邮箱" class="border-bottom p-2" />
				</view>
				<view class="mb-2 flex align-stretch">
					<input type="text" v-model="password" placeholder="请输入密码" class="border-bottom p-2 flex-1" />
					<view class="border-bottom flex align-center justify-center font text-muted" style="width: 150rpx;">忘记密码？</view>
				</view>
			</template>
			<template v-else>
				<view class="mb-2 flex align-stretch">
					<view class="border-bottom flex align-center justify-center font px-2">+86</view>
					<input type="text" v-model="phone" placeholder="手机号" class="border-bottom p-2 flex-1" />
				</view>
				<view class="mb-2 flex align-stretch">
					<input type="text" v-model="code" placeholder="请输入验证码" class="border-bottom p-2 flex-1" />
					<view class="border-bottom flex align-center justify-center font text-white" :class="codeTime > 0 ? 'bg-main-disabled' : ' bg-main'" style="width: 180rpx;" @click="getCode()">{{codeTime > 0 ? codeTime+' s' : '获取验证码'}}</view>
				</view>
			</template>
		</view>

		<view class="py-2 py-3 px-2">
			<button class="text-white" style="border-radius: 50rpx; border: 0;" :disabled="disabled" :class="disabled ? 'bg-main-disabled': 'bg-main'"
			 type="primary" @click="submit()">登录</button>
		</view>

		<view class="flex align-center justify-center pt-2 pb-4">
			<view class="text-primary font" @click="changeStatus">
				{{status ? '账号密码登录' : '验证码登录'}}
			</view>
			<text class="text-muted mx-2">|</text>
			<view class="text-primary font">登录遇到问题</view>
		</view>

		<view class="flex align-center justify-center">
			<view style="width: 120rpx; height: 3rpx; background-color: #DDDDDD;"></view>
			<view class="mx-2 text-muted font">社交帐号登录</view>
			<view style="width: 120rpx; height: 3rpx; background-color: #DDDDDD;"></view>
		</view>

		<view class="flex align-center px-7 py-3">
			<view class="flex-1 flex align-center justify-center">
				<view class="iconfont icon-weixin font-lg text-white bg-green flex align-center justify-center rounded-circle"
				 style="width: 100rpx; height: 100rpx;"></view>
			</view>
			<view class="flex-1 flex align-center justify-center">
				<view class="iconfont icon-qq font-lg text-white bg-blue flex align-center justify-center rounded-circle" style="width: 100rpx; height: 100rpx;"></view>
			</view>
			<view class="flex-1 flex align-center justify-center">
				<view class="iconfont icon-weibo font-lg text-white bg-red flex align-center justify-center rounded-circle" style="width: 100rpx; height: 100rpx;"></view>
			</view>
		</view>

		<view class="flex align-center justify-center text-muted">
			注册即代表同意<text class="text-primary font">《XXX社区协议》</text>
		</view>
	</view>
</template>

<script>
	import uniStatusBar from '@/components/uni-ui/uni-status-bar/uni-status-bar.vue';
	export default {
		data() {
			return {
				status: false,
				username: '',
				password: '',
				phone: '',
				code: '',
				codeTime: 0
			}
		},
		components: {
			uniStatusBar
		},
		computed: {
			disabled() {
				if ((this.username === '' || this.password === '') && (this.phone === '' || this.code === '')) {
					return true;
				}
				return false;
			}
		},
		methods: {
			back() {
				uni.navigateBack({
					delta: 1
				});
			},
			// 初始化表单字段
			initForm() {
				this.username = '';
				this.password = '';
				this.phone = '';
				this.code = '';
			},
			// 切换登录方式
			changeStatus() {
				// 初始化表单
				this.initForm();
				this.status = !this.status;
			},
			// 获取验证码
			getCode() {
				// 防止重复点击
				if (this.codeTime > 0) {
					return;
				}
				// 验证手机号
				if(!this.validate()) return;
				// 倒计时
				this.codeTime = 60;
				let timer = setInterval(()=>{
					if (this.codeTime > 0){
						this.codeTime--;
					}
					else {
						clearInterval(timer);
					}
				}, 1000);
			},
			// 验证表单
			validate() {
				//手机号正则表达式
				var mPattern = /^1[345789]\d{9}$/;
				if(!mPattern.test(this.phone)) {
					uni.showToast({
						title: '手机号格式不正确'
					});
					return false;
				}
				return true;
			},
			// 提交
			submit() {
				// 表单验证
				if(!this.validate()) return;
				// 提交后端
				
				// 登录成功处理
			}
		}
	}
</script>

<style>

</style>

