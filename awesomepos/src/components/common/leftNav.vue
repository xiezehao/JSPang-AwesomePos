<template>
  <div class="left-nav">
       <ul>
          <li>
              <i class="icon iconfont icon-goumai"></i>
              <div>收银</div>
          </li>
          <li>
              <i class="icon iconfont icon-dianpu"></i>
              <div>店铺</div>
          </li>
          <li>
              <i class="icon iconfont icon-hanbao"></i>
              <div>商品</div>
          </li>
          <li>
              <i class="icon iconfont icon-huiyuanqia"></i>
              <div>会员</div>
          </li>

          <li>
              <i class="icon iconfont icon-tongji"></i>
              <div>统计</div>
          </li>

          <li>
              <i class="icon iconfont icon-goumai"></i>
              <div>设置</div>
          </li>
      </ul> 
  </div>
</template>

<script>
export default {
  data () {
      return {
          
      }
  }
}
</script>

<style>
.left-nav{
    color: #fff;
    font-size: 10px;
    height: 100%;
    background-color: #1d8ced;
    float: left;
    width: 5%;
}
.iconfont{
    font-size: 24px;

}
.left-nav ul{
    margin: 0px;
    padding: 0px;
}
.left-nav li{
    list-style: none;
    text-align: center;
    border-bottom: 1px solid #20a0ff;
    padding: 10px;
}
</style>
