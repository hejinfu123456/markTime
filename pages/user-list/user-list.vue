<template>
	<view>
		<!-- 标签栏 -->
		<view class="flex align-center" style="height: 100rpx;">
			<view class="flex-1 flex align-center justify-center" v-for="(item ,index) in tabBars" :key="'tab'+index" :class="index === tabIndex ? 'font-lg font-weight-bold text-main' : 'font-md'"
			 @click="changeTab(index)">{{item.name}} <text v-if="item.num > 0" class="ml-2">{{item.num}}</text></view>
		</view>
		<!-- 好友列表 -->
		<!-- 滑块 -->
		<swiper :duration="150" :current="tabIndex" @change="onChangeTab" :style="'height: '+scrollH+'px;'">
			<swiper-item v-for="(item, index) in userList" :key="index">
				<scroll-view scroll-y="true" :style="'height: '+scrollH+'px;'" @scrolltolower="loadMore(index)">
					<!-- 有数据 -->
					<template v-if="item.list.length > 0">
						<!-- 列表 -->
						<block v-for="(item2, index2) in item.list" :key="index2">
							<!-- 列表组件 -->
							<user-list :item="item2" :index="index2"></user-list>
						</block>
						<!-- 上拉加载 -->
						<template v-if="item.list.length > 10">
							<load-more :loadmore="item.loadmore"></load-more>
						</template>
					</template>
					<!-- 无数据 -->
					<template v-else>
						<no-thing></no-thing>
					</template>
				</scroll-view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	const test_data1 = {
			avatar: '/static/img/userpic/16.jpg',
			username: 'Corley',
			sex: 1, // 0未知、1女性、2男性
			age: 23,
			isFollow: true
	}
	const test_data2 = {
			avatar: '/static/img/userpic/7.jpg',
			username: 'Casey',
			sex: 0, // 0未知、1女性、2男性
			age: 15,
			isFollow: false
	}
	const test_data3 = {
			avatar: '/static/img/userpic/20.jpg',
			username: 'Henry',
			sex: 2, // 0未知、1女性、2男性
			age: 18,
			isFollow: true
	}
	import loadMore from '@/components/common/load-more/load-more.vue';
	import userList from '@/components/user-list/user-list.vue';
	export default {
		data() {
			return {
				tabIndex: 0,
				tabBars: [{
						name: '互关',
						num: 5
					},
					{
						name: '关注',
						num: 3
					},
					{
						name: '粉丝',
						num: 12
					}
				],
				// 列表高度
				scrollH: 600,
				userList: []
			}
		},
		// 监听点击输入框事件
		onNavigationBarSearchInputClicked() {
			uni.navigateTo({
				url: '../search/search',
			});
		},
		// 监听点击按钮事件
		onNavigationBarButtonTap() {
			uni.navigateBack({
				delta: 1
			});
		},
		onLoad() {
			uni.getSystemInfo({
				success: function(res) {
					console.log(res);
					this.scrollH = res.windowHeight - uni.upx2px(100);
				}
			});
			// 根据选项生成列表
			this.getData();
		},
		components: {
			loadMore,
			userList
		},
		methods: {
			changeTab(index) {
				this.tabIndex = index;
			},
			// 监听滑动
			onChangeTab(e) {
				console.log(e);
				this.changeTab(e.detail.current);
			},
			// 上拉加载更多
			loadMore(index) {
				// 获取当前列表
				let item = this.userList[index];
				// 判断是否处于可加载状态
				if (item.loadmore !== '上拉加载更多') return;
				// 修改当前列表加载状态
				item.loadmore = '加载中...';
				// 模拟数据请求
				setTimeout(() => {
					// 加载数据
					item.list = [...item.list, ...item.list];
					// 恢复加载状态
					this.userList[index].loadmore = '上拉加载更多';
				}, 2000)
			},
			// 获取数据
			getData() {
				var arr = [];
				for (let i = 0; i < this.tabBars.length; i++) {
					// 生成列表模板
					let obj = {
						// 3种状态：1.上拉加载更多；2.加载中...；3.没有更多了。
						loadmore: "上拉加载更多",
						list: []
					}
					for (let j = 0; j < this.tabBars[i].num; j++) {
						if (j % 3 === 0) {
							obj.list.push(test_data1);
						}
						else if (j % 3 === 1) {
							obj.list.push(test_data2);
						}
						else {
							obj.list.push(test_data3);
						}
					}
					arr.push(obj);
				}
				this.userList = arr;
				//console.log(this.userList);
			}
		}
	}
</script>

<style>

</style>

