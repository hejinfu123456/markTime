<template>
	<view>
		<!-- 个人信息栏 -->
		<navigator url="../login/login">
			<!-- 个人信息栏 -->
			<view class="flex align-center p-2" hover-class="bg-light">
				<image src="/static/img/userpic/16.jpg" class="rounded-circle" style="width: 100rpx; height: 100rpx;"></image>
				<view class="flex flex-column flex-1 px-2">
					<text class="font-lg font-weight-bold">Corley</text>
					<text class="font text-muted">总帖子13 今日发帖1</text>
				</view>
				<text class="iconfont icon-bangzhujinru"></text>
			</view>
		</navigator>
		<!-- 互动信息栏 -->
		<view class="flex align-center px-3 py-2">
			<block v-for="(item, index) in myData" :key="index">
				<view class="flex flex-column align-center justify-center flex-1">
					<text class="font-lg font-weight-bold">{{item.num}}</text>
					<text class="font text-muted">{{item.name}}</text>
				</view>
			</block>
		</view>
		<!-- 广告位 -->
		<view class="px-3 py-2">
			<image src="/static/img/banner/banner1.jpg" mode="aspectFill" style="height: 170rpx; width: 100%;" class="rounded"></image>
		</view>
		<!-- 浏览历史、社区认证和审核帖子 -->
		<uni-list-item title="浏览历史" showExtraIcon>
		    <text slot="icon" class="iconfont icon-liulan"></text>
		</uni-list-item>
		<uni-list-item title="社区认证" showExtraIcon>
		    <text slot="icon" class="iconfont icon-huiyuanvip"></text>
		</uni-list-item>
		<uni-list-item title="审核帖子" showExtraIcon>
		    <text slot="icon" class="iconfont icon-shenhe"></text>
		</uni-list-item>
	</view>
</template>
<script>
	import uniListItem from '../../components/uni-ui/uni-list-item/uni-list-item.vue';
	export default{
		data(){
			return {
				myData:[{
						name:'帖子',
						num:1,
					},{
						name:'动态',
						num:1,
					},{
						name:'评论',
						num:2,
					},{
						name:'粉丝',
						num:3,
					}
				],
			}
		},
		components:{
			uniListItem
		},
		onNavigationBarButtonTap() {
		    uni.navigateTo({
		        url: '../user-set/user-set',
		    });
		},

	}
</script>
