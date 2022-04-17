<template>
  <view>
    <!-- 轮播图区域 -->
    <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
      <!-- 循环渲染轮播图的 item 项 -->
      <swiper-item v-for="(item, i) in swiperList" :key="i">
        <navigator class="swiper-item" :url="'/subpkg/goods_detail/goods_detail?goods_id=' + item.goods_id">
          <!-- 动态绑定图片的 src 属性 -->
          <image :src="item.image_src"></image>
        </navigator>
      </swiper-item>
    </swiper>

    <!-- 分类导航区 -->
    <view class="nav-list">
      <view class="nav-item" v-for="(item, i) in navList" :key="i" @click="navClickHandler(item)">
        <!-- 注意:src= -->
        <image :src="item.image_src" class="nav-img"></image>
      </view>
    </view>

    <!-- 楼层区域 -->
    <view class="floor-list">
      <!-- 楼层item项 -->
      <view class="floor-item" v-for="(item, i) in floorList" :key="i">
        <!-- 楼层标题 -->
        <image :src="item.floor_title.image_src" class="floor-title"></image>
        <!-- 楼层图片区域 -->
        <view class="floor-img-box">
          <!-- 左侧大图的盒子 -->
          <navigator class="left-img-box" :url="item.product_list[0].url">
               <image :src="item.product_list[0].image_src" :style="{width: item.product_list[0].image_width + 'rpx'}" mode="widthFix"></image>
          </navigator>
          <!-- 右侧4个小图片的盒子 -->
          <view class="right-img-box">
               <navigator class="right-img-item" v-for="(item2, i2) in item.product_list" :key="i2" v-if="i2 !== 0" :url="item2.url">
                 <image :src="item2.image_src" mode="widthFix" :style="{width: item2.image_width + 'rpx'}"></image>
               </navigator>
             </view>
        </view>
      </view>
    </view>


  </view>
</template>

<script>
  export default {
    data() {
      return {
        // 1. 轮播图的数据列表，默认为空数组 
        swiperList: [],
        // 分类的数据
        navList: [],
        //楼层的数据
        floorList: []
      }
    },
    onLoad() {
      // 在小程序页面刚加载的时候，调用获取轮播图数据的方法 
      this.getSwiperList(),
        // 导航标签 
        this.getNavList(),
        // 楼层区域
        this.getFloorList()
    },
    methods: {
      // 获取轮播图
      async getSwiperList() {
        // 发起请求
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/home/swiperdata')
        // 请求失败
        if (res.meta.status !== 200) {
          return uni.$showMsg();
        }
        // 请求成功，为 data 中的数据赋值
        this.swiperList = res.message
      },

      // 获取标签
      async getNavList() {
        // 发起请求
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/home/catitems')
        //请求失败
        if (res.meta.status !== 200) {
          return uni.$showMsg();
        }
        //请求成功，为data中的参数赋值
        this.navList = res.message
      },

      // 获取楼层
      async getFloorList() {
        const {
          data: res
        } = await uni.$http.get('/api/public/v1/home/floordata')
        if (res.meta.status !== 200) {
          return uni.$showMsg();
        }
        //通过双层forEach循环，处理URL地址
        res.message.forEach(floor => {
          floor.product_list.forEach(prod => {
            prod.url = '/subpkg/goods_list/goods_list?' + prod.navigator_url.split('?')[1]
          })
        })
        //请求成功，为data中的参数赋值
        this.floorList = res.message
      },

      // 点击分类图片进行跳转
      navClickHandler(item) {
        // 判断是哪个分类
        if (item.name === '分类') {
          uni.switchTab({
            url: '/pages/cate/cate'
          })
        }
      }
    }
  }
</script>

<style lang="scss">
  swiper {
    height: 330rpx;

    .swiper-item,
    image {
      width: 100%;
      height: 100%;
    }
  }

  .nav-list {
    display: flex;
    justify-content: space-around;
    margin: 10px 0;

    .nav-img {
      width: 128rpx;
      height: 140rpx;
    }
  }
  
  .floor-title{
    height: 60rpx;
    width: 100%;
    display: flex;
  }
  .right-img-box {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }
  .floor-img-box {
    display: flex;
    padding-left: 10rpx;
  }
</style>
