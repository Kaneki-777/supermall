<template>
	<div class="base-info" v-if="Object.keys(goods).length !== 0">
		<div class="info-title">{{goods.title}} </div>
		<div class="info-price">
			<span class="n-price">{{goods.newPrice}}</span>
			<span class="o-price">{{goods.oldPrice}}</span>
			<span v-if="goods.discount" class="discount">{{goods.discount}}</span>
		</div>
		<div class="info-other">
			<span>{{goods.columns[0]}} </span>
			<span>{{goods.columns[1]}} </span>
			<span>{{goods.services[goods.services.length-1].name}} </span>
		</div>
		<div class="info-service" v-if="goods.services">
			<span class="info-service-item" v-for="(item,index) in goods.services" :key="index">
				<img :src="item.icon" alt="">
				<span>{{item.name}} </span>
			</span>
		</div>
	</div>
</template>

<script>
export default {
	name:'DetailBaseInfo',
	props:{
		goods:{
			type:Object,
			default(){
				return {}
			}
		}
	}
}
</script>

<style scoped>
.base-info{
	width: 100%;
	padding:10px 10px 20px ;
	border-bottom: 4px solid rgba(100, 100, 100, 0.1);
}

.info-title{
	font-size: 16px;
	font-weight: bold;
}

.info-price{
	margin-top: 8px;
}

.info-price .n-price{
	color: var(--color-high-text);
	font-size: 20px;
	margin-right: 8px;
}

.info-price .o-price{
	font-size: 10px;
	color: var(--color-text);
	text-decoration: line-through;
	margin-right: 6px;
}

.info-price .discount{
	background: var(--color-tint);
	color: #fff;
	border-radius: 6px ;
	position: relative;
	top: -8px;
	font-weight: bold;
}

.info-other{
	margin-top: 12px;
	font-size: 14px;
	display: flex;
	justify-content: space-between;
	padding-bottom: 4px;
	color: var(--color-text);
	border-bottom: 2px solid rgba(100, 100, 100, 0.1);
}

.info-service{
margin-top: 18px;
display: flex;
justify-content: space-between;
font-size: 14px;
}

.info-service img {
    width: 12px;
    height: 12px;
}

</style>