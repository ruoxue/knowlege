<template>
	<view>
		<view class="content ">
			<!-- #ifdef MP-WEIXIN -->
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
						uni.showToast({
							title: ret.code
						});
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
								
								
								// {errMsg: "getUserInfo:ok", rawData: "{"nickName":"夏虫不可语冰²","gender":1,"language":"zh_CN…axCshP8RELz5PKowqpk20nib5opPI2HY5D3KRqMDtlQ/132"}", userInfo: {…}, signature: "1360b587ce3b16e80ba3259e66de244338c60d91", encryptedData: "clu0yMyKeOu0EY2TiM4dSUBGXQ08x7CCeXESAFvDOm4vmvjIS6…1mh60nAFa7D06nv4q/Inx8QSDTe4xMGdUjJi9sNwu4KS7/eJs", …}
								// encryptedData: "clu0yMyKeOu0EY2TiM4dSUBGXQ08x7CCeXESAFvDOm4vmvjIS64AI/jZVbx/5jID0MHUgrXhInKNcS5et0Bu55kPvJaHryK1c92TzBkE4Y9PkdrWTz9KOYDBaK3GYCR5agL5oouvoe8ApBHGpD9UIbWEC2/z1CeFaCjOXRargO/6FZh/BGF8NViQrH/a0KXQK5Qa8DsaknE43hqj+cj1dRJNmVatXoPR4yRfvby4eGRVlWClvxZhZkLB75WxFr5iv/+Ne6kfYpzGq940uxYfPxA/a7QxpNSFqqFz1KsXtoORN4mT7XwkgRzEXtZCOkTHrsPyiT5bX64pVwUxJbKKr5SYoV+7IdXp3K0qdV/dvTF4vjRhZhA5g33/nCvGwEnEeR4Xbg/0rhdxziRh/MfjlYxCAasH6mEKVQDnAUde/T7emMXbXmlQnn5Y8co5dW4Om9yjLAsPf0qKNTc1mh60nAFa7D06nv4q/Inx8QSDTe4xMGdUjJi9sNwu4KS7/eJs"
								// errMsg: "getUserInfo:ok"
								// iv: "1UfhvHIKjywYZWyDVDzd9Q=="
								// rawData: "{"nickName":"夏虫不可语冰²","gender":1,"language":"zh_CN","city":"Kasukabe-shi","province":"Saitama-ken","country":"JP","avatarUrl":"https://thirdwx.qlogo.cn/mmopen/vi_32/Q0j4TwGTfTIrpnDbfoibPIicwicaxtSqDr4fiasKf6Alaic4EiaxCshP8RELz5PKowqpk20nib5opPI2HY5D3KRqMDtlQ/132"}"
								// signature: "1360b587ce3b16e80ba3259e66de244338c60d91"
								// userInfo: {nickName: "夏虫不可语冰²", gender: 1, language: "zh_CN", city: "Kasukabe-shi", province: "Saitama-ken", …}
								console.log(info);
								// var userInfo=info.userInfo;
								
								var param={
									 code: res.code,
									user_info: info.rawData,
									encrypted_data: info.encryptedData,
									iv: info.iv,
									signature: info.signature
								}
								
							
								
								
								thus.$net.fetch(function(v) {
									uni.setStorageSync('info', JSON.stringify(v));
								}, thus.$net.wxLogin,param,'post');
								// thus.loadData();
								
								
								
								
							},
							fail() {
								
							}
						 })
							
						
						
						
						
						 
					},
					fail(info) {
						console.log(info);
						uni.showToast({
							icon: 'none',
							title: '授权失败，登录失败'
						})
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
