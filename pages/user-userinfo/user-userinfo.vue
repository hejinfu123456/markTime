<template>
	<view>
		<uni-list-item showIcon clickable :icon="iconType" @click="changeUserPic">
			<text slot="body" class="slot-box slot-text">头像</text>
			<image slot="footer" :src="userpic" style="width: 100rpx; height: 100rpx;" class="rounded-circle"></image>
		</uni-list-item>
		<uni-list-item title="昵称" showIcon clickable :icon="iconType">
			<input slot="footer" type="text" v-model="username" class="edit-input text-right font" />
		</uni-list-item>
		<uni-list-item title="性别" showIcon clickable :icon="iconType" @click="changeSex">
			<text slot="footer" class="text-right font">{{sexText}}</text>
		</uni-list-item>
		<uni-list-item title="生日" showIcon clickable :icon="iconType">
			<view slot="footer">
				<picker mode="date" :value="birthday" @change="onDateChange">
					<text class="text-right font">{{birthday}}</text>
				</picker>
			</view>
		</uni-list-item>
		<uni-list-item title="情感" showIcon clickable :icon="iconType" @click="changeAffection">
			<text slot="footer" class="text-right font">{{affectionText}}</text>
		</uni-list-item>
		<uni-list-item title="职业" showIcon clickable :icon="iconType" @click="changeJob">
			<text slot="footer" class="text-right font">{{job}}</text>
		</uni-list-item>
		<uni-list-item title="家乡" showIcon clickable :icon="iconType" @click="showCityPicker">
			<text slot="footer" class="text-right font">{{cityText}}</text>
		</uni-list-item>
		<view class="py-2 py-3">
			<button class="bg-main text-white" style="border-radius: 50rpx; border: 0;" type="primary">完成</button>
		</view>
		<!-- 城市选择器 -->
		<mpvue-city-picker :themeColor="themeColor" ref="mpvueCityPicker" @onConfirm="onCityConfirm" :pickerValueDefault="cityPickerValueArray"></mpvue-city-picker>
	</view>
</template>

<script>
	const sexArray = ['保密', '男', '女'];
	const affectionArray = ['保密', '单身', '恋爱', '已婚'];
	import uniListItem from '@/components/uni-ui/uni-list-item/uni-list-item.vue';
	import mpvueCityPicker from '@/components/uni-ui/mpvue-citypicker/mpvueCityPicker.vue';
	export default {
		data() {
			return {
				iconType: 'compose',
				userpic: '/static/img/userpic/16.jpg',
				username: 'Corley',
				sex: 0,
				affection: 0,
				job: '保密',
				birthday: '2020-01-01',
				themeColor: '#007AFF',
				cityPickerValueArray: [0, 0, 1],
				cityText: '北京市市辖区西城区'
			}
		},
		components: {
			uniListItem,
			mpvueCityPicker
		},
		computed: {
			sexText() {
				return sexArray[this.sex];
			},
			affectionText() {
				return affectionArray[this.affection];
			}
		},
		// 监听返回
		onBackPress() {
			if (this.$refs.mpvueCityPicker.showPicker) {
				this.$refs.mpvueCityPicker.pickerCancel();
				return true;
			}
		},
		// 监听页面卸载
		onUnload() {
			if (this.$refs.mpvueCityPicker.showPicker) {
				this.$refs.mpvueCityPicker.pickerCancel()
			}
		},
		methods: {
			// 修改头像
			changeUserPic() {
				uni.chooseImage({
					count: 1,
					sizeType: ["compressed"],
					sourceType: ["album", "camera"],
					success: (res) => {
						console.log(res);
						this.userpic = res.tempFilePaths[0];
					}
				});
			},
			// 修改性别
			changeSex() {
				uni.showActionSheet({
					itemList: sexArray,
					success: res => {
						console.log(res);
						this.sex = res.tapIndex;
					}
				});
			},
			// 修改情感
			changeAffection() {
				uni.showActionSheet({
					itemList: affectionArray,
					success: res => {
						console.log(res);
						this.affection = res.tapIndex;
					}
				});
			},
			// 修改职业
			changeJob() {
				let jobArray = ['保密', 'IT', '金融', '教育', '其他']
				uni.showActionSheet({
					itemList: jobArray,
					success: res => {
						console.log(res);
						this.job = jobArray[res.tapIndex];
					}
				});
			},
			// 修改生日
			onDateChange(e) {
				console.log(e);
				this.birthday = e.detail.value;
			},
			// 三级联动修改城市
			onCityConfirm(e) {
				this.cityText = e.label.replace(/-/g, '');
			},
			showCityPicker() {
				this.$refs.mpvueCityPicker.show();
			}
		}
	}
</script>

<style>
	.slot-box {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		align-items: center;
	}

	.slot-image {
		/* #ifndef APP-NVUE */
		display: block;
		/* #endif */
		margin-right: 10px;
		width: 30px;
		height: 30px;
	}

	.slot-text {
		flex: 1;
		font-size: 14px;
		color: #3b4144;
		overflow: hidden;
		margin-right: 10px;
	}

	.edit-input {
		background: #FFF;
	}
</style>

