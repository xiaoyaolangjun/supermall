<template>
  <div class="goods-item">
    <img :src="goodsItem.show.img" alt="">
    <div class="goods-info">
      <p>{{goodsItem.title}}</p>
      <span class="price">{{goodsItem.price}}</span>
      <span class="collect">{{goodsItem.cfav}}</span>
    </div>
  </div>
</template>

<script>
  export default{
    name: "GoodsListItem",
    props: {
    	goodsItem: {
        type: Object,
        dafault() {
          return {}
        }
      }
    }
  }
</script>

<style scoped>
  .goods-item {
    position: relative;
    padding-top: 40px;
    margin-bottom: 10px;
    width: 48%;
    z-index: -1;
  }
  .goods-item img {
    width: 100%;
    border-radius: 5px;
  }
  .goods-info {
    position: absolute;
    left: 0;
    right: 0;
    bottom: -35px;
    text-align: center;
    overflow: hidden;
    font-size: 12px;
  }
  .goods-info p {
    margin-top: 3px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;    
  }
  .goods-info .price {
    margin-right: 20px;
    color: var(--color-high-text);
  }
  .goods-info .collect {
    position: relative;
  }
  .goods-info .collect::before {
    position: absolute;
    width: 14px;
    height: 14px;
    left: -15px;
    top: -1px;
    content: '';
    background: url("~assets/img/common/collect.svg") 0 0/14px 14px;
  }

</style>