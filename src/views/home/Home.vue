<template>
  <div id="home">
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <HomeSwiper :banners="banners" />
    <recommend-view :recommends="recommends" />
    <FeatureView />
    <tab-control class="tab-control" :titles="['流行','爆款','精选']" @tabClick="tabClick"/>
    <goods-list :goods="showGoods" />
    <ul>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
      <li>ssss</li>
    </ul>
  </div>
</template>

<script>

import HomeSwiper from "./childComps/HomeSwiper";
import RecommendView from "./childComps/RecommendView";
import FeatureView from "./childComps/FeatureView";

import TabControl from "components/content/tabControl/TabControl"
import NavBar from "components/common/navbar/NavBar";
import GoodsList from "../../components/content/goods/GoodsList";

import {getHomeMultidata, getHomeGoods} from "../../network/home";


  export default {
    name: "Home",
    components: {
      GoodsList,
      NavBar,
      HomeSwiper,
      RecommendView,
      FeatureView,
      TabControl
    },
    data() {
      return {
        banners: [],
        recommends: [],
        goods: {
          'pop': {page: 0, list: []},
          'new': {page: 0, list: []},
          'sell': {page: 0, list: []},
        },
        currentType: 'pop',

      }
    },
    computed: {
      showGoods() {
        return this.goods[this.currentType].list
      }
    },
    created() {
    //  1、请求多个数据
      this.getHomeMultidata();
      this.getHomeGoods('pop')
      this.getHomeGoods('new')
      this.getHomeGoods('sell')
    },
    methods: {
      /*
      * 网络请求相关事件
      *
      * */
      getHomeMultidata() {
        getHomeMultidata().then(res => {
          console.log(res);
          this.banners = res.data.banner.list;
          this.recommends = res.data.recommend.list;
        })
      },
      getHomeGoods(type) {
        const page = this.goods[type].page + 1
        getHomeGoods(type, page).then(res => {
          console.log(res);
          this.goods[type].list.push
          (...res.data.list)
          this.goods[type].page += 1
        })
      },

      /*
       * 事件监听事件
       */
      tabClick(index) {
        switch (index) {
          case 0:
            this.currentType = 'pop'
             break
          case 1:
            this.currentType = 'new'
                break
          case 2:
            this.currentType= 'sell'
                break

        }
      }
    }

  }
</script>

<style scoped>
  #home {
    /*padding-top: 44px;*/
    height: 100vh;
    position: relative;
  }

  .home-nav {
    background-color: var(--color-tint);
    color: #fff;

    /*在使用浏览器原生滚动时, 为了让导航不跟随一起滚动*/
    /*position: fixed;*/
    /*left: 0;*/
    /*right: 0;*/
    /*top: 0;*/
    /*z-index: 9;*/
  }

  .content {
    overflow: hidden;

    position: absolute;
    top: 44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

  .tab-control {
    position: relative;
    z-index: 9;
  }
</style>

