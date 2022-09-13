<template>
	<view class="my-container">
		<!-- 用户未登录时，显示登录组件 -->
		<my-login v-if="!token"></my-login>

		<!-- 用户登录后，显示用户信息组件 -->
		<my-userinfo v-else></my-userinfo>
	</view>
</template>

<script>
	// 导入自己封装的 mixin 模块
	import badgeMix from '@/mixins/tabbar-badge.js'
	// 1. 从 vuex 中按需导入 mapState 辅助函数
	import {
		mapState
	} from 'vuex'

	export default {
		// 将 badgeMix 混入到当前的页面中进行使用
		mixins: [badgeMix],
		data() {
			return {

			};
		},
		computed: {
			// 2. 从 m_user 模块中导入需要的 token 字符串
			...mapState('m_user', ['token']),
		},
	}
</script>

<style lang="scss">
	.my-userinfo-container {
		height: 100%;
		// 为整个组件的结构添加浅灰色的背景
		background-color: #f4f4f4;

		.top-box {
			height: 400rpx;
			background-color: #c00000;
			display: flex;
			flex-direction: column;
			align-items: center;
			justify-content: center;

			.avatar {
				display: block;
				width: 90px;
				height: 90px;
				border-radius: 45px;
				border: 2px solid white;
				box-shadow: 0 1px 5px black;
			}

			.nickname {
				color: white;
				font-weight: bold;
				font-size: 16px;
				margin-top: 10px;
			}
		}
	}

	page,
	.my-container {
		height: 100%;
	}
</style>
