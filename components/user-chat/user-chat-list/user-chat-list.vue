<template>
	<view class="">
		<!-- 时间 -->
		<template v-if="shortTime">
			<view class="flex align-center justify-center py-2 font-sm text-light-muted">
				{{shortTime}}
			</view>
		</template>
		
		<!-- 消息气泡 -->
		<view class="flex align-start px-2 my-2" :style="isSelf ? 'flex-direction: row-reverse;' : ''">
			<image :src="item.avatar" class="rounded-circle" style="width: 100rpx; height: 100rpx;"></image>
			<view class="bg-light p-2 rounded mx-2" style="min-width: 100rpx; max-width: 400rpx;">
				{{item.data}}
			</view>
		</view>
	</view>
</template>

<script>
	// 模拟当前登录用户userid
	const uid = 1;
	import $T from '@/common/time.js';
	export default {
		props: {
			item: Object,
			index: Number,
			preTime: [Number, String]
		},
		computed: {
			// 是否是登录用户本人
			isSelf() {
				return uid === this.item.user_id;
			},
			// 转化时间
			shortTime() {
				return $T.getChatTime(this.item.create_time, this.preTime);
			}
		},
	}
</script>

<style>
</style>

