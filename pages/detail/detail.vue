<template>
	<view>
		<!-- 帖子详情页 -->
		<common-list :item="info" isdetail @doComment="doComment" @doShare="doShare" @follow="follow" @doSupport="doSupport">
			<view>{{info.content}}</view>
			<view class="">
				<block v-for="(item, index) in info.images">
					<image :src="item.url" class="w-100" mode="widthFix" @click="preview(index)"></image>
				</block>
			</view>
		</common-list>
		<!-- 占位 -->
		<divider></divider>
		<!-- 评论 -->
		<view class="p-2 font-md font-weight-bold">
		    最新评论 3
		</view>
		<view class="uni-comment-list px-2">
		    <view class="uni-comment-face">
		        <image src="../../static/img/userpic/12.jpg" mode="widthFix"></image>
		    </view>
		    <view class="uni-comment-body">
		        <view class="uni-comment-top"><text>Olivia</text>
		        </view>
		        <view class="uni-comment-content">大佬，写的不错</view>
		        <view class="uni-comment-date">
		            <view>2天前</view>
		        </view>
		    </view>
		</view>
		<uni-popup ref="popup" type="bottom">
		    <view class="popup-content text-center py-2 font-md border-bottom border-light-secondary">分享到</view>
		    <view class="popup-content flex align-center">
		        <block v-for="(item, index) in shareList" :key="index">
		            <view  class="flex-1 flex flex-column align-center justify-center py-2" hover-class="bg-light">
		                <view class="iconfont text-white font-lg rounded-circle flex align-center justify-center" :class="item.icon+' '+item.color" style="width: 100rpx; height: 100rpx;"></view>
		                <text class="font-sm mt-1 text-muted">{{item.name}}</text>
		            </view>
		        </block>
		    </view>
		    <view class="popup-content text-center py-2 font-md border-top border-light-secondary" hover-class="bg-light">取消</view>
		</uni-popup>
		<view style="height:100rpx;"></view>
		<bottom-input @submit="submit"></bottom-input>
		<more-share ref="share"></more-share>
		
	</view>
</template>

<script>
	import commonList from '@/components/common/common-list/common-list.vue';
	import bottomInput from '@/components/common/bottom-input/bottom-input.vue';
	import moreShare from '@/components/common/more-share/more-share';
	import uniPopup from '@/components/uni-ui/uni-popup/uni-popup.vue';
	export default {
		data() {
			return {
				// 当前帖子信息
				info: {},
				shareList: [
					{
						icon: 'icon-weixin',
						color: 'bg-green',
						name:'微信好友'
					},
					{
						icon: 'icon-pengyouquan',
						color: 'bg-brown',
						name:'朋友圈'
					},
					{
						icon: 'icon-weibo',
						color: 'bg-red',
						name:'微博'
					},
					{
						icon: 'icon-qq',
						color: 'bg-blue',
						name:'QQ好友'
					}
				]
			}
		},
		components: {
			commonList,
			bottomInput,
			moreShare,
			uniPopup
		},
		computed: {
			imagesList() {
				return this.info.images.map(item=>item.url);
			}
		},
		onLoad(e) {
			console.log(e);
			// 初始化操作
			if (e.detail) {
				this.__init(JSON.parse(e.detail));
			}
		},
		onNavigationBarButtonTap() {
			this.$refs.popup.open();
		},
		onBackPress() {
			this.$refs.popup.close();
		},
		methods: {
			__init(data) {
				// 修改标题
				uni.setNavigationBarTitle({
					title: data.title
				});
				// 请求API
				this.info = data;
			},
			// 关注
			follow() {
				this.info.isFollow = true;
				uni.showToast({
					title: '关注成功'
				});
			},
			// 顶踩操作
			doSupport(e) {
				let msg = e.type === 'support' ? '顶' : '踩';
				// 之前操作过
				if(this.info.support.type === e.type) {
					return uni.showToast({
						title: '您已经' + msg + '过了'
					});
				}
				// 之前未操作过
				if(this.info.support.type === '') {
					this.info.support[e.type + '_count']++;
				}
				// 之前已顶过并且现在的操作为踩，则顶-1、踩+1
				else if(this.info.support.type === 'support' && e.type === 'unsupport') {
					this.info.support.support_count--;
					this.info.support.unsupport_count++;
				}
				// 之前已踩过并且现在的操作为顶，则踩-1、顶+1
				else if(this.info.support.type === 'unsupport' && e.type === 'support') {
					this.info.support.unsupport_count--;
					this.info.support.support_count++;
				}
				this.info.support.type = e.type;
				uni.showToast({
					title: msg + '成功'
				});
			},
			// 点击评论
			doComment() {
				console.log('Commenting...');
			},
			// 点击分享
			doShare() {
				console.log('Sharing...');
			},
			// 预览图片
			preview(index) {
				uni.previewImage({
					urls: this.imagesList,
					current: index
				})
			},
			// 提交评论
			submit(data) {
				console.log(data);				
			}
		}
	}
</script>

<style>

</style>

