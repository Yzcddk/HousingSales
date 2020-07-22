<template>
	<view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		// 监听原生标题导航事件
		onNavigationBarButtonTap(e) {
			if(e.index == 0){
				uni.navigateBack({
					delta:1
				});
			}
		},
		// 监听搜索框文本变化
		onNavigationBarSearchInputChanged() {
			
		},
		// 监听点击搜索按钮事件
		onNavigationBarSearchInputConfirmed() {
			
		},
		methods: {
			
		}
	}
</script>

<style>

</style>
