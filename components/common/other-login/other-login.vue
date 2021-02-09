<template>
	<view class="flex align-center px-7 py-3">
		<block v-for="(item, index) in providerList" :key="index">
			<view class="flex-1 flex align-center justify-center">
				<view class="iconfont font-lg text-white flex align-center justify-center rounded-circle" :class="item.icon+' '+item.bgColor"
				 style="width: 100rpx; height: 100rpx;"></view>
			</view>
		</block>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				providerList: []
			}
		},
		mounted() {
			uni.getProvider({
				service: 'oauth',
				success: (result) => {
					result = result.provider.splice(1);
					this.providerList = result.map((value) => {
						let providerName = '';
						let icon = '';
						let bgColor = '';
						switch (value) {
							case 'weixin':
								providerName = '微信登录'
								icon = 'icon-weixin'
								bgColor = 'bg-green'
								break;
							case 'qq':
								providerName = 'QQ登录'
								icon = 'icon-qq'
								bgColor = 'bg-blue'
								break;
							case 'sinaweibo':
								providerName = '新浪微博登录'
								icon = 'icon-weibo'
								bgColor = 'bg-red'
								break;
						}
						return {
							name: providerName,
							id: value,
							icon: icon,
							bgColor: bgColor
						}
					});
				},
				fail: (error) => {
					console.log('获取登录通道失败', error);
				}
			});
		}
	}
</script>

<style>
</style>

