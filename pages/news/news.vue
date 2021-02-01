<template>
	<view>
		<!-- 导航栏 -->
		<uni-nav-bar :border="false" :fixed="true" :statusBar="true" @clickRight="openAddInput">
			<view class="flex align-center justify-center font-weight-bold w-100">
				<view class="mx-1" v-for="(item, index) in tabBars" :key="index" :class="tabIndex === index ? 'font-lg text-main' : 'font-md text-light-muted'"
				 @click="changeTab(index)">{{item.name}}</view>
			</view>
			<text slot="right" class="iconfont icon-fatie_icon"></text>
		</uni-nav-bar>
		<!-- 滑块 -->
		<swiper :current="tabIndex" :duration="150" :style="'height: '+scrollH+'px;'" @change="onChangeTab">
			<!-- 关注 -->
			<swiper-item>
				<scroll-view scroll-y="true" :style="'height: '+scrollH+'px;'" @scrolltolower="loadmoreEvent">
					<block v-for="(item, index) in list" :key="index">
						<common-list :item="item" :index="index" @doSupport="doSupport"></common-list>
						<divider></divider>
					</block>
					<load-more :loadmore="loadmore"></load-more>
				</scroll-view>
			</swiper-item>
			<!-- 话题 -->
			<swiper-item>
				<scroll-view scroll-y="true" :style="'height: '+scrollH+'px;'">
					<!-- 热门分类 -->
					<hot-cate :hotCate="hotCate"></hot-cate>
					<!-- 搜索框 -->
					<view class="p-2">
						<view class="bg-light rounded flex align-center justify-center py-2 text-secondry">
							<text class="iconfont icon-sousuo mr-2"></text>搜索话题
						</view>
					</view>
					<!-- 轮播图 -->
					<swiper class="px-2 pb-2" :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000">
						<swiper-item>
							<image src="/static/img/banner/banner1.jpg" style="height: 300rpx;" class="w-100 rounded" mode=""></image>
						</swiper-item>
						<swiper-item>
							<image src="/static/img/banner/banner2.jpeg" style="height: 300rpx;" class="w-100 rounded" mode=""></image>
						</swiper-item>
						<swiper-item>
							<image src="/static/img/banner/banner3.jpg" style="height: 300rpx;" class="w-100 rounded" mode=""></image>
						</swiper-item>
					</swiper>
					<!-- 最近更新 -->
					<view class="p-2 font-md">最近更新</view>
					<!-- 话题列表 -->
					<block v-for="(item, index) in topicList" :key="index">
						<topic-list :item="item" :index="index"></topic-list>
					</block>
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
			isFollow: true,
			title: "uni-app入门教程",
			titlepic: "/static/img/datapic/20.jpg",
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
			isFollow: true,
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
			titlepic: "/static/img/datapic/12.jpg",
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
	import uniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue';
	import commonList from '@/components/common/common-list/common-list.vue';
	import loadMore from '../../components/common/load-more/load-more.vue';
	import hotCate from '@/components/news/hot-cate/hot-cate.vue';
	import topicList from '@/components/news/topic-list/topic-list.vue';
	export default {
		data() {
			return {
				tabBars: [{
						name: '关注'
					},
					{
						name: '话题'
					}
				],
				tabIndex: 0,
				scrollH: 700,
				// 关注列表
				list: [],
				loadmore: "上拉加载更多",
				hotCate: [
					{
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
						
					}
				],
				topicList: [
					{
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
				]
			}
		},
		components: {
			uniNavBar,
			commonList,
			loadMore,
			hotCate,
			topicList
		},
		onLoad() {
			uni.getSystemInfo({
					success: function(res) {
						console.log(res);
						this.scrollH = res.windowHeight - res.statusBarHeight - 44;
					}
				}),
				// 加载测试数据
				this.list = test_data;
		},
		methods: {
			// 打开发布页
			openAddInput() {
				uni.navigateTo({
					url: '../add-input/add-input'
				});
			},
			// 切换选项卡
			changeTab(index) {
				this.tabIndex = index;
			},
			// 滑动
			onChangeTab(e) {
				this.tabIndex = e.detail.current;
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
			// 上拉加载更多
			loadmoreEvent() {
				// 验证当前是否处于可加载状态
				if (this.loadmore !== '上拉加载更多') return;
				// 设置加载状态
				this.loadmore = '加载中...';
				// 模拟请求数据
				setTimeout(() => {
					// 加载数据
					this.list = [...this.list, ...this.list];
					// 设置加载状态
					this.loadmore = '上拉加载更多';
				}, 2000)
			}
		}
	}
</script>

<style>

</style>

