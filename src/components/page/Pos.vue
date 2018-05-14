<template>
  <div class="pos">
      <div>
          <el-row>
              <el-col :span='7' id="order-list">
                <el-tabs type="card" style="width: 100%">
                    <el-tab-pane label="点餐">
                        <el-table :data="tableData" border style="width: 100%">
                            <el-table-column prop="goodsName" label="商品"></el-table-column>
                            <el-table-column prop="count" label="数量" width="60"></el-table-column>
                            <el-table-column prop="price" label="金额" width="60"></el-table-column>
                            <el-table-column label="操作" fixed="right" width="100">
                                <template slot-scope="scope">
                                    <el-button type="text" size="small">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                            </el-table-column>
                        </el-table>
                        <div style="margin-top:5px">
                          <small>数量：</small>{{totalCount}}<small>个</small> <small>金额：</small>{{totalMoney}}<small>元</small>
                        </div>
                        <div class="btn" style="clear:both">
                            <el-button type="warning">挂单</el-button>
                            <el-button type="danger">删除</el-button>
                            <el-button type="success">结账</el-button>
                        </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单">挂单</el-tab-pane>
                    <el-tab-pane label="外卖">外卖</el-tab-pane>
                </el-tabs>
              </el-col>
              <el-col :span='17'>
                  <div class="popularGoods">
                      <div class="title">热销商品</div>
                      <div class="popularGoodsList">
                          <ul>
                              <li v-for="goods in popularGoods" @click="addOrderList(goods)">
                                  <span>{{goods.goodsName}}</span>
                                  <span class="o-price">￥{{goods.price}}元</span>
                              </li>
                          </ul>
                      </div>
                  </div>
                  <div class="goodType">
                      <el-tabs type="card">
                          <el-tab-pane label="汉堡">
                              <ul class="cookList">
                                  <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                                      <span><img :src="goods.goodsImg" width="100%"></span>
                                      <span>{{goods.goodsName}}</span>
                                      <span>￥{{goods.price}}元</span>
                                  </li>
                              </ul>
                          </el-tab-pane>
                          <el-tab-pane label="小食">
                              <ul class="cookList">
                                  <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                                      <span><img :src="goods.goodsImg" width="100%"></span>
                                      <span>{{goods.goodsName}}</span>
                                      <span>￥{{goods.price}}元</span>
                                  </li>
                              </ul>
                          </el-tab-pane>
                          <el-tab-pane label="饮料">
                              <ul class="cookList">
                                  <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                                      <span><img :src="goods.goodsImg" width="100%"></span>
                                      <span>{{goods.goodsName}}</span>
                                      <span>￥{{goods.price}}元</span>
                                  </li>
                              </ul>
                          </el-tab-pane>
                          <el-tab-pane label="套餐">
                             <ul class="cookList">
                                  <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                                      <span><img :src="goods.goodsImg" width="100%"></span>
                                      <span>{{goods.goodsName}}</span>
                                      <span>￥{{goods.price}}元</span>
                                  </li>
                              </ul>
                          </el-tab-pane>
                      </el-tabs>
                  </div>
              </el-col>
          </el-row>
      </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Pos",
  data() {
    return {
      tableData: [
      ],
      popularGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalCount:0,
      totalMoney:0
    };
  },
  created: function() {
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(response => {
        this.popularGoods = response.data;
      })
      .catch(error => {
        alert("服务器炸了");
      });
    axios
      .get("http://jspang.com/DemoApi/typeGoods.php")
      .then(response => {
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      })
      .catch(error => {
        alert("服务器炸了");
      });
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order-list").style.height = orderHeight + "px";
  },
  methods:{
    //商品添加到订单
    addOrderList(goods){
      this.totalCount=0;
      this.totalMoney=0;
      let isHave=false;
      //遍历一下订单里是否存在订单列表
      for(let i=0;i<this.tableData.length;i++){
        console.log(this.tableData[i].goodsId);
        if(this.tableData[i].goodsId==goods.goodsId){
          isHave=true;
        }
      }
      if(isHave==true){
        let arr=this.tableData.filter(o=>o.goodsId == goods.goodsId);
        arr[0].count++;
      }else{
        let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1}
        this.tableData.push(newGoods);
      }

      this.tableData.forEach((element) => {
        this.totalCount+=element.count;
        this.totalMoney=this.totalMoney+(element.price*element.count);
      })
    }
  }
};
</script>

<style>
#order-list {
  border-right: 1px solid #c0ccda;
  background-color: #f9fafc;
}
.el-table th {
  background: #f4f4f4;
  font-size: 16px;
  text-align: center;
}
div.btn {
  margin-top: 10px;
}
.title {
  height: 20px;
  background: #f9fafc;
  padding: 10px;
  border-bottom: 1px solid #d3dce6;
  text-align: left;
}
.popularGoodsList ul {
  height: 300px;
}

.popularGoodsList li {
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background: #fff;
  border-radius: 5px;
}
.o-price {
  color: #58b7ff;
}
.goodType {
  clear: both;
  background-color: #f9fafc;
  height: 600px;
}
.cookList li {
  float: left;
  width: 23%;
  border: 1px solid #e5e9f2;
  padding: 2px;
  margin: 2px;
  overflow: hidden;
  height: auto;
}
.cookList li span {
  display: block;
  float: left;
}
.cookList li span:nth-of-type(1) {
  width: 40%;
}
.cookList li span:nth-of-type(2) {
  width: 50%;
  font-size: 18px;
  padding-left: 10px;
  padding-top: 10px;
  color: brown;
  text-align: left;
}
.cookList li span:nth-of-type(3) {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
  text-align: center;
}
</style>
