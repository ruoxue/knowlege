<template>
	<view>

		<canvas canvas-id="canvas" id="canvas"></canvas>
		<button type="default" class="cu-btn" @click="crateCanve">new</button>
		<button type="default" class="cu-btn" @click="saveCanve">save</button>
		<button type="default" class="cu-btn" @click="img">img</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				ctx: {},
			}
		},
		methods: {

			crateCanve() {

				var ctx = uni.createCanvasContext('canvas');
				this.ctx = ctx;

				ctx.setFillStyle('white');

				ctx.fillText("123", 40, 40);
				ctx.fillRect(10, 10, 100, 100)


				ctx.draw();

			},
			img() {
				var thus = this;
				uni.chooseImage({
					success: function(res) {
						
						console.log(res.tempFilePaths[0]);
						
						thus.ctx.drawImage(res.tempFilePaths[0], 0, 0, 150, 100)
						thus.ctx.draw()
					}
				})

			},
			saveCanve() {
				uni.canvasToTempFilePath({
					x: 100,
					y: 200,
					width: 50,
					height: 50,
					destWidth: 100,
					destHeight: 100,
					canvasId: 'canvas',
					success: function(res) {
						// 在H5平台下，tempFilePath 为 base64
						console.log(res.tempFilePath)
					}
				})
			}
		}
	}
</script>

<style>

</style>
