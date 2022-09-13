<template>
	<view>
		<!-- 使用自定义的搜索组件 -->
		<view class="search-box">
			<my-search @click="gotoSearch"></my-search>
		</view>
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" circular="true">
			<swiper-item v-for="(item,index) in swiperList" :key="index">
				<view class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
					<image :src="item" mode=""></image>
				</view>
			</swiper-item>
		</swiper>
		<!-- 分类导航区域 -->
		<view class="nav-list">
			<view class="nav-item" v-for="(item, i) in navList" :key="i" @click="navClickHandler(item)">
				<image :src="item.image_src" class="nav-img"></image>
			</view>
		</view>
		<!-- 楼层区域 -->
		<view class="floor-list">
			<!-- 楼层 item 项 -->
			<view class="floor-item" v-for="(item, i) in floorList" :key="i">
				<!-- 楼层标题 -->
				<image :src="item.floor_title.image_src" class="floor-title"></image>
				<!-- 楼层图片区域 -->
				<view class="floor-img-box">
					<!-- 左侧大图片的盒子 -->
					<navigator class="left-img-box" :url="item.product_list[0].url">
						<image :src="item.product_list[0].image_src" :style="{width:
				item.product_list[0].image_width + 'rpx'}" mode="widthFix"></image>
					</navigator>
					<!-- 右侧 4 个小图片的盒子 -->
					<view class="right-img-box">
						<navigator class="right-img-item" v-for="(item2, i2) in item.product_list" :key="i2"
							v-if="i2 !== 0" :url="item2.url">
							<image :src="item2.image_src" mode="widthFix" :style="{width:
				item2.image_width + 'rpx'}"></image>
						</navigator>
					</view>
				</view>

			</view>
		</view>

	</view>

</template>

