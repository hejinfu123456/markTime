<template>
	<view>
		<view class="flex align-center justify-between px-2">
			<text class="font-md">热门分类</text>
			<view class="flex align-center font text-secondry animate__animated" hover-class="animate__jello" @click="openMore()">
				更多<text class="iconfont icon-bangzhujinru"></text>
			</view>
		</view>
		<view class="flex align-center px-2 py-3 border-bottom">
			<view class="border rounded bg-light mx-1 px-2 animate__animated" hover-class="animate__jello" v-for="(item, index) in hotCate" :key="index" @click="openDetail(item)">
				{{item.name}}
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		props: ['hotCate'],
		methods: {
			openMore() {
			    console.log('opening more');
			    uni.navigateTo({
			        url: '../../pages/topic-nav/topic-nav'
			    });
			},

			openDetail(item) {
				console.log('opening detail');
			}
		}
	}
</script>

<style>
</style>

