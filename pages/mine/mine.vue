<template>
	<view>
		<scroll-view  >
			<view>
				<view class="text-left " style="background-color: #f00;height: 220rpx;vertical-align: middle;">
					<view @click="login" v-if="info.avatarUrl == ''" class="cu-avatar cu-item round lg" style="background-image: url(../../static/img/tabBar/user_on.png);margin: 60rpx"></view>
				
				<view @click="login" v-if="info.avatarUrl != ''" class="cu-avatar cu-item round lg" 
				:style="'background-image: url('+info.avatarUrl+');margin: 60rpx'"></view>
				
					<text class="text-black" v-if="info.nickName == ''">{{ info.nickName }}</text>
					<text class="text-black" v-if="info.nickName != ''"> 饭用户</text>
				</view>
 

				<view class="cu-list menu margin-top-xl"> 
					  
				
				<!-- #ifdef MP-WEIXIN -->
				<view class="cu-item arrow">
					<button open-type="contact" class="content">
						<view class="content">
							<text class="cuIcon-community text-gray"></text>
							<text class="text-gray">联系客服</text>
						</view>
					</button>
				</view>
				
				<!-- #endif -->
					<view class="cu-item" @click="exit">
						<view>
							<view class="content">
								<text class="cuIcon-exit text-gray"></text>
								<text class="text-gray">退出</text>
							</view>
						</view>
					</view>
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			info: {
				headimg:'',
				nickname:'',
				username:''
			},
		};
	},
	onLoad() {
		this.load();
	},
	methods: {
		login() {
			uni.navigateTo({
				url: '../login/login'
			});
		},
		load() {
			var thus = this;
			this.$net.fetch(
				function(ret) {
					thus.info = ret.userInfo;
					uni.setStorageSync('info', JSON.stringify(thus.info));
				},
				this.$net.getUserInfo,
				{},
				'post'
			);
		},
		onPullDownRefresh() {
			 
			this.load();
		},
		exit() {
			uni.showModal({
				cancelText: '取消',
				confirmColor: '退出',
				content: '是否退出?',
				title: '提示',

				success(ret) {
					console.log(ret);
					if (ret.confirm) {
						uni.clearStorage();

						uni.navigateTo({
							url: '../login/login'
						});
					} else {
					}
				}
			});
		}
	}
};
</script>

<style>
.page {
	height: 100vh;
	width: 100vw;
}

.page.show {
	overflow: hidden;
}

.switch-sex::after {
	content: '\e716';
}

.switch-sex::before {
	content: '\e7a9';
}

.switch-music::after {
	content: '\e66a';
}

.switch-music::before {
	content: '\e6db';
}
</style>
