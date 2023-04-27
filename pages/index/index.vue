<template>
	<view class="content">
		<!-- <image class="logo" src="/static/logo.png"></image> -->
		<view class="text-area">
			<!-- <text class="title">{{title}}</text> -->
			<scan-code></scan-code>
			<view>{{code}}</view>
		</view>
		<!-- <web-view src="../../test_html/html/test.html"></web-view> -->

		<!-- 弹出选择菜单 -->
<!-- 		<view class="cu-item arrow animation-slide-bottom" :style="[{animationDelay: '0.7s'}]"   @click="open()"  >
			<view class="content" >
			    <text class="cuIcon-exit text-cyan"></text>
				<text class="text-grey">退出</text>
			</view>
		</view>	 -->
		
		<web-view :src="url" fullscreen="false" update-title="false"></web-view>
		<!-- <web-view src="http://192.168.50.97/oa2/cad/terminal_scanner.php"></web-view> -->
			
<!-- 		<nav>
			<ul>
				<li>首页</li>
				<li>刷新</li>
				<li>设置页面</li>
			</ul>
		</nav> -->
		
<!-- 		<view :hidden="userFeedbackHidden" class="uni-popup-dialog">
			<view class="uni-dialog-content">
				<modal v-if="areaShow" title="设置页面" confirm-text="保存" cancel-text="取消" @cancel="cancelAdd" @confirm="confirmAdd">
					<input type="text" v-model="areaTxt" placeholder="输入网页地址" class="uni-dialog-input" />
				</modal>
			</view>
			<view class="uni-dialog-button-group">
				<view class="uni-dialog-button" @click="closeDialog">
					<text class="uni-dialog-button-text">关闭</text>
				</view>
				<view class="uni-dialog-button uni-border-left" @click="onOk">
					<text class="uni-dialog-button-text uni-button-color">确定</text>
				</view>
			</view>
		</view> -->

	</view>
</template>

<script>
	import scanCode from "../../scanCode.vue";
	export default {
		components: { scanCode },
		data() {
			return {
				title: 'Hello',
				code: "",
				str: "",
				wv: "",
				url: '',
				currentWebview: null,
				isEditStatus: true,
				areaTxt: '',
				feedbackContent: '',
			};
		},
		onBackPress() {
			return true
		},
		onUnload() {
			uni.$off('updata')
		},
		onLoad() {
			uni.$on('updata', (data) => {
				console.log(data)
				this.url = data
			})
		},
		mounted() {
			// wv = this.$scope.$getAppWebview().children()[0]
			// let info = {str: this.code}
			// wv.evalJS(`aaa(${JSON.stringify(info)})`)
		},
		onReady() {
			// var currentwebView = this.$scope.$getAppWebview()
			// wv = currentwebView.evaluateJavascript("javascript:aaa('123')");
			// const currentWebview = this.$parent.$scope.$getAppWebview().children()[0]
			// currentWebview.evalJS(`test(${123})`)
			// var height = -40;
			// this.str = this.code
			// const self = this
			// var wv = self.$scope.$getAppWebview().children()[0]
			// setTimeout(function() {
			// 	wv.setStyle({
			// 		top:0,
			// 		height: height,
			// 		scalable: false,
			// 	})
			// 	wv.evalJS(`aaa(${JSON.stringify(self.str)})`)
			// })
			
			// this.wv.evalJS(`aaa(${JSON.stringify(this.str)})`)
			
			uni.getStorage({
				key: 'id',
				success(res) {
					console.log(res.data)
					this.url = res.data
					console.log(this.url)
				}
			})
			
		},
		onShow() {
			const _this = this
			
			uni.$off('scancodedate')
			uni.$on('scancodedate', function(data){
				console.log('扫描数据： ', data.code)
				_this.code = data.code
				_this.test()
				// var that = this;
				// let currentWebview = getCurrentPages()[0];
				// that.wv = currentWebview.$getAppWebview();
				// currentwebView.evaluateJavascript("javascript:aaa('123')");
			})
			this.url = uni.getStorageSync('id');
			
		},
		methods: {
			// open(){
			// 	uni.showModal({
			// 	    title: '提示',
			// 	    content: '确定退出登录吗？',
			// 	    success: function (res) {
			// 	        if (res.confirm) {
			// 	            console.log('用户点击确定');
			// 				uni.navigateTo({
			// 					url: '/pages/common/exit'  //点击要跳转的页面路径
			// 				});
							
			// 	        } else if (res.cancel) {
			// 	            console.log('用户点击取消');
			// 	        }
			// 	    }.bind(this)//可处理uni.shuowModal的异步（即拿不到data里面数据的解决办法）
			// 	});
			// },
				
			test(){
				var wv = this.$scope.$getAppWebview().children()[0]
				wv.evalJS(`aaa(${JSON.stringify(this.code)})`)
			},
			
			openWindows() {
				const subNVue = uni.getSubNVueById('popup');
				subNVue.show()
			}
		},
		
		onNavigationBarButtonTap(e) {
			// let rightText = ''
			// if (this.isEditStatus) {
			// 	rightText = '菜单'
			// } else {
			// 	rightText = ''
			// }
			// this.isEditStatus = !this.isEditStatus
			// const currentWebview = this.$mp.page.$getAppWebview();
			// currentWebview.setTitleNViewButtonStyle(e.index, {
			// 	text:rightText
			// });
			if (e.index === 1) {
				console.log("你点击了刷新按钮")
				var wv = this.$scope.$getAppWebview().children()[0]
				wv.reload(true)
			} else {
				console.log("你点击了设置页面按钮")
				this.openWindows()
			}
		},
		
		
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
	
	.uni-popup-dialog {
		width: 300px;
		border-radius: 11px;
		border-style: double;
		background-color: #fff;
	}
	
	.uni-dialog-content {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		justify-content: center;
		align-items: center;
		padding: 20px;
	}
	
	.uni-dialog-content-text {
		font-size: 14px;
		color: #6C6C6C;
	}
	
	.uni-dialog-input {
		flex: 1;
		font-size: 14px;
		border: 1px #eee solid;
		height: 40px;
		padding: 0 10px;
		border-radius: 5px;
		color: #555;
	}
	
	.uni-dialog-button-group {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		flex-direction: row;
		border-top-color: #f5f5f5;
		border-top-style: solid;
		border-top-width: 1px;
	}
	
	.uni-dialog-button {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
	
		flex: 1;
		flex-direction: row;
		justify-content: center;
		align-items: center;
		height: 45px;
	}
	
	.uni-dialog-button-text {
		font-size: 16px;
		color: #333;
	}
	
	.uni-border-left {
		border-left-color: #f0f0f0;
		border-left-style: solid;
		border-left-width: 1px;
	}
	
	.uni-button-color {
		color: #007aff;
	}
</style>
