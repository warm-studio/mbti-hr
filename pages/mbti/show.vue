<template>
	<view>
		<view class="mbti">{{type}}</view>
		<uni-card :is-shadow="false">
			<text class="uni-body desc">{{desc}}</text>
		</uni-card>
	</view>
</template>

<script>
	import {infoList} from './data/mbti.js';
	export default {
		onLoad: function (option) { 
			this.type = option.type; 
			this.name = option.name; 
			this.desc = infoList.find(item => item.name == this.type).desc
		},
		data() {
			return {
				type: '',
				name: '',
				desc: ''
			}
		},
		methods: {
			
		}
	}
</script>

<style>

.mbti {
	margin-top: 20rpx;
	font-size: 80rpx;
	text-align: center;
	padding-bottom: 20rpx;
}

.desc {
	text-indent: 2em
}
</style>
