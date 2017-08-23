<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id="order-list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border width="100%">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="price" label="金额"></el-table-column>
              <el-table-column prop="count" label="数量"></el-table-column>
              <el-table-column label="操作" fixed="right">
                <template scope="scope">
                  <el-button type="text" size="small">删除</el-button>
                  <el-button type="text" size="small">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-tab-pane>
          <el-tab-pane label="挂单">
            大多数
          </el-tab-pane>
          <el-tab-pane label="外卖">
            颠三倒四多
          </el-tab-pane>
        </el-tabs>


      </el-col>
      <el-col>chanping</el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    name: 'pos',
    data (){
      return {
        tableData:[
          {
            goodsName:'汉堡',
            price:13,
            count:2

          },
          {
            goodsName:'汉堡',
            price:13,
            count:2

          },
          {
            goodsName:'汉堡',
            price:13,
            count:2

          },
          {
            goodsName:'汉堡',
            price:13,
            count:2

          }
        ]
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
</style>
