<template>
	<view>
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" circular="true">
			<swiper-item v-for="(item,i) in swiperList" :key="item.goods_id">
				<navigator :url="'/subpackge/little/little?goods_id='+item.goods_id" class="swiper-item">

					<img :src="item.image_src" alt="">
				</navigator>
				<!-- <text>{item.goods_id}</text> -->
			</swiper-item>
		</swiper>
		<view class="nav">
			<view v-for="(item,i) in navList" :key="i" class="navitem" @click="navclick(item)">
				<img :src="item.image_src" alt="">
			</view>
		</view>
		<view class="floorlist">
			<view class="flooritem" v-for="(item,i) in floorList" :key="i">
				<img :src="item.floor_title.image_src" alt="" class="floortitle">
				<view class="imgbox">
					<navigator class="left" :url="item.product_list[0].url">
						<img :src="item.product_list[0].image_src"
							:style="{width:item.product_list[0].image_width+'rpx'}" mode="widthFix"
							>
					</navigator>
					<view class="right">
						<navigator class="rightitem" v-for="(item1,i1) in item.product_list" :key="i1" v-if="i1!=0" :url="item1.url">
							<img :src="item1.image_src" :style="{width:item.product_list[0].image_width+'rpx'}"
								mode="widthFix">
						</navigator>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swiperList: [{
					"image_src": "https://api-ugo-web.itheima.net/pyg/banner1.png",
					"open_type": "navigate",
					"goods_id": 129,
					"navigator_url": "/pages/goods_detail/index?goods_id=129"
				}, {
					"image_src": "https://api-ugo-web.itheima.net/pyg/banner2.png",
					"open_type": "navigate",
					"goods_id": 395,
					"navigator_url": "/pages/goods_detail/index?goods_id=395"
				}, {
					"image_src": "https://api-ugo-web.itheima.net/pyg/banner3.png",
					"open_type": "navigate",
					"goods_id": 38,
					"navigator_url": "/pages/goods_detail/index?goods_id=38"
				}],
				navList: [],
				floorList: []
			};
		},
		onLoad() {
			this.getswiperList(),
				this.getnavList(), this.getfloorList()
		},
		methods: {
			async getswiperList() {
				const res = await uni.$http.get('/api/public/vi/home/swiperdata')
				console.log(res)
				if (res.data.meta.status !== 200)
					return uni.$showmsg()
				this.swiperList = res.data.message

			},
			async getnavList() {
				const res = await uni.$http.get('/api/public/v1/home/catitems')
				console.log(res)
				if (res.data.meta.status !== 200)
					return uni.$showmsg()
				this.navList = res.data.message
				uni.$showmsg('成功')
			},
			navclick(item) {
				if (item.name == "分类") {
					uni.switchTab({
						url: '/pages/cate/cate'
					})
				}
			},
			async getfloorList() {
				const res = await uni.$http.get('/api/public/v1/home/floordata')
				console.log(res)
				if (res.data.meta.status !== 200)
					return uni.$showmsg()
					res.data.message.forEach(floor=>{
						floor.product_list.forEach(prod=>{
							prod.url ='/subpackge/goodslist/goodslist?'+prod.navigator_url.split("?")[1]
						})
					})
				this.floorList = res.data.message
				uni.$showmsg('成功')
			},
			goodsclick(item) {
				console.log(2112)
				uni.switchTab({
					url: '.../../subpackge/little/little'
				})
			},
		}
	}
</script>

<style lang="scss">
	swiper {
		height: 330rpx;

		.swiper-item,
		image {
			height: 100%;
			width: 100%;
		}
	}

	.nav {
		display: flex;
		justify-content: space-around;
		margin: 15rpx 0;
		height: 150rpx;

		.navitem,
		image {
			width: 128rpx;
			height: 100%;
		}

	}

	.floortitle {
		height: 60rpx;
		width: 100%;
		display: flex;
	}

	.imgbox {
		display: flex;
		padding-left: 10rpx;
	}

	.right {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}
</style>
