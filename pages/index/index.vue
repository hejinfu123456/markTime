<template>
	<view>
		<!-- 顶部选项卡 -->
		<scroll-view scroll-x="true" class="scroll-row" :scroll-into-view="scrollInto" :scroll-with-animation="true" style="height: 100rpx;">
			<view v-for="(item, index) in tabBars" :key="index" class="scroll-row-item px-3 py-2 font-md" :id="'tab'+index"
			 :class="tabIndex === index ? 'text-main font-lg font-weight-bold' : ''" @click="changeTab(index)">{{item.name}}</view>
		</scroll-view>
		<!-- 滑块 -->
		<swiper :duration="150" :current="tabIndex" @change="onChangeTab" :style="'height: '+scrollH+'px;'">
			<swiper-item v-for="(item, index) in newsList" :key="index">
				<scroll-view scroll-y="true" :style="'height: '+scrollH+'px;'" @scrolltolower="loadMore(index)">
					<!-- 有数据 -->
					<template v-if="item.list.length > 0">
						<!-- 列表 -->
						<block v-for="(item2, index2) in item.list" :key="index2">
							<!-- 列表组件 -->
							<common-list :item="item2" :index="index2" @follow="follow" @doSupport="doSupport"></common-list>
							<!-- 全局分割线 -->
							<divider></divider>
						</block>
						<!-- 上拉加载 -->
						<load-more :loadmore="item.loadmore"></load-more>
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
	// 测试数据
	const test_data = [{
			username: "Corley",
			userpic: "/static/img/userpic/12.jpg",
			newstime: "2021-01-24 上午11:30",
			isFollow: false,
			title: "关于喀纳斯",
			titlepic: "/static/img/datapic/12.jpg",
			support: {
				type: "support", // 顶
				support_count: 1,
				unsupport_count: 2
			},
			comment_count: 2,
			share_count: 2,
			content: '“喀纳斯”是蒙古语，意为“美丽而神秘的湖”。喀纳斯湖位于新疆维吾尔自治区阿勒泰地区布尔津县北部，湖水来自奎屯、友谊峰等山的冰川融水和当地降水，湖面海拔1374米，面积45.73㎞²，湖泊最深处高程1181.5米，湖深188.5米，蓄水量达53.8亿立方米，是中国最深的冰碛堰塞湖，是一个坐落在阿尔泰深山密林中的高山湖泊、内陆淡水湖。喀纳斯湖景区由高山、河流、森林、湖泊、草原等奇异的自然景观、成吉思汗西征军点将台、古代岩画等历史文化遗迹与蒙古族图瓦人独特的民俗风情于一体，有驼颈湾、变色湖、卧龙湾、观鱼台等主要景点，具有极高的旅游观光、自然保护、科学考察和历史文化价值。',
			images: [{
					url: 'http://p3.music.126.net/zmmDgQyeMa94xZEGQvhksA==/109951165094046496.jpg?param=400y400'
				},
				{
					url: 'http://p4.music.126.net/nI-a9J3lU6xaqtKsMT5itA==/109951165462021126.jpg?param=400y400'
				}
			]
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
			title: "Django+Vue开发生鲜电商平台",
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


	import commonList from '@/components/common/common-list/common-list.vue';
	import loadMore from '@/components/common/load-more/load-more.vue';
	export default {
		data() {
			return {
				newsList: [],
				// 顶部选项卡
				tabBars: [{
						name: '关注'
					},
					{
						name: '推荐'
					},
					{
						name: '体育'
					},
					{
						name: '热点'
					},
					{
						name: '财经'
					},
					{
						name: '娱乐'
					},
					{
						name: '军事'
					},
					{
						name: '历史'
					},
					{
						name: '本地'
					}

				],
				tabIndex: 0,
				scrollInto: '',
				// 列表高度
				scrollH: 600
			}
		},
		components: {
			commonList,
			loadMore
		},
		onLoad() {
			uni.getSystemInfo({
					success: function(res) {
						//console.log(res);
						this.scrollH = res.windowHeight - uni.upx2px(100);
					}
				}),
				// 根据选项生成列表
				this.getData();
		},
		methods: {
			// 关注
			follow(e) {
				// console.log('Index followed');
				console.log(e);
				this.list[e].isFollow = true;
				uni.showToast({
					title: '关注' + this.list[e].username + '成功'
				})
			},
			// 顶踩操作
			doSupport(e) {
				console.log(e);
				// 获取当前列表项
				let item = this.list[e.index];
				let msg = e.type === 'support' ? '顶' : '踩';
				// 之前未顶踩过
				if (item.support.type === '') {
					item.support[e.type + '_count']++;
				}
				// 之前已顶过并且现在的操作为踩，则顶-1、踩+1
				else if (item.support.type === 'support' && e.type === 'unsupport') {
					item.support.support_count--;
					item.support.unsupport_count++;
				}
				// 之前已踩过并且现在的操作为顶，则踩-1、顶+1
				else if (item.support.type === 'unsupport' && e.type === 'support') {
					item.support.unsupport_count--;
					item.support.support_count++;
				}
				item.support.type = e.type;
				uni.showToast({
					title: msg + '成功'
				})
			},
			// 切换选项
			changeTab(index) {
				if (this.tabIndex === index) {
					return;
				}
				this.tabIndex = index;
				this.list = this.newsList[this.tabIndex].list;
				// 滚动到指定元素
				this.scrollInto = 'tab' + index;
			},
			// 监听滑动
			onChangeTab(e) {
				console.log(e);
				this.changeTab(e.detail.current);
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
					if (i % 3 !== 2) {
						obj.list = test_data;
					}
					arr.push(obj)
				}
				this.newsList = arr;
				this.list = this.newsList[this.tabIndex].list;
				console.log(this.list);
			},
			// 上拉加载更多
			loadMore(index) {
				// 获取当前列表
				let item = this.newsList[index];
				// 判断是否处于可加载状态
				if (item.loadmore !== '上拉加载更多') return;
				// 修改当前列表加载状态
				item.loadmore = '加载中...';
				// 模拟数据请求
				setTimeout(() => {
					// 加载数据
					item.list = [...item.list, ...item.list];
					// 恢复加载状态
					this.newsList[index].loadmore = '上拉加载更多';
				}, 2000)
			}
		},
		// 监听导航栏搜索框
		onNavigationBarSearchInputClicked() {
			uni.navigateTo({
				url: '../search/search?type=post'
			})
		},
		// 监听导航按钮点击事件
		onNavigationBarButtonTap() {
			uni.navigateTo({
				url: '../add-input/add-input'
			})
		},
	}
</script>

<style>

</style>
