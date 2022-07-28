<template>
	<view>
		<view class="scroll-view">
			<scroll-view class="left" scroll-y="true" :style="{height:wh+'px'}">
				<block v-for="(item,i) in cateList" :key="item.cat_id">
					<view :class="['leftitem',i===active?'active':'']" @click="activechange(i)">{{item.cat_name}}</view>
				</block>
			</scroll-view>
			<scroll-view class="right" scroll-y="true" :style="{height:wh+'px'}">
				<view></view>
			</scroll-view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				wh: 0,
				active: 0,
				cateList: [{
					"cat_id": 1,
					"cat_name": "大家电",
					"cat_pid": 0,
					"cat_level": 0,
					"cat_deleted": false,
					"cat_icon": "",
					"children": [{
						"cat_id": 3,
						"cat_name": "电视",
						"cat_pid": 1,
						"cat_level": 1,
						"cat_deleted": false,
						"cat_icon": "",
						"children": [{
							"cat_id": 5,
							"cat_name": "曲面电视",
							"cat_pid": 3,
							"cat_level": 2,
							"cat_deleted": false,
							"cat_icon": "https://api-ugo-web.itheima.net/full/2fb113b32f7a2b161f5ee4096c319afedc3fd5a1.jpg"
						}]
					}]
				},{
					"cat_id":2,
					"cat_name": "大家电1",
					"cat_pid": 0,
					"cat_level": 0,
					"cat_deleted": false,
					"cat_icon": "",
					"children":[]
				},{
					"cat_id":3,
					"cat_name": "大家电2",
					"cat_pid": 0,
					"cat_level": 0,
					"cat_deleted": false,
					"cat_icon": "",
					"children":[]
				}],
				catelv2:[]
			};
		},
		onLoad() {
			const info = uni.getSystemInfoSync()
			console.log(info)
			this.wh = info.windowHeight
			this.getcateList()
		},
		methods: {
			async getcateList() {
				const res = await uni.$http.get('/api/public/v1/categories')
				console.log(res)
				// if (res.data.meta.status !== 200)
					// return uni.$showmsg()
				// this.cateList = res.data.message
				this.catelv2=this.cateList[0].children
			},
			activechange(i){
				this.active=i,
				this.catelv2=this.cateList[i].children
			}
		}
	}
</script>

<style lang="scss">
	.scroll-view {
		display: flex;
	}

	.left {
		width: 120px;
	}

	.leftitem {
		background-color: #f7f7f7;
		line-height: 60px;
		text-align: center;
		font-size: 12px;

		&.active {
			background-color: #fff;
			position: relative;

			&::before {
				content: "";
				display: block;
				width: 3px;
				height: 30px;
				background-color: red;
				position: absolute;
				top: 50%;
				left: 0;
				transform: translateY(-50%);
			}
		}
	}
</style>
