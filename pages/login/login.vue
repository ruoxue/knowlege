<template>
	<view>
		<view class="content ">
			<!-- #ifdef MP-WEIXIN -->
			<view class="">微信登录</view>
			<view @click="login" class="cu-avatar lg text-center cuIcon-weixin round bg-gradual-green" style=" text-align: center; margin-top: 90%;"></view>

			<!-- #endif -->

			<!-- #ifndef MP-WEIXIN -->


			<form>
				<view class="cu-form-group margin-top">
					<view class="title">手机号</view>
					<input placeholder="手机号" v-model="loginData.phone" type="number" name="input"></input>
				</view>
				<view class="cu-form-group margin-top">
					<view class="title">密码</view>
					<input placeholder="密码" type="password" v-model="loginData.password" name="input"></input>
				</view>
			</form>


			<view style=" text-align: center;  ">
				<button @click="loginOther" class=" bg-red  margin-top-xl   text-center   radius  ">登录</button>

				<view @click="regedit" class=" text-blue  margin-top-xl  fr   radius margin-right-xs">去注册</view>
			</view>
			<!-- #endif -->
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				loginData: {}

			};
		},
		methods: {
			regedit() {
				uni.navigateTo({
					url: 'regedit'
				})
			},
			login() {
				var thus = this;
				uni.login({
					success: ret => {
					 
						thus.loginWx(ret.code);
					},
					fail: ret => {
						uni.showToast({
							title: ret.code
						});
					},
					complete: ret => {}
				});
			},
			loginWx(code) {
				var thus=this;

				uni.login({
					provider: 'weixin',
					success(res) {
						console.log(res);
						 
						
						 uni.getUserInfo({
						  
							success(info) {
								 
								
								var param={
									 code: res.code,
									user_info: info.rawData,
									encrypted_data: info.encryptedData,
									iv: info.iv,
									signature: info.signature
								}
								
							
								
								
								thus.$net.fetch(function(v) {
									uni.setStorageSync('info', JSON.stringify(v));
									uni.navigateBack({
										animationDuration:100
									})
									
								}, thus.$net.wxLogin,param,'post');
								 
								
								
								
								
							},
							fail(ret) {
						uni.showToast({
							icon:"none",
							title:"授权失败"
						})	;
						
						var param={
							 code: res.code,
							 user_info:JSON.stringify( {'nickName':'饭用户'}),
							 encrypted_data:{},
							 iv: '',
							 signature: ''
							 
						} 
						
						thus.$net.fetch(function(v) {
							uni.setStorageSync('info', JSON.stringify(v));
							uni.navigateBack({
								animationDuration:100
							})
							
						}, thus.$net.wxLogin,param,'post');
						
						
						
						
						
						
						
						
						
								
							}
						 })
							
						
						
						
						
						 
					},
					fail(info) {
						 
						 
					}
				})

				 

			
			},
			loginOther() {
				this.$net.fetch(function(v) {
					uni.setStorageSync('info', JSON.stringify(v));

					uni.navigateBack({
						animationDuration: 300
					})

				}, this.$net.loginin + '?phone=' + this.loginData.phone + "&password=" + this.loginData.password);
			}
		}
	};
</script>

<style></style>
