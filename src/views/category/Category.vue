<template>
  <div class="wrapper" ref="aaa">
    <div>
      <!-- 1.无论是否设置click:flase, button都可以点击 -->
      <button @click="btnClick1">按钮1</button>
      <!-- 2.必须设置click：true， 那么div才能监听点击 -->
      <div @click="divClick">哈哈哈哈</div>
    <ul class="content">
      <button @click="btnClick">按钮</button>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
      <li>shangpin</li>
    </ul>
    </div>
  </div>
</template>

<script>
import BScroll from "better-scroll";

export default {
  name: "Category",
  data() {
    return {
      scroll: null,
    };
  },
  mounted() {
    this.scroll = new BScroll(this.$refs.aaa, {
      probeType:3,
      pullUpLoad:true,
      click:true
    });
    this.scroll.on('scroll',(position)=>{
      console.log(position);
    })
    this.scroll.on('pullingUp',()=>{
      console.log('上拉加载更多');
    })
  },
  methods:{
      btnClick(){
       console.log('123');
      },
      btnClick1(){
        console.log('456');
      },
      divClick(){
        console.log('789');
      }
  }
};
</script>

<style scoped>
/* 原生滚动 */
/* .content{
  height: 100px;
  background-color: aqua;
  overflow-y:scroll ;
} */

.wrapper {
  height: 150px;
  background-color: aqua;
  overflow: hidden;
}
</style>
