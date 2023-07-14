<template class="warp">
	<view class="content">		
	<view class="title-area">
		<view class="title">车位信息</view>
	</view>
		
	<view class="car">
		
		<view class="garge" v-if = "Garge1 != 0">
			<image class="logo" src="/static/Car.png"></image>
			<view class="text-area">
				车位1
			</view>
		</view>
		
		<view class="garge" v-else>
			<image class="logo" src="/static/Free.png"></image>
			<view class="text-area">
				车位1
			</view>
		</view>
		
		
		<view class="garge" v-if = "Garge2 != 0">
			<image class="logo" src="/static/Car.png"></image>
			<view class="text-area">
				车位2
			</view>
		</view>
		
		<view class="garge" v-else>
			<image class="logo" src="/static/Free.png"></image>
			<view class="text-area">
				车位2
			</view>
		</view>
		
		<view class="garge">
			<image class="logo" src="/static/Free.png"></image>
			<view class="text-area">
				车位3
			</view>
		</view>
	</view>
	
	<view class="title-area">
		<view class="title">停车信息</view>
	</view>
	
	<view v-if = "Garge1 == 0&&Garge2 == 0" >
			<image class="empty" src="/static/empty.png"></image>
			<view class="empty-text">暂无数据</view>
	</view>
	
	<view class="top_view" v-if = "Garge1 == 1" >
		<view class="temp_comment">
			<view class="comment_title">
				<view class="avatar_content"><image class="avatar_img" src="/static/User.png" mode=""></image></view>
				<view class="title_wrapper">
					<text class="user_name">VIP1\n</text>
					<text class="comment_date">{{DataTime1}}</text>
				</view>
			</view>
			<view calss="">
				<text class="comment_content">车位：1\n</text>
				<text class="comment_content">停车时长：{{Time1}} s\n</text>
				<text class="comment_content">停车计费：{{Time1*2}} 元\n</text>
				<text class="comment_content">距离：{{Distance}} cm</text>
			</view>
		</view>
	</view>	

	
	<view class="top_view" v-if = "Garge1 == 2" >
		<view class="temp_comment">
			<view class="comment_title">
				<view class="avatar_content"><image class="avatar_img" src="/static/User.png" mode=""></image></view>
				<view class="title_wrapper">
					<text class="user_name">VIP2\n</text>
					<text class="comment_date">{{DataTime1}}</text>
				</view>
			</view>
			<view calss="">
				<text class="comment_content">车位：1\n</text>
				<text class="comment_content">停车时长：{{Time1}} s\n</text>
				<text class="comment_content">停车计费：{{Time1*2}} 元\n</text>
				<text class="comment_content">距离：{{Distance}} cm</text>
			</view>
		</view>
	</view>	
	
	<view class="top_view" v-if = "Garge2 == 1" >
		<view class="temp_comment">
			<view class="comment_title">
				<view class="avatar_content"><image class="avatar_img" src="/static/User.png" mode=""></image></view>
				<view class="title_wrapper">
					<text class="user_name">VIP1\n</text>
					<text class="comment_date">{{DataTime2}}</text>
				</view>
			</view>
			<view calss="">
				<text class="comment_content">车位：2\n</text>
				<text class="comment_content">停车时长：{{Time2}} s\n</text>
				<text class="comment_content">停车计费：{{Time2*2}} 元\n</text>
				<text class="comment_content">距离：{{Distance}} cm</text>
			</view>
		</view>
	</view>	
		
	
	<view class="top_view" v-if = "Garge2 == 2" >
		<view class="temp_comment">
			<view class="comment_title">
				<view class="avatar_content"><image class="avatar_img" src="/static/User.png" mode=""></image></view>
				<view class="title_wrapper">
					<text class="user_name">VIP2\n</text>
					<text class="comment_date">{{DataTime2}}</text>
				</view>
			</view>
			<view calss="">
				<text class="comment_content">车位：2\n</text>
				<text class="comment_content">停车时长：{{Time2}} s\n</text>
				<text class="comment_content">停车计费：{{Time2*2}} 元\n</text>
				<text class="comment_content">距离：{{Distance}} cm</text>
			</view>
		</view>
	</view>	
		
	</view>
</template>



