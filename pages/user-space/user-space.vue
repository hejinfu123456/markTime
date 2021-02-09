<template>
	<view>
		<!-- 头部 -->
		<view class="flex align-center p-3 border-bottom">
			<image src="/static/img/userpic/16.jpg" style="width: 180rpx; height: 180rpx;" class="rounded-circle"></image>
			<view class="pl-3 flex flex-column flex-1">
				<view class="flex align-center">
					<view class="flex-1 flex flex-column align-center justify-center">
						<text class="font-lg font-weight-bold">2</text>
						<text class="font text-muted">获赞</text>
					</view>
					<view class="flex-1 flex flex-column align-center justify-center">
						<text class="font-lg font-weight-bold">1</text>
						<text class="font text-muted">关注</text>
					</view>
					<view class="flex-1 flex flex-column align-center justify-center">
						<text class="font-lg font-weight-bold">6</text>
						<text class="font text-muted">粉丝</text>
					</view>
				</view>
				<view class="flex align-center justify-center">
					<button type="primary" size="mini" class="bg-main" style="width: 400rpx;">关注</button>
				</view>
			</view>
		</view>
		
		<!-- 选项卡 -->
		<view class="flex align-center" style="height: 100rpx;">
			<view class="flex-1 flex align-center justify-center" v-for="(item ,index) in tabBars" :key="'tab'+index" :class="index === tabIndex ? 'font-lg font-weight-bold text-main' : 'font-md'"
			 @click="changeTab(index)">{{item.name}}</view>
		</view>
		
		<template v-if="tabIndex === 0">
			<view class="animate__animated animate__fast animate__fadeIn">
				<view class="p-3 border-bottom">
					<view class="font-md">账号信息</view>
					<view class="font">账号年龄：3个月</view>
					<view class="font">账号id：1</view>
				</view>
				<view class="p-3 border-bottom">
					<view class="font-md">个人信息</view>
					<view class="font">星座：天蝎座</view>
					<view class="font">职业：IT</view>
					<view class="font">家乡：江苏苏州</view>
					<view class="font">情感：保密</view>
				</view>
			</view>
		</template>
		<template v-else>
			<view class="animate__animated animate__fast animate__fadeIn">
				<block v-for="(item, index) in list" :key="index">
					<common-list :item="item" :index="index" @follow="follow" @doSupport="doSupport"></common-list>
					<divider></divider>
				</block>
				<load-more :loadmore="loadmore"></load-more>
			</view>
		</template>
		
		<!-- 弹出层 -->
		<uni-popup ref="popup" type="top">
			<view class="popup-content flex align-center justify-center font-md border-bottom" hover-class="bg-light">
				<text class="iconfont icon-heimingdan mr-2"></text> 加入黑名单
			</view>
			<view class="popup-content flex align-center justify-center font-md" hover-class="bg-light">
				<text class="iconfont icon-conversation_icon mr-2"></text> 聊天
			</view>
		</uni-popup>
	</view>
</template>

<script>
	const test_data = [{
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
			share_count: 2,
			content: 'uni-app是DCloud官方推出的使用Vue.js开发跨平台应用的前端框架，一套代码可编译到iOS、Android、微信小程序等多个平台，学习和开发成本较低。在进行uni-app开发之前需要先搭建环境，下载并安装HBuilderX、微信开发者工具；新建项目时选择类型，创建之后会自动生成项目的默认目录，可以通过多种方式编译运行。一个典型的项目包括App.vue、main.js等文件和pages、static等目录；uni-app遵守Vue单文件组件规范，vue文件包括模板、脚本和样式3个顶级语言块。更多内容可点击https://blog.csdn.net/CUFEECR/article/details/111088889。',
			images: [
				{
					url: 'https://img-blog.csdnimg.cn/20210202134847418.png'
				},
				{
					url: 'https://img-blog.csdnimg.cn/20210202135211479.png'
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
	import uniPopup from '@/components/uni-ui/uni-popup/uni-popup.vue';
	export default {
		data() {
			return {
				tabIndex: 0,
				tabBars: [{
						name: '主页'
					},
					{
						name: '帖子',
						list: test_data,
						loadmore: '上拉加载更多'
					},
					{
						name: '动态',
						list: test_data,
						loadmore: '上拉加载更多'
					}
				],
			}
		},
		components: {
			commonList,
			loadMore,
			uniPopup
		},
		computed: {
			list() {
				return this.tabBars[this.tabIndex].list; 
			},
			loadmore() {
				return this.tabBars[this.tabIndex].loadmore; 
			}
		},
		onNavigationBarButtonTap() {
			this.$refs.popup.open();
		},
		methods: {
			changeTab(index) {
				this.tabIndex = index;
			},
			// 关注
			follow(e) {
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
				console.log(0);
				// 之前未顶踩过
				if (item.support.type === '') {
					console.log(1);
					item.support[e.type + '_count']++;
				}
				// 之前已顶过并且现在的操作为踩，则顶-1、踩+1
				else if (item.support.type === 'support' && e.type === 'unsupport') {
					
					console.log(2);
					item.support.support_count--;
					item.support.unsupport_count++;
				}
				// 之前已踩过并且现在的操作为顶，则踩-1、顶+1
				else if (item.support.type === 'unsupport' && e.type === 'support') {
					console.log(3);
					item.support.unsupport_count--;
					item.support.support_count++;
				}
				item.support.type = e.type;
				uni.showToast({
					title: msg + '成功'
				})
			},
		}
	}
</script>

<style>

</style>

