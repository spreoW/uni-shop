<template>
  <view>
    <image src="../../static/my-icons/icon2.png"></image>
    <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
      <swiper-item>
        <view class="swiper-item">
          <!-- 动态绑定图片的 src 属性 -->
          <image src="https://www.uinav.com/pyg/banner1.png"></image>
        </view>
      </swiper-item>
      <swiper-item>
        <view class="swiper-item">
          <image src="../../static/tab_icons/cate-active.png"></image>
        </view>
      </swiper-item>
    </swiper>
  </view>
</template>

<script>
  export default {
    data() {
      return {
        
      };
    }
  }
</script>

<style lang="scss">

</style>
