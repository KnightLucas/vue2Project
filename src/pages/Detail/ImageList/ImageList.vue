<template>
  <div class="swiper-container" ref="mySwiper">
    <div class="swiper-wrapper">
      <div class="swiper-slide" v-for="(img,index) in skuInfo.skuImageList" :key="img.id">
        <img :src="img.imgUrl" :class="{active:currentIndex==index}" @click="changeIndex(index)">
      </div>
    </div>
    <div class="swiper-button-next"></div>
    <div class="swiper-button-prev"></div>
  </div>
</template>

<script>

  import Swiper from 'swiper'
  import { mapGetters } from 'vuex'
  export default {
    name: "ImageList",
    data() {
      return {
        currentIndex:0
      }
    },
    computed:{
      ...mapGetters(["skuInfo"])
    },
    watch:{
      immediate:true,
      skuInfo(newValue,oldValue){
        this.$nextTick(()=>{
          var mySwiper = new Swiper(
            this.$refs.mySwiper,
            {
              // 如果需要前进后退按钮
              navigation: {
                nextEl: ".swiper-button-next",
                prevEl: ".swiper-button-prev",
              },
              //显示三个图片
              slidesPerView:3,
              //每一次切换图片个数
              slidesPerGroup:1
            }
          );
        })
      }
    },
    methods:{
      changeIndex(index){
        this.currentIndex = index
        //通知兄弟组件改变索引
        this.$bus.$emit('getIndex',index)
      }
    }
  }
</script>

<style lang="less" scoped>
  .swiper-container {
    height: 56px;
    width: 412px;
    box-sizing: border-box;
    padding: 0 12px;

    .swiper-slide {
      width: 56px;
      height: 56px;

      img {
        width: 100%;
        height: 100%;
        border: 1px solid #ccc;
        padding: 2px;
        width: 50px;
        height: 50px;
        display: block;

        &.active {
          border: 2px solid #f60;
          padding: 1px;
        }

      }
    }

    .swiper-button-next {
      left: auto;
      right: 0;
    }

    .swiper-button-prev {
      left: 0;
      right: auto;
    }

    .swiper-button-next,
    .swiper-button-prev {
      box-sizing: border-box;
      width: 12px;
      height: 56px;
      background: rgb(235, 235, 235);
      border: 1px solid rgb(204, 204, 204);
      top: 0;
      margin-top: 0;
      &::after {
        font-size: 12px;
      }
    }
  }
</style>