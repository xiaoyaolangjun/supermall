<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>

    <tab-control 
      class="tab-control"
      ref="tabControlOne" 
      :titles="['流行','新款','精选']"
      @tabClick="tabClick" v-show="isTabFixed">
    </tab-control>

    <scroll class="content" 
    ref="scroll" 
    :probe-type="3" 
    @scroll="contentScroll"
    :pull-up-load="true"
    @pullingUp="loadMore">
      <home-swiper :banners="banners" @swiperImageLoad="swiperImageLoad"></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control 
        ref="tabControlTwo" 
        :titles="['流行','新款','精选']"
        @tabClick="tabClick">
      </tab-control>
      <goods-list :goods="showGoods"></goods-list>
      <!-- <goods-list :goods="goods['pop'].list"></goods-list> -->
    </scroll>

    <back-top @click.native="backTopClick" v-show="showBoo"></back-top>
  </div>
</template>

<script>
  import NavBar from 'components/common/navbar/NavBar';
  import Scroll from 'components/common/scroll/Scroll'
  import TabControl from 'components/content/tabControl/TabControl';
  import GoodsList from 'components/content/goods/GoodsList';
  import BackTop from 'components/content/backTop/BackTop'

  import HomeSwiper from './childComps/HomeSwiper';
  import RecommendView from './childComps/RecommendView';
  import FeatureView from './childComps/FeatureView';

  import {getHomeMultidata, getHomeGoods} from 'network/home';
  import {debounce} from 'common/utils';

  export default {
    name: "Home",
    components: {
      NavBar,
      Scroll,
      TabControl,
      GoodsList,
      BackTop,

      HomeSwiper,
      RecommendView,
      FeatureView,
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []}
        },
        currentType: 'pop',
        showBoo: false,
        tabOffsetTop: 0,
        isTabFixed: false,
        saveY: 0,
      }
    },
    created() {
      this.getHomeMultidata();
      this.getHomeGoods('pop');
      this.getHomeGoods('new');
      this.getHomeGoods('sell');
    },
    mounted() {
      // 监听item中图片加载完成

      // 调用methods中的防抖函数
      const refresh = debounce(this.$refs.scroll.refresh, 500)
      this.$bus.$on('itemImageLoad', () => {
        refresh();
      })
    },
    destroyed() {
      console.log('destroy');
    },
    activated() {
      this.$refs.scroll.scrollTo(0, this.saveY, 0);
      this.$refs.scroll.refresh();
    },
    deactivated() {
      this.saveY = this.$refs.scroll.getScrollY();
    },
    methods: {

      // 封装防抖函数
      // debounce(func, delay) {
      //   let timer = null;
      //   return function (...args) {
      //     if (timer) {clearTimeout(timer)};
      //     timer = setTimeout(() => {
      //       func.apply(this, args);
      //     },delay)
      //   }
      // },
      tabClick(index){
        switch (index) {
          case 0: this.currentType = 'pop';
            break;
          case 1: this.currentType = 'new';
            break;
          case 2: this.currentType = 'sell';
        }
        this.$refs.tabControlOne.currentIndex = index; 
        this.$refs.tabControlTwo.currentIndex = index; 
      },

      getHomeMultidata() {
        getHomeMultidata().then(res => {
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
          // console.log(res);
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1;
        getHomeGoods(type, page).then(res => {
          this.goods[type].list.push(...res.data.list);
          this.goods[type].page +=1;

          // 完成上拉加载更多
          this.$refs.scroll.finishPullUp();
        })
      },
      backTopClick() {
        this.$refs.scroll.scrollTo(0, 0)
      },
      contentScroll(position) {
        // 判断BackTop是否显示
        this.showBoo = Math.abs(position.y) > 800;
        // this.showBoo = Math.abs(position.y) > 800 ? true : false;

        // 决定tabControl是否吸顶
        this.isTabFixed = Math.abs(position.y) > this.tabOffsetTop;
      },

      loadMore() {
        this.getHomeGoods(this.currentType);
        // this.$refs.scroll.scroll.refresh();
      },
      swiperImageLoad() {
        this.tabOffsetTop = this.$refs.tabControlTwo.$el.offsetTop;
      }
    },
    computed: {
      showGoods(){
        return this.goods[this.currentType].list;
      }
    }
  }
</script>

<style scoped>
  #home {
    position: relative;
    height: 100vh;
  }
  .content {
    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0px;
    right: 0px;
    /*height: calc(100% - 93px);*/
    overflow: hidden;
  }
  .home-nav {
/*    position: fixed;
    left: 0;
    right: 0;
    top: 0;
    z-index: 9;
*/
    background-color: var(--color-tint); 
    color: #fff;
  }

  .tab-control {
    position: relative;
    z-index: 9;
    background: #fff;
/*    
    top: 44px;
    padding: 10px 0;
    background: rgba(0, 12, 212, .1);
*/    
  }

</style>
