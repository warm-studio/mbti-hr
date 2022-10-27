<template>
	<view class="main" :style="{ 'background': 'url(' + backgroundImg + ') no-repeat'}">
		<view class="container">
			<view class="title">16型人格</view>
			<view class="uni-padding-wrap button-sp-area">
				<button type="primary" @click="goto('/pages/mbti/index')">开始测试</button>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				backgroundImg:"./static/images/mbti.png"
			}
		},
		methods: {
			goto(url) {
				uni.navigateTo({
					url:url
				})
			}
		}
	}
</script>

<style lang="scss">
	/* 根元素样式 设置页面背景、字体大小、字体颜色，字符间距、长单词换行 */
	.main {
	  background-color: #fff !important;
	  font-size: 28rpx;
	  box-sizing: border-box;
	  color: #333;
	  letter-spacing: 0;
	  word-wrap: break-word;
		height: 100vh;
		width: 100%;
		background-size: contain !important;
	}
	
	.title {
		padding-top: 10vh;
		font-size: 84rpx;
		text-align: center;
		color: #fff;
		font-weight: bold;
	}
	
	.button-sp-area {
		padding-top: 30vh;
		margin: 0 auto;
		width: 60%;
	}
	
</style>