<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <ul class="tab-list border-1px">
        <li>
          <a v-link="{path: '/goods'}">商品</a>
        </li>
        <li>
          <a v-link="{path: '/ratings'}">评论</a>
        </li>
        <li>
          <a v-link="{path: '/seller'}">商家</a>
        </li>
      </ul>
    </div>
    <router-view :seller="seller" keep-alive></router-view>
  </div>

</template>

<script type="text/ecmascript-6">
import {urlParse} from 'common/js/util';
import header from './components/header/header';
export default {
  data() {
    return {
      seller: {
        id: (() => {
          let queryParam = urlParse();
          return queryParam.id;
        })()
      }
    };
  },
  created() {
    this.$http.get(`/api/seller?id=${this.seller.id}`).then((res) => {
      res = res.body;
      if (res.errNo) {
        this.seller = Object.assign({}, this.seller, res.data);
        console.log(this.seller);
      }
    });
  },
  components: {
    'v-header': header
  }
};
</script>

<style lang="stylus" type="stylesheet/stylus">
  @import './common/stylus/mixin.styl'

  .tab-list
    display: flex
    width: 100%
    height: .4rem
    line-height: .4rem
    border-1px(rgba(7, 17, 27, 0.1))
    li
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: .14rem
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)
</style>
