<template>
	<view>
		<!-- 话题信息 -->
		<topic-info :info="info"></topic-info>
		<divider></divider>
		<!-- 精华帖 -->
		<block v-for="(item, index) in hotList" :key="'hot'+index">
			<view class="p-2 flex align-center border-bottom" hover-class="bg-light">
				<text class="iconfont icon-zhiding text-main"></text>
				<text class="font text-dark text-ellipsis">{{item.title}}</text>
			</view>
		</block>
		<divider></divider>
		<!-- 标签栏 -->
		<view class="flex align-center py-2">
			<view class="flex-1 flex align-center justify-center" v-for="(item ,index) in tabBars" :key="'tab'+index" :class="index === tabIndex ? 'font-lg font-weight-bold text-main' : 'font-md'"
			 @click="changeTab(index)">{{item.name}}</view>
		</view>
		<!-- 列表 -->
		<template v-if="listData.length > 0">
			<block v-for="(item, index) in listData" :key="index">
				<common-list :item="item" :index="index"></common-list>
				<divider></divider>
			</block>
		</template>
		<template v-else>
			<no-thing></no-thing>
		</template>
		<!-- 上拉加载 -->
		<load-more :loadmore="loadtext"></load-more>
	</view>
</template>

<script>
	// 测试数据
	const test_data = [{
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
	import topicInfo from '@/components/topic-detail/topic-info/topic-info.vue';
	import commonList from '@/components/common/common-list/common-list.vue';
	import noThing from '@/components/common/no-thing/no-thing.vue';
	import loadMore from '@/components/common/load-more/load-more.vue';
	export default {
		data() {
			return {
				info: {
					cover: '/static/img/topicpic/14.jpg',
					title: '毛伟明当选湖南省人民政府省长',
					desc: '毛伟明当选湖南省人民政府省长',
					news_count: 10,
					today_count: 1
				},
				hotList: [{
						title: '天若有情天亦老'
					},
					{
						title: '人间正道是沧桑'
					}
				],
				tabIndex: 0,
				tabBars: [{
						name: '默认'
					},
					{
						name: '最新'
					}
				],
				// 默认
				list1: [],
				loadtext1: '上拉加载更多',
				// 最新
				list2: [],
				loadtext2: '上拉加载更多'
			}
		},
		components: {
			topicInfo,
			commonList,
			noThing,
			loadMore
		},
		computed: {
			listData() {
				return this['list' + (this.tabIndex + 1)];
			},
			// 当前下拉加载
			loadtext() {
				return this['loadtext' + (this.tabIndex + 1)];
			}
		},
		// 触底事件
		onReachBottom() {
			console.log('Reaching bottom');
			this.loadmore();
		},
		onLoad(e) {
			console.log(e);
			if (e.detail) {
				let res = JSON.parse(e.detail);
				console.log(res);
			}
			this.list1 = test_data;
		},
		methods: {
			changeTab(index) {
				this.tabIndex = index;
			},
			// 上拉加载更多
			loadmore() {
				// 获取当前列表
				let index = this.tabIndex;
				// 是否处于可加载状态
				if (this.loadtext !== '上拉加载更多') return;
				// 设置上拉加载状态
				this['loadtext' + (index + 1)] = '加载中...';
				// 请求数据
				setTimeout(() => {
					this['list' + (index + 1)] = [...this['list' + (index + 1)], ...this['list' + (index + 1)]];
					this['loadtext' + (index + 1)] = '上拉加载更多';
				}, 2000);
			}
		}
	}
</script>

<style>
	.filter {
		filter: blur(10px);
	}
</style>

