<template>
	<view>
		<view class="mbti">MBTI测试</view>
		
		<view class="uni-padding-wrap uni-common-mt">
			<view class="progress-box">
				<progress :percent="percent" :activeColor="percent == 100 ? '#09BB07' : '#007aff'"/>
			</view>
			<view class="container main">
				<view>
					<view class="title question">{{ current.q || '您已完成测试，请输入姓名提交'}}</view>
					<uni-data-checkbox mode="list" selectedTextColor="#fff" v-model="currentAnswer" :localdata="current.a" @change="selectAnswer">
					</uni-data-checkbox>
				</view>
			</view>
			<view v-if="percent == 100" >
				<view class="uni-common-mt">
				<view class="uni-form-item uni-column">
					<view class="title">姓名</view>
					<input class="uni-input m-b" @blur="bindName" focus placeholder="请输入您的姓名" />
				</view>
				</view>
				<button class="m-b" type="primary" @click="getType">提交</button>
				<button class="mini-btn" size="mini" @click="reset">重新测试</button>
			</view>
		</view>
	</view>
	
</template>

<script>
	import {qnaList} from './data/mbti.js';
	export default {
		onLoad() {
			this.init();
		},
		data() {
			return {
				questionList: [],
				current: {},
				currentAnswer: '',
				answerList: [],
				percent: 0,
				name: ''
			}
		},
		methods: {
			init() {
				let app = this
				app.questionList = []
				app.current = {}
				app.currentAnswer = ''
				app.answerList = []
				app.percent = 0
				app.name = ''
				// 打乱题目顺序
				app.questionList = qnaList.sort(function () {
					return Math.random() - 0.5;
				})
				app.nextAnswer();
			},
			nextAnswer() {
				
				
				if (this.questionList.length == 0)
				{
					this.percent = 100
				} else {
					this.currentAnswer = ''
					this.percent = this.answerList.length * (100/72)
					this.current = this.questionList.shift();
				}
				
			},
			preAnswer() {
				this.currentAnswer = ''
				
				if (this.questionList.length == 0)
				{
					this.percent = 100
				} else {
					this.percent = this.answerList.length * (100/72)
					this.current = this.questionList.shift();
				}
				
			},
			selectAnswer() {
				let app = this
				app.current.s = app.currentAnswer
				if(app.percent != 100) {
					app.answerList.push(app.current)
				}
				setTimeout(function() {
					app.nextAnswer();
				}, 200);
			},
			getType () {
				if (this.name == '') {
					uni.showToast({
						icon: 'error',
						title: '请输入您的姓名',
						duration: 1000
					});
					return
				}
				const answerArray = this.answerList.map((item)=>{
					return item.s
				})
				const obj = {}
				const types = [['E', 'I'], ['S', 'N'], ['T', 'F'], ['J', 'P']]
				// 统计答案次数
				answerArray.forEach(item=>{
				    if( obj[item] ){
				        obj[item]++
				    }else{
				        obj[item] = 1
				    }
				})
				console.log(obj)
				let type = ''
				types.forEach(item=>{
					let a1 = obj[item[0]] || 0
					let a2 = obj[item[1]] || 0
				    if( a1 > a2 ){
				        type += item[0]
				    } else {
				        type += item[1]
				    }
				})
				uni.request({
					// 将结果提交oa系统
				    url: '',
					method: 'POST',
				    data: {
				        type: type,
						name: this.name
				    },
				    success: (res) => {
				        uni.navigateTo({
							url: 'show?type=' + type + '&name=' + this.name
						});
				    }
				});
				
			},
			bindName (event) {
				this.name = event.target.value;
			},
			reset() {
				uni.showModal({
					title: '重新测试？',
					content: '已有的测试结果将重置！',
					success: function (res) {
						if (res.confirm) {
							location.reload()
						}
					}
				});
				
			}
		}
	}
</script>

<style>
	
.progress-box {
	display: flex;
	height: 50rpx;
	margin-bottom: 60rpx;
}

.mbti {
	margin-top: 20rpx;
	font-size: 50rpx;
	text-align: center;
	padding-bottom: 20rpx;
}

.question {
	float: left;
	text-align: left;
	font-size: 40rpx;
	line-height: 50rpx;
	margin-bottom: 50rpx;
	width: 100%;
}

.main {
	margin-bottom: 80rpx;
}

>>>	.uni-data-checklist {
		flex: 0 1 auto;
	}
	
>>>	.uni-data-checklist .checklist-group .checklist-box{
		display: -webkit-flex;
		display: -ms-flexbox;
		display: flex;
		-webkit-flex-direction: column;
		-ms-flex-direction: column;
		flex-direction: column;
		-webkit-justify-content: center;
		-ms-flex-pack: center;
		justify-content: center;
		border-left: .17067rem solid #007AFF;
		background: -webkit-gradient(linear,right top,left top,color-stop(50%,white),color-stop(50%,#6c9afc),to(#007AFF)) right;
		background: -webkit-linear-gradient(right,white 50%,#6c9afc 50%,#007AFF) right;
		background: -o-linear-gradient(right,white 50%,#6c9afc 50%,#007AFF) right;
		background: linear-gradient(to left,white 50%,#6c9afc 50%,#007AFF) right;
		background-size: 200%;
		-webkit-box-shadow: 0 0 .21333rem #ddd;
		box-shadow: 0 0 .21333rem #ddd;
	}
>>> .uni-data-checklist .checklist-group .checklist-box.is--list {
		margin: 1.3rem 0;
		padding-right: 0 1.06667rem 0 0;
	}
	
>>>	.uni-data-checklist .checklist-group .checklist-box .checklist-content {
			width: 100%;
		}
>>>	.uni-data-checklist .checklist-group .checklist-box .checklist-content .checklist-text {
		font-size: 30rpx !important;
		color: #41464b ;
		margin-left: 10rpx !important;
		line-height: 30rpx !important;
		padding: 20rpx;
	}
	
>>>	.uni-data-checklist .checklist-group .checklist-box .radio__inner {
		display: none;
	}
	
>>>	.uni-data-checklist .checklist-group .is-checked {
		color: #fff;
		background-position: left;
		-webkit-box-shadow: var(--button-strong-box-shadow);
		box-shadow: var(--button-strong-box-shadow);
	}

	.m-b {
		margin-bottom: 40rpx;
	}

</style>
