<template>
	<view>
		<input type="text" class="uni-input" v-model="email" placeholder="请输入你要绑定的邮箱"/>
		<input type="text" class="uni-input" v-model="password" placeholder="请输入密码"/>
		<view class="py-2 py-3">
			<button class="bg-main text-white" :class="disabled ? 'bg-main-disabled' : ''" style="border-radius: 50rpx; border: 0;"
			 type="primary" :disabled="disabled" @click="submit()">绑定</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				email: '',
				password: ''
			}
		},
		computed: {
			disabled() {
				return this.email === '' || this.password === '';
			}
		},
		methods: {
			check() {
				// 验证
				let rule = /^([A-Za-z0-9_\-\.])+\@([A-Za-z0-9_\-\.])+\.([A-Za-z]{2,4})$/;
				if (!rule.test(this.email)) {
					uni.showToast({
						title: '邮箱格式不正确'
					});
					return false;
				}
				return true;
			},
			submit() {
				if (!this.check()) {
					return;
				}
				console.log('Submitted successfully!!!');
			}
		}
	}
</script>

<style>

</style>

