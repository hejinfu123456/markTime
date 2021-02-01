<template>
	<view>
		<template v-if="searchList.length === 0">
			<!-- 搜索历史 -->
			<view class="py-2 font-md px-2">搜索历史</view>
			<view class="flex flex-wrap">
			    <view class="border rounded font mx-2 my-1 px-2" hover-class="bg-light" v-for="(item, index) in list" :key="index"
			    @click="clickSearchHistory(item)">{{item}}</view>
			</view>
		</template>
		<template v-else>
			<!-- 搜索结果列表 -->
			<block v-for="(item, index) in searchList" :key="index">
				<common-list :item="item" :index="index"></common-list>
			</block>
		</template>
	</view>
</template>

<script>
	// 测试数据
	const test_data = [
		{
			username: "Corley",
			userpic: "/static/img/userpic/12.jpg",
			newstime: "2021-01-24 上午11:30",
			isFollow: false,
			title: "在路上，遇见最美的秋天",
			titlepic: "/static/img/datapic/12.jpg",
			support: {
				type: "support", // 顶
				support_count: 1,
				unsupport_count: 2
			},
			comment_count: 2,
			share_count: 2
		},
		{
			username: "Brittany",
			userpic: "/static/img/userpic/16.jpg",
			newstime: "2021-01-24 下午14:00",
			isFollow: false,
			title: "商业数据分析从入门到入职",
			support: {
				type: "unsupport", // 踩
				support_count: 2,
				unsupport_count: 3
			},
			comment_count: 5,
			share_count: 1
		},
		{
			username: "Jessica",
			userpic: "/static/img/userpic/7.jpg",
			newstime: "2021-01-24 下午14:44",
			isFollow: true,
			title: "来一次与新疆的邂逅",
			titlepic: "/static/img/datapic/20.jpg",
			support: {
				type: "", // 未操作
				support_count: 2,
				unsupport_count: 7
			},
			comment_count: 0,
			share_count: 2
		},
		{
			username: "Ashley",
			userpic: "/static/img/userpic/20.jpg",
			newstime: "2021-01-24 下午18:20",
			isFollow: true,
			title: "九寨沟，一个秋天的童话",
			titlepic: "/static/img/datapic/30.jpg",
			support: {
				type: "support",
				support_count: 5,
				unsupport_count: 1
			},
			comment_count: 3,
			share_count: 0
		}
	];
	import commonList from '@/components/common/common-list/common-list.vue';
	export default {
		data() {
			return {
				list: [
					'新疆',
					'九寨沟',
					'面试之算法基础系列',
					'Python全栈',
					'商业数据分析从入门到入职',
					'Python数据分析实战',
					'Django+Vue开发生鲜电商平台'
				],
				searchText: '',
				// 搜索结果
				searchList: []
			}
		},
		components: {
			commonList
		},
		// 监听导航栏搜索框输入
		onNavigationBarSearchInputChanged(e) {
			console.log(e);
			this.searchText = e.text;
		},
		// 监听点击导航栏搜索按钮
		onNavigationBarButtonTap(e) {
			console.log(e);
			if (e.index === 0) {
				this.searchEvent();
			}
		},
		methods: {
			// 搜索事件
			searchEvent() {
				// 收起键盘
				uni.hideKeyboard();
				// 请求搜索
				setTimeout(()=>{
					this.searchList = test_data;
				}, 1000)
			},
			// 点击搜索历史
			clickSearchHistory(text) {
			    this.searchText = text;
			    this.searchEvent();
			}
		}
	}
</script>

<style>

</style>

