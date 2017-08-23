<template>
  <div class="pos">
    <el-row>
      <el-col :span="8" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border width="100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="price" label="金额"></el-table-column>
              <el-table-column prop="count" label="数量"></el-table-column>
              <el-table-column label="操作" fixed="right">
                <template scope="scope">
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="totalCount">
              <small>数量：</small>
              {{totalCount}}&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
              <small>金额：</small>
              {{totalMoney}}元
            </div>
            <div class="o-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods()">删除</el-button>
              <el-button type="success" @click="checkOut()">结账</el-button>
            
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">
            大多数
          </el-tab-pane>
          <el-tab-pane label="外卖">
            颠三倒四多
          </el-tab-pane>
        </el-tabs>
      
      
      </el-col>
      <el-col :span="16">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span> <span class="o-price">¥{{goods.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span> <span
                    class="foodName">{{goods.goodsName}}</span> <span class="foodPrice">{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span> <span
                    class="foodName">{{goods.goodsName}}</span> <span class="foodPrice">{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span> <span
                    class="foodName">{{goods.goodsName}}</span> <span class="foodPrice">{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span> <span
                    class="foodName">{{goods.goodsName}}</span> <span class="foodPrice">{{goods.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import axios from 'axios'
  
  export default {
    name: 'pos',
    data() {
      return {
        tableData: [],
        oftenGoods: [],
        type0Goods: [],
        type1Goods: [],
        type2Goods: [],
        type3Goods: [],
        totalCount: 0,
        totalMoney: 0
      }
    },
    created: function () {
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
        .then(response => {
          // console.log(response)
          this.oftenGoods = response.data
        })
        .catch(error => {
          alert("网络错误，不能访问")
        })
      
      axios.get('http://jspang.com/DemoApi/typeGoods.php')
        .then(response => {
          // console.log(response)
          this.type0Goods = response.data[0];
          this.type1Goods = response.data[1];
          this.type2Goods = response.data[2];
          this.type3Goods = response.data[3];
        })
        .catch(error => {
          alert("网络错误，不能访问")
        })
    },
    methods: {
      addOrderList(goods) {
        this.totalCount = 0;
        this.totalMoney = 0;
        let isHave = false;
        //首先判断存在订单没有
        for (let i = 0; i < this.tableData.length; i++) {
          if (this.tableData[i].goodsId == goods.goodsId) {
            isHave = true;
          }
        }
        if (isHave) {
          let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
          arr[0].count++;
        } else {
          let newGoods = {goodsId: goods.goodsId, goodsName: goods.goodsName, price: goods.price, count: 1};
          this.tableData.push(newGoods);
        }
        this.getAllMoney();
        
      },
      delSingleGoods: function (goods) {
        this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
        this.getAllMoney();
      },
      getAllMoney: function () {
        this.totalMoney = 0;
        this.totalCount = 0;
        if (this.tableData) {
          this.tableData.forEach((element) => {
            this.totalCount += element.count;
            this.totalMoney = this.totalMoney + (element.price * element.count);
          });
        }
      },
      delAllGoods:function () {
        this.tableData=[];
        this.totalCount=0;
        this.totalCount=0;
      },
      checkOut:function () {
        if(this.totalCount !=0){
          this.tableData=[];
          this.totalCount=0;
          this.totalMoney=0;
          this.$message({
            message:"结账成功！！！",
            type:'success'
          })
        }else {
          this.$message.error('不能空结账，老板也了解你急切的心里！')
        }
      }
    },
    
    mounted: function () {
      var orderHeight = document.body.clientHeight;
      document.getElementById('order-list').style.height = orderHeight + 'px';
    }
  }
</script>

<style scoped>
  .pos-order {
    background-color: #f9fafc;
    border-right: 1px solid #c0ccda;
  }
  
  .el-table th > .cell {
    text-align: center;
  }
  
  .o-btn {
    margin-top: 10px;
  }
  
  .title {
    height: 20px;
    border-bottom: 1px solid #d3dce6;
    background-color: #f9fafc;
    padding: 10px;
    text-align: left;
  }
  
  .often-goods-list ul li {
    list-style: none;
    float: left;
    border: 1px solid #e5e9f2;
    padding: 10px;
    margin: 10px;
    background-color: #ffffff;
    cursor: pointer;
  }
  
  .o-price {
    color: #58b7ff;
  }
  
  .goods-type {
    clear: both;
  }
  
  .cookList li {
    list-style: none;
    width: 23%;
    border: 1px solid #E5E9F2;
    height: auto;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;
    cursor: pointer;
  }
  
  .cookList li span {
    display: block;
    float: left;
  }
  
  .foodImg {
    width: 40%;
  }
  
  .foodName {
    font-size: 18px;
    color: brown;
    width: 60%;
    margin-top: 25px;
  }
  
  .foodPrice {
    font-size: 16px;
    padding-top: 10px;
    width: 60%
  }
  
  .totalCount {
    background-color: #fff;
    padding: 10px;
    border-bottom: 1px solid #E5E9F2;
  }
</style>
