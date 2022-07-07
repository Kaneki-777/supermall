<template>
	<div class="recommend">
		<div v-for="(item,id) in recommends" :key="id">
			<a :href="item.link">
				<img :src="item.image" alt="">
				<div>{{item.title}} </div>
			</a>
		</div>
	</div>
</template>

<script>

  export default {
    name: "RecommendView",
		props:{
			recommends:{
				type:Array,
				default(){
					return []
				}
			}
		}
  }
</script>

<style scoped>
	.recommend{
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		text-align: center;
		width: 100%;
		font-size: 12px;
		padding: 10px 0 20px;
		border-bottom: 10px solid #eee ;
	}

	.recommend img{
		width: 70px ;
		height: 70px;
		margin-bottom: 10px ;	
	}
</style>

