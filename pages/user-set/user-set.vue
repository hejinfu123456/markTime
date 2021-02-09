<template>
	<view>
		<uni-list-item title="账号与安全" :clickable="true" :showIcon="true" @click="open('user-password')"></uni-list-item>
		<uni-list-item title="绑定邮箱" :clickable="true" :showIcon="true" @click="open('user-email')"></uni-list-item>
		<uni-list-item title="资料编辑" :clickable="true" :showIcon="true" @click="open('user-userinfo')"></uni-list-item>
		<uni-list-item title="清除缓存" :clickable="true" :showIcon="true" @click="clearCache()">
			<view slot="footer" class="text-muted">
				{{currentSize | format}}
			</view>
		</uni-list-item>
		<uni-list-item title="意见反馈" :clickable="true" :showIcon="true" @click="open('user-feedback')"></uni-list-item>
		<uni-list-item title="关于社区" :clickable="true" :showIcon="true" @click="open('about')"></uni-list-item>
		<view class="py-2 py-3 px-2">
			<button class="bg-main text-white" style="border-radius: 50rpx; border: 0;" type="primary">退出登录</button>
		</view>
	</view>
</template>

<script>
	import uniListItem from '@/components/uni-ui/uni-list-item/uni-list-item.vue';
	export default {
		data() {
			return {
				currentSize: 0
			}
		},
		components: {
			uniListItem
		},
		filters: {
			format(value) {
				return value > 1024 ? (value / 1024).toFixed(2) + 'MB' : value.toFixed(2) + 'KB';
			}
		},
		onLoad() {
			this.getStorageInfo();
		},
		methods: {
			open(path) {
				uni.navigateTo({
					url: `../${path}/${path}`
				});
			},
			// 获取缓存数据
			getStorageInfo() {
				let res = uni.getStorageInfoSync();
				console.log(res);
				this.currentSize = res.currentSize;
			},
			// 清除缓存
			clearCache() {
				uni.showModal({
					title: '清除缓存提示',
					content: '是否要清除所有缓存？',
					showCancel: true,
					cancelText: '不清除',
					confirmText: '清除',
					success: res => {
						if (res.confirm) {
							uni.clearStorageSync();
							this.getStorageInfo();
							uni.showToast({
								title: '清除成功'
							});
						}
					}
				});
			}
		}
	}
</script>

<style>

</style>

