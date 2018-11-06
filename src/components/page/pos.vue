<template>

    <div class="pos">
        <el-row>
            <el-col :span="10" class="pos-order" id="order-list">
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
                                <template slot-scope="scope">
                                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">添加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div class="total">
                          <span>  数量：{{this.totalCount}} </span>
                          <span>  金额：{{this.totalMoney}} </span>
                        </div>
                        <div class="divbtn">
                            <el-button type="primary" @click="sellLimit()">挂单</el-button>
                            <el-button type="danger" @click="delAllGoods()">删除</el-button>
                            <el-button type="success" @click="chechout">结账</el-button>
                        </div>

                    </el-tab-pane>
                    <el-tab-pane label="挂单">
                        <el-table :data="sellLimtGoods">
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
                                <template slot-scope="scope">
                                    <el-button type="text" size="small" @click="sellLimitDelSingleGoods(scope.row)">删除</el-button>
                                    <el-button type="text" size="small" @click="">添加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div class="total">
                          <span>  数量：{{this.sellLimtTotalCount}} </span>
                          <span>  金额：{{this.sellLimtTotalMoney}} </span>
                        </div>
                        <div class="divbtn">
                            <el-button type="danger" @click="sellLimtDel()">删除</el-button>
                            <el-button type="success" @click="sellLimtChechout">结账</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="外卖">
                         <div>外卖</div>
                    </el-tab-pane>
                </el-tabs>
            </el-col>
            <el-col :span="14">
                <div class="highfrequency-goods">
                    <div class="tit">常用商品</div>
                    <div>
                        <ul class="goodslist clearfix">
                            <li v-for="goods in oftenCoods" @click='addOrderList(goods)'>
                                <span>{{goods.goodsName}}</span><span class="price">￥{{goods.price}}元</span>
                            </li>
                        </ul>
                    </div>
                    <div class="tit">常用商品</div>
                    <el-tabs>
                        <el-tab-pane  label = "主食">
                            <ul class="goodslist2 clearfix">
                                <li class="clearfix" v-for="goods in type0Goods" @click='addOrderList(goods)' >
                                    <span class="foodimg"><img v-bind:src="goods.goodsImg" alt=""></span>
                                    <div class="fontdinfo">
                                        <p>{{goods.goodsName}}</p>
                                        <p>￥{{goods.price}}元</p>
                                    </div>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane  label = "小吃">
                            <ul class="goodslist2 clearfix">
                                <li class="clearfix" v-for="goods in type1Goods" @click='addOrderList(goods)'>
                                    <span class="foodimg"><img v-bind:src="goods.goodsImg" alt=""></span>
                                    <div class="fontdinfo">
                                        <p>{{goods.goodsName}}</p>
                                        <p>￥{{goods.price}}元</p>
                                    </div>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane  label = "饮料">
                            <ul class="goodslist2 clearfix">
                                <li class="clearfix" v-for="goods in type2Goods" @click='addOrderList(goods)'>
                                    <span class="foodimg"><img v-bind:src="goods.goodsImg" alt=""></span>
                                    <div class="fontdinfo">
                                        <p>{{goods.goodsName}}</p>
                                        <p>￥{{goods.price}}元</p>
                                    </div>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane  label = "套餐">
                            <ul class="goodslist2 clearfix">
                                <li class="clearfix" v-for="goods in type3Goods" @click='addOrderList(goods)'>
                                    <span class="foodimg"><img v-bind:src="goods.goodsImg" alt=""></span>
                                    <div class="fontdinfo">
                                        <p>{{goods.goodsName}}</p>
                                        <p>￥{{goods.price}}元</p>
                                    </div>
                                </li>
                            </ul>
                        </el-tab-pane>
                    </el-tabs>


                </div>
            </el-col>
        </el-row>
    </div>
</template>

