<template>
  <div>
    <swiper>
      <swiper-item v-for="(items,index) in banners" :key="index">
        <a :href="items.link">
          <img :src="items.image" alt="">
        </a>>
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
	import {Swiper, SwiperItem} from 'components/common/swiper'
  export default{
    name: "HomeSwiper",
    props: {
      banners: {
        type: Array,
        default() {
          return []
        }
      }
    },
    components: {
      Swiper,SwiperItem
    }
  }
</script>

<style scoped>
  
</style>