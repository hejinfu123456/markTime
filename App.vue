<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
			// 网络监听
			this.$U.onNetwork();
			// 检测更新
			this.$U.update();
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import url("./common/uni.css");
	/* 自定义图标库 */
	@import url('./common/icon.css');
	/* 动画库 */
	@import url("./common/animate.css");
	/* 自定义样式库 */
	@import url("./common/base.css");
	/* 全局样式 */
	@import url("./common/common.css");
	*{
		font-family:"Times New Roman",Times,serif;
		/* 2021020203002411 */
	}
</style>
