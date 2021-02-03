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
			<block v-for="(item, index) in searchList" :key="index">
				<!-- 帖子搜索结果列表 -->
				<template v-if="type === 'post'">
					<common-list :item="item" :index="index" :key="'user'+index"></common-list>
				</template>
				<!-- 话题搜索结果列表 -->
				<template v-else-if="type === 'topic'">
					<topic-list :item="item" :index="index" :key="'user'+index"></topic-list>
				</template>
				<!-- 用户搜索结果列表 -->
				<template v-else>
					<user-list :item="item" :index="index" :key="'user'+index"></user-list>
				</template>
			</block>
		</template>
	</view>
</template>

<script>
	// 测试数据
	const post_test_data = [{
			username: "Corley",
			userpic: "/static/img/userpic/12.jpg",
			newstime: "2021-01-24 上午11:30",
			isFollow: false,
			title: "uni-app入门教程",
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
			username: "Ashley",
			userpic: "/static/img/userpic/20.jpg",
			newstime: "2021-01-24 下午18:20",
			isFollow: true,
			title: "uni-app实战之社区交友APP",
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
	const topic_test_data = [{
			cover: '/static/img/topicpic/16.jpg',
			title: '毛伟明当选湖南省人民政府省长',
			desc: '毛伟明当选湖南省人民政府省长',
			news_count: 10,
			today_count: 1
		},
		{
			cover: '/static/img/topicpic/14.jpg',
			title: '个别自美赴华乘客篡改阳性记录',
			desc: '中国驻旧金山总领馆:个别自美赴华乘客篡改、隐瞒阳性记录',
			news_count: 7,
			today_count: 2
		},
		{
			cover: '/static/img/topicpic/17.jpg',
			title: '中纪委披露安徽太和县骗保事件',
			desc: '安徽省太和县发生骗保事件,中纪委网站披露骗保百般花样',
			news_count: 21,
			today_count: 3
		},
		{
			cover: '/static/img/topicpic/18.jpg',
			title: '篮网正式签约佩莱',
			desc: '对阵篮网4次封盖,13分钟7个篮板,怒帽小乔丹的佩莱加盟篮网',
			news_count: 11,
			today_count: 2
		},
		{
			cover: '/static/img/topicpic/19.jpg',
			title: '被孟佳团队抄袭图片的模特发文',
			desc: '孟佳团队背锅承认抄袭,外国模特发文回应:没有在第一时间联系',
			news_count: 7,
			today_count: 0
		},
		{
			cover: '/static/img/topicpic/16.jpg',
			title: 'FF将通过并购在纳斯达克上市',
			desc: 'FF将通过与PSAC合并在纳斯达克上市;传蚂蚁集团将重组为央行监管的...',
			news_count: 15,
			today_count: 4
		},
		{
			cover: '/static/img/topicpic/17.jpg',
			title: '"现实版樊胜美"弟弟疑遭人肉网暴',
			desc: '现实版樊胜美弟弟疑遭人肉网暴 挂出其母弟弟住址电话',
			news_count: 6,
			today_count: 0
		}
	];
	const user_test_data = [{
			avatar: '/static/img/userpic/12.jpg',
			username: 'Corley',
			sex: 1, // 0未知、1女性、2男性
			age: 23,
			isFollow: true
		},
		{
			avatar: '/static/img/userpic/7.jpg',
			username: 'Casey',
			sex: 0,
			age: 15,
			isFollow: false
		},
		{
			avatar: '/static/img/userpic/16.jpg',
			username: 'Henry',
			sex: 2,
			age: 18,
			isFollow: true
		}
	]
	import commonList from '@/components/common/common-list/common-list.vue';
	import topicList from '@/components/news/topic-list/topic-list.vue';
	import userList from '@/components/user-list/user-list.vue';
	export default {
		data() {
			return {
				list: [
					'uni-app实战之社区交友APP',
					'uni-app入门教程',
					'面试之算法基础系列',
					'Python全栈',
					'商业数据分析从入门到入职',
					'Python数据分析实战',
					'Django+Vue开发生鲜电商平台'
				],
				searchText: '',
				// 搜索结果
				searchList: [],
				// 当前搜索类型
				type: 'post'
			}
		},
		components: {
			commonList,
			topicList,
			userList
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
		onLoad(e) {
			console.log(e);
			if (e.type) {
				this.type = e.type;
			}
			let pageTitle = '帖子';
			switch (this.type) {
				case 'post':
					pageTitle = '帖子'
					break;
				case 'topic':
					pageTitle = '话题'
					break;
				case 'user':
					pageTitle = '用户'
					break;
				default:
					break;
			}
			// 修改搜索占位
			// #ifdef APP-PLUS
			let currentWebview = this.$scope.$getAppWebview(); // 当前窗口实例
			let tn = currentWebview.getStyle().titleNView; // 当前窗口原生导航栏
			tn.searchInput.placeholder = '搜索' + pageTitle; // 修改placeholder
			currentWebview.setStyle({
				titleNView: tn
			}); // 修改原生导航栏
			// #endif

		},
		methods: {
			// 搜索事件
			searchEvent() {
				// 收起键盘
				uni.hideKeyboard();
				// 显示loading状态
				uni.showLoading({
					title: '加载中...',
					mask: false
				});
				// 请求搜索
				setTimeout(() => {
					switch (this.type) {
						case 'post':
							this.searchList = post_test_data;
							break;
						case 'topic':
							this.searchList = topic_test_data;
							break;
						case 'user':
							this.searchList = user_test_data;
							break;
						default:
							break;
					}

					// 隐藏loading状态
					uni.hideLoading();
				}, 2500)
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

