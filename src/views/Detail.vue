<template>
  <!-- 商品详情页 -->
  <div class="detail-page">
    <!-- 详情页头部 -->
    <DetailHeader :list="list"></DetailHeader>
    <!-- 导航部分 -->
    <div class="tab">
      <div class="tab-item">
        <p :class="{show:color}" @click="diancan">点餐</p>
      </div>
      <div class="tab-item">
        <p :class="{show:color1}" @click="pingjia">评价</p>
      </div>
      <div class="tab-item">
        <p :class="{show:color2}" @click="shangjia">商家</p>
      </div>
    </div>
    <!-- 商品列表 -->
    <ShopList :goods="data" v-show="isShow"></ShopList>
    <!-- 评价列表 -->
    <ShopRatings v-show="showRatings" :info="list"></ShopRatings>
    <!-- 商家信息列表 -->
    <ShopInfo v-show="showInfo" :info="list"></ShopInfo>
    <!-- 购物车部分 -->
    <!-- <div class="buyCar">
      <h1>另需配送费</h1>
    </div> -->
  </div>
</template>

<style type="text/css" lang="scss">
.tab {
  height: 40px;
  line-height: 40px;
  background: #fff;
  margin-top: 120px;
  font-size: 20px;
  border-bottom: solid 1px  rgba(7, 17, 27, 0.1);
  .tab-item {
    float: left;
    width: 33.33333%;
    text-align: center;
    font-size: 14px;
    color: rgb(77, 85, 93);
    p {
      display: block;
      position: relative;
      &.show {
        color: #02a774;
        &::after {
          content: "";
          position: absolute;
          left: 50%;
          bottom: 1px;
          width: 35px;
          height: 2px;
          transform: translateX(-50%);
          background: #02a774;
        }
      }
    }
  }
}

// 购物车
.buyCar {
  z-index: 90;
  background-color: #141d27;
  height: 50px;
  width: 100%;
  position: fixed;
  bottom: 0;
  left: 0;
}
</style>

<script type="text/javascript">
import DetailHeader from "../components/DetailHeader";
import ShopList from "../components/ShopList";
import ShopRatings from "../components/ShopRatings";
import ShopInfo from "../components/ShopInfo";
// 组件
export default {
  // 注册组件
  components: { DetailHeader, ShopList, ShopRatings, ShopInfo },
  // 数据
  data() {
    return {
      list: {},
      data: {},
      ratings: {},
      isShow: true,
      showRatings: '',
      showInfo: '',
      color: true,
      color1: '',
      color2: ''
    };
  },
  methods: {
    // 获取数据方法
    getData() {
      let { params } = this.$route;
      this.$http.get("/data/data.json", { params }).then(({ data }) => {
        console.log(data);
        // 存储数据
        this.list = data.info;
        this.data = data.goods;
        this.ratings = data.ratings;
        // console.log(this.list)
      });
    },
    diancan() {
      this.isShow = true;
      this.showRatings = false;
      this.showInfo = false;
      this.color2 = false;
      this.color1 = false;
      this.color = true;
    },
    pingjia() {
      this.isShow = false;
      this.showRatings = true;
      this.showInfo = false;
      this.color = false;
      this.color2 = false;
      this.color1 = true;
    },
    shangjia() {
      this.isShow = false;
      this.showRatings = false;
      this.showInfo = true;
      this.color1 = false;
      this.color = false;
      this.color2 = true;
    }
  }, 
  // 组件创建完成
  created() {
    // 获取数据
    this.getData();
  },
  // 监听器
  watch: {
    // 路由
    $route() {
      // 获取数据
      this.getData();
    }
  }
};
</script>