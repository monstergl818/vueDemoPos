<template>
    <div class="left-nav">
        <ul>
            <li>
                <i class="icon iconfont icon-qianmoney113"></i>
                <div>收银</div>
            </li>
            <li>
                <i class="icon iconfont icon-shop"></i>
                <div>店铺</div>
            </li>
            <li>
                <i class="icon iconfont icon-hanbao"></i>
                <div>商品</div>
            </li>
             <li>
                <i class="icon iconfont icon-huiyuanqiagoumai"></i>
                <div>会员</div>
            </li>
            <li>
                <i class="icon iconfont icon-tongji"></i>
                <div>统计</div>
            </li>
             <li>
                <i class="icon iconfont icon-tongji"></i>
                <div>设置</div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
  name: "leftnav"
};
</script>

<style scoped>
.left-nav {
  color: #fff;
  font-size: 10px;
  height: 100%;
  background-color: #1d8ce0;
  float: left;
  width: 5%;
}
.iconfont {
  font-size: 24px;
}
.left-nav ul {
  padding: 0px;
  margin: 0px;
}
.left-nav ul li {
  list-style: none;
  text-align: center;
  border-bottom: 1px solid #3e9ffa;
  height:auto;
  padding: 10px 0
}

</style>
