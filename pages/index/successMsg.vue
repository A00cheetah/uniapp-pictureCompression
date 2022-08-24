<template>
	<view style="background-color: #FFFFFF;background-position: center;position: relative;background-size: cover;">
		<view class="bg-white padding-lr" style="height: 1500upx;">
			<view class=" padding-xs flex align-center">
				<view class="flex-sub text-center">
					<view class="solid-bottom text-xsl padding">
						<text class=" cuIcon-roundcheckfill text-green"></text>
					</view>
					<h2 style="padding-bottom: 20upx;">操作成功</h2>
					<div style="width: 80%; margin: auto;">
						<button type="primary" @click="close">确定</button>
					</div>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		methods: {
			close() {
				uni.navigateBack();
				// uni.switchTab({
				// 	url: './companyList'
				// })
			}
		}
	}
</script>

<style>
</style>