<script>
	// 导入自己封装的 mixin 模块
	import badgeMix from '@/mixins/tabbar-badge.js'
	export default {
		// 将 badgeMix 混入到当前的页面中进行使用
		mixins: [badgeMix],
		data() {
			return {
				// 1. 轮播图的数据列表，默认为空数组
				swiperList: [
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/bqQfVz5yrrGYSXMvKr.cqdUy7f4vwyiXyVs0v9IX.m*uyh7W.Fa2ntG17caTjMTzORi*2qNZFb0VkkxnF8PuEbCflCN9ZANHd0qZ1UHa2D4!/b&bo=lQWAAgASEAgBCTY!&rf=viewer_4',
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/bqQfVz5yrrGYSXMvKr.cqVjG*jClP7x39KPZXBhzg0BvoYqSXUgNJ9D6C85icxU0Lsv9VgPOKiC4S4HrwGwvvau1ge*5dkSajioJJpRVz2I!/b&bo=iwWAAgAQOAcBCQ0!&rf=viewer_4',
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/bqQfVz5yrrGYSXMvKr.cqaLItO2ontU3H94X4VWzdj3sGHlkZgwzDcvBJpjYIRM2HyHrJ5CiQ.*Y8f95STFHRHg3IaCi6di.9Lc5hm4h3jk!/b&bo=lQWAAgAMYAUBCVU!&rf=viewer_4',
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/ruAMsa53pVQWN7FLK88i5mr8*hquCarAajogNiW*17M3KrK5ZiOdjd2XSkebuzl5bbbzkj6Q6zlsU0T7GwO7gna96NGUbolOS7i6.aYvufo!/b&bo=iwWAAoAZgAsBGSA!&rf=viewer_4',
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/bqQfVz5yrrGYSXMvKr.cqSYuVMTaa5si3HxKaNl0p2.jfBx1soCNbGDSSsE.hCf1TkyYlQqlMbOHcRYJRtO8aLL6u3Xplk*9AhpKhZ2E08s!/b&bo=iwWAAoAZgAsBCTA!&rf=viewer_4',
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/bqQfVz5yrrGYSXMvKr.cqfpxtnm5QR8pvqYMCxeNkEj5Jk7J2TUn.VYTeuQwF5B3eurF3uVvbXQwtkn*6jA3RoY4E3rpDT.J9xa3AyhLWrQ!/b&bo=AAXQAgAF0AIBCS4!&rf=viewer_4',
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/ruAMsa53pVQWN7FLK88i5rsyDZCsq51QJ7qbM3vFtxPiEW2dPLMPZNHIb4hrLnci5SHTrsj00bAeVE0yIG*Pd*Tsikg2bGkztzLqVXTl2.g!/b&bo=AAXQAgAF0AIBGT4!&rf=viewer_4',
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/ruAMsa53pVQWN7FLK88i5sXsTf4H5PkWnN8D1imhxPXfDj77JOxqg.dvXcSbajzbV5ouBFxzrp0LQGKDINVcmzkbfmsli*0pEam2DEsKBsY!/b&bo=AAXQAgAF0AIBGT4!&rf=viewer_4',
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/ruAMsa53pVQWN7FLK88i5guKMA.2RgIEfcsI4QK9Mut1XG4izUFvMk9XBmXwvH05P8nDiKYR3HIL4UM9rGBFBNte.gcDDWvFN.mZY3re.Pw!/b&bo=AAXQAgAF0AIBGT4!&rf=viewer_4',
					'http://photogz.photo.store.qq.com/psc?/V517nLzN1XnCpP1SuO063hd0HJ49fzCN/ruAMsa53pVQWN7FLK88i5h7xJEdBrPB3gDRDmd4lWU*UHTcihFP1rl0cSDXQUHc7LbiCpJf9ajT3rsjq0z1vIlIk9F3458fN5dlYM*eWps0!/b&bo=AAXQAgAF0AIBKQ4!&rf=viewer_4',
					'http://r.photo.store.qq.com/psc?/V13Q7hRv3w7Vkb/ruAMsa53pVQWN7FLK88i5p.S1mP2BJn4Mtwlbl6SNe071TNkdExEm9kJlqSriNKoEqIZmXwu95bynvYdTi83k.9vU5pie0J2Xho77M5B0VE!/r&ek=1&kp=1&pt=0&bo=UAY4BFAGOAQBGT4!&tl=3&vuin=487554983&tm=1662739200&dis_t=1662742269&dis_k=6aa915afc0d72ededf01e47eae9453ed&sce=60-0-0&rf=viewer_4',
					'http://r.photo.store.qq.com/psc?/V13Q7hRv3w7Vkb/ruAMsa53pVQWN7FLK88i5pM0xD6BaaCv*r1TFCQMSakHfss1jzCJBY9IjEq.eYWgmxNzDvQlFZiO*Rqy85GDftdSJmtNqsvG3zIeirv*MKY!/r&ek=1&kp=1&pt=0&bo=UAY4BFAGOAQBGT4!&tl=3&vuin=487554983&tm=1662739200&dis_t=1662742269&dis_k=08980b248cc8d21c9ccdfb911523aa37&sce=60-0-0&rf=viewer_4'
				],
				//分类导航的数据列表
				navList: [],
				// 1. 楼层的数据列表
				floorList: [],

			};
		},
		onLoad() {
			// 2. 在小程序页面刚加载的时候，调用获取轮播图数据的方法
			// this.getSwiperList(),
			// 2. 在 onLoad 中调用获取数据的方法
			this.getNavList()
			// 2. 在 onLoad 中调用获取楼层数据的方法
			this.getFloorList()
		},
		methods: {
			// 3. 获取轮播图数据的方法
			// async getSwiperList() {
			// 	// 3.1 发起请求
			// 	const {
			// 		data: res
			// 	} = await uni.$http.get('/api/public/v1/home/swiperdata')
			// 	// console.log(res);
			// 	// 3.2 请求失败
			// 	if (res.meta.status !== 200) {
			// 		return uni.showToast({
			// 			title: '数据请求失败！',
			// 			duration: 1500,
			// 			icon: 'none',
			// 		})
			// 	}
			// 	console.log(res);
			// 	// 3.3 请求成功，为 data 中的数据赋值
			// 	this.swiperList = res.message
			// },
			// 3. 在 methods 中定义获取数据的方法
			async getNavList() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/catitems')
				if (res.meta.status !== 200) return uni.$showMsg()
				this.navList = res.message
			},
			// nav-item 项被点击时候的事件处理函数
			navClickHandler(item) {
				// 判断点击的是哪个 nav
				if (item.name === '分类') {
					uni.switchTab({
						url: '/pages/cate/cate'
					})
				}
			},
			// 3. 定义获取楼层列表数据的方法
			async getFloorList() {
				const {
					data: res
				} = await uni.$http.get('/api/public/v1/home/floordata')
				if (res.meta.status !== 200) return uni.$showMsg()
				// 通过双层 forEach 循环，处理 URL 地址
				res.message.forEach(floor => {
					floor.product_list.forEach(prod => {
						prod.url = '/subpkg/goods_list/goods_list?' +
							prod.navigator_url.split('?')[1]
					})
				})
				this.floorList = res.message
			},
			gotoSearch() {
				uni.navigateTo({
					url: '/subpkg/search/search'
				})
			},
		},
	}
</script>

<style lang="scss">
	swiper {
		height: 400rpx;

		.swiper-item,
		image {
			width: 100%;
			height: 100%;
		}
	}

	.nav-list {
		display: flex;
		justify-content: space-around;
		margin: 15px 0;

		.nav-img {
			width: 128rpx;
			height: 140rpx;
		}
	}

	.floor-title {
		height: 60rpx;
		width: 100%;
		display: flex;
	}

	.right-img-box {
		display: flex;
		flex-wrap: wrap;
		justify-content: space-around;
	}

	.floor-img-box {
		display: flex;
		padding-left: 10rpx;
	}

	.search-box {
		// 设置定位效果为“吸顶”
		position: sticky;
		// 吸顶的“位置”
		top: 0;
		// 提高层级，防止被轮播图覆盖
		z-index: 999;
	}
</style>
