<template>

    <div class="pos">
        <el-row>
            <el-col :span="7" class="pos-order" id="order-list">
                <el-tabs>
                    <el-tab-pane label="点餐">
                        <el-table :data="tableData">
                            <el-table-column align="center"
                                prop="goodsName"
                                label="商品名称">
                            </el-table-column>
                            <el-table-column align="center"
                                prop="count"
                                label="数量">
                            </el-table-column>
                            <el-table-column align="center"
                                prop="price"
                                label="金额">
                            </el-table-column>
                            <el-table-column fixed="right" align="center"
                                label="操作">
                                <template scope="scope">
                                    <el-button type="text" size="small">删除</el-button>
                                    <el-button type="text" size="small">添加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div class="divbtn">
                            <el-button type="primary" @click="">挂单</el-button>
                            <el-button type="danger" @click="">删除</el-button>
                            <el-button type="success" @click="">结账</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">
                        <div>
                            挂单
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="外卖">
                         <div>外卖</div>
                    </el-tab-pane>
                </el-tabs>
            </el-col>
            <el-col :span="17">
                <div class="highfrequency-goods">
                    <div class="tit">常用商品</div>
                    <div>
                        <ul class="goodslist clearfix">
                            <li v-for="goods in oftenCoods">
                                <span>{{goods.goodsName}}</span><span class="price">￥{{goods.price}}元</span>
                            </li>
                        </ul>
                    </div>
                    <div class="tit">常用商品</div>
                    <el-tabs>
                        <el-tab-pane  label = "主食">
                            <ul class="goodslist2 clearfix">
                                <li class="clearfix" v-for="goods in type0Goods" >
                                    <span class="foodimg"><img v-bind:src="goods.goodsImg" alt=""></span>
                                    <div class="fontdinfo">
                                        <p>{{goods.goodsName}}</p>
                                        <p>￥{{goods.price}}元</p>
                                    </div>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane  label = "小吃">小吃</el-tab-pane>
                        <el-tab-pane  label = "饮料">饮料</el-tab-pane>
                    </el-tabs>


                </div>
            </el-col>
        </el-row>
    </div>
</template>

<script>
export default {
  name: "pos",
  data() {
    return {
      tableData: [
        {
          goodsName: "汉堡",
          count: "1",
          price: "10"
        },
        {
          goodsName: "薯条",
          count: "1",
          price: "11"
        },
        {
          goodsName: "可口可乐",
          count: "1",
          price: "3.5"
        }
      ],
      oftenCoods: [
        {
          goodsId: 1,
          goodsName: "香辣鸡腿堡",
          price: 18
        },
        {
          goodsId: 1,
          goodsName: "可口可乐",
          price: 3.5
        },
        {
          goodsId: 1,
          goodsName: "薯条",
          price: 11
        },
        {
          goodsId: 1,
          goodsName: "橙子",
          price: 8
        }
      ],
      type0Goods:[
          {
              goodsId:1,
              goodsImg:'http://www.jq22.com/demo/jquerydraw201807262342/images/back1.jpg',
              goodsName:'杭椒小炒',
              price:32
          },
          {
              goodsId:2,
              goodsImg:'http://www.jq22.com/demo/jquerydraw201807262342/images/back1.jpg',
              goodsName:'酸辣土豆丝',
              price:18
          },
          {
              goodsId:3,
              goodsImg:'http://www.jq22.com/demo/jquerydraw201807262342/images/back1.jpg',
              goodsName:'小锅米线',
              price:9
          },
          {
              goodsId:4,
              goodsImg:'../../static/codelogo.png',
              goodsName:'煎饼',
              price:8
          },
      ]
    };
  },
  mounted: function() {
    //虚拟DOM加载完成
    var orderHeight = document.body.clientHeight;
    console.log(orderHeight);
    document.getElementById("order-list").style.height = orderHeight + "px";
  }
};
</script>

<style scoped>
.pos-order {
  background: #f9fafc;
  border-right: 1px solid #dbdcdc;
}
.divbtn{
  padding: 20px 0;
}
.goodslist {
  padding: 15px;

  padding-bottom: 15px;

  clear: both;

  border-bottom: 1px solid #dbdcdc;

  padding-bottom: 0px;
}
.goodslist li {
  float: left;
  border: 1px solid #e6e8ea;
  border-radius: 5px;
  margin-right: 15px;
  margin-bottom: 15px;
  list-style: none;
  text-align: center;
  color: #333;
  padding: 10px 10px;
  background: #fff;
}
.goodslist .price {
  color: #1691fa;
}
.highfrequency-goods .tit {
  height: 38px;
  line-height: 38px;
  border-bottom: 1px solid #dbdcdc;
  text-align: left;
  padding: 0px 13px;
  font-size: 14px;
}

.goodslist2 {
  padding: 15px;

  padding-bottom: 15px;

  clear: both;

  border-bottom: 1px solid #dbdcdc;

  padding-bottom: 0px;
}
.goodslist2 li {
 float: left;
    border: 1px solid #e6e8ea;
    border-radius: 5px;
    margin-right: 15px;
    margin-bottom: 15px;
    list-style: none;
    text-align: center;
    color: #333;
    padding: 10px 10px;
    background: #fff;
    text-align: left;
    width: 164px;
}
.goodslist2 li .foodimg{width: 50px;height: 50px;float: left;margin-right: 10px;}
.goodslist2 li .foodimg img{width: 50px;height: 50px;}
.goodslist2 li .foodinfo{float: left;}

.clearfix:after {
  visibility: hidden;
  display: block;
  font-size: 0;
  content: " ";
  clear: both;
  height: 0;
}

.clearfix {
  *zoom: 1;
}
</style>
