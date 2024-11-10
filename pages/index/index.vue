<template>
	<view class="mainBox">
		<view class="title mb4">需要替换的字符串：</view>
		<up-textarea 
			v-model="originText" 
			class="commonTextArea mb8" 
			style="width: 94.5%;" 
			placeholder="请输入内容" 
			maxlength='-1'
		>
		</up-textarea>
		<view class="otherBtnArea">
			<view class="otherBtn" @click="pasteText">
				粘贴
			</view>
		</view>
		<view class="formatArea mb4">
			<view class="flex1">
				<text>替换前字符：</text>
				<up-input
					placeholder="请输入内容"
					border="surround"
					v-model="charBefore"
					clearable
				></up-input>
			</view>
			<view style="margin-top: 36rpx;width: 60rpx;text-align: center;">-></view>
			<view class="flex1">
				<text>替换后字符：</text>
				<up-input
					placeholder="请输入内容"
					border="surround"
					v-model="charAfter"
					clearable
				></up-input>
			</view>
		</view>
		<view class="commonTip mb8">
			tip: 特殊字符也可替换，如输入 \n 为换行
		</view>
		<view class="title mb4">转换完成后的文本：</view>
		<up-textarea 
			v-model="resultText" 
			class="commonTextArea mb8" 
			style="width: 94.5%;" 
			placeholder="请输入内容"
			maxlength='-1'
		 ></up-textarea>
		<view class="otherBtnArea">
			<view class="otherBtn" @click="copyText">
				复制
			</view>
		</view>
		<up-button type="primary" text="转换" @click="transText"></up-button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				originText: '', // 源字符
				resultText: '', // 替换结果
				charBefore: '', // 规则 - 替换前字符
				charAfter: '', // 规则 - 替换后字符
			}
		},
		onLoad() {

		},
		methods: {
			/**
			 * 点击转换按钮的回调
			 */
			transText(){
				console.log(this.charBefore, this.charBefore == '\\n', this.charAfter);
				if(this.charBefore == '\\n') {
					this.resultText = this.originText.replace(/\n/g, this.charAfter);
				} else {
					this.resultText = this.originText.replaceAll(this.charBefore, this.charAfter)
					
				}
			},
			/**
			 * 点击粘贴按钮的回调
			 */
			pasteText(){
				uni.getClipboardData({
					success: (res) => {
						this.originText = res.data
						uni.showToast({
							title: '粘贴成功',
							icon: 'success'
						})
					},
					fail: (e) => {
						uni.showToast({
							title: '粘贴失败',
							icon: 'fail'
						})
					}
				})
			},
			/**
			 * 点击复制按钮的回调
			 */
			copyText(){
				uni.setClipboardData({
					data: this.resultText,
					showToast: true
					// success: () => {
					// 	uni.showToast({
					// 		title: '复制成功',
					// 		icon: 'success'
					// 	})
					// },
					// fail: (e) => {
					// 	uni.showToast({
					// 		title: '复制失败',
					// 		icon: 'fail'
					// 	})
					// }
				})
			},
		}
	}
</script>

<style lang="less">
	.content {
		display: flex;
		flex-direction: column;
		align-items: start;
		justify-content: center;
		margin: 0 10px;
	}
	.title {
		color: #333;
		font-weight: bold;
	}
	.commonTextArea {
		// min-height: 200px;
	}
	.formatArea {
		display: flex;
		align-items: center;
	}
	.otherBtnArea {
		width: 100%;
		position: relative;
		.otherBtn {
			position: absolute;
			left: 86%;
			bottom: 14px;
			background: #3c9cff;
			color: #fff;
			padding: 6px;
			width: 32px;
			border-radius: 50%;
			height: 32px;
			line-height: 32px;
		}
	}
	.u-textarea__field {
		height: unset !important;
	}
</style>
