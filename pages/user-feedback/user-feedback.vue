<template>
	<view>
		<uni-collapse>
			<block v-for="(item, index) in feedbackInfo" :key="index">
				<uni-collapse-item :title="item.title" showAnimation>
					<view class="bg-white my-2 p-2">
						<text>{{item.content}}</text>
					</view>
				</uni-collapse-item showAnimation>
			</block>
		</uni-collapse>
		<view class="py-2 py-3">
			<button class="bg-main text-white" style="border-radius: 50rpx; border: 0;" type="primary">意见反馈</button>
		</view>
	</view>
</template>

<script>
	import uniCollapse from '@/components/uni-ui/uni-collapse/uni-collapse.vue';
	import uniCollapseItem from '@/components/uni-ui/uni-collapse-item/uni-collapse-item.vue';
	export default {
		data() {
			return {
				feedbackInfo: [{
						title: '客服什么时候在线？',
						content: '亲爱的Corley，客服的工作时间为：周一至周六10:00 - 12:00、14:00 - 19:00，非工作时间的咨询反馈会自动转为留言，客服上班后会第一时间回复哦~\n\nPS: 紧急问题可以加官方010-88888888进行反馈。'
					},
					{
						title: '忘记账号/昵称/密码怎么办？',
						content: '可以联系客服重置密码，电话010-88888888'
					},
					{
						title: '如何搜索文章？？',
						content: '您好，在资讯首页上方有一个搜索的图标，点击后即可进入到搜索页面，然后输入关键词搜索您想要的文章。'
					},
					{
						title: '闪退/操作卡顿/停止响应怎么回事？',
						content: '可能是app缓存过多导致的，您可以清理app缓存后再试一下。'
					},
					{
						title: '如何删除/注销帐号？',
						content: '(2）通过010-88888888客户服务电话，联系平台工作人员，协助进行删除/注销。'
					}
				]
			}
		},
		components: {
			uniCollapse,
			uniCollapseItem
		},
		methods: {

		}
	}
</script>

<style>


</style>

