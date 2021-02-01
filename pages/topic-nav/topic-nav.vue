<template>
	<view>
		<!-- 顶部选项卡 -->
		<scroll-view scroll-x="true" class="scroll-row" :scroll-into-view="scrollInto" :scroll-with-animation="true" style="height: 100rpx;">
			<view v-for="(item, index) in tabBars" :key="index" class="scroll-row-item px-3 py-2 font-md" :id="'tab'+index"
			 :class="tabIndex === index ? 'text-main font-lg font-weight-bold' : ''" @click="changeTab(index)">{{item.name}}</view>
		</scroll-view>
		<!-- 滑块 -->
		<swiper :duration="150" :current="tabIndex" @change="onChangeTab" :style="'height: '+scrollH+'px;'">
			<swiper-item v-for="(item, index) in topicArray" :key="index">
				<scroll-view scroll-y="true" :style="'height: '+scrollH+'px;'" @scrolltolower="loadMore(index)">
					<!-- 有数据 -->
					<template v-if="item.list.length > 0">
						<!-- 列表 -->
						<block v-for="(item2, index2) in item.list" :key="index2">
							<!-- 列表组件 -->
							<topic-list :item="item2" :index="index2"></topic-list>
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
			cover: '/static/img/topicpic/14.jpg',
			title: '毛伟明当选湖南省人民政府省长',
			desc: '毛伟明当选湖南省人民政府省长',
			news_count: 10,
			today_count: 1
		},
		{
			cover: '/static/img/topicpic/16.jpg',
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
			cover: '/static/img/topicpic/14.jpg',
			title: '"现实版樊胜美"弟弟疑遭人肉网暴',
			desc: '现实版樊胜美弟弟疑遭人肉网暴 挂出其母弟弟住址电话',
			news_count: 6,
			today_count: 0
		}
	];
	import topicList from '@/components/news/topic-list/topic-list.vue';
	import loadMore from '@/components/common/load-more/load-more.vue';
	export default {
		data() {
			return {
				topicArray: [],
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
			topicList,
			loadMore
		},
		onLoad() {
			uni.getSystemInfo({
					success: function(res) {
						console.log(res);
						this.scrollH = res.windowHeight - uni.upx2px(100);
					}
				}),
				// 根据选项生成列表
				this.getData();
		},
		methods: {
			// 切换选项
			changeTab(index) {
				if (this.tabIndex === index) {
					return;
				}
				this.tabIndex = index;
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
				this.topicArray = arr;
			},
			// 上拉加载更多
			loadMore(index) {
				// 获取当前列表
				let item = this.topicArray[index];
				// 判断是否处于可加载状态
				if (item.loadmore !== '上拉加载更多') return;
				// 修改当前列表加载状态
				item.loadmore = '加载中...';
				// 模拟数据请求
				setTimeout(() => {
					// 加载数据
					item.list = [...item.list, ...item.list];
					// 恢复加载状态
					this.topicArray[index].loadmore = '上拉加载更多';
				}, 2000)
			}
		}
	}
</script>

<style>

</style>

