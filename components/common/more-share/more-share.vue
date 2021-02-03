<template>
	<uni-popup ref="popup" type="bottom">
		<view class="popup-content text-center py-2 font-md border-bottom border-light-secondary">分享到</view>
		<view class="popup-content flex align-center">
			<block v-for="(item, index) in providerList" :key="index">
				<view class="flex-1 flex flex-column align-center justify-center py-2" hover-class="bg-light" @tap="share(item)">
					<view class="iconfont text-white font-lg rounded-circle flex align-center justify-center" :class="item.icon+' '+item.color"
					 style="width: 100rpx; height: 100rpx;"></view>
					<text class="font-sm mt-1 text-muted">{{item.name}}</text>
				</view>
			</block>
		</view>
		<view class="popup-content text-center py-2 font-md border-top border-light-secondary" hover-class="bg-light">取消</view>
	</uni-popup>
</template>

<script>
	import uniPopup from '@/components/uni-ui/uni-popup/uni-popup.vue';
	export default {
		data() {
			return {
				providerList: [],
				title: 'share',
				shareText: 'uni-app是DCloud官方推出的使用Vue.js开发跨平台应用的前端框架，一套代码可编译到iOS、Android、微信小程序等多个平台，学习和开发成本较低。在进行uni-app开发之前需要先搭建环境，下载并安装HBuilderX、微信开发者工具；新建项目时选择类型，创建之后会自动生成项目的默认目录，可以通过多种方式编译运行。一个典型的项目包括App.vue、main.js等文件和pages、static等目录；uni-app遵守Vue单文件组件规范，vue文件包括模板、脚本和样式3个顶级语言块。更多内容可点击https://blog.csdn.net/CUFEECR/article/details/111088889。',
				href: "https://blog.csdn.net/CUFEECR",
				image: 'https://img-blog.csdnimg.cn/20210202134847418.png',
				shareType: 1
			}
		},
		components: {
			uniPopup
		},
		computed: {
			isDisableButton() {
				return function(item) {
					if (this.shareType === 0 && item.id === 'qq') {
						return true;
					}
					if (this.shareType === 5 && item.name !== '分享到微信好友') {
						return true;
					}
					return false;
				}
			}
		},
		mounted() {
			uni.getProvider({
				service: 'share',
				success: (e) => {
					console.log('success', e);
					let data = []
					for (let i = 0; i < e.provider.length; i++) {
						switch (e.provider[i]) {
							case 'weixin':
								data.push({
									name: '微信好友',
									icon: 'icon-weixin',
									color: 'bg-green',
									id: 'weixin',
									sort: 0
								})
								data.push({
									name: '朋友圈',
									icon: 'icon-pengyouquan',
									color: 'bg-brown',
									id: 'weixin',
									type: 'WXSenceTimeline',
									sort: 1
								})
								break;
							case 'sinaweibo':
								data.push({
									name: '微博',
									icon: 'icon-weibo',
									color: 'bg-red',
									id: 'sinaweibo',
									sort: 2
								})
								break;
							case 'qq':
								data.push({
									name: 'QQ好友',
									icon: 'icon-qq',
									color: 'bg-blue',
									id: 'qq',
									sort: 3
								})
								break;
							default:
								break;
						}
					}
					this.providerList = data.sort((x, y) => {
						return x.sort - y.sort
					});
				},
				fail: (e) => {
					console.log('获取分享通道失败', e);
					uni.showModal({
						content: '获取分享通道失败',
						showCancel: false
					})
				}
			});
		},
		onShareAppMessage() {
			return {
				title: this.shareText ? this.shareText : "欢迎体验uni-app",
				path: '/pages/tabBar/component/component',
				imageUrl: this.image ? this.image : 'https://img-cdn-qiniu.dcloud.net.cn/uniapp/app/share-logo@3.png'
			}
		},
		beforeDestroy() {
			this.shareText = 'uni-app可以同时发布成原生App、小程序、H5，邀请你一起体验！',
				this.href = 'https://uniapp.dcloud.io',
				this.image = '';
		},
		methods: {
			open() {
				this.$refs.popup.open();
			},
			close() {
				this.$refs.popup.close();
			},
			async share(e) {
				console.log('分享通道:' + e.id + '； 分享类型:' + this.shareType);

				if (!this.shareText && (this.shareType === 1 || this.shareType === 0)) {
					uni.showModal({
						content: '分享内容不能为空',
						showCancel: false
					})
					return;
				}

				if (!this.image && (this.shareType === 2 || this.shareType === 0)) {
					uni.showModal({
						content: '分享图片不能为空',
						showCancel: false
					})
					return;
				}

				let shareOPtions = {
					provider: e.id,
					scene: e.type && e.type === 'WXSenceTimeline' ? 'WXSenceTimeline' : 'WXSceneSession', //WXSceneSession”分享到聊天界面，“WXSenceTimeline”分享到朋友圈，“WXSceneFavorite”分享到微信收藏     
					type: this.shareType,
					success: (e) => {
						console.log('success', e);
						uni.showModal({
							content: '已分享',
							showCancel: false
						})
					},
					fail: (e) => {
						console.log('fail', e)
						uni.showModal({
							content: e.errMsg,
							showCancel: false
						})
					},
					complete: function() {
						console.log('分享操作结束!')
					}
				}

				switch (this.shareType) {
					case 0:
						shareOPtions.summary = this.shareText;
						shareOPtions.imageUrl = this.image;
						shareOPtions.title = '欢迎体验uniapp';
						shareOPtions.href = 'https://uniapp.dcloud.io';
						break;
					case 1:
						shareOPtions.summary = this.shareText;
						break;
					case 2:
						shareOPtions.imageUrl = this.image;
						break;
					case 5:
						shareOPtions.imageUrl = this.image ? this.image :
							'https://img-cdn-qiniu.dcloud.net.cn/uniapp/app/share-logo@3.png'
						shareOPtions.title = '欢迎体验uniapp';
						shareOPtions.miniProgram = {
							id: 'gh_33446d7f7a26',
							path: '/pages/tabBar/component/component',
							webUrl: 'https://uniapp.dcloud.io',
							type: 0
						};
						break;
					default:
						break;
				}

				if (shareOPtions.type === 0 && plus.os.name === 'iOS') { //如果是图文分享，且是ios平台，则压缩图片 
					shareOPtions.imageUrl = await this.compress();
				}
				if (shareOPtions.type === 1 && shareOPtions.provider === 'qq') { //如果是分享文字到qq，则必须加上href和title
					shareOPtions.href = 'https://uniapp.dcloud.io';
					shareOPtions.title = '欢迎体验uniapp';
				}
				uni.share(shareOPtions);
			},
			compress() { //压缩图片 图文分享要求分享图片大小不能超过20Kb
				console.log('开始压缩');
				let img = this.image;
				return new Promise((res) => {
					var localPath = plus.io.convertAbsoluteFileSystem(img.replace('file://', ''));
					console.log('after' + localPath);
					// 压缩size
					plus.io.resolveLocalFileSystemURL(localPath, (entry) => {
						entry.file((file) => { // 可通过entry对象操作图片 
							console.log('getFile:' + JSON.stringify(file));
							if (file.size > 20480) { // 压缩后size 大于20Kb
								plus.zip.compressImage({
									src: img,
									dst: img.replace('.jpg', '2222.jpg').replace('.JPG', '2222.JPG'),
									width: '10%',
									height: '10%',
									quality: 1,
									overwrite: true
								}, (event) => {
									console.log('success zip****' + event.size);
									let newImg = img.replace('.jpg', '2222.jpg').replace('.JPG', '2222.JPG');
									res(newImg);
								}, function(error) {
									uni.showModal({
										content: '分享图片太大,需要请重新选择图片!',
										showCancel: false
									})
								});
							}
						});
					}, (e) => {
						console.log('Resolve file URL failed: ' + e.message);
						uni.showModal({
							content: '分享图片太大,需要请重新选择图片!',
							showCancel: false
						})
					});
				})
			}
		}
	}
</script>

<style>
</style>

