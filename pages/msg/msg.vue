<template>
	<view>
		<template v-if="list.length > 0">
			<!-- 消息列表 -->
			<block v-for="(item, index) in list" :key="index">
				<msg-list :item="item" :index="index"></msg-list>
			</block>
		</template>
		<template v-else>
			<no-thing></no-thing>
		</template>
		<!-- 弹出层 -->
		<uni-popup ref="popup" type="top">
			<view class="popup-content flex align-center justify-center font-md border-bottom" hover-class="bg-light" @click="popupEvent('friend')">
				<text class="iconfont icon-sousuo mr-2"></text> 添加好友
			</view>
			<view class="popup-content flex align-center justify-center font-md" hover-class="bg-light" @click="popupEvent('clear')">
				<text class="iconfont icon-shanchu mr-2"></text> 清除列表
			</view>
		</uni-popup>
	</view>
</template>
<script>
	const test_data = [{
			avatar: '/static/img/userpic/16.jpg',
			username: 'Corley',
			update_time: 1612075613,
			data: '我前几天问了没回我',
			noread: 5
		},
		{
			avatar: '/static/img/userpic/7.jpg',
			username: 'Brynn',
			update_time: 1612075606,
			data: '今天天气真不错',
			noread: 0
		},
		{
			avatar: '/static/img/userpic/20.jpg',
			username: 'Ellie',
			update_time: 1612075255,
			data: '那我也有点懵了',
			noread: 3
		},
		{
			avatar: '/static/img/userpic/12.jpg',
			username: 'Ainsley',
			update_time: 1612075983,
			data: '我就用浏览器和微信开发者工具调试的。',
			noread: 1
		},
		{
			avatar: '/static/img/userpic/16.jpg',
			username: 'Bella',
			update_time: 1612074571,
			data: '要不你也试试',
			noread: 2
		}
	];
	import msgList from '@/components/msg/msg-list/msg-list.vue';
	import noThing from '@/components/common/no-thing/no-thing.vue';
	import uniPopup from '@/components/uni-ui/uni-popup/uni-popup.vue';
	export default {
		data() {
			return {
				list: []
			}
		},
		components: {
			msgList,
			noThing,
			uniPopup
		},
		// 页面加载
		onLoad() {
			this.list = test_data;
		},
		// 监听下拉刷新
		onPullDownRefresh() {
			this.refresh();
		},
		// 监听原生导航栏按钮事件
		onNavigationBarButtonTap(e) {
		    console.log(e);
		    switch (e.index) {
		        case 0: // 左边
		        uni.navigateTo({
		            url: '../user-list/user-list',
		        });
		            // 关闭弹出层
		            this.$refs.popup.close();
		            break;
		        case 1: // 右边
		            this.$refs.popup.open();
		            break;
		        default:
		            break;
		    }
		},

		methods: {
			// 下拉刷新
			refresh() {
				setTimeout(() => {
					this.list = test_data;
					// 停止下拉刷新
					uni.stopPullDownRefresh();
				}, 2000)
			},
			// 弹出层选项点击事件
			popupEvent(e) {
			    switch (e) {
			        case 'friend':
			            console.log('Adding friend');
			            uni.navigateTo({
			                url: '../search/search?type=user'
			            });
			            break;
			        case 'clear':
			            console.log('Clearing list');
			            break;
			        default:
			            break;
			    }
			    // 关闭弹出层
			    this.$refs.popup.close();
			}
		}
	}
</script>

<style>

</style>

