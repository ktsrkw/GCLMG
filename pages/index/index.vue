<template>
	<view class="content">
		<image class="logo" src="/static/logo2.jpg" mode="widthFix"></image>
		<view class="text-area">
			<navigator url="../game/game" class="bigen">开始游戏</navigator>
			<navigator url="../gameRule/gameRule" class="ruler">游戏规则</navigator>
		</view>
		
		<navigator url="../gameInfo/gameInfo" class="info">游戏详情></navigator>
	</view>
</template>

<script>


</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		width: 600rpx;
		margin-top: 60rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		flex-direction: column;
		justify-content: center;
	}
	.bigen{
		width: 200rpx;
		height: 100rpx;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: #FF2958;
		font-size: 40rpx;
		color: #FFFFFF;
	}
	.ruler{
		width: 200rpx;
		height: 100rpx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.info{
		position: absolute;
		top: 1100rpx;
		left: 540rpx;
		width: 200rpx;
		height: 100rpx;
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
