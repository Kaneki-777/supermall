<template>
  <div id="detail">
		<detail-nav-bar class="detail-nav" @titleClick="titleClick" ref="nav"></detail-nav-bar>

		<scroll class="content" ref="scroll"  :probe-type="3" @scroll="contentScroll">
		<detail-swiper :top-images="topImages"></detail-swiper>
		<detail-base-info :goods="goods"></detail-base-info>
		<detail-shop-info :shop="shop"></detail-shop-info>
		<detail-goods-info :detail-info="detailInfo" @imgLoad='imageLoad'></detail-goods-info>
		<detail-param-info ref="params" :param-info="paramInfo"></detail-param-info>
		<detail-comment-info ref="comment" :comment-info="commentInfo"></detail-comment-info>
		<goods-list ref="recommend" :goods='recommends'></goods-list>
		</scroll>

		<detail-bottom-bar></detail-bottom-bar>
		<back-top @click.native="backTop" v-show="isShowBackTop"></back-top>
	</div>
</template>

<script>
import DetailNavBar from './childComps/DetailNavBar.vue'
import DetailSwiper from './childComps/DetailSwiper.vue'
import DetailBaseInfo from './childComps/DetailBaseInfo.vue'
import DetailShopInfo from './childComps/DetailShopInfo.vue'
import DetailGoodsInfo from './childComps/detailGoodsInfo.vue'
import DetailParamInfo from './childComps/DetailParamInfo.vue'
import DetailCommentInfo from './childComps/DetailCommentInfo.vue'
import DetailBottomBar from './childComps/DetailBottomBar.vue'



import Scroll from 'components/common/scroll/Scroll'
import GoodsList from 'components/content/goods/GoodsList'

import {getDetail, Goods, Shop, GoodsParam, getRecommend, } from 'network/detail.js'
import {debounce} from 'components/common/utils'
import {itemListenerMixin, backTopMixin} from 'components/common/mixin.js'

  export default {
    name: "Detail",
		components:{
			DetailNavBar,
			DetailSwiper,
			DetailBaseInfo,
			DetailShopInfo,
			Scroll,
			DetailGoodsInfo,
			DetailParamInfo,
			DetailCommentInfo,
			GoodsList,
			DetailBottomBar,	

		},
		mixins:[itemListenerMixin, backTopMixin ],
    data(){
			return{
				iid:null,
				topImages:[],
				goods:{},
				shop:{},
				detailInfo:{},
				paramInfo:{},
				commentInfo:{},
				recommends:[],
				themeTopYs:[],
				currentIndex:0,


			}
		},
		created(){
			// 1.保存传入的iid
			this.iid = this.$route.params.iid

			// 2.根据iid请求详情数据
			 getDetail(this.iid).then(res =>{
				// 1.获取顶部的图片轮播数据
				// console.log(res);
				const data = res.result
				this.topImages = res.result.itemInfo.topImages

				// 2.获取商品信息
				this.goods = new Goods(res.result.itemInfo, res.result.columns,  res.result.shopInfo.services) 

				// 3.获取商家信息
				this.shop = new Shop(res.result.shopInfo)

				// 4.保存商品的详情数据
				this.detailInfo = data.detailInfo

				// 5.保存参数的数据
				this.paramInfo = new GoodsParam(data.itemParams.info , data.itemParams.rule)

				// 6.取出评论信息
				if(data.rate.cRate !== 0){
					this.commentInfo = data.rate.list[0]
				}
			 })

			// 3.请求推荐数据
			getRecommend().then(res =>{
				// console.log(res);
				this.recommends = res.data.list
			})	

			this.getThemeTopY = debounce(()=>{
				this.themeTopYs = []

				this.themeTopYs.push(0)
				this.themeTopYs.push(this.$refs.params.$el.offsetTop)
				this.themeTopYs.push(this.$refs.comment.$el.offsetTop)
				this.themeTopYs.push(this.$refs.recommend.$el.offsetTop)
				this.themeTopYs.push(Number.MAX_VALUE)
			})
	},

		mounted(){
			// console.log('mounted');
		},

		updated(){
},

		destroyed() {
				this.$bus.$off('itemImgLoad', this.itemImgListener)
		},
		
		methods:{
			imageLoad(){
				this.$refs.scroll.refresh()
				this.getThemeTopY()
			},
			titleClick(index){
				// console.log(index);
				this.$refs.scroll.scrollTo(0, -this.themeTopYs[index], 100)
			},
			contentScroll(position){
				// 1.获取y值
				const positionY = -position.y
				// 2.positionY和主题里的值进行对不
				let length = this.themeTopYs.length
				for(let i = 0 ; i < length-1; i++){
					// 条件1 ： 防止赋值的过于频繁
					// if(this.currentIndex !== i && ((i < length - 1 && positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i+1]) ||(i === length - 1 && positionY >= this.themeTopYs[i]))){
					// 	this.currentIndex = i
					// 	this.$refs.nav.currentIndex = this.currentIndex
					// }
					if(this.currentIndex !== i && (positionY >= this.themeTopYs[i] && positionY < this.themeTopYs[i+1])){
						this.currentIndex = i
						this.$refs.nav.currentIndex = this.currentIndex


					}
				}
				// 3.是否显示回到顶部
				this.isShowBackTop = (-position.y) > 1000
			},

		}
	}
</script>

<style scoped>
	#detail{
		position: relative;
		z-index: 9;
		background-color: #fff;
		height: 100vh;
	}

	.detail-nav{
		position: relative;
		z-index: 9;
		background-color: #fff;
	}

	.content{
		height: calc(100% - 44px);
	}
</style>