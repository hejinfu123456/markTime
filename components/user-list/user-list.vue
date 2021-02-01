<template>
	<view class="flex align-center p-2 border-bottom" hover-class="bg-light">
		<image :src="item.avatar" style="width: 100rpx; height: 100rpx;" class="rounded-circle mr-2"></image>
		<view class="flex flex-column flex-1">
			<text class="font-md text-dark">{{item.username}}</text>
			<uni-badge :text="item.age" :type="item.sex === 1 ? 'error' : (item.sex === 2 ? 'primary' : 'default')" size="small">
				<template v-if="item.sex > 0">
					<text class="iconfont text-white font-sm" :class="item.sex === 1 ? 'icon-nvxing' : 'icon-nanxing'" style="margin-right: 5rpx;"></text>
				</template>
			</uni-badge>
		</view>
		<view class="uni-icon uni-icon-checkbox-filled" :class="item.isFollow ? 'text-light-muted' : 'text-main'"></view>
	</view>
</template>

<script>
	import uniBadge from '@/components/uni-ui/uni-badge/uni-badge.vue';
	export default {
		props: {
			item: Object,
			index: Number
		},
		components: {
			uniBadge
		}
	}
</script>

<style>
</style>