<script>
import axios from "axios"; //引入axios
export default {
  name: "pos",
  data() {
    return {
      //已选商品
      tableData: [],
      //高频商品
      oftenCoods: [],
      //分类商品
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalMoney: 0,
      totalCount: 0,
      //挂单商品
      sellLimtGoods:[],
      sellLimtTotalMoney:0,
      sellLimtTotalCount:0
    };
  },
  created: function() {
    //创建时拉起数据
    //常用商品
    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods"
      )
      .then(response => {
        //成功
        console.log(response);
        this.oftenCoods = response.data;
      })
      .catch(error => {
        //失败
        console.log(error);
        alert("网络错误，不能访问");
      });

    //分类商品
    axios
      .get(
        "https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods"
      )
      .then(response => {
        console.log(response);
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(error => {
        alert("网络错误");
      });
  },
  mounted: function() {
    //虚拟DOM加载完成
    var orderHeight = document.body.clientHeight;
    console.log(orderHeight);
    document.getElementById("order-list").style.height = orderHeight + "px";
  },
  methods: {
    addOrderList(goods) {
      //  清零
      this.totalMoney = 0;
      this.totalCount = 0;
      //业务逻辑
      //商品是否在ordrelist内
      let isHave = false;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      //如果存在加数量 如果没有加入数组中
      if (isHave) {
        //改变列表中商品数量
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      this.getAllMoney();
    },
    //删除单个商品
    delSingleGoods(goods) {
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId); //筛选出除了选择删除的数据以外的数据
      this.getAllMoney();
    },
    //删除所有
    delAllGoods(){
        this.tableData=[];
        this.totalCount=0;
        this.totalMoney=0
    },
    //模拟结账
    chechout(){
        if(this.totalCount!=0){
            this.tableData=[];
            this.totalCount=0;
            this.totalMoney=0;
            this.$message({
                message:'结账成功',
                type:'success'
            })
        }else{
            this.$message.error('不能空结')
        }
    },
    //汇总数量和金额
    getAllMoney() {
      this.totalMoney = 0;
      this.totalCount = 0;
      if (this.tableData) {
        //循环计算汇总数量和金额总数
        this.tableData.forEach(element => {
          //箭头函数不影响this作用域
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + (element.price * element.count);
        });
      }
    },
    //挂单汇总数量
    sellLimtGetTotalMoney(){
        this.sellLimtTotalCount = 0;
        this.sellLimtGetAllMoney = 0;
        if(this.sellLimtGoods){
            this.sellLimtGoods.forEach(element=>{
                this.sellLimtTotalCount += element.count;
                this.sellLimtGetAllMoney = this.totalMoney + element.price*element.count
            })
        }
    },
    //挂单
    sellLimit(){
        if(this.tableData && this.sellLimtGoods==0){
            this.sellLimtGoods = this.tableData;
            this.sellLimtTotalMoney = this.totalMoney;
            this.sellLimtTotalCount = this.totalCount;
            this.tableData=[];
            this.totalCount=0;
            this.totalMoney=0;
            this.$message({
                message:'已挂单',
                type:'success'
            })
        }else if(this.sellLimtGoods){
            this.$message.error("不能进行挂单操作，已经有一个未结账挂单，请处理挂单")
        }

    },
    //挂单结账
    sellLimtChechout(){
        if(this.sellLimtTotalCount){
            this.sellLimtTotalCount=0;
            this.sellLimtTotalMoney=0;
            this.sellLimtGoods=[];
            this.$message({
                message:'结账成功',
                type:'success'
            })
        }else{
            this.$message.error('空挂单不能结账')
        }
    },
    //删除挂单
    sellLimtDel(){
        if(this.sellLimtTotalCount){
            this.sellLimtTotalCount=0;
            this.sellLimtTotalMoney=0;
            this.sellLimtGoods=[];
            this.$message({
                message:'挂单删除成功',
                type:'success'
            })
        }else{
            this.$message.error('空挂单不能删除')
        }
    },
    //删除挂单单个商品
    sellLimitDelSingleGoods(goods){
        this.sellLimtGoods=this.sellLimtGoods.filter(o=>o.goodsId!==goods.goodsId);
        sellLimtGetTotalMoney()
    }
  }
};
</script>

<style scoped>
.pos-order {
  background: #f9fafc;
  border-right: 1px solid #dbdcdc;
}
.divbtn {
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
  cursor: pointer;
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
  cursor: pointer;
  padding-bottom: 15px;

  clear: both;

  border-bottom: 1px solid #dbdcdc;

  padding-bottom: 0px;
}
.goodslist2 li {
  cursor: pointer;
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
.goodslist2 li .foodimg {
  width: 50px;
  height: 50px;
  float: left;
  margin-right: 10px;
}
.goodslist2 li .foodimg img {
  width: 50px;
  height: 50px;
}
.goodslist2 li .foodinfo {
  float: left;
}

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
.total {
  text-align: center;
  padding: 10px;
  border-bottom: 1px solid #ededed;
  font-size: 13px;
  background: #fff;
}
.total span {
  margin: 0px 10px;
}
</style>
