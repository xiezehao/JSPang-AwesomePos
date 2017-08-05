<template>
  <div>
        <el-col :span='7' class='pos-order' id="order-list">
            <el-tabs>
                <el-tab-pane label="点餐">
                <el-table border style="width:100%;" :data="tableData">
                    <el-table-column label="商品" prop="goodsName"></el-table-column>
                    <el-table-column label="量" prop="count" width="50"></el-table-column>
                    <el-table-column label="金额" prop="price" width="70"></el-table-column>
                    <el-table-column label="操作" width="100">
                        <template scope="scope">
                            <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                            <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                        </template>
                    </el-table-column>
                </el-table>
                <div>
                    数量：{{totalCount}} &nbsp;&nbsp;&nbsp;&nbsp; 总金额：{{totalMoney}}元
                </div>
                <div class="div-btn">
                    <el-button type="warning">挂单</el-button>
                    <el-button type="danger" @click="delAllGoods()">删除</el-button>
                    <el-button type="success" @click="checlOut">结账</el-button>
                </div>
                </el-tab-pane>
                <el-tab-pane label="挂单">
                挂单
                </el-tab-pane>
                <el-tab-pane label="外卖">
                外卖
                </el-tab-pane>
            </el-tabs>
        </el-col>
        <el-col :span='17'>
            <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
                <ul>
                    <li v-for="item in oftenGoods" @click="addOrderList(item)">
                        <span>{{item.goodsName}}</span>
                        <span class="o-price">¥{{item.price}}元</span>
                    </li>
                </ul>

            </div>
            </div>

            <div class="goods-type">
                <el-tabs>
                <el-tab-pane label="汉堡">
                    <div>
                        <ul class="cookList">
                            <li v-for="item in type0Goods" @click="addOrderList(item)">
                                <span class="foodImg"><img :src="item.goodsImg" width="100%"/></span>
                                <span class="foodName">{{item.goodsName}}</span>
                                <span class="foodPrice">￥{{item.price}}元</span>
                            </li>
                        </ul>
                    </div>
                </el-tab-pane>
                <el-tab-pane label="小食">
                    <ul class="cookList">
                        <li v-for="item in type1Goods" @click="addOrderList(item)">
                            <span class="foodImg"><img :src="item.goodsImg" width="100%"/></span>
                            <span class="foodName">{{item.goodsName}}</span>
                            <span class="foodPrice">￥{{item.price}}元</span>
                        </li>
                    </ul>
                </el-tab-pane>
                <el-tab-pane label="饮料">
                    <ul class="cookList">
                        <li v-for="item in type2Goods" @click="addOrderList(item)">
                            <span class="foodImg"><img :src="item.goodsImg" width="100%"/></span>
                            <span class="foodName">{{item.goodsName}}</span>
                            <span class="foodPrice">￥{{item.price}}元</span>
                        </li>
                    </ul>
                </el-tab-pane>
                <el-tab-pane label="套餐">
                    <ul class="cookList">
                        <li v-for="item in type3Goods">
                            <span class="foodImg"><img :src="item.goodsImg" width="100%"/></span>
                            <span class="foodName">{{item.goodsName}}</span>
                            <span class="foodPrice">￥{{item.price}}元</span>
                        </li>
                    </ul>
                </el-tab-pane>
                </el-tabs>
            </div>
        </el-col>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name:'pos',
  data () {
      return {
            msg:"pos",
            tableData: [],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            totalMoney:0,
            totalCount:0,
        }
  },
  created () {
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(response=>{
        // console.log(response);
        this.oftenGoods=response.data;
      })
      .catch(error=>{
        console.log(error);
        alert("网络不好");
      });

      axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(response=>{
        // console.log(response);
        this.type0Goods=response.data[0];
        this.type1Goods=response.data[1];
        this.type2Goods=response.data[2];
        this.type3Goods=response.data[3];
      })
      .catch(error=>{
        console.log(error);
        alert("网络不好");
      });
  },
  mounted () {
      var orderHight=document.body.clientHeight;
      document.getElementById('order-list').style.height=orderHight+"px";
  },
  methods: {
      addOrderList(goods){
        //判断是否这个商品已经存在于订单列表
        let isHave=false;
        for (var i = 0; i < this.tableData.length; i++) {
            if (this.tableData[i].goodsId==goods.goodsId) {
                isHave=true;
                //若存在则数量+1
                // this.tableData[i].count++;
                break;
            }
        }

        //根据isHave的值判断订单列表中是否已经有此商品
        if (isHave) {
            // let arr=this.tableData.filter(o=>o.goodsId==goods.goodsId);
            // console.log(arr);
            //这是this.tableData.filter(o=>o.goodsId==goods.goodsId);原型
            let arr=this.tableData.filter(function(params, index, array) {
                if (params.goodsId==goods.goodsId) {
                    console.log(params);
                    console.log(index);
                    console.log(array);
                    return true;
                }
                return false;
            });
            arr[0].count++;
        }else{
            let newGoods={
                goodsId:goods.goodsId,
                goodsName:goods.goodsName,
                price:goods.price,
                count:1
            };
            this.tableData.push(newGoods);
        }
        //计算总金额和总数量
        this.getTotal();
      },
      delSingleGoods(goods){
        this.tableData=this.tableData.filter(o=>o.goodsId!=goods.goodsId);
        //计算总金额和总数量
        this.getTotal();
      },
      //计算总金额和总数量
      getTotal(){
        this.totalCount=0;
        this.totalMoney=0;
        this.tableData.forEach(function(element) {
            this.totalCount+=element.count;
            this.totalMoney+=element.count*element.price;
        }, this);
      },
      //删除所有商品
      delAllGoods(){
          this.tableData=[];
          this.totalMoney=0;
          this.totalCount=0;
      },
      //结账
      checlOut(){
          if (this.totalCount!=0) {
              this.delAllGoods();
              this.$message({
                message:"结账成功！"
              });
          }else{
              this.$message.error("不能空接");
          }
      }
  }
}
</script>

<style scoped>
div{
    text-align: center;
}
.pos-order{
    background-color: #f9fafc;
    border-right: 1px solid #c0ccda;
    height: 100%;
}
.div-btn{
    margin-top: 10px;
}
.title{
    height: 20px;
    border-bottom: 1px solid #d3dce6;
    background-color: #f9fafc;
    padding: 10px;
}
.often-goods-list ul li{
    list-style: none;
    float: left;
    border: 1px solid #e5e9f2;
    padding: 10px;
    margin: 5px;
    background-color: #fff;
}
.o-price{
    color: #58b7ff;
}
.goods-type{
    clear: both;
}
.cookList li{
    list-style: none;
    width: 23%;
    border: 1px solid #e5e9f2;
    height: auto;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;
}
.cookList li span{
    display: block;
    float: left;
}
.foodImg{
    width: 40%;
}
.foodName{
    font-size: 18px;
    padding-left: 10px;
    color: brown;
}
.foodPrice{
    font-size: 16px;
    padding-left: 10px;
    padding-top: 10px;
}
</style>

