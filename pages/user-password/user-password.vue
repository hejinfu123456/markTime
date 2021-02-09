<template>
	<view>
		<input type="text" class="uni-input" placeholder="输入旧密码" v-model="oldpassword" />
		<input type="text" class="uni-input" placeholder="输入新密码" v-model="newpassword" />
		<input type="text" class="uni-input" placeholder="输入确认密码" v-model="renewpassword" />
		<view class="py-2 py-3">
			<button class="bg-main text-white" :class="disabled ? 'bg-main-disabled' : ''" style="border-radius: 50rpx; border: 0;"
			 type="primary" :disabled="disabled" @click="submit()">设置</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				oldpassword: '',
				newpassword: '',
				renewpassword: ''
			}
		},
		computed: {
			disabled() {
				return this.oldpassword === '' || this.newpassword === '' || this.renewpassword === '';
			}
		},
		methods: {
			check() {
				// 验证
				if (this.newpassword !== this.renewpassword) {
					uni.showToast({
						title: '两次密码不一致'
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

