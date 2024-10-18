<template>
	<view class="pack">
		<view class="container1">
			<form @submit="formSubmit" @reset="formReset">
				<view class="uni-form-item uni-column">
					<view class="title">UI图片透明度选择</view>
					<slider max="1" min="0" step="0.1" name="opacity" :value="data.opacity" show-value></slider>
				</view>
				<view class="uni-form-item uni-column">
					<view class="title">请输入webview网页链接</view>
					<input class="uni-input" name="Baseurl" :value="data.Baseurl"/>
				</view>
				<view class="uni-form-item uni-column">
					<view class="title">请输入webview的宽度大小</view>
					<input class="uni-input" name="webViewWidth" :value="data.webViewWidth"/>
				</view>
				<view class="uni-form-item uni-column">
					<view class="title">请输入webview的高度大小</view>
					<input class="uni-input" name="webViewHeight" :value="data.webViewHeight"/>
				</view>
				<view class="uni-form-item uni-column">
					<view class="title">请输入UI图片宽度大小</view>
					<input class="uni-input" name="imageWidth" :value="data.imageWidth"/>
				</view>
				<view class="uni-form-item uni-column">
					<view class="title">请输入UI图片高度大小</view>
					<input class="uni-input" name="imageHeight" :value="data.imageHeight"/>
				</view>
				<view class="uni-form-item uni-column">
					<view class="title">请输入UI图片地址(建议CSDN或者gitee等图床图片)</view>
					<input class="uni-input" name="BaseImage" :value="data.BaseImage"/>
				</view>
				<view class="uni-btn-v" style="margin-top: 100px;">
					<button form-type="submit">保存配置信息</button>
					<button type="default" form-type="reset">清空表单数据</button>
					<button @click="clearlocal">清空配置信息</button>
				</view>
				<br />
				<view>注意: https协议下,webView指向的链接正常来说也是https协议的,为了兼容http协议的测试,可以参考这个方法 <text @click="qianwang" style="cursor: pointer;color: blue;">点击前往</text></view>
			</form>
		</view>
		<view class="container">
			<web-view :src="data.Baseurl"
				:style="{position:'absolute',left:'50%',transform: 'translate(-50%, 0%)',zIndex:'-2',width:data.webViewWidth,height:data.webViewHeight}"></web-view>
			<image class="imageStyle" :src="data.BaseImage"
				:style="{position:'absolute',left:'50%',transform: 'translate(-50%, 0%)',opacity:data.opacity,width:data.imageWidth,height:data.imageHeight}"
				mode=""></image>
		</view>
	</view>
</template>

<script setup>
	import {
		onMounted,
		ref
	} from 'vue';
	const search = ref(false)
	const data = ref({
		imageHeight: '',
		imageWidth: '',
		webViewHeight: '',
		webViewWidth: '',
		opacity: 0,
		Baseurl: '',
		BaseImage: ''
	})

	// 表单提交
	function formSubmit(e) {
		console.log('form发生了submit事件，携带数据为：' + JSON.stringify(e.detail.value))
		var formdata = e.detail.value
		localStorage.setItem('data', JSON.stringify(formdata))
		data.value = formdata
	}

	// 清空数据
	function formReset() {
		console.log('清空数据');
	}

	onMounted(() => {
		let res = localStorage.getItem('data')
		if (res) {
			data.value = JSON.parse(res)
		} else {
			data.value = {
				imageHeight: '932px',
				imageWidth: '430px',
				webViewHeight: '932px',
				webViewWidth: '430px',
				opacity: 0.8,
				Baseurl: 'https://xiaojiblog.netlify.app/project/database/0.%E6%95%B0%E6%8D%AE%E5%BA%93%E5%88%86%E7%B1%BB%E7%AE%80%E4%BB%8B.html',
				BaseImage: 'https://gitee.com/xiaojisengren/tuchuang/raw/master/uitest/example.png'
			}
		}
	})
	
	const qianwang = ()=>{
		location.href='https://jinweizhe.netlify.app/illustrate/'
	}

	const clearlocal = () => {
		data.value = {
			imageHeight: '932px',
			imageWidth: '430px',
			webViewHeight: '932px',
			webViewWidth: '430px',
			opacity: 0.8,
			Baseurl: 'https://xiaojiblog.netlify.app/project/database/0.%E6%95%B0%E6%8D%AE%E5%BA%93%E5%88%86%E7%B1%BB%E7%AE%80%E4%BB%8B.html',
			BaseImage: 'https://gitee.com/xiaojisengren/tuchuang/raw/master/uitest/example.png'
		}
		localStorage.clear()
	}
</script>

<style>
	.container1 {
		position: fixed;
		width: 30%;
		top: 50%;
		transform: translate(0%,-50%) !important;
	}

	iframe {
		position: absolute !important;
		left: 50% !important;
		transform: translate(-50%, 0%) !important;
	}

	.container {
		display: flex;
		width: 100vw;
		height: 100vh;
		flex-direction: column;
	}

	.imageStyle {
		pointer-events: none;
		position: relative;
		z-index: 10;
	}
	.title{
		font-weight: bold;
	}
</style>