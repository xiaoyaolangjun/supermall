<template>
  <div class="recommend">
    <div v-for="(item,index) in recommends" :key="index" class="recommend-item">
      <a :href="item.link">
        <img :src="item.image" alt="">
        <div>{{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
  export default{
    name: "RecommendView",
    props: {
      recommends:{
        type: Array,
        default(){
          return []
        }
      }
    }
  }
</script>

<style scoped>
  .recommend {
    display: flex;
    width: 100%;
    text-align: center;
    border-bottom: 10px solid rgba(139,101,8,.2);
  }
  .recommend-item {
    flex: 1;
    font-size: 16px;
  }
  .recommend-item img {
    width: 90%;
    margin-bottom: 4px;
  }
  .recommend-item div {
    margin-bottom: 3px;
    color: rgba(255, 106, 106);
  }

</style>