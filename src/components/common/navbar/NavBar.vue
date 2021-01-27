<template>
  <div>
    <div class="nav-bar">
    	<div class="left"><slot name="left"></slot></div>
    	<div class="center"><slot name="center"></slot></div>
    	<div class="right"><slot name="right"></slot></div>
    </div>
  </div>
</template>

<script>
  export default{
    name: "NavBar"
  }
</script>

<style scoped>
  .nav-bar {
    display: flex;
    height: 44px;
    line-height: 44px;
  }
  .left, .right {
    width: 60px;
    /*background: rgba(0, 238, 118, .5);*/
  }
  .center {
    flex: 1; 
    /*background: rgba(132, 122, 255, .5);*/
    text-align: center; /*此处的文字居中未写到.nav-bar里*/

  }
</style>