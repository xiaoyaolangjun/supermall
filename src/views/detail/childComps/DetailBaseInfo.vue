<!-- 判断一个对象之中是否有数据可用方法“Object.keys(obj)” -->
<template>
  <div v-if="Object.keys(goods).length !== 0" class="base-info">
    <div class="info-title">{{goods.title}}</div>
    <div class="info-price">
    	<span class="n-price">{{goods.newPrice}}</span>
    	<span class="o-price">{{goods.oldPrice}}</span>
    	<span v-if="goods.discount" class="discount">{{goods.discount}}</span>
    </div>
    <div class="info-other">
    	<span>{{goods.columns[0]}}</span>
    	<span>{{goods.columns[1]}}</span>
    	<span>{{goods.services[goods.services.length-1].name}}</span>
    </div>
    <div class="info-service">
    	<span class="info-service-item" v-for="index in goods.services.length - 1" :key="index">
    		<img :src="goods.services[index-1].icon" alt="">
    		<span>{{goods.services[index-1].name}}</span>
    	</span>
    </div>
  </div>
</template>

<script>
  export default{
    name: "DetailBaseInfo",
    props: {
    	goods: {
        type:Object,
        default() {
          return {}
        }
      }
    }
  }
</script>

<style scoped>
  .base-info {
    margin-top: 15px;
    padding: 0 8px;
    border-bottom: 5px solid #f2f5f8;
  }
  .info-title {
    font-size: 20px;
    color: #222;
  }
  .info-price {
    margin-top: 10px;
  }
  .info-price .n-price {
    font-size: 24px;
    color: var(--color-high-text);
  }
  .info-price .o-price {
    margin-left: 5px;
    font-size: 13px;
    text-decoration: line-through;
  }
  .info-price .discount {
    position: relative;
    top: -8px;
    margin-left: 5px;
    padding: 2px 5px;
    border-radius: 8px;
    font-size: 12px;
    background: var(--color-high-text);
    color: #fff;
  }
  .info-other {
    display: flex;
    margin-top: 5px;
    border-bottom: 1px solid rgba(100, 100, 100, .1);
    line-height: 30px;
    font-size: 16px;
    justify-content: space-between;
  }
  .info-service {
    display: flex;
    line-height: 60px;
    justify-content: space-around;
  }
  .info-service-item img {
    position: relative;
    top: 2px;
    width: 14px;
    height: 14px;
    background: rgba(255, 250, 125);
    border-radius: 7px;
  }
  .info-service-item span {
    margin: 5px;
    font-size: 16px;
    color: #333;
  }
</style>