<script>
	export default {
		data() {
			return {
				Garge1:0,
				Garge2:0,
				Time1:0,
				Time2:0,
				Distance:0,
				DataTime1:0,
				DataTime2:0,
				DataTime1Swi:false,
				DataTime2Swi:false,
			
			}
			
		},
		onShow() {
			let that = this
			this.GetDatapoints()
			setInterval(function() {
				that.GetDatapoints()
			}, 1000);
		},
		methods: {
			
		GetDatapoints() {
		uni.request({
			method: 'GET',
			url: 'http://api.heclouds.com/devices/1081152794/datapoints', //接口API。
			header: {
				'api-key': 'xrXOFe50Hmp4CakHLqZIjSfMcPQ=' //API KEY	
			},
			success: (res) => {
				this.Garge1 = res.data.data.datastreams[6].datapoints[0].value;
				this.Garge2 = res.data.data.datastreams[3].datapoints[0].value;
				this.Time1 = res.data.data.datastreams[1].datapoints[0].value;
				this.Time2 = res.data.data.datastreams[0].datapoints[0].value;
				this.Distance = res.data.data.datastreams[5].datapoints[0].value;
				if(this.Garge1 == 0)
				{
					this.DataTime1Swi = true;
				}
				if(this.Garge1!=0 && this.DataTime1Swi == true)
				{
					this.DataTime1Swi = false;
					this.DataTime1 = res.data.data.datastreams[6].datapoints[0].at.substring(0,19);
				}
				
				if(this.Garge2 == 0)
				{
					this.DataTime2Swi = true;
				}	
				if(this.Garge2!=0 && this.DataTime2Swi == true)
				{
					this.DataTime2 = res.data.data.datastreams[3].datapoints[0].at.substring(0,19);
					this.DataTime2Swi = false;					
				}

			}
		});
			
		},
		}
	}
</script>


<style lang="scss">
	.warp{
		background-color: #666;
	}
	.empty{
		height: 200rpx;
		width: 200rpx;
		margin-top: 120rpx;
		justify-content:center
	}
	.empty-text{
		text-align: center;
	}
	.car{
		display: flex;
	}
	.garge{
		border-left: 2rpx solid #E9E9E9;
		border-right: 2rpx solid #E9E9E9;
	}
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;

	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-left: 20rpx;
		margin-right: 20rpx;
		margin-bottom: 10rpx;
	}

	.text-area {
		display: flex;
		margin-left: 20rpx;
		justify-content: center;
		font-weight: bold;
		font-size: 40rpx;
	}

	.title-area
	{
		border-radius: 10rpx; //椭圆边框
		background-color: #186FED;
		width: 730rpx;
		height: 120rpx;
		margin-top: 30rpx;
		margin-bottom: 30rpx;
		box-shadow: 0 0 15rpx #8f8f94; //投影.
		
	}
	.title {
		text-align: center;
		margin-top: 20rpx;
		font-size: 55rpx;
		color: white;
		
	}
	
	.top_view {
		border-bottom: 1px solid #E9E9E9;
		border-top: 1px solid #E9E9E9;
		width: 750rpx;
		padding: 20rpx 32rpx;
	}
	
	
	.temp_comment {
		/* #ifndef APP-NVUE */
		display: flex;
		box-sizing: border-box;
		/* #endif */
		flex-direction: column;
		padding: 20rpx 0rpx;
	}
	
	.comment_title {
		/* #ifndef APP-NVUE */
		display: flex;
		box-sizing: border-box;
		/* #endif */
		flex-direction: row;
		align-items: stretch;
	}
	.avatar_img {
		height: 80rpx;
		width: 80rpx;
	}
	
	.avatar_content {
		/* #ifndef APP-NVUE */
		display: flex;
		box-sizing: border-box;
		/* #endif */
		flex-direction: row;
		align-items: center;
		margin-right: 16rpx;
		margin-bottom: 20rpx;
	}
	
	.title_wrapper {
		padding: 4rpx 0rpx;
	}
	
	.user_name {
		color: #333;
		font-weight: bold;
		font-size: 28rpx;
	}
	
	.comment_date {
		color: #999999;
		font-size: 24rpx;

	}
	.comment_content {
		color: #666;
		font-size: 26rpx;

	}

</style>
