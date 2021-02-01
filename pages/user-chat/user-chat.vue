<template>
	<view>
		<!-- 聊天消息列表 -->
		<scroll-view scroll-y="true" style="position: absolute; left: 0; top: 0; right: 0; bottom: 100rpx;" :scroll-into-view="scrollInto" scroll-with-animation>
			<block v-for="(item, index) in list">
				<view :id="'chat'+index">
					<user-chat-list :item="item" :index="index" :preTime="index > 0 ? list[index-1].create_time : 0"></user-chat-list>
				</view>
			</block>
		</scroll-view>
		<!-- 底部操作条 -->
		<view style="height: 100rpx;" class="fixed-bottom flex align-center border-top bg-white">
			<input type="text" v-model="content" value="" class="flex-1 rounded bg-light ml-2" style="padding: 5rpx 0;" placeholder="文明发言" @confirm="submit()" />
			<view class="iconfont icon-fasong flex align-center justify-center font-lg animate__animated" hover-class="animate__jello text-main" style="width: 100rpx;" @click="submit()"></view>
		</view>
	</view>
</template>

<script>
	import userChatList from '@/components/user-chat/user-chat-list/user-chat-list.vue';
	export default {
		data() {
			return {
				list: [
					{
						user_id: 2,
						username: 'Natalia',
						avatar: '/static/img/userpic/16.jpg',
						data: '大佬，你好',
						type: 'text', // text、image、video、audio、link
						create_time: 1612156712
					},
					{
						user_id: 2,
						username: 'Corley',
						avatar: '/static/img/userpic/16.jpg',
						data: '我想请教一个关于uni-app的问题，不知道是否方便？',
						type: 'text',
						create_time: 1612156872
					},
					{
						user_id: 1,
						username: 'Natalia',
						avatar: '/static/img/userpic/7.jpg',
						data: '你好啊，大佬不敢当😅',
						type: 'text',
						create_time: 1612156905
					},
					{
						user_id: 1,
						username: 'Corley',
						avatar: '/static/img/userpic/7.jpg',
						data: '有什么你就说吧',
						type: 'text',
						create_time: 1612157023
					},
					{
						user_id: 1,
						username: 'Corley',
						avatar: '/static/img/userpic/7.jpg',
						data: '只要我会的都会解答',
						type: 'text',
						create_time: 1612157029
					},
					{
						user_id: 2,
						username: 'Corley',
						avatar: '/static/img/userpic/16.jpg',
						data: '有几个问题',
						type: 'text',
						create_time: 1612157411
					},
					{
						user_id: 2,
						username: 'Natalia',
						avatar: '/static/img/userpic/16.jpg',
						data: '1.在导航栏上单击搜索输入监听搜索框的事件该写在什么位置啊，为什么我写的触发不了？',
						type: 'text',
						create_time: 1612157439
					},
					{
						user_id: 2,
						username: 'Corley',
						avatar: '/static/img/userpic/16.jpg',
						data: '2.关注顶踩的动画css怎么获取到的啊？',
						type: 'text',
						create_time: 1612157455
					},
					{
						user_id: 2,
						username: 'Natalia',
						avatar: '/static/img/userpic/16.jpg',
						data: '3.首页开发最后代码写完，再点击关注和点赞，踩，就会报错。辛苦看一看啊',
						type: 'text',
						create_time: 1612157503
					},
					{
						user_id: 1,
						username: 'Corley',
						avatar: '/static/img/userpic/7.jpg',
						data: '好的，我马上看',
						type: 'text',
						create_time: 1612157821
					}
				],
				content: '',
				scrollInto: ''
			}
		},
		components: {
			userChatList
		},
		// 页面加载完成
		onReady() {
			this.pageToBottom();
		},
		onLoad(e) {
		    console.log(e);
		    this.username = e.username;
		},
		// 页面加载完成
		onReady() {
		    this.pageToBottom();
		    uni.setNavigationBarTitle({
		        title: this.username
		    });
		},
		methods: {
			// 发送消息
			submit() {
				if (this.content === ''){
					return uni.showToast({
						title: '消息不能为空',
						icon: 'none'
					});
				}
				let obj = {
					user_id: 1,
					username: 'Corley',
					avatar: '/static/img/userpic/11.jpg',
					data: this.content,
					type: 'text',
					create_time: (new Date()).getTime()
				}
				this.list.push(obj);
				// 清空输入框
				this.content = '';
				// 滚动到底部
				this.pageToBottom();
			},
			// 滚动到底部
			pageToBottom() {
				let lastIndex = this.list.length - 1;
				if (lastIndex < 0) return;
				this.scrollInto = 'chat' + lastIndex;
			}
		}
	}
</script>

<style>

</style>